# Authentication

Authentication is crucial for ensuring that only authorized users can access private APIs. This document outlines the authentication mechanisms used for EdgeX V2 public and private APIs, based on the official Golang SDK implementation.

---

## Public API

Public APIs do not require authentication. These interfaces are accessible to anyone without the need for any credentials.

```text
No authentication is required for public endpoints such as:
- /api/v2/public/meta/*
- /api/v2/public/quote/*
- /api/v2/public/funding/*
```

---

## Private API Authentication

Private APIs require **HMAC-SHA256 signature authentication** to ensure that only authorized users can access them. Authentication is achieved using custom HTTP headers that include a timestamp and a cryptographic signature.

### Authentication Headers

The following headers **must** be included in requests to private API endpoints:

| Header Name | Type | Required | Description |
|------------|------|----------|-------------|
| `X-edgeX-Api-Key` | string | true | Your API Key (obtained from EdgeX platform) |
| `X-edgeX-Passphrase` | string | true | Your API Passphrase (set during API key creation) |
| `X-edgeX-Api-Timestamp` | string | true | Request timestamp in milliseconds (Unix timestamp) |
| `X-edgeX-Api-Signature` | string | true | HMAC-SHA256 signature of the request |

### API Credentials

To use private APIs, you need to obtain three credentials from the EdgeX platform:

1. **API Key** (`apiKey`) - Unique identifier for your application
2. **API Passphrase** (`apiPassphrase`) - Password you set when creating the API key
3. **API Secret** (`apiSecret`) - Secret string used for signing requests

**⚠️ Security Warning**: Keep your API Secret secure and never share it or commit it to version control. Anyone with access to your API Secret can make requests on your behalf.

### How to Get API Credentials

#### Step 1: Navigate to API Management

1. **Log in** to EdgeX platform
2. Click your **profile icon** in the top-right corner
3. Select **"API Management"** from the dropdown menu
4. Go to the **"Perps V2"** tab

#### Step 2: Generate API Credentials

1. Find your account (e.g., "Main Account") in the list
2. In the **SDK Signer** column, click the **"Detail"** button
3. A dialog will appear showing all credentials:
   - **API Key** - For authentication
   - **Secret** - For HMAC signature
   - **Passphrase** - Your set password
4. **Copy and save all credentials immediately**

