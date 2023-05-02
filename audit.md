 <script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/highlight.min.js"></script>
 <script>hljs.initHighlightingOnLoad();</script>
 <script src="https://cdn.jsdelivr.net/npm/highlightjs-solidity@2.0.6/dist/solidity.min.js"></script>

# {Audit Name}

​
{Description}

**by ChainSafe Systems**
​
<div class="page-break"></div>

## 1. Introduction

| Date | Auditors|
| ----------------- | ----------------- |
| May 2022 | Oleksii Matiiasevych, Tanya Bushenyova, Anderson Lee |


**{Company Name}** requested **ChainSafe Systems** to perform a review of the {Audit Name} smart contracts. The contracts can be identified by the following git commit hash:
`{commit hash}`  

There are {number} contracts, interfaces and libraries in scope.  ​

After the initial review, {Company Name} team applied a number of updates which can be identified by the following git commit hash:
`{commit hash}`  ​

Additional verification was performed after that.​

## 2. Introduction

### Defining Severity

Each finding is assigned a severity level.
![Note ](https://img.shields.io/badge/-note-ded3b6)  Notes are subjective in nature. They are typically suggestions around best practices or readability. Code maintainers should use their own judgment as to whether to address such issues.
![Optimization](https://img.shields.io/badge/-optimization-bbde81) Optimizations are objective in nature but are not security vulnerabilities. These should be addressed unless there is a clear reason not to.
![Major](https://img.shields.io/badge/-major-orange) Major issues are security vulnerabilities that may not be directly exploitable or may require certain conditions in order to be exploited. All major issues should be addressed.
![Critical](https://img.shields.io/badge/-critical-critical)  Critical issues are directly exploitable security vulnerabilities that need to be fixed.
​
### Referencing updated code

The following labels indicate whether the finding has been resolved, or rejected by the team.
![Fixed](https://img.shields.io/badge/-Resolved-49d100) The finding has been acknowledged and the team has since updated the code.
![Rejected](https://img.shields.io/badge/-Rejected-lightgrey) The team dismissed this finding and no changes will be made.

### Disclaimer

The review makes no statements or warranties about the utility of the code, safety of the code, suitability of the business model, regulatory regime for the business model, or any other statements about the fitness of the contracts for any specific purpose, or their bug free status.​

## 3. Executive Summary​

All the initially identified high severity issues were fixed and are not present in the final version of the contract.
​
There are **no** known compiler bugs for the specified compiler version (0.8.14), that might affect the contracts’ logic.
​
There were **{number} critical**, **{number} major**, **{number} minor**, {number} informational/optimization issues identified in the initial version of the contracts. The critical issues found in the contracts were not present in the final version of the contracts, though new minor issues were found. They are described below for historical purposes. A number of substantial logic changes were introduced to
​
the code which is out of scope of the verification step.
​
We are looking forward to future engagements with the {Company Name}.​

## 4. Critical Bugs and Vulnerabilities

**Two** critical issues (5.17, 5.19) were identified in the contracts that would allow a malicious actor to borrow all the deposited assets from the protocol without leaving anything for collateral.​

## 5. Line-by-line review​

 **src/contracts/Contract.sol**
​
L16-23 ![Note ](https://img.shields.io/badge/-note-ded3b6)

```solidity
uint256 updatedTotalShares = totalShares + newShares;
require(updatedTotalShares >= MIN_NONZERO_TOTAL_SHARES,
    "StrategyBase.deposit: updated totalShares amount would be nonzero but below MIN_NONZERO_TOTAL_SHARES");
```

The `{Contract Name}` contract is the entry point for deposits into and withdrawals from strategies. More specifically, to deposit into a strategy, a staker calls `depositIntoStrategy` (or anyone calls).
​
L43 ![Note ](https://img.shields.io/badge/-note-ded3b6)
The return param `smartPoolDebtReduction` is not described.
​
 **src/contracts/FixedLender.sol**
L139. ![Optimization](https://img.shields.io/badge/-optimization-bbde81)
The maxFuturePools value is not validated in the constructor and could be set to 0.
