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

- boolean: initialized as false by default
- signed and unsigned int: (unsigned is always positive)
  - int8 to int256, uint8 to uint256 in multiples of 8
  - int8 -> -128 to +127
  - int16 -> -32768 to +32767
  - uint8 -> 0 - 255 (2^n - 1)
  - uint16 -> 0 - 65535
- simply writing int or uint is initialized with int256 / uint256

```solidity
...
    bool public sold;

    uint public price;
    int public location;
...

```

# arrays

fixed-size arrays

- fixed size at compile time
- bytes1, bytes2, ..., bytes32
- can hold any type
- member: length

```solidity
...
    // default value: 0x00
    bytes1 public b1;
    // default value: 0x0000
    bytes2 public b2;
    // default value: 0x000000
    bytes3 public b3;
    // ... upto 32 bytes


    // initialized as [0, 0, 0]
    uint[3] public three_zeros;
    uint[3] public numbers = [2, 3, 4];
...

```

dynamically-sized arrays

- Bytes
- String (utf8 encoded) is a dynamic array similar to bytes
- can hold any type
- member: length and push

```solidity
...
    uint[] public numbers;
...

```

# Bytes and String types

- special dynamic array
- String
  - equal to bytes
  - does not allow length or index access
  - utf-8 encoded
  - does not allow push method
- Bytes and Strings are reference types, not value types

```solidity
...
    // stores as 0x616263
    bytes public b1 = 'abc';
    // stores as abc
    string public s1 = 'abc';
...

```

# mappings

- similar to hashmaps / dict
- key -> value pairs
- all keys must be of same type
- all values must be of same type
- keys cannot be of types mapping, dynamic array, enum or struct
- values can be of any type
- mappings can only have a data location of storage
- provides constant time lookup
- not iterable
- search for unexisting key will return a default value

```solidity
...
    mapping (address => uint) public bids;
...

```

# structs and enums

```solidity
struct Planet {
    uint age;
    string name;
    address addr;
}

...
    Planet public universePlanet;

    enum State {Inhabited, Uninhabited, Unknown}
    State public universeState;
...

```

# storage vs memory vs calldata

- storage is the most expensive, but persisted to the blockchain
- memory is cheapper than storage and offers read and writes, but its data is not persisted outside of the function it lives in (scoped).
- calldata is cheaper than storage, but is immutable and read only

# built-in global variables

**msg**

- msg.sender: account address that generates the transaction
- msg.value: eth value (in wei) sent with the function call
- msg.gas: remaining gas, replaced by `gasleft()`
- msg.data: data field from the transaction or call that executed the transaction

**this**

- the current contract, explicitly convertible to Address `address(this)`

**block**

- block.timestamp: unix epoch
- block.number: current block number
- block.difficulty: difficulty of mining a block
- block.gaslimit: max gas limit all the transactions inside block allowed to consume.

**tx**

- tx.gasprice: gas price of the transaction
- tx.origin: sender of the transaction (!= msg.sender)

# modifiers

- `pure` for functions: disallows modification or access of state
- `view` for functions: disallows modification of state
- `payable` '' : allows them to receive ether together with a call
- `constant` for state vars:
  - dissallows assignment
  - does not occupy storage slot
- `immutable` for state vars:
  - allows exactly one assignment
  - stored in code
- `virtual` for function and modifiers: allows the function's or modifier's behaviour to be changed in derived contracts.
- `override` states that this function, modifier or public state variable changes the behaviour of a function or modifier in a base contract.

# contract address and balance: payable, receive, and fallback.
