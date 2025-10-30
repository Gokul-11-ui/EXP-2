# Experiment 2: Blockchain-Based Crowdfunding (Kickstarter Alternative)
## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
A project owner starts a campaign with a funding goal and deadline.


Contributors can send ETH to the campaign.


If the goal is met before the deadline, funds are released to the project owner.


If the goal is not met, contributors can withdraw their funds.


## Program:
```
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
# Expected Output:
Users can contribute ETH to the campaign.


If the goal is met, the creator can withdraw funds.


If the goal is not met, contributors can claim a refund.

#output:

![output 1](https://github.com/user-attachments/assets/64a7d049-1c67-4e6e-92b2-b63951de98f8)

![output 2](https://github.com/user-attachments/assets/efbba652-ef0d-4d71-953e-585d8eb60e3c)

![output 3](https://github.com/user-attachments/assets/6475b519-32d1-4c70-8efb-9a9c4b4befc9)

![output 4](https://github.com/user-attachments/assets/946ec447-46dd-408d-9d22-2ce216bf230a)


# High-Level Overview:
Teaches decentralized fundraising.


Avoids fraud by ensuring funds are only transferred if the goal is met.

# RESULT: 
Thus, the result shows whether the campaign succeeded (funds go to creator) or failed (backers get refunds), with all actions transparently recorded on the blockchain.
