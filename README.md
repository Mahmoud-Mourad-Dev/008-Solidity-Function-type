# 008-Solidity-Function-type
Functions are the most important element of a smart contract after state variable,function accept parameters and return values,Functions are the heart of the code
Function types come in two flavours - internal and external functions:
Function Visibility
|visibility| Definition|
|----------|---------|
|Internal|Restricted to the current contract and inherited contracts|
|External|Called only from outside the contract but accessible via this for internal calls|
|Public|Accessible from both inside and outside the contract|
Private|Accessible only within the contract that defines it, not even by inherited contracts|

Internal Functions
Definition: Functions marked as internal can only be accessed inside the contract where they are defined or from derived contracts (contracts that inherit from the current contract).
Scope: They cannot be called externally, including by other contracts or users.
```solidity
contract BaseContract {
    function internalFunction() internal pure returns (string memory) {
        return "This is an internal function";
    }
}

contract DerivedContract is BaseContract {
    function callInternalFunction() public pure returns (string memory) {
        return internalFunction(); // Can call inherited internal function
    }
}
```
External Functions
Definition: Functions marked as external can only be called from outside the contract. They can be called internally using this.functionName().
Scope: They are accessible to other contracts or external users via transactions or external function calls.
Usage: external functions are typically used when interaction with the contract is required from outside (e.g., users or other contracts interacting with the contract on the blockchain).
```solidity
/SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
    contract ExternalFunctionContract {
    function externalFunction() external pure returns (string memory) {
        return "This is an external function";
    }

    function callExternalFunction() public view returns (string memory) {
        // Cannot call externalFunction() directly within the same contract
        // return externalFunction();  // This will cause an error

        // You can call it via 'this'
        return this.externalFunction();
    }
}
```
Public Functions
Definition: Functions marked as public can be called both externally and internally. They are accessible by any external entity as well as by other functions within the contract.
Scope: Public functions are open to both internal calls (within the contract or derived contracts) and external calls (from users or other contracts).
Usage: These functions are commonly used when you want them to be accessible from both inside and outside the contract.
```solidity
contract PublicFunctionContract {
    function publicFunction() public pure returns (string memory) {
        return "This is a public function";
    }

    function callPublicFunction() public view returns (string memory) {
        return publicFunction();  // Can call internally
    }
}
```
Private Functions
Definition: Functions marked as private can only be called inside the contract where they are defined. They are not accessible by derived contracts or externally.
Scope: Only the contract that defines the private function can access it.
Usage: private functions are useful when you want to restrict a function's use entirely to the contract that defines it, even excluding inherited contracts.
```solidity
contract PrivateFunctionContract {
    function privateFunction() private pure returns (string memory) {
        return "This is a private function";
    }

    function callPrivateFunction() public pure returns (string memory) {
        return privateFunction();  // Can call internally within the same contract
    }
}

contract DerivedContract is PrivateFunctionContract {
    // function callPrivate() public pure returns (string memory) {
    //     return privateFunction(); // This will cause an error
    // }
}
```
|Mutability|Defintion|
|------------|----------------|
|view|Can read but not modify state|
|pure|Cannot read or modify state; pure computation|
|payable|Can receive Ether|
|Non-payable|Cannot receive Ether (default behavior unless payable is specified)|

View Functions
Definition: Functions marked as view promise not to modify the state of the blockchain.
Use case: They can read state variables, call other view functions, and do computations, but cannot alter state variables (like updating a balance, storing data, etc.).
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
    contract ViewFunction {
        uint public storedData;

function getStoredData() public view returns (uint) {
    return storedData;
}
    
}
```
 Pure Functions
Definition: Functions marked as pure promise not only not to modify the state but also not to read any state variables.
Use case: These are strictly computational functions that rely only on input parameters and do not interact with contract storage or other functions that read/write state.
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
    contract PureFunction {
       function add(uint a, uint b) public pure returns (uint) {
    return a + b;
}    
}
```
Payable Functions
Definition: Functions marked as payable allow the contract to receive Ether.
Use case: This is necessary when you want to write a function that can accept Ether transfers. A payable function can also execute other logic beyond receiving Ether, such as updating balances or triggering events.
```solidity
//SPDX-License-Identifier:MIT
pragma solidity ^0.8.0;
    contract payableFunction {
    mapping(address => uint256) balances;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
    }
}
```










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

Incorrect Usage (causes a compilation error):
```Solidity
function logData() public pure returns () {
    // This will cause an error because return types cannot be empty
}
```
