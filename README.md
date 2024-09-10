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
When an internal function is called, there is no external call overhead because the function resides within the same code unit (i.e., the same contract or an inherited contract). Solidity simply "jumps" to the location of the internal function’s code, making the execution fast and gas-efficient.
Call Internall Function from inherit contract
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
contract iheritExample is internalFun{
    function callInternalInheri() public view returns(uint){
        return internalFunction();
    }
}
```
By default, function types are internal,
Internal Function References
```solidity
function(<paramTypes>) internal returns (<returnTypes>) functionVariable;
```



