---
layout: post
title: Unusual Behaviors in Solidity and the EVM through a cross-bridge lens.
subtitle: Behaviors and interactions that can lead to security issues when dealing with multiple smart contract languages.
toc: true
#cover-img: /assets/img/solarwindsceocv.png
thumbnail-img: /aassets/img/post5/eth_ss.png
share-img: /aassets/img/post5/eth_ss.png
tags: [eth, solidity]
---

#### Background

Solidity is a high-level language used in the creation of Ethereum smart contracts, compiling to Ethereum Virtual Machine(EVM) Bytecode. It looks a little like Javascript, and was heavily influenced by C++, Python and of course Javascript. It is not the only language that can be used for creating smart contracts.

The nature of smart contracts and Solidity create some interesting security challenges. When consulting this, consider that every smart contract attribute can be read by others. Marking an attribute as 'private' has no bearing on it's visibility; it only has bearing on the vector of accesibility.


#### 1. Inheritance

This issue moves to the top of the list given recent news/issues, like the Wormhole exchange attack. Although Solana uses Rust, Ethereum uses Solidity and pairing the two together - in fact any different language pairing increases the complexities and risk of security issues through the idiosyncrasies of the different languages.
Solidity supports multiple inheritance,(and Rust does not) so figuring out what functions get called is difficult for all parties. If the inheritance order is incorrect, it'll result in errors in the final contract.

Couple this with the fact that cross-chain bridges are an amazing attack surface; not just because of the above complexities but the risk of large pools coordinating to [perform 51% attacks](https://old.reddit.com/r/ethereum/comments/rwojtk/ama_we_are_the_efs_research_team_pt_7_07_january/hrngyk8/) outlines cross-chain bridges as one of the richest attack surfaces in the cryptocurrency space, and one of the most complex to sure.

Understanding these idiosyncrasies for all involved languages is a critical part of smart contract security.


#### 2. Call to the unknown


If you call a non-existent function in a contract, - or transfer ether unexpectedly -, this may lead to an error.
The contract will attempt to call the contract's fall-back function.
 Call to the unknown has resulted in many significant thefts over the years, including from MultiSig Wallets before it was mitigated via a logic split in v0.6.0, so I won't delve deeply into technical examples right now.
 It's important when working with cross-chain bridges that great care is taken to ensure that `fallback` functions and `recieve` functions remain split, given that Solidity is handling more of this process logic.


#### 3. Exception handling

 Errors do not necessarily propagate across the call-stack in all cases.  This can mean in certain cases a developer may not even be aware that funds are being stolen.

 In this example below, the contract `bar` contains the value of the `x` variable. The `x` variable varies depends on the method of the `send` function call:


```contract foo {
    function send(uint) {
        returns (uint)}}
contract Bar {
    uint x=0;
    function call(foo c){
        x=1;
        c.send(42);
        x=2;
        } }
  ```
if the `send` function of the `foo` contract is called directly, the value will be `x=0`.
If the same is called using the inbuilt function call of solidity, the value will be `x=2`.
It's also likely during failed exception handling that where no bound is specified, the gas fee will be lost.

#### 4. On chain type-Mappings (malleability risk)

This issue is documented in solidity directly, as a minor note.
Transaction malleability refers to the ability of an attacker to modify or alter the hash that identifies a transaction within the blockchain, which usually results in a denial of service.
Mappings can be seen as hash tables which are virtually initialized such that every possible key exists and is mapped to a value whose byte-representation is all zeros, however types that do not occupy the full 32 bytes on-chain will be stored with 'dirty-bits' - arbitrary data in unused fields.
"[You can craft transactions that call a function f(uint8 x)](https://docs.soliditylang.org/en/v0.8.4/security-considerations.html#minor-details]) with a raw byte argument of 0xff000001 and with 0x00000001. Both are fed to the contract and both will look like the number 1 as far as x is concerned.""
This means that any hash-based use cases of `keccak256(msg.data)` will yield different results.

#### 5. Wrapping up

By detailing some of the foibles present in Solidity, with an eye on cross-chain bridges and the interoperability of contracts written in different languages, it's apparent how complex and rich this attack surface is, and ripe for many new ideas around their synchronization and monitoring.
