# Use `_safeMint` function instead of `_mint` function to NFTs from being locked

## Links

https://github.com/code-423n4/2023-09-delegate/blob/main/src/PrincipalToken.sol#L35

## Vulnerability Details

`_mint` function does not check whether the receiver accepts the ERC721 tokens or not such as smart contracts.

On the other hand, the `_safeMint` function call the `onERC721Received` hook on the receiver end (if it is the contract) which validates that the contract accept the ERC721 tokens.

## Impact

The NFT will be permanatly locked

## Tools Used

Manual Analysis

## Recommended Mitigation Steps

```diff
/// @notice Mints a PT if and only if the DT contract calls and has authorized
function mint(address to, uint256 id) external {
    _checkDelegateTokenCaller();

-    _mint(to, id);
+    _safeMint(to, id);

    IDelegateToken(delegateToken).mintAuthorizedCallback();
}
```

Use `_safeMint` instead of `_mint` function.
