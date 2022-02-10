# NFT Worlds Web3J Contract Wrappers

This repository provides the Web3J contract wrappers necessary for interacting with the NFT Worlds smart contracts on the Ethereum and Polygon blockchain.

Additionally, it also provides standard Web3J wrappers for ERC20 (Token) and ERC721 (Non-Fungible Token) contracts, allowing you to develop integrations with any ERC20 and ERC721 smart contracts on the Ethereum or Polygon blockchain.

This repository is intended as a resource for developers interested in building experiences on top of NFT Worlds smart contracts from the ground up within the Java ecosystem.

If you're developing integrations directly into your NFT World's server you can use our convenience blockchain plugin for NFT Worlds that includes all of these wrappers and convenience methods for using them within the Spigot/Paper based Minecraft server environment: **To Be Released**

## Web3J Quickstart

We use Web3J to power our interactions with smart contracts on the blockchain within Java environments. 

Please review the quickstart guide below, as well as the link explaining how to use Web3J to interact with smart contracts to get familiar. You will need to add the web3j dependencie to your Java project. This repository is at this time only a provider of the smart contract wrapper classes.

[Web3J Quickstart Guide](https://docs.web3j.io/4.8.7/quickstart/)
<br>
[Web3J Smart Contract Interaction](http://docs.web3j.io/4.8.7/smart_contracts/interacting_with_smart_contract/)

## Web3J Contract Interaction Example

Once you've added web3j to your project, interacting with a contract is fairly straightforward.

The following example uses a random wallet address for reading contract data from the NFT Worlds Players contract on the Polygon network. The players contract is a mapping contract that associates Minecraft player UUIDs to their associated wallets. Additionally, it exposes a way to set state data JSON blob via an IPFS hash in order to store player's in-game state data in a decentralized manner. 

If you want to write contract data to the blockchain through a smart contract, you will need to use your own wallet credentials.

Polygon read example
```java
  import org.web3j.protocol.Web3j;
  import org.web3j.protocol.http.HttpService;
  import org.web3j.tx.gas.DefaultGasProvider;
  import org.web3j.crypto.Credentials;
  import org.web3j.crypto.Keys;

  Web3j polygonRpc = Web3j.build(new HttpService('https://polygon-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY_FOR_POLYGON'));
  DefaultGasProvider gasProvider = new DefaultGasProvider();
  Credentials credentials = null;
  
  try {
    credentials = Credentials.create(Keys.createEcKeyPair()); // Only reading, if writing set your own credentials
  } catch (Exception e) {
    e.printStackTrace();
  }
  
  PolygonPlayers nftwPolygonPlayersContract = PolygonPlayers.load(
    "0x285984c5d7a9D37D1805872F051C6b8aFa7418A4", // NFT Worlds Polygon Players Contract Address (https://polygonscan.com/address/0x285984c5d7a9D37D1805872F051C6b8aFa7418A4)
    polygonRpc,
    credentials,
    gasProvider
  );
  
  // Contract interactions / reads / etc example.
  String playerMinecraftUUID = "8fa28e61-a75c-4ad5-8dac-435e0951ef09";
  nftwPolygonPlayersContract.getPlayerPrimaryWallet(playerMinecraftUUID).send(); // string
  nftwPolygonPlayersContract.getPlayerSecondaryWallets(playerMinecraftUUID).send(); // string array
  // etc, check the contract wrappers for available methods.
```

## Web3J & RPC nodes

In order to interact with the blockchain, you'll need access to an Ethereum or Polygon RPC (remote procedure call) node. If you're reading or writing data on Ethereum, you need access to an Ethereum RPC node. If you're reading or writing on Polygon, you need a Polygon RPC node.  At this time, NFT Worlds does not provide this. You will need to use a service like Alchemy.io to get an authenticated http endpoint that is the RPC node web3j interacts with for your needs. Alchemy provides Ethereum and Polygon RPC nodes with very reasonable pricing. You will get an HTTP Endpoint for RPC interactions from these services, it can be used like so:

```java
Web3j polygonRpc = Web3j.build(new HttpService('https://polygon-mainnet.g.alchemy.com/v2/YOUR_ALCHEMY_KEY_FOR_POLYGON'));
Web3j ethereumRpc = Web3j.build(new HttpService('https://eth-mainnet.alchemyapi.io/v2/YOUR_ALCHEMY_KEY_FOR_ETHEREUM'));
```

[Visit Alchemy.io](https://alchemyapi.io/)

## NFT Worlds Contract Addresses

NFT Worlds has various smart contracts used within its ecosystem. Depending on what you need, you can find the correct contract addresses for our various contracts on the Ethereum and Polygon network below.

**Ethereum $WRLD Token Contract:** https://etherscan.io/address/0xD5d86FC8d5C0Ea1aC1Ac5Dfab6E529c9967a45E9<br>
**Polygon $WRLD Token Contract:** https://polygonscan.com/address/0xD5d86FC8d5C0Ea1aC1Ac5Dfab6E529c9967a45E9<br>
**Polygon Players Contract:** https://polygonscan.com/address/0x285984c5d7a9D37D1805872F051C6b8aFa7418A4#code
