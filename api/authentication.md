# Authentication

Authentication is crucial for ensuring that only authorized users can access private APIs. This document outlines the authentication mechanisms used for public and private APIs.

## Public API

Public APIs do not require authentication. These interfaces are accessible to anyone without the need for any credentials.

```text
No authentication is required for public interfaces.
```

## Private API

Private APIs require authentication to ensure that only authorized users can access them. Authentication is achieved using custom headers that include a timestamp and a signature.

### Auth Header

The following headers must be included in the request to authenticate access to private APIs:

| Name                    | Location | Type    | Required | Description                                                                 |
| ----------------------- | -------- | ------- | -------- | --------------------------------------------------------------------------- |
| `X-edgeX-Api-Timestamp` | header   | string  | must     | The timestamp when the request was made. This helps prevent replay attacks. |
| `X-edgeX-Api-Signature` | header   | string  | must     | The signature generated using the private key and request details.          |

### CURL Examble

```curl
curl --location --request GET 'https://pro.edgex.exchange/api/v1/private/account/getPositionTransactionPage?filterTypeList=SETTLE_FUNDING_FEE&size=10&accountId=544159487963955214' \
--header 'X-edgeX-Api-Signature: 06d28020763542c0afc296dc8743797c6fda8ea9727745b57b671f70326dfed6077cd******************************aff3162e39d05d9df1c3ddf9648650382d6e62ff1076b14c0e6c687088d3917d8490e5412a080a6e9ea940c720ddd' \
--header 'X-edgeX-Api-Timestamp: 1736313025024'
```

### Signature Elements

The signature is generated using the following elements:

| **Signature Element**                  | **Description**                                                                 |
|----------------------------------------|---------------------------------------------------------------------------------|
| `X-edgeX-Api-Timestamp`                | The timestamp when the request was made. This is retrieved from the request header. |
| **Request Method (Uppercase)**         | The HTTP method of the request, converted to uppercase (e.g., GET, POST).        |
| **Request Path**                       | The URI path of the request (e.g., `/api/v1/resource`).                          |
| **Request Parameter/Body**             | The query parameters or request body, sorted alphabetically.                     |

#### Request Parameter To Signature Content

The request parameters are concatenated into a single string that forms the signature content. This string includes the timestamp, HTTP method, request path, and sorted query parameters or request body, ensuring the integrity and authenticity of the request.

For example, the following request parameters are concatenated into a single string:

>1735542383256GET/api/v1/private/account/getPositionTransactionPageaccountId=543429922991899150&filterTypeList=SETTLE_FUNDING_FEE&size=10

#### Request Body To Body String Code Example

The following Java code example demonstrates how to convert a JSON request body into a sorted string format suitable for signature generation:

```java
import com.google.gson.JsonArray;
import com.google.gson.JsonElement;
import com.google.gson.JsonObject;
import java.util.ArrayList;
import java.util.List;
import java.util.TreeMap;
import java.util.stream.Collectors;

public class RequestBodyToString {
    private static final String EMPTY_STRING = "";

    private static String getValue(JsonElement valueJson) {
        if (valueJson.isJsonNull()) {
            return EMPTY_STRING;
        } else if (valueJson.isJsonPrimitive()) {
            return valueJson.getAsString();
        } else if (valueJson.isJsonArray()) {
            JsonArray valueArray = valueJson.getAsJsonArray();
            if (valueArray.isEmpty()) {
                return EMPTY_STRING;
            }
            List<String> values = new ArrayList<>();
            for (JsonElement itemValue : valueArray) {
                values.add(getValue(itemValue));
            }
            return String.join("&", values);
        } else if (valueJson.isJsonObject()) {
            TreeMap<String, String> sortedDataMap = new TreeMap<>();
            JsonObject valueJsonObj = valueJson.getAsJsonObject();
            for (String key : valueJsonObj.keySet()) {
                sortedDataMap.put(key, getValue(valueJsonObj.get(key)));
            }
            return sortedDataMap.keySet().stream()
                    .map(key -> key + "=" + sortedDataMap.get(key))
                    .collect(Collectors.joining("&"));
        }
        return EMPTY_STRING;
    }
}
```

### Signature Algorithm

The signature algorithm used is Ecdsa (Elliptic Curve Digital Signature Algorithm). For more information on the signature algorithm, see the [Sign](sign.md) page.