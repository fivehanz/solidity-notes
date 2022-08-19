# solidity

[docs](https://docs.soliditylang.org)

# structure of a smart contract

- SPDX License
- **pragma** keyword: indicates the version of the compiler to be used.
- **contract** keyword

```solidity
// SPDX-License-Identifier: MIT

pragma solidity ^0.8.13;

contract Space {

    // contract code

}

```

# basic syntax rules

- high-level statically-typed domain specific language
- influnced by javascript
- solc is solidity compiler
- // represents single line comment
- /\* ... \*/ represents a multi-line comment
- /// represents a single line **natspec** comment
- /\*\* ... \*/ represent multi line **natspec** comment
- natspec is used for inline code documentation

# state and local variables

- variables also have visibility

state vars

- declared at the contract level
- permanently stored in **contract storage**
- can be set as **contstants**
- expensive to use
- initialized at declaration, set using a constructor or after contract deployment by calling setters

- constants
  - _constant_ keyword
  - ALLCAPS as per best practices

local vars

- declared inside functions
- have access to calldata, memory and storage ketwords (for non primitive vars)
- can be cheaper to read/write than storage vars (context dependant)

default values

- boolean: fals
- string: ""
- int: 0
- uint: 0
- enum: _the first element of the enum_
- address: address(0)
- function:

  - internal: empty function, returning initial values (if return is needed)
  - external: function that throws when called

- reference types:

  - mapping: empty mapping
  - struct: a struct where all members are set to initial values
  - array:
    - dynamically-sized: [ ]
    - fixed-sized: an array of the fixed size where all elements are set to initial values

- delete keyword will assign the initial value to the variable, except for mappings, where it doesn't have any effect.
- for structs the delete keyword will recurse into the members, unless they are mappings.

# functions, setters & getters

```solidity

...
    // stored in storage
    uint256 public price = 100;

    // _price is stored in memory
    function setPrice (uint256 _price) public returns(uint) {
        price = _price;
        return _price;
    }
...

```

# the constructor

- special function that executes only once when the contract is deployed.
- optional
- when deploying a contract that has a constructor with parameters, these values must be supplied in order to successfully deploy

```solidity

...
    // cannot be changed after initially set
    address public immutable owner;

    constructor (uint256) {
        price = _price;
        owner = msg.sender;
    }
...

```

# boolean and int types
