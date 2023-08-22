

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
//...More Code
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







Missing function to check if a `token` is `whitelisted`,causing loss of funds



## Summary
The protocol lacks a `public` function which can be called by any `organiser` or `sponsor` to ensure the intended token for distribution is `whitelisted`.
## Vulnerability Details
As time passes the number of available tokens worldwide will continue to increase, therefore it is vital for an `organiser` or `sponsor` to be able to check if a `token` intended for `distribution` is `whitelisted` to prevent funds from being `lost` forever in the proxy contracts.
This is a simple but very important function that will prevent huge losses in the future.
After calling `getProxyAddress()` the `check function` will also be called before the `transfer` is made.
## Impact
This will prevent loss of tokens because of ignorance.
## Tools Used
Manual Review
## Recommendations
A `public` function to check if a desired token is whitelisted should be added to `ProxyFactory.sol`.
```solidity
function Checktoken (address token) public view returns (bool) {
        return whitelistedTokens(token);
    }
```

