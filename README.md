# 008-Solidity-Function-type
Function types come in two flavours - internal and external functions:
```solidity
function (<parameter types>) {internal|external} [pure|view|payable] [returns (<return types>)]
```
Internal Function 
In Solidity, internal functions are functions that can only be called from within the same contract or contracts that inherit from it
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.24;
contract internalFun{
uint Data = 10;
function internalFunction() internal view returns(uint){
    return Data;
}
function callInternalFunction() public view returns(uint){
    return internalFunction();
}
}
```
