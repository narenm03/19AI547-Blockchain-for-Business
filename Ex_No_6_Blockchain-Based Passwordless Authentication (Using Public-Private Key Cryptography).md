# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuth {
    mapping(address => bool) public registeredUsers;

    event UserRegistered(address user);
    event UserAuthenticated(address user);

    function registerUser() public {
        require(!registeredUsers[msg.sender], "Already registered");
        registeredUsers[msg.sender] = true;
        emit UserRegistered(msg.sender);
    }

    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(registeredUsers[msg.sender], "User not registered");
        address signer = ecrecover(hash, v, r, s);
        return signer == msg.sender;
    }
}
```

# Expected Output:
![Screenshot 2025-05-03 082835](https://github.com/user-attachments/assets/993e2988-a48a-4c3f-9753-f79294988359)
![Screenshot 2025-05-03 082854](https://github.com/user-attachments/assets/0b245266-1bf1-4a72-a9dd-42fb4c2fa43e)
![Screenshot 2025-05-03 082909](https://github.com/user-attachments/assets/0f9d8d77-4eef-48f1-b7e2-f7ca26514d81)
![Screenshot 2025-05-03 082926](https://github.com/user-attachments/assets/9d7abe4c-0f4b-448d-9df5-17f594d285c3)



# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks is completed successfully.
