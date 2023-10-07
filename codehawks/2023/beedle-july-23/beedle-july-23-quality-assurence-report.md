# Beedle - Quality Assurance Report

# Low Risk Findings

## Table Of Content

| Number                                                                                                           | Issue                                                   | Status    | Instances |
| :--------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ | :-------- | --------: |
| [L-01](#l-01---use-fixed-pragma-version-for-smart-contracts)                                                     | Use fixed `pragma` version for smart contracts          | VALIDATED |        11 |
| [L-02](#l-02---missing-validation-for-address0-in-the-constructors-or-in-the-functions-changing-state-variables) | Missing validation for `address(0)` in the constructors | VALIDATED |         2 |
| [L-03](#l-03---setpool-should-revert-on-else-condition)                                                          | `setPool` should `revert` on else condition.            | VALIDATED |         1 |

### [L-01] - Use fixed `pragma` version for smart contracts.

**Details**

Pragma versions are designed in a way that `0.x.0` introduces bracking changes and `0.0.x` introduces bug fixes in the previous versions.

Now using `^` with the pragma vesions opens the code compilation till the next `< 0.x.0` version, there will be no breaking changes until version `0.x.0`, you can be sure that your code compiles the way you intended but due to the exact version of the compiler is not fixed, newly introduced bugfix can still affect the code.

It is recommeded by [Solidity Docs](https://docs.soliditylang.org/en/v0.8.21/layout-of-source-files.html#version-pragma) to use fixed version for your projects.

Which version should we choose?

There are pros and cons in using both older and newer versions and we should always use the latest stable(tested) version.

Newer versions fix the bugs, introduces new ways to optimize the code and release new features but can introduces new bugs as well.

Older versions are battle tested but lacks the pros of newer versions.

According to my knowledge until now `0.8.17` is the recommended choice.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Beedle.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Beedle.sol#L2)

[Fees.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L2)

[Fees.sol - Line 11](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L11)

[Lender.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L2)

[Lender.sol - Line 10](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L10)

[Staking.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L2)

[IERC20.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/interfaces/IERC20.sol#L2)

[ISwapRouter.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/interfaces/ISwapRouter.sol#L2)

[Errors.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/utils/Errors.sol#L2)

[Ownable.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/utils/Ownable.sol#L2)

[Structs.sol - Line 2](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/utils/Structs.sol#L2)

</details>

**Recommended Mitigation Steps**

Use the stable fixed pragma version.

### [L-02] - Missing validation for `address(0)` in the constructors or in the functions changing state variables.

**Details**

It is recommended to check for `address(0)` when updating state variables through constructor or functions. This can save the protocol from accidental updates.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Fees.sol - Line 19 - 22](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L19-L22)

[Lender.sol - Line 100 - 101](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L100-L101)

</details>

### [L-03] - `setPool` should `revert` on else condition.

**Details**

The `setPool` function checks whether the pool balance is greater than or less than the previous balance of the pool and based on it either it creates the pool or updates it.

But if the pool balance will be equal then no transfer will happen and no balance will be updated, and we will unnessesary emit these events `PoolBalanceUpdated` and either `PoolCreated` or `PoolUpdated`.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Lender.sol - Line 150 - 163](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L150-L163)

</details>

**Recommended Mitigation Steps**

We should add the `else` condition which reverts when no balance changes.

# Non Critical Findings

## Table Of Content

| Number                                                                                                                                       | Issue                                                                                                                             | Status              | Instances |
| :------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- | :------------------ | --------: |
| [N-01](#n-01---use-natspec-comments-for-smart-contracts-interfaces-and-libraries)                                                            | Use `Natspec` comments for smart contracts, interfaces and libraries                                                              | VALIDATED           |         6 |
| [N-02](#l-02---missing-validation-for-address0-in-the-constructors-or-in-the-functions-changing-state-variables)                             | Unnecessary Inheritance                                                                                                           | SELECTED FOR REPORT |         1 |
| [N-03](#n-03---set-the-internal-layout-of-contracts-interfaces-and-libraries)                                                                | Set the Internal Layout of Contracts, Interfaces, and Libraries                                                                   | VALIDATED           |         3 |
| [N-04](#n-04---use-natspec-comments-for-events-state-variables-constructor-functions-and-all-other-things-which-will-be-included-in-the-abi) | Use `Natspec` comments for events, state variables, constructor, functions and all other things which will be included in the ABI | VALIDATED           |         6 |
| [N-05](#n-05---always-imports-explicitly)                                                                                                    | Always imports explicitly                                                                                                         | VALIDATED           |         2 |
| [N-06](#n-06---variables-should-follow-the-naming-convention)                                                                                | Variables should follow the naming convention.                                                                                    | VALIDATED           |         3 |
| [N-07](#n-07---create-the-onlylender-modifier-and-use-with-this-functions-for-better-readability)                                            | Create the `onlyLender` modifier and use with this functions for better readability                                               | VALIDATED           |         5 |
| [N-08](#n-08---use-the-meaningful-errors-with-trace-location)                                                                                | Use the meaningful `errors` with trace location.                                                                                  | VALIDATED           |         8 |

### [N-01] - Use `Natspec` comments for smart contracts, interfaces and libraries.

**Details**

It is recommended by solidity docs that solidity contracts should be fully annotated using NatSpec for all public interfaces (everything in the ABI).

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Beedle.sol - Line 9](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Beedle.sol#L9)

[Staking.sol - Line 7](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L7)

[Staking.sol - Line 11](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L11)

[IERC20.sol - Line 4](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/interfaces/IERC20.sol#L4)

[ISwapRouter.sol - Line 4](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/interfaces/ISwapRouter.sol#L4)

[Ownable.sol - Line 4](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/utils/Ownable.sol#L4)

</details>

**Recommended Mitigation Steps**

Add the `Natspec` comments in the above mentioned contracts.

### [N-02] - Unnecessary Inheritance.

**Details**

Unnessasary inheritance, ERC20Votes inherits ERC20Permit and it inherits ERC20, so only ERC20Votes inherits is enough.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Beedle.sol - Line 9](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Beedle.sol#L9)

</details>

**Recommended Mitigation Steps**

Only inherit ERC20Votes.

### [N-03] - Set the Internal Layout of Contracts, Interfaces, and Libraries.

**Details**

According to the Solidity [Style Guide](https://docs.soliditylang.org/en/v0.8.21/style-guide.html#style-guide) consistency in the projects layout is very important. If all projects apply the style guides provided by solidity docs then understanding the projects and its code will be much easier for developers, and auditors.

Solidity docs;

> A style guide is about consistency. Consistency with this style guide is important. Consistency within a project is more important. Consistency within one module or function is most important.

According to the solidity styles guide;

Inside each contract, library or interface, this order should be followed:

1. Type declarations
2. State variables
3. Events
4. Errors
5. Modifiers
6. Functions

And this should be the order of functions:

-   constructor
-   receive function (if exists)
-   fallback function (if exists)
-   external
-   public
-   internal
-   private
-   View and pure functions last

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Beedle.sol - Line 9 - 40](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Beedle.sol#L9-L40)

[Lender.sol - Line 11 - 735](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L11-L735)

[Ownable.sol - Line 5 - 22](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/utils/Ownable.sol#L5-L22)

</details>

**Recommended Mitigation Steps**

Apply the recommeded layout sturcture in all mentioned contracts according to solidity styles guide.

### [N-04] - Use `Natspec` comments for events, state variables, constructor, functions and all other things which will be included in the ABI.

**Details**

It is recommended by solidity docs that solidity contracts should be fully annotated using NatSpec for all public interfaces (everything in the ABI).

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Beedle.sol - Line 11 - 38](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Beedle.sol#L11-L38)

[Fees.sol - Line 12 - 22](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L12-L22)

[Lender.sol - Line 11 - 735](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L11-L735)

[IERC20.sol - Line 5 - 12](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/interfaces/IERC20.sol#L5-L12)

[ISwapRouter.sol - Line 16](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/interfaces/ISwapRouter.sol#L16)

[Ownable.sol - Line 5 - 22](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/utils/Ownable.sol#L5-L22)

</details>

**Recommended Mitigation Steps**

Add the `Natspec` comments in the above mentioned functions.

### [N-05] - Always imports explicitly.

**Details**

It is recommended by the solidity docs that the contracts should always import explicitly because just import will import all of the things from the file.

And also sometimes is easy for the reader to understand where the contract or other things comming from.

here is the syntax;

```solidity
import {contractName} from "./path";
```

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Fees.sol - Line 4 - 5](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L4-L5)

[Lender.sol - Line 4 - 5](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L4-L5)

</details>

### [N-06] - Variables should follow the naming convention.

**Details**

Solidity style guides and all general programming languages suggests the following naming convention of variables:

-   Variable should be `mixedCase`.
-   `constant` variables should be capitalized and words saperated using `_` underscore.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

`WETH` should be `wETH`

[Fees.sol - Line 12](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L12)

`swapRouter` should be `SWAP_ROUTER`

[Fees.sol - Line 16](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Fees.sol#L16)

`TKN` should be `tKN`
`WETH` should be `wETH`

[Staking.sol - Line 26 - 29](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L26-L29)

</details>

**Recommended Mitigation Steps**

Convert the above mentioned variables to `mixedCase`.

### [N-07] - Create the `onlyLender` modifier and use with this functions for better readability.

**Details**

There are many functions which use this line at the top;

```solidity
if (pools[poolId].lender != msg.sender) revert Unauthorized();
```

Which repeats the code and decrease the readability. Using the `onlyLender` modifier with these functions can increase the readability and remove the repeated code.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Lender.sol - Line 130](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L130)

[Lender.sol - Line 183](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L183)

[Lender.sol - Line 199](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L199)

[Lender.sol - Line 211](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L211)

[Lender.sol - Line 222](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L222)

</details>

**Recommended Mitigation Steps**

Use the `onlyLender` modifier with the above mentioned functions.

### [N-08] - Use the meaningful `errors` with trace location.

**Details**

We are the developers and for us these types of errors (`PoolConfig()`) are easy to understand but these are very difficult to understand for normal users.

In addition to that, addting trace such as contract name with underscore before error can help us later on to understand which contracts actually throw the error because sometimes similar error exist in multiple contracts.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Lender.sol - Line 139](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L139)

[Lender.sol - Line 146](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L146)

[Lender.sol - Line 184](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L184)

[Lender.sol - Line 200](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L200)

[Lender.sol - Line 212](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L212)

[Lender.sol - Line 223](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L223)

[Lender.sol - Line 240](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L240)

`AuctionStarted` this should be `AuctionAlreadyStarted`

[Lender.sol - Line 444 - 445](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L444-L445)

</details>

**Recommended Mitigation Steps**

Use meaningful errors and follow this convention for errors `ContractName_ErrorName` for better understanding later on.
