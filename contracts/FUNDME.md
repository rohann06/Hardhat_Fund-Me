## FundMe
[![MIT License](https://img.shields.io/apm/l/atomic-design-ui.svg?)](https://github.com/tterb/atomic-design-ui/blob/master/LICENSEs)

➡️ [FundMe.sol](https://github.com/rohanA6/Hardhat_Fund-Me/blob/main/contracts/FundMe.sol)

This contract is the main contract in this project Which provides the functionality to funders to funding and the deployer(owner) to withdrowing ETHs.

This is very simple and a basic smart contract to under stand...
- How to import, create and use libaries and interfaces,
- How to use modifier, constructor, and use error insted of using "revert()",
- Also how to build a own library in solidity.

## Work Flow of the contract 

As you read the story of the project(contract), the main aim o this project is to collect the funds, hold it, and when the owner of the contract call the withdrawal function all fund should be transfer to the account, after the withdraw the funders array will empty and the amount of ETHs in the contract should be 0.

Now break the sotory and turn it into a questions...

#### 1] How to collect the fund from the  funders?
- To answer this question create the function called ```fund()```, this funnction is for funders means for everyone so we make it ```public``` and because the funders can transfer the ETHs means there is a transection happaning so we make it ```payable```. 
- Then there is a condition that the funds should be >=0.5 ETH, for this first declare the minimum fund, after that use ```require()``` and put the condition, if it is false then console log the error.
```
function fund() public payable {
        require(
            msg.value.getConversionRate(s_priceFeed) >= MINIMUM_USD,
            "You need to spend more ETH!"
        );
        s_addressToAmountFunded[msg.sender] += msg.value;
        s_funders.push(msg.sender);
    } 
```

#### 2] How to withdraw the funds from the contract?
- To answer this create the function called ```withdraw()```, this function is for the owner of the contract to withdraw the funds from the contract. and because this function is for only for owner create ```Modifier()```
```
modifier onlyOwner() {
        // require(msg.sender == i_owner);
        if (msg.sender != i_owner) revert FundMe__NotOwner();
        _;
    }
```
- Now make this function ```public``` and ```payable``` because there is a transection happaning in the function
- Now as a story after the withdraw the funders array will empty and the amount of ETHs in the contract should be 0. as below
 ```
 function withdraw() public payable onlyOwner {
        for (
            uint256 funderIndex = 0;
            funderIndex < s_funders.length;
            funderIndex++
        ) {
            address funder = s_funders[funderIndex];
            s_addressToAmountFunded[funder] = 0;
        }
        s_funders = new address[](0);
        (bool success, ) = i_owner.call{value: address(this).balance}("");
        require(success);
    }
  ```
  - there are some view functions are also in this contract like...
  ```
  function getAddressToAmountFunded(address fundingAddress)
        public
        view
        returns (uint256)
    {
        return s_addressToAmountFunded[fundingAddress];
    }

    function getVersion() public view returns (uint256) {
        return s_priceFeed.version();
    }

    function getFunder(uint256 index) public view returns (address) {
        return s_funders[index];
    }

    function getOwner() public view returns (address) {
        return i_owner;
    }

    function getPriceFeed() public view returns (AggregatorV3Interface) {
        return s_priceFeed;
    }
  ```
    
    
    
