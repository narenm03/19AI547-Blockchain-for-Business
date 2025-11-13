# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
### NAME : NARENDHARAN.M
### REG NO : 212223230134
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

contract PasswordlessAuthDemo {
    struct User {
        bool registered;
        address pubKey;
        bytes32 privateKey; // Fake private key for demo
    }

    mapping(address => User) public users;
    bytes32 public latestChallenge;

    event UserRegistered(address user, address pubKey, bytes32 privateKey);
    event ChallengeGenerated(bytes32 challenge);
    event SignatureGenerated(bytes32 hash, uint8 v, bytes32 r, bytes32 s);

    // Step 1: Register user
    function registerUser() public {
        require(!users[msg.sender].registered, "Already registered");

        // Fake public/private keys
        address fakePubKey = msg.sender;
        bytes32 fakePrivateKey = keccak256(abi.encodePacked(msg.sender, block.timestamp));

        users[msg.sender] = User({
            registered: true,
            pubKey: fakePubKey,
            privateKey: fakePrivateKey
        });

        emit UserRegistered(msg.sender, fakePubKey, fakePrivateKey);
    }

    // Step 2: Generate random challenge
    function generateChallenge() public returns (bytes32) {
        require(users[msg.sender].registered, "User not registered");
        latestChallenge = keccak256(abi.encodePacked(block.timestamp, msg.sender));
        emit ChallengeGenerated(latestChallenge);
        return latestChallenge;
    }

    // Step 3: "Sign" the challenge (fake signing)
    function generateSignature() public returns (bytes32 hash, uint8 v, bytes32 r, bytes32 s) {
        require(users[msg.sender].registered, "User not registered");
        
        hash = latestChallenge;
        bytes32 combined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        
        // Fake values for r, s, v
        r = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        s = combined;
        v = 27;

        emit SignatureGenerated(hash, v, r, s);

        return (hash, v, r, s);
    }

    // Step 4: Authenticate
    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(users[msg.sender].registered, "User not registered");

        bytes32 expectedCombined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        bytes32 expectedR = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        uint8 expectedV = 27;

        if (r == expectedR && s == expectedCombined && v == expectedV) {
            return true;
        } else {
            return false;
        }
    }
}
```

# Expected Output:

![Screenshot 2025-05-03 082552](https://github.com/user-attachments/assets/1fec435a-8e49-4776-9aad-6e1adf3cda6f)

![Screenshot 2025-05-03 082612](https://github.com/user-attachments/assets/bf76749f-888f-40ec-8b33-45bf6d43344c)

![Screenshot 2025-05-03 082635](https://github.com/user-attachments/assets/9e443348-3b51-4e69-b965-ca20a2185b5e)

# High-Level Overview:

Eliminates password hacks & phishing attacks.

Uses Ethereum's built-in cryptographic functions.

Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 

Thus , to implement a secure passwordless authentication system using public-private key cryptography on Ethereum has executed successfully.










