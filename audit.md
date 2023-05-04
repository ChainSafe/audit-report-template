<script src="https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.7.0/highlight.min.js"></script>

<script>hljs.initHighlightingOnLoad();</script>

<script src="https://cdn.jsdelivr.net/npm/highlightjs-solidity@2.0.6/dist/solidity.min.js"></script>


![Cover](/cover.png)
# {Company Name} Smart Contract Review | {Month} 2023 

### by ChainSafe Systems | {Month} 2023

<div class="page-break"></div>
<br>
<br>

# 1. Introduction
<br>

<table>
    <tr>
        <td>
            Date
        </td>
        <td>Auditor(s)</td>
    </tr>
    <tr>
        <td>
            {Month} {YYYY}
        </td>
        <td>Oleksii Matiiasevych, Tanya Bushenyova, Anderson Lee</td>
    </tr>
</table>

<br>

**{Company Name}** requested **ChainSafe Systems** to perform a review of the {Audit Name} smart contracts. The contracts can be identified by the following git commit hash:
`{commit hash}`

There are {number} contracts, interfaces and libraries in scope.  

After the initial review, {Company Name} team applied a number of updates which can be identified by the following git commit hash:
`{commit hash}`  

Additional verification was performed after that.
<br>
<br>

## Defining Severity

Each finding is assigned a severity level.

<table>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-note-ded3b6"/>
        </td>
        <td>Notes are informational in nature. They are typically suggestions around best practices or readability. Code maintainers should use their own judgment as to whether to address such issues.</td>
    </tr>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-optimization-bbde81"/>
        </td>
        <td>Optimizations are objective in nature but are not security vulnerabilities. These should be addressed unless there is a clear reason not to. </td>
    </tr>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-minor-gray"/>
        </td>
        <td>Minor issues are objective in nature but are not security vulnerabilities. These should be addressed unless there is a clear reason not to. </td>
    </tr>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-major-orange"/>
        </td>
        <td>Major issues are security vulnerabilities that may not be directly exploitable or may require certain conditions in order to be exploited. All major issues should be addressed.</td>
    </tr>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-critical-critical"/>
        </td>
        <td>Critical issues are directly exploitable security vulnerabilities that need to be fixed.</td>
    </tr>
</table>

<br>

### Referencing updated code

<table>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-Resolved-49d100"/>
        </td>
        <td>The finding has been acknowledged and the team has since updated the code.</td>
    </tr>
    <tr>
        <td>
            <img align="left" width="auto" src="https://img.shields.io/badge/-Rejected-lightgrey"/>
        </td>
        <td>The team dismissed this finding and no changes will be made. </td>
    </tr>
</table>
<br>

### Disclaimer
The review makes no statements or warranties about the utility of the code, safety of the code, suitability of the business model, regulatory regime for the business model, or any other statements about the fitness of the contracts for any specific purpose, or their bug free status.
<br>
<br>

# 2. Executive Summary

All the initially identified high severity issues were fixed and are not present in the final version of the contract.

There are **no** known compiler bugs for the specified compiler version (0.8.14), that might affect the contracts’ logic.

There were **{number} critical**, **{number} major**, **{number} minor**, {number} informational/optimization issues identified in the initial version of the contracts. The critical issues found in the contracts were not present in the final version of the contracts, though new minor issues were found. They are described below for historical purposes. A number of substantial logic changes were introduced to the code which is out of scope of the verification step.

We are looking forward to future engagements with the {Company Name}.
<br>
<br>

# 3. Critical Bugs and Vulnerabilities

**Two** critical issues (5.17, 5.19) were identified in the contracts that would allow a malicious actor to borrow all the deposited assets from the protocol without leaving anything for collateral.
<br>
<br>

# 4. Line-by-line review

### src/contracts/Contract.sol

**L16-23** <span class="severity minor">Minor</span>

```solidity
uint256 updatedTotalShares = totalShares + newShares;
require(updatedTotalShares >= MIN_NONZERO_TOTAL_SHARES,
    "StrategyBase.deposit: updated totalShares amount would be nonzero but below MIN_NONZERO_TOTAL_SHARES");
```

The `{Contract Name}` contract is the entry point for deposits into and withdrawals from strategies. More specifically, to deposit into a strategy, a staker calls `depositIntoStrategy` (or anyone calls).

**L43** <span class="severity note">Note</span>
The return param `smartPoolDebtReduction` is not described.
<br>
<br>

### src/contracts/FixedLender.sol

**L139**  <span class="severity optim">Optimization</span>
<br>
The maxFuturePools value is not validated in the constructor and could be set to 0.

**L139**  <span class="severity critical">Critical</span>
<br>
The maxFuturePools value is not validated in the constructor and could be set to 0.

**L139**  <span class="severity major">Major</span>
<br>
The maxFuturePools value is not validated in the constructor and could be set to 0.
