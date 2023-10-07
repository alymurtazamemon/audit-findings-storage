# BeedleFi - Gas Optimization Report

### Platform - `CodeHawks`

### Dates - `Jul-Aug 2023`

## Table Of Content

| Number   | Issue                                                                           | Status              | Instances |
| :------- | :------------------------------------------------------------------------------ | :------------------ | --------: |
| [G-01]() | Do not add the data which is alreday included in the tx to save users gas cost  | SELECTED FOR REPORT |         5 |
| [G-02]() | Multiple reads of `storage` variable which can be cached to save users gas cost | VALIDATED           |         3 |
| [G-03]() | Convert multiple `mappings` to single mapping using `struct`                    | VALIDATED           |         1 |

### [G-01] - Do not add the data which is alreday included in the tx to save users gas cost.

**Details**

In the `Borrowed` event we added the `startTimestamp` and gave it value `block.timestamp`. This increases the gas of users and because this data is already present in the transaction then we can save users gas cost by removing it from event information.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Lender.sol - Line 29](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L29)

[Lender.sol - Line 284](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L284)

[Lender.sol - Line 429](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L429)

[Lender.sol - Line 531](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L531)

[Lender.sol - Line 706](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Lender.sol#L706)

`Each emit event will save 273 units of gas`

| Calculation Type | Before | After  | Gas Saved |
| :--------------- | :----- | :----- | --------: |
| Avg              | 364364 | 364091 |       273 |

```diff
    emit Borrowed(
        msg.sender,
        pool.lender,
        loans.length - 1,
        debt,
        collateral,
        pool.interestRate,
-       block.timestamp
+       pool.interestRate
    );
```

</details>

### [G-02] - Multiple reads of `storage` variable which can be cached to save users gas cost.

**Details**

SLOAD (is the opcode used of read storage variable) cost 100 units of gas and MLOAD & MSTORE (are the opcodes used to read and write to memory respectivily) cost 3, 3 units of gas respectivily.

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Staking.sol - Line 65 - 66](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L65-L66)

```diff
-            if (_balance > balance) {
-                uint256 _diff = _balance - balance;

+            uint256 balance_ = balance;
+            if (_balance > balance_) {
+                uint256 _diff = _balance - balance_;
```

[Staking.sol - Line 85 - 86](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L85-L86)

[Staking.sol - Line 92](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L92)

```diff
     function updateFor(address recipient) public {
         update();
+
         uint256 _supplied = balances[recipient];
+
+        uint256 index_ = index;
+
         if (_supplied > 0) {
             uint256 _supplyIndex = supplyIndex[recipient];
-            supplyIndex[recipient] = index;
+            supplyIndex[recipient] = index_;

-            uint256 _delta = index - _supplyIndex;
+            uint256 _delta = index_ - _supplyIndex;
+
             if (_delta > 0) {
              uint256 _share = _supplied * _delta / 1e18;
              claimable[recipient] += _share;
             }
         } else {
-            supplyIndex[recipient] = index;
+            supplyIndex[recipient] = index_;
         }
     }
```

</details>

### [G-03] - Convert multiple `mappings` to single mapping using `struct`.

**Details**
In the `Stacking` contract we are using 3 differnt mappings to store `supplyIndex`, `balances` and `claimable` rewards. These all 3 mappings connected with `user` address. We can convert all these 3 mappings to single mapping using struct like this;

```solidity
struct User {
    uint256 supplyIndex;
    uint256 balance;
    uint256 claimable;
}

mapping(address => User) public users;
```

<details>
  <summary>Spotted Findings</summary>
  <p></p>

[Staking.sol - Line 19 - 24](https://github.com/Cyfrin/2023-07-beedle/blob/main/src/Staking.sol#L19-L24)

Just need to update places like this;

```diff
-       balances[msg.sender] += _amount;
+       users[msg.sender].balance += _amount;

-       claimable[msg.sender] = 0;
+       users[msg.sender].claimable = 0;

-       supplyIndex[recipient] = index_;
+       users[recipient].supplyIndex = index_;
```

</details>
