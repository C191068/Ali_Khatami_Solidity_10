## Ali_Khatami_Solidity_10(Learning from the video of Pattrick Collins)

### Array and Struct (part2)

When people actually send money to the contract created at this link https://github.com/C191068/Ali_Khatami_solidity_09.git<br>
And we want to keep track of all people who send us money<br>

For that at first we need the data structure ```array```  and we will create array of addresses called ```funders``` <br>

We will keep adding all the funders who send money to us<br>

```

//SPDX-License-Identifier:MIT

pragma solidity ^0.8.8;

import "@chainlink/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";

contract akrkFundMe  {

    uint256 public minimumUSD=50 * 1e18; //as getConversionRate() returns 18 zeroes after decimel place
 
    address[] public funders;// we create an address array make it public 
// we have use payable keyword with the function below to make it payable with any native blockchain currency
    //below we have done mapping of addresses to how much money each of the people sent
    mapping(addresses => uint256) public addressToAmountFunded ;
     
    function fund() public payable {
     
     //as we want to convert msg.value to USD we made the following changes
        require(getConversionRate(msg.value) >= minimumUSD , "Not send enough");// to get accees to the 'value' at deploy at run transaction tab
        //1e18 is 1 ether, here the value must be greater than 1 ether
        //require is a checker it checks whether the value is greater than 1 ether
        // if not it is going to revert with an error messsage shown above
        //adding funders to the array 
         funders.push(msg.sender);

    }// To send money. We made it public so that anybody can call it

  //The function below is created so that we can get price of ethereum in terms of USD , so that we can convert msg.value to USD
//Both functions are public because we can do whatever we want with them
//By using the getPrice() function below we are going to interact with contract outside of our project
  
  
    function getPrice() public view returns(uint256) {

        //ABI
        //Address 0x694AA1769357215DE4FAC081bf1f309aDC325306\
        // below we we create AggregatorV3Interface object called priceFeed equal to AggregatorV3Interface contract at address 0x694AA1769357215DE4FAC081bf1f309aDC325306
        // this address 0x694AA1769357215DE4FAC081bf1f309aDC325306 will have all the functinality at AggregatorV3Interface
        AggregatorV3Interface priceFeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306);
        //below we will call latestRoundData() function of AggregatorV3Interface on price feed
        (,int256 price,,,)= priceFeed.latestRoundData();

        return uint256(price * 1e10); //typecasting = converting int256 to uint256

        
    }

    //below we have created a new function

    function getVerion() public view returns (uint256)
    {
        AggregatorV3Interface priceFeed = AggregatorV3Interface(0x694AA1769357215DE4FAC081bf1f309aDC325306); //it is of type 'AggregatorV3Interface'
        return priceFeed.version();

    }
//below is a function by which we gonna convert msg.value from etherium to terms of dollars
//to this function we are going to pass some ethAmount in one side
//on the other side we gonna find how much of that ethAQmount is in terms of USD
    function getConversionRate(uint256 ethAmount)  public view returns (uint256) {

        uint256 ethPrice = getPrice(); // at first we gonna call getPrice function to get the price of ethereum

        uint256 ethAmountInUSD= (ethPrice * ethAmount)/1e18;

        return ethAmountInUSD;

       



    }
    

}

```


like ```msg.value``` , ```msg.sender```  is an always available global keyword<br>

```msg.value``` stands for how much ethereum or how much native blockchain currency is sent <br>

```msg.sender``` is the address of whoever calls the fund function<br>

![f69](https://user-images.githubusercontent.com/89090776/236680143-41275d6b-1bf7-4739-af45-ab87dc065f50.jpg)
Figure1: (msg.sender) is going to be equivalent the address of our wallet account at metamask as it is calling the fund functtion<br>

as our metamask address is sending the ether we gonna add our metamask wallet address to the funders list<br>
By this way we can keep tract of donators who are donating to our contract<br>







