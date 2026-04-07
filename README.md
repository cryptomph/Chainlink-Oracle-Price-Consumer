# Chainlink-Oracle-Price-Consumer
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "https://github.com/smartcontractkit/chainlink/blob/develop/contracts/src/v0.8/interfaces/AggregatorV3Interface.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v5.0.0/contracts/access/Ownable.sol";

contract ETHPriceConsumer is Ownable {
    AggregatorV3Interface public priceFeed;

    // Base Mainnet ETH/USD Feed (check latest on chain.link)
    constructor(address _feed) Ownable(msg.sender) {
        priceFeed = AggregatorV3Interface(_feed);
    }

    function getLatestPrice() public view returns (int256 price, uint8 decimals) {
        (, price,,,) = priceFeed.latestRoundData();
        decimals = priceFeed.decimals();
        return (price, decimals);
    }

    function updateFeed(address newFeed) external onlyOwner {
        priceFeed = AggregatorV3Interface(newFeed);
    }
}
