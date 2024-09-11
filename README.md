# 008-Solidity-Function-type
Functions are the most important element of a smart contract after state variable,function accept parameters and return values,Functions are the heart of the code
Function types come in two flavours - internal and external functions:

```solidity
function (<parameter types>) {internal|external} [pure|view|payable] [returns (<return types>)]
```
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
contract functionNoParameter{
function appleProduct() public pure returns(uint){
uint ipadPro=1000;
uint ipadAir=1200;
return ipadPro+ipadAir;
}
}
```
Function take single incoming parameter
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.1 <0.9.0;
contract functionOneParameter{
function appleProduct(uint ipadPrice) public pure returns(uint){
return ipadPrice*5;
}
}

```
Function take maltiple incoming parameter
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
contract functionTwoParameter{
function appleProduct(uint iphone, uint ipad) public pure returns(uint){
return iphone + ipad;
}
}
```
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
contract functionTwoOutput{
function appleProduct(uint ipadPrice) public pure returns(uint doupleIpadPrice,uint thirdIpadPrice){
doupleIpadPrice=ipadPrice*2;
thirdIpadPrice=ipadPrice*3;
}
}
```
Function return maltiple values as tuple
tuple :- is custom data structure consisting of maltiple varaiables in group
```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;
contract functionTupleOutput{
function appleProduct(uint ipadPrice) public pure returns(uint doupleIpadPrice,uint thirdIpadPrice){
(doupleIpadPrice,thirdIpadPrice)=(ipadPrice*2,ipadPrice*3);
}
}
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
This line is not a function, but rather a declaration of a variable that can hold a reference to a function
```solidity
function(<paramTypes>) internal returns (<returnTypes>) functionVariable;
```

The variable addFunction is a function type variable that can store a reference to an internal function which takes two uint parameters and returns a uint value.
This variable itself doesn't implement any logic; it's a placeholder that can later be assigned a specific internal function (like AddFunction).
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;

contract FunctionReferenceExample {
    // Declare a function type variable
    function(uint, uint) internal returns (uint) addFunction;

    // Internal function to add two numbers
    function AddFunction(uint a, uint b) internal pure returns (uint) {
        return a + b;
    }

    // Assign and use the function type variable
    function setFunction() public {
        addFunction = AddFunction;  // Assign the internal function to the function type variable
    }

    function callAddFunction(uint a, uint b) public  returns (uint) {
        return addFunction(a, b);  // Call the referenced function
    }
}
```
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
contract CompareValues {
    
    function(uint, uint) internal pure returns (uint) compareFunction;

    constructor() {
        compareFunction = getMax;  
    }

    function getMax(uint a, uint b) internal pure returns (uint) {
        return a >= b ? a : b;
    }

    function compare(uint x, uint y) public view returns (uint) {
        return compareFunction(x, y);  
    }
}
```

In Solidity, internal functions can be called from the current contract, but this also includes internal functions from libraries and inherited contracts. Let’s break this down further
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
library MathLib {
    function add(uint a, uint b) internal pure returns (uint) {
        return a + b;
    }
}
contract Example {
    function useAdd(uint a, uint b) public pure returns (uint) {
        return MathLib.add(a, b);  // Calling an internal library function
    }
    }
```
External functions
When a contract calls an external function (whether in the same contract or another contract), it refers to both the address of the contract where the function resides and the function signature.
The address tells the Ethereum Virtual Machine (EVM) where the contract is located.
Function Signature: The function signature is composed of the function name and parameter types, 
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract ContractA {
    // External function in ContractA
    function externalFunction(uint a, uint b) external pure returns (uint) {
        return a + b;
    }
}

contract ContractB {
    // Function to call an external function in another contract
    function callExternalFunction(address contractAddress, uint a, uint b) public  returns (uint) {
        //  defines a variable externalFunc that refers to an external function
        function (uint, uint) external returns (uint) externalFunc;

        // Assign the external function from ContractA
        externalFunc = ContractA(contractAddress).externalFunction;

        // Call the external function and return the result
        return externalFunc(a, b);
    }
}
```
Function types are notated as follows:
```solidity

function (<parameter types>) {internal|external} [pure|view|payable] [returns (<return types>)]
```






