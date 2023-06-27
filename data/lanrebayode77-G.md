## [G-01] Remove `public` visibility from `constant` variables

`constant` variables are custom to the contract and won't need to be read on-chain - anyone can just see their values from the source code and, if needed, hardcode them into other contracts. Removing the `public` visibility will optimise deployment cost since no automatically generated getters will exist in the bytecode of the contract.