For Signer Key retrieval steps and security notes, see [Signer Key Signature Guide](sign.md#obtaining-your-signer-key).

<figure><img src="../../.gitbook/assets/get-api-credentials.png" alt=""><figcaption><p>How to get API credentials from EdgeX dashboard</p></figcaption></figure>

**⚠️ Security Warnings**: 
- All credentials are shown **only once**. Save them immediately in a secure location.
- Each address can create a maximum of 10 L2 Keys (Signer Keys).
- Keep your credentials secure to prevent possible asset losses.
- Binding your L2 Key to third-party platforms may pose security risks.

---

## Signature Generation

### Signature Algorithm: HMAC-SHA256

EdgeX uses **HMAC-SHA256** (Hash-based Message Authentication Code with SHA-256) for request signing. This ensures:
- **Authenticity**: Verifies the request comes from the legitimate API key owner
- **Integrity**: Ensures the request hasn't been tampered with
- **Replay Protection**: Timestamps prevent old requests from being reused

### Signature Components

The signature is generated from the following components concatenated in order:

| Component | Description | Example |
|-----------|-------------|---------|
| **Timestamp** | Request timestamp in milliseconds | `1773302047172` |
| **HTTP Method** | Request method in UPPERCASE | `GET`, `POST` |
| **Request URI** | Full request path | `/api/v2/private/account/getAccountAsset` |
| **Request Body** | Sorted query string or request body | `accountId=724625476626153743&size=10` |

### Signature Formula

```
signatureMessage = timestamp + method + requestURI + requestBody
signature = HMAC-SHA256(base64(apiSecret), signatureMessage)
```

### Step-by-Step Signature Process

#### Step 1: Generate Timestamp

```go
timestamp := time.Now().UnixMilli()
timestampStr := fmt.Sprintf("%d", timestamp)
// Example: "1773302047172"
```

#### Step 2: Build Request URI

For private endpoints, ensure the URI includes `/api` prefix:

```go
requestURI := "/api/v2/private/account/getAccountAsset"
```

If the path contains "private" but doesn't start with "/api", add it:

```go
if strings.Contains(path, "private") && !strings.HasPrefix(path, "/api") {
    requestURI = "/api" + path
}
```

#### Step 3: Build Request Body String

**For GET requests**, sort query parameters alphabetically:

```go
// Example parameters
params := map[string]string{
    "accountId": "724625476626153743",
    "size": "10",
    "filterTypeList": "SETTLE_FUNDING_FEE",
}

// Sort keys alphabetically
keys := []string{"accountId", "filterTypeList", "size"}
sort.Strings(keys)

// Build query string
requestBody := "accountId=724625476626153743&filterTypeList=SETTLE_FUNDING_FEE&size=10"
```

**For POST requests**, convert request body to sorted key-value pairs:

```go
// Example request body
data := map[string]interface{}{
    "contractId": "10000001",
    "accountId": "724625476626153743",
    "price": "97463.4",
}

// Convert to sorted string
requestBody := "accountId=724625476626153743&contractId=10000001&price=97463.4"
```

#### Step 4: Create Signature Message

```go
message := timestamp + method + requestURI + requestBody

// Example:
// "1773302047172GET/api/v2/private/account/getAccountAssetaccountId=724625476626153743&size=10"
```

#### Step 5: Compute HMAC-SHA256 Signature

```go
import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/base64"
    "encoding/hex"
)

// Base64 encode the API Secret
base64Key := base64.StdEncoding.EncodeToString([]byte(apiSecret))

// Create HMAC with SHA256
mac := hmac.New(sha256.New, []byte(base64Key))
mac.Write([]byte(message))

// Get hex-encoded signature
signature := hex.EncodeToString(mac.Sum(nil))
```

---

## Complete Golang Example

### Basic Authentication

```go
package main

import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/base64"
    "encoding/hex"
    "fmt"
    "net/http"
    "sort"
    "strings"
    "time"
)

// API Credentials (example values - replace with your actual credentials)
const (
    apiKey        = "your-api-key"
    apiPassphrase = "your-api-passphrase"
    apiSecret     = "your-api-secret"
)

// BuildSortedQueryString sorts parameters alphabetically and builds query string
func BuildSortedQueryString(params map[string]string) string {
    if len(params) == 0 {
        return ""
    }
    
    // Get sorted keys
    keys := make([]string, 0, len(params))
    for k := range params {
        if params[k] != "" {  // Skip empty values
            keys = append(keys, k)
        }
    }
    sort.Strings(keys)
    
    // Build key=value pairs
    pairs := make([]string, 0, len(keys))
    for _, k := range keys {
        pairs = append(pairs, fmt.Sprintf("%s=%s", k, params[k]))
    }
    
    return strings.Join(pairs, "&")
}

// BuildHMACSignature creates HMAC-SHA256 signature
func BuildHMACSignature(timestamp, method, requestURI, requestBody, apiSecret string) string {
    // Create signature message
    message := timestamp + method + requestURI + requestBody
    
    // Base64 encode API Secret
    base64Key := base64.StdEncoding.EncodeToString([]byte(apiSecret))
    
    // Compute HMAC-SHA256
    mac := hmac.New(sha256.New, []byte(base64Key))
    mac.Write([]byte(message))
    
    // Return hex-encoded signature
    return hex.EncodeToString(mac.Sum(nil))
}

// MakeAuthenticatedRequest makes an authenticated request to EdgeX API
func MakeAuthenticatedRequest(method, path string, params map[string]string) (*http.Request, error) {
    // 1. Generate timestamp
    timestamp := fmt.Sprintf("%d", time.Now().UnixMilli())
    
    // 2. Build request URI (ensure /api prefix for private endpoints)
    requestURI := path
    if strings.Contains(path, "private") && !strings.HasPrefix(path, "/api") {
        requestURI = "/api" + path
    }
    
    // 3. Build request body (for GET requests, this is the sorted query string)
    requestBody := BuildSortedQueryString(params)
    
    // 4. Build full URL
    baseURL := "https://edgex-testnet-internal-v2.edgex.exchange"
    fullURL := baseURL + path
    if method == "GET" && requestBody != "" {
        fullURL += "?" + requestBody
    }
    
    // 5. Generate signature
    signature := BuildHMACSignature(timestamp, method, requestURI, requestBody, apiSecret)
    
    // 6. Create HTTP request
    req, err := http.NewRequest(method, fullURL, nil)
    if err != nil {
        return nil, err
    }
    
    // 7. Add authentication headers
    req.Header.Set("X-edgeX-Api-Key", apiKey)
    req.Header.Set("X-edgeX-Passphrase", apiPassphrase)
    req.Header.Set("X-edgeX-Api-Signature", signature)
    req.Header.Set("X-edgeX-Api-Timestamp", timestamp)
    req.Header.Set("Accept", "application/json")
    
    return req, nil
}

func main() {
    // Example: Get account asset
    req, err := MakeAuthenticatedRequest(
        "GET",
        "/api/v2/private/account/getAccountAsset",
        map[string]string{
            "accountId": "724625476626153743",
        },
    )
    if err != nil {
        fmt.Printf("Error: %v\n", err)
        return
    }
    
    // Print request details
    fmt.Printf("URL: %s\n", req.URL.String())
    fmt.Printf("Method: %s\n", req.Method)
    fmt.Printf("Headers:\n")
    for key, values := range req.Header {
        fmt.Printf("  %s: %s\n", key, values[0])
    }
    
    // Execute request
    client := &http.Client{}
    resp, err := client.Do(req)
    if err != nil {
        fmt.Printf("Request failed: %v\n", err)
        return
    }
    defer resp.Body.Close()
    
    fmt.Printf("Response Status: %s\n", resp.Status)
}
```

### Using EdgeX Golang SDK

The EdgeX Golang SDK handles all authentication automatically:

```go
package main

import (
    "context"
    "fmt"
    "log"
    
    "github.com/edgex-Tech/edgex-golang-sdk/sdk"
)

func main() {
    // Create client with API credentials
    client, err := sdk.NewClient(&sdk.Config{
        BaseURL:       "https://edgex-testnet-internal-v2.edgex.exchange",
        AccountID:     724625476626153743,
        APIKey:        "your-api-key",
        APIPassphrase: "your-api-passphrase",
        APISecret:     "your-api-secret",
    })
    if err != nil {
        log.Fatal(err)
    }
    
    ctx := context.Background()
    
    // SDK handles authentication automatically
    accountAsset, err := client.GetAccountAsset(ctx)
    if err != nil {
        log.Fatal(err)
    }
    
    fmt.Printf("Account ID: %s\n", accountAsset.Data.Account.Id)
    fmt.Printf("Collateral: %v\n", accountAsset.Data.CollateralList)
}
```

---

## CURL Example

```bash
#!/bin/bash

API_KEY="your-api-key"
API_PASSPHRASE="your-api-passphrase"
API_SECRET="your-api-secret"
TIMESTAMP=$(date +%s%3N)  # Milliseconds timestamp
METHOD="GET"
REQUEST_URI="/api/v2/private/account/getAccountAsset"
REQUEST_BODY="accountId=724625476626153743"

# Build signature message
MESSAGE="${TIMESTAMP}${METHOD}${REQUEST_URI}${REQUEST_BODY}"

# Base64 encode API secret
BASE64_KEY=$(echo -n "$API_SECRET" | base64)

# Generate HMAC-SHA256 signature
SIGNATURE=$(echo -n "$MESSAGE" | openssl dgst -sha256 -hmac "$BASE64_KEY" -hex | awk '{print $2}')

# Make request
curl --location --request GET \
  "https://edgex-testnet-internal-v2.edgex.exchange${REQUEST_URI}?${REQUEST_BODY}" \
  --header "X-edgeX-Api-Key: ${API_KEY}" \
  --header "X-edgeX-Passphrase: ${API_PASSPHRASE}" \
  --header "X-edgeX-Api-Signature: ${SIGNATURE}" \
  --header "X-edgeX-Api-Timestamp: ${TIMESTAMP}" \
  --header "Accept: application/json"
```

---

## Request Body Serialization Rules

When building the request body string for signature, follow these rules:

### 1. NULL Values
- Treat as empty string: `""`

### 2. Primitive Values
- String: Use as-is
- Boolean: Lowercase `"true"` or `"false"`
- Numbers: Convert to string

### 3. Arrays
- If empty: return `""`
- If non-empty: join values with `&`
- Example: `["value1", "value2"]` → `"value1&value2"`

### 4. Objects
- Convert to sorted key=value pairs
- Sort keys alphabetically
- Join with `&`
- Example: `{"b": "2", "a": "1"}` → `"a=1&b=2"`

### Golang Implementation

```go
func GetValue(data interface{}) string {
    switch v := data.(type) {
    case nil:
        return ""
    case string:
        return v
    case bool:
        return strings.ToLower(fmt.Sprintf("%v", v))
    case int, int32, int64, float32, float64:
        return fmt.Sprintf("%v", v)
    case []interface{}:
        if len(v) == 0 {
            return ""
        }
        var values []string
        for _, item := range v {
            values = append(values, GetValue(item))
        }
        return strings.Join(values, "&")
    case map[string]interface{}:
        // Convert to sorted map
        sortedMap := make(map[string]string)
        for key, val := range v {
            sortedMap[key] = GetValue(val)
        }
        
        // Sort keys
        keys := make([]string, 0, len(sortedMap))
        for k := range sortedMap {
            keys = append(keys, k)
        }
        sort.Strings(keys)
        
        // Build key=value pairs
        var pairs []string
        for _, k := range keys {
            pairs = append(pairs, fmt.Sprintf("%s=%s", k, sortedMap[k]))
        }
        return strings.Join(pairs, "&")
    default:
        return fmt.Sprintf("%v", v)
    }
}
```

---

## Common Errors

### Error: Invalid Signature

**Cause**: Signature doesn't match server calculation

**Solutions**:
1. Check timestamp is current (within 60 seconds)
2. Verify all signature components are correct
3. Ensure request body is sorted alphabetically
4. Check for trailing spaces in parameters
5. Verify API Secret is correct and properly base64 encoded

### Error: Invalid Timestamp

**Cause**: Request timestamp is too old or in the future

**Solutions**:
1. Ensure your system clock is accurate
2. Use current timestamp in milliseconds
3. Timestamp should be within ±60 seconds of server time

### Error: Invalid API Key

**Cause**: API Key not found or inactive

**Solutions**:
1. Verify API Key is correct
2. Check if API Key is still active
3. Ensure API Key has correct permissions

---

## Security Best Practices

1. **Never expose your API Secret**
   - Don't commit to git
   - Don't log in production
   - Use environment variables

2. **Rotate credentials periodically**
   - Change API keys every 90 days
   - Immediately rotate if compromised

3. **Use HTTPS only**
   - Never send credentials over HTTP
   - Verify SSL certificates

4. **Implement rate limiting**
   - Respect API rate limits
   - Implement exponential backoff

5. **Monitor API usage**
   - Track unusual activity
   - Set up alerts for suspicious patterns

---

**Document Version**: V2.0  
**Last Updated**: 2026-03-12  
**Based On**: EdgeX Golang SDK V2 Implementation
