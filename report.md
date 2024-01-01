# Aderyn Analysis Report

This report was generated by [Aderyn](https://github.com/Cyfrin/aderyn), a static analysis tool built by [Cyfrin](https://cyfrin.io), a blockchain security company. This report is not a substitute for manual audit or security review. It should not be relied upon for any purpose other than to assist in the identification of potential security vulnerabilities.
# Table of Contents

- [Summary](#summary)
  - [Files Summary](#files-summary)
  - [Files Details](#files-details)
  - [Issue Summary](#issue-summary)
- [High Issues](#high-issues)
  - [H-1: Arbitrary `from` passed to `transferFrom` (or `safeTransferFrom`)](#h-1-arbitrary-from-passed-to-transferfrom-or-safetransferfrom)
- [Medium Issues](#medium-issues)
  - [M-1: Centralization Risk for trusted owners](#m-1-centralization-risk-for-trusted-owners)
  - [M-2: Using `ERC721::_mint()` can be dangerous](#m-2-using-erc721mint-can-be-dangerous)
- [Low Issues](#low-issues)
  - [L-1: Unsafe ERC20 Operations should not be used](#l-1-unsafe-erc20-operations-should-not-be-used)
  - [L-2: PUSH0 is not supported by all chains](#l-2-push0-is-not-supported-by-all-chains)
- [NC Issues](#nc-issues)
  - [NC-1: Missing checks for `address(0)` when assigning values to address state variables](#nc-1-missing-checks-for-address0-when-assigning-values-to-address-state-variables)
  - [NC-2: Functions not used internally could be marked external](#nc-2-functions-not-used-internally-could-be-marked-external)
  - [NC-3: Constants should be defined and used instead of literals](#nc-3-constants-should-be-defined-and-used-instead-of-literals)
  - [NC-4: Event is missing `indexed` fields](#nc-4-event-is-missing-indexed-fields)


# Summary

## Files Summary

| Key | Value |
| --- | --- |
| .sol Files | 4 |
| Total nSLOC | 101 |


## Files Details

| Filepath | nSLOC |
| --- | --- |
| src/L1BossBridge.sol | 64 |
| src/L1Token.sol | 8 |
| src/L1Vault.sol | 12 |
| src/TokenFactory.sol | 17 |
| **Total** | **101** |


## Issue Summary

| Category | No. of Issues |
| --- | --- |
| Critical | 0 |
| High | 1 |
| Medium | 2 |
| Low | 2 |
| NC | 4 |


# High Issues

## H-1: Arbitrary `from` passed to `transferFrom` (or `safeTransferFrom`)

Passing an arbitrary `from` address to `transferFrom` (or `safeTransferFrom`) can lead to loss of funds, because anyone can transfer tokens from the `from` address if an approval is made.  

- Found in src/L1BossBridge.sol [Line: 62](src/L1BossBridge.sol#L62)

	```solidity
	        token.safeTransferFrom(from, address(vault), amount);
	```



# Medium Issues

## M-1: Centralization Risk for trusted owners

Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds.

- Found in src/L1BossBridge.sol [Line: 15](src/L1BossBridge.sol#L15)

	```solidity
	contract L1BossBridge is Ownable, Pausable, ReentrancyGuard {
	```

- Found in src/L1BossBridge.sol [Line: 37](src/L1BossBridge.sol#L37)

	```solidity
	    function pause() external onlyOwner {
	```

- Found in src/L1BossBridge.sol [Line: 41](src/L1BossBridge.sol#L41)

	```solidity
	    function unpause() external onlyOwner {
	```

- Found in src/L1BossBridge.sol [Line: 45](src/L1BossBridge.sol#L45)

	```solidity
	    function setSigner(address account, bool enabled) external onlyOwner {
	```

- Found in src/L1Vault.sol [Line: 12](src/L1Vault.sol#L12)

	```solidity
	contract L1Vault is Ownable {
	```

- Found in src/L1Vault.sol [Line: 19](src/L1Vault.sol#L19)

	```solidity
	    function approveTo(address target, uint256 amount) external onlyOwner {
	```

- Found in src/TokenFactory.sol [Line: 11](src/TokenFactory.sol#L11)

	```solidity
	contract TokenFactory is Ownable {
	```

- Found in src/TokenFactory.sol [Line: 23](src/TokenFactory.sol#L23)

	```solidity
	    function deployToken(string memory symbol, bytes memory contractBytecode) public onlyOwner returns (address addr) {
	```



## M-2: Using `ERC721::_mint()` can be dangerous

Using `ERC721::_mint()` can mint ERC721 tokens to addresses which don't support ERC721 tokens. Use `_safeMint()` instead of `_mint()` for ERC721.

- Found in src/L1Token.sol [Line: 10](src/L1Token.sol#L10)

	```solidity
	        _mint(msg.sender, INITIAL_SUPPLY * 10 ** decimals());
	```



# Low Issues

## L-1: Unsafe ERC20 Operations should not be used

ERC20 functions may not behave as expected. For example: return values are not always meaningful. It is recommended to use OpenZeppelin's SafeERC20 library.

- Found in src/L1BossBridge.sol [Line: 87](src/L1BossBridge.sol#L87)

	```solidity
	                abi.encodeCall(IERC20.transferFrom, (address(vault), to, amount))
	```

- Found in src/L1Vault.sol [Line: 20](src/L1Vault.sol#L20)

	```solidity
	        token.approve(target, amount);
	```



## L-2: PUSH0 is not supported by all chains

Solc compiler version 0.8.20 switches the default target EVM version to Shanghai, which means that the generated bytecode will include PUSH0 opcodes. Be sure to select the appropriate EVM version in case you intend to deploy on a chain other than mainnet like L2 chains that may not support PUSH0, otherwise deployment of your contracts will fail.

- Found in src/L1BossBridge.sol [Line: 3](src/L1BossBridge.sol#L3)

	```solidity
	pragma solidity 0.8.20;
	```

- Found in src/L1Token.sol [Line: 2](src/L1Token.sol#L2)

	```solidity
	pragma solidity 0.8.20;
	```

- Found in src/L1Vault.sol [Line: 2](src/L1Vault.sol#L2)

	```solidity
	pragma solidity 0.8.20;
	```

- Found in src/TokenFactory.sol [Line: 2](src/TokenFactory.sol#L2)

	```solidity
	pragma solidity 0.8.20;
	```



# NC Issues

## NC-1: Missing checks for `address(0)` when assigning values to address state variables

Assigning values to address state variables without checking for `address(0)`.

- Found in src/L1Vault.sol [Line: 16](src/L1Vault.sol#L16)

	```solidity
	        token = _token;
	```



## NC-2: Functions not used internally could be marked external



- Found in src/TokenFactory.sol [Line: 23](src/TokenFactory.sol#L23)

	```solidity
	    function deployToken(string memory symbol, bytes memory contractBytecode) public onlyOwner returns (address addr) {
	```

- Found in src/TokenFactory.sol [Line: 31](src/TokenFactory.sol#L31)

	```solidity
	    function getTokenAddressFromSymbol(string memory symbol) public view returns (address addr) {
	```



## NC-3: Constants should be defined and used instead of literals



- Found in src/L1Token.sol [Line: 10](src/L1Token.sol#L10)

	```solidity
	        _mint(msg.sender, INITIAL_SUPPLY * 10 ** decimals());
	```



## NC-4: Event is missing `indexed` fields

Index event fields make the field more quickly accessible to off-chain tools that parse events. However, note that each index field costs extra gas during emission, so it's not necessarily best to index the maximum allowed per event (three fields). Each event should use three indexed fields if there are three or more fields, and gas usage is not particularly of concern for the events in question. If there are fewer than three fields, all of the fields should be indexed.

- Found in src/L1BossBridge.sol [Line: 28](src/L1BossBridge.sol#L28)

	```solidity
	    event Deposit(address from, address to, uint256 amount);
	```

- Found in src/TokenFactory.sol [Line: 14](src/TokenFactory.sol#L14)

	```solidity
	    event TokenDeployed(string symbol, address addr);
	```


