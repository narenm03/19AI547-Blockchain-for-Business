# Experiment 2: Blockchain-Based Crowdfunding (Kickstarter Alternative)
## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
1. **Create Campaign** – Deploy smart contract with goal, deadline, and details.  
2. **Contribute Funds** – Backers send funds to the smart contract.  
3. **Check Outcome (After Deadline)** –  
   - If goal met → funds go to creator.  
   - If goal not met → backers can refund.  
4. **Withdraw/Refund** – Creator withdraws or backers claim refund.  
5. **Ensure Transparency** – All actions are logged on blockchain.
## Program:
```
Name:Jude Clement Jose G
Reg no:212224230109

// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    struct Campaign {
        address creator;
        uint256 goal;
        uint256 deadline;
        uint256 amountRaised;
        bool goalMet;
        mapping(address => uint256) contributions;
    }

    Campaign public campaign;

    constructor(uint256 _goal, uint256 _duration) {
        campaign.creator = msg.sender;
        campaign.goal = _goal;
        campaign.deadline = block.timestamp + _duration;
    }

    function contribute() public payable {
        require(block.timestamp < campaign.deadline, "Campaign ended");
        campaign.amountRaised += msg.value;
        campaign.contributions[msg.sender] += msg.value;
    }

    function withdrawFunds() public {
        require(msg.sender == campaign.creator, "Only creator can withdraw");
        require(campaign.amountRaised >= campaign.goal, "Goal not met");
        payable(msg.sender).transfer(campaign.amountRaised);
        campaign.goalMet = true;
    }

    function refund() public {
        require(block.timestamp > campaign.deadline, "Campaign still active");
        require(campaign.amountRaised < campaign.goal, "Goal was met");
        uint256 amount = campaign.contributions[msg.sender];
        campaign.contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```
# Output:
![Screenshot 2025-04-16 094619](https://github.com/user-attachments/assets/16b74a24-7170-4f4c-91fd-ab520ed2c2dc)
![Screenshot 2025-04-16 094801](https://github.com/user-attachments/assets/ad272de7-9d5b-4bb1-9f99-38ace96cdcb5)



1. **Successful Campaign**: Goal met, funds released to creator.  
2. **Unsuccessful Campaign**: Goal not met, backers can claim refunds.  
3. **Transparent Records**: All contributions and transactions visible on blockchain.  
4. **No Manual Control**: Smart contract automates fund handling securely.

# RESULT: 
The result shows whether the campaign succeeded (funds go to creator) or failed (backers get refunds), with all actions transparently recorded on the blockchain.


