# Ethereum-Proof-Intermediate-Module-1
# Vehicle Management Smart Contract

This Solidity program is a simple Vehicle Management smart contract that allows the owner to manage the parking and removal of vehicles at different bays. The contract includes functionalities to park and unpark vehicles, ensure the availability of bays, and handle errors effectively.

## Description

This program is a basic smart contract written in Solidity, a programming language used for developing smart contracts on the Ethereum blockchain. The contract allows the owner to:

1. Park vehicles at available bays.
2. Unpark vehicles from bays.
3. Ensure the availability of bays and handle errors effectively using required statements.

## Getting Started

### Executing Program

To run this program, we can use Remix, an online Solidity IDE. Follow the steps below to get started:

1. Go to the Remix website.
2. Create a new file by clicking on the "+" icon in the left-hand sidebar.
3. Save the file with a `.sol` extension (e.g., `VehicleManagement.sol`).
4. Copy and paste the following code into the file:

```solidity
// SPDX-License-Identifier: MIT
pragma solidity 0.8.25;

contract VehicleBay {
    address public admin;
    uint public maxBays = 6;
    uint public currentVehicleCount = 0;

    constructor() {
        admin = msg.sender;
    }

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only the admin can access this function");
        _;
    }

    mapping(uint => bool) public isBayOccupied;
    mapping(uint => address) public vehicleAtBay;

    function parkVehicle(address vehicleAddress) public onlyAdmin {
        require(currentVehicleCount < maxBays, "No bay available");
        for (uint i = 1; i <= maxBays; i++) {
            if (!isBayOccupied[i]) {
                isBayOccupied[i] = true;
                vehicleAtBay[i] = vehicleAddress;
                currentVehicleCount++;
                break;
            }
        }
    }

    function unparkVehicle(address vehicleAddress) public onlyAdmin {
        require(currentVehicleCount > 0, "No vehicle to remove");
        uint bayOfVehicle = 0;
        bool vehicleFound = false;

        for (uint i = 1; i <= maxBays; i++) {
            if (vehicleAtBay[i] == vehicleAddress) {
                bayOfVehicle = i;
                vehicleFound = true;
                break;
            }
        }

        if (!vehicleFound) {
            revert("Vehicle not found!");
        }

        vehicleAtBay[bayOfVehicle] = address(0);
        isBayOccupied[bayOfVehicle] = false;
        currentVehicleCount--;
    }

    function verifyVehicleCount() public view {
        assert(currentVehicleCount > 0 && currentVehicleCount <= maxBays);
    }
}
```

### Compiling the Code

1. Click on the "Solidity Compiler" tab in the left-hand sidebar.
2. Ensure the "Compiler" option is set to "0.8.25" (or another compatible version).
3. Click on the "Compile VehicleManagement.sol" button.

### Deploying the Contract

1. Go to the "Deploy & Run Transactions" tab in the left-hand sidebar.
2. Select the "VehicleManagement" contract from the dropdown menu.
3. Click on the "Deploy" button.

### Interacting with the Contract

You can interact with the contract by calling the functions once the contract is deployed. Ensure you are connected to the owner's address to perform owner-restricted functions.

#### Park Vehicle:
1. Select the `parkVehicle` function.
2. Enter the vehicle's address to park.
3. Click on the "transact" button.

#### Unpark Vehicle:
1. Select the `unparkVehicle` function.
2. Enter the vehicle's address to unpark.
3. Click on the "transact" button.

#### Verify Vehicle Count:
1. Select the `verifyVehicleCount` function.
2. Relevant output will be visible on the screen.

## Authors
Sneha Thapa
[https://www.linkedin.com/in/sneha-thapa-00a353270/]
