# Storage Factory Contract Deployment 

This repo contains three Solidity smart contracts: `SimpleStorage`, `StorageFactory`, and `AddFiveStorage`. The `SimpleStorage` contract allows for basic storage and retrieval of data, `StorageFactory` demonstrates how one contract can deploy another, and `AddFiveStorage` overrides a function from `SimpleStorage`.

## Contracts

### SimpleStorage.sol

`SimpleStorage` is a basic contract that allows you to store, retrieve, and manage a list of people's favorite numbers. It also includes some additional placeholder contracts (`SimpleStorage2`, `SimpleStorage3`, `SimpleStorage4`) for demonstration purposes.

#### Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

contract SimpleStorage {
    uint256 myFavoriteNumber;

    struct Person {
        uint256 favoriteNumber;
        string name;
    }

    Person[] public listOfPeople;
    mapping(string => uint256) public nameToFavoriteNumber;

    function store(uint256 _newNumber) public virtual {
        myFavoriteNumber = _newNumber + 5;
    }

    function retrieve() public view returns (uint256) {
        return myFavoriteNumber;
    }

    function addPerson(string memory _name, uint256 _favoriteNumber) public {
        listOfPeople.push(Person(_favoriteNumber, _name));
        nameToFavoriteNumber[_name] = _favoriteNumber;
    }
}

contract SimpleStorage2 {}

contract SimpleStorage3 {}

contract SimpleStorage4 {}
```

### StorageFactory.sol

`StorageFactory` is a contract that can deploy instances of the `SimpleStorage` contract and interact with them.

#### Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import {SimpleStorage} from "./SimpleStorage.sol";

contract StorageFactory {
    SimpleStorage[] public listOfSimpleStorageContracts;

    function createSimpleStorageContract() public {
        SimpleStorage simpleStorageContractVariable = new SimpleStorage();
        listOfSimpleStorageContracts.push(simpleStorageContractVariable);
    }

    function sfStore(
        uint256 _simpleStorageIndex,
        uint256 _simpleStorageNumber
    ) public {
        listOfSimpleStorageContracts[_simpleStorageIndex].store(
            _simpleStorageNumber
        );
    }

    function sfGet(uint256 _simpleStorageIndex) public view returns (uint256) {
        return listOfSimpleStorageContracts[_simpleStorageIndex].retrieve();
    }
}
```

### AddFiveStorage.sol

`AddFiveStorage` is a contract that inherits from `SimpleStorage` and overrides the `store` function.

#### Contract Code

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import {SimpleStorage} from "./SimpleStorage.sol";

contract AddFiveStorage is SimpleStorage {
    function store(uint256 _favoriteNumber) public override {
        myFavoriteNumber = _favoriteNumber + 5;
    }
}
```

## Explanation

1. **SimpleStorage Contract**: Allows storing, retrieving a favorite number, and managing a list of people's favorite numbers.
2. **StorageFactory Contract**: Deploys instances of the `SimpleStorage` contract and allows interaction with them.
3. **AddFiveStorage Contract**: Inherits from `SimpleStorage` and overrides the `store` function to add a different value to the stored number.

### How the Deployment Works

- **Deploying SimpleStorage**: The `createSimpleStorageContract` function in `StorageFactory` deploys a new `SimpleStorage` contract and stores its address in an array.

#### Key Part of the StorageFactory Contract

```solidity
function createSimpleStorageContract() public {
    SimpleStorage simpleStorageContractVariable = new SimpleStorage();
    listOfSimpleStorageContracts.push(simpleStorageContractVariable);
}
```

## Conclusion

The `StorageFactory` contract demonstrates how one contract can deploy another contract in Solidity using the `new` keyword. The `AddFiveStorage` contract shows how to override functions in Solidity. This allows for creating new instances of contracts dynamically, which can be very useful in various decentralized applications.
