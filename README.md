Blockchain core properties include 
1. Decentralized
2. Transparent
3. Immutable

However, when we need to update our smart contract to resolve bugs or optimize the code to increase the gas efficiency web3 ecosystem developed the concept of upgradable smart contracts

*Logic vs state*
Logic : contract code - includes the functions, rules and operations (IMMUTABLE)
State : data stored within the contract variables 

Example : 
```solidity
contract MyContract{
  uint256 public val; // This is STATE
  function myFunction() public{ // This is LOGIC
    // logic
  }
}
```

Upgradability swaps out the logic while keeping the state intact

**3 Methods for Managing the contract changes**
1. Parameterization method : Preplanned configuration (generally uses `setter` functions)
2. Social Migration method : Instead of modifying the existing contract the development team deploys an entirely new improved version of the contract and leads the social migration encuraging the community to move their activity and assets to new contract address
3. Proxy pattern :
The proxy pattern is the most common and powerful method
Works using 2 main components
1. Proxy contract : Contract that users interact with. It holds contract's state 
2. Implementation contract : Contains all the business logic(STATELESS)

<img width="1241" height="687" alt="image" src="https://github.com/user-attachments/assets/122f8bf9-6580-4077-a26e-5b96c44273d8" />

*The Golden Rule of Proxy Storage: When upgrading, you can only append new state variables. You must never reorder, remove, or change the type of existing state variables.*

**Security considerations for proxy patterns**
1. Storage clashes
The `delegatecall` applies the implemetation logic to the proxy storage layout
Solidity assigns storage variables to *slots* based on the order of execution not based on names or types
2. Function selector clashes
A function selector is the first four bytes of the cryptographic hash of a function's signature. Because this identifier is so short, it is possible for two different functions (e.g., `transfer(address,uint256)` and `destroy(string))` to have the exact same 4-byte selector. This is known as a function selector clash.

The risk arises if an administrative function in the proxy (like changeAdmin(address)) has the same selector as a user-facing function in the implementation. A regular user could inadvertently call the proxy's admin function, potentially leading to a hostile takeover of the contract.

**Common Proxy patterns and their solutions**
1. Transparent proxy patterns
2. UUPS (Universal Upgradable Proxy Standard - EIP-1822)
3. Diamond proxy pattern (EIP-2535)
This is a highly advanced, modular pattern. Instead of pointing to a single implementation, a Diamond proxy can delegate calls to multiple implementation contracts, known as "facets." A central mapping within the proxy routes each function selector to its corresponding facet. This allows for granular upgrades (updating only one part of a complex system) and helps developers manage contracts that would otherwise exceed the maximum contract size limit.

<img width="1663" height="734" alt="image" src="https://github.com/user-attachments/assets/48363ba9-5692-4028-9477-591906e9a8f1" />

