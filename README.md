

## Summary
`Ownable.sol` is initialized without an input address for the `initialOwner`
## Vulnerability Details
From openzeppelins `Ownable.sol` contract an address is inputed into the constructor to initialize the first owner.

```solidity
  constructor(address initialOwner) {
        _transferOwnership(initialOwner);
    }
```

But in `ProxyFactory.sol` contract, no input value is passed in when ownable is called.
```solidity
 constructor(address[] memory _whitelistedTokens) EIP712("ProxyFactory", "1") Ownable() {
        if (_whitelistedTokens.length == 0) revert ProxyFactory__NoEmptyArray();
        for (uint256 i; i < _whitelistedTokens.length;) {
...More Code
```


## Impact
This missing input value may cause failure in deployment and other unforseen issues.
## Tools Used
Manual Review
## Recommendations
An appropriate input value should be added to ensure proper functioning.
```solidity
 constructor(address[] memory _whitelistedTokens) EIP712("ProxyFactory", "1") Ownable(msg.sender) {
        if (_whitelistedTokens.length == 0) revert ProxyFactory__NoEmptyArray();
        for (uint256 i; i < _whitelistedTokens.length;) {
```
