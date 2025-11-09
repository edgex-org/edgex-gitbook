# eStrategy

**Overview**

#### **edgeX eStrategy**

**Liquidity Engine and Strategy Trading Protocol Library in the edgeX Ecosystem**

As a liquidity provision and strategy trading protocol, **eStrategy** not only supplies the edgeX trading system with robust market depth but also enhances capital efficiency and risk management through advanced strategy design. In its initial phase, **eStrategy** supports the **Automated Market Maker (AMM) strategy**. This strategy enhances market liquidity by integrating with the platform’s AMM, thereby improving execution quality for traders by minimizing price impact. Depositors assume a share of the AMM’s positions, effectively acting as counterparties to other edgeX traders. For instance, if an edgeX trader takes a short position, depositors will hold the corresponding long position. The revenue of edgeX eLP is derived from **passive market-making profits, liquidation fees, and a portion of the platform’s trading fees**. In the second phase, **eStrategy** will introduce **customizable pools managed by individual traders or institutions**. Users can allocate funds to specific pools based on their historical performance and risk profiles. Each strategy carries inherent risks. Users should conduct thorough due diligence on a vault’s performance history and associated risks before making a deposit.

**Lock-up Period and Withdrawal Rules**

Users can submit withdrawal requests at any time; however, a maximum **2-day redemption lock-up period** applies. This is because positions corresponding to the withdrawal request must be closed before funds can be released. For example, if you make a deposit on **March 15 at 08:00**, you will be able to withdraw your funds no later than **March 17 at 08:00**. Once the lock-up period ends, the funds will be automatically transferred to your account. Please note that no returns will be accrued during the redemption period.

What happens to open positions in a vault when someone withdraws?

When a user withdraws from a vault, open positions remain unaffected if there is sufficient initial margin to support the set leverage. If the initial margin is insufficient, a proportional amount of all open positions is closed. For example, if a user represents 20% of the vault's total deposits, 20% of all open positions will be closed upon withdrawal, keeping liquidation prices relatively stable. Vault leaders can also configure the vault to always close positions proportionally during withdrawals to maintain consistent liquidation prices. Additionally, if a withdrawal causes insufficient margin, open margin-based orders will be canceled, starting with those using the least margin.



**FAQ**

**What are the risks of participating in the project?**\
Despite professional strategy management, there are still market risks, such as high volatility and liquidity risks in the cryptocurrency market. In addition, changes in regulatory policies may also have an impact on the project.

**Where does APR come from?**\
APR comes from AMM income, liquidation fees and part of the edgeX platform trading fees.

**How to Participate in the Vault**

* Deposit to Vault: Please first deposit funds to your Main Account, then go to the eStrategy page and complete the transfer into the eLP Vault.
* Withdraw from Vault: When you submit a withdrawal request from the Vault, your funds will be transferred back to your Main Account within 48 hours.

