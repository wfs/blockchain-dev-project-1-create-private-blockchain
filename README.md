# Overview

---

![Alt text](/dependency-graph-html-unhighlighted.png?raw=true "dependency graph html unhighlighted")

```bash
$ npx dependency-cruiser -v -T dot ./ | dot -T svg | npx depcruise-wrap-stream-in-html > dependency-graph.html
```

# Project Rubic

---

## 1. Complete unfinished block.js implementation

![Alt text](/blocksjs_dependency_graph.png?raw=true "blocks.js dependency graph highlighted")

### 1.1.1. Criteria

Modify the validate() function to validate if the block has been tampered or not.

### 1.1.2. Meets Specifications

- Return a new promise to allow the method be called asynchronous.
- Create an auxiliary variable and store the current hash of the block in it (this represent the block object)
- Recalculate the hash of the entire block (Use SHA256 from crypto-js library)
- Compare if the auxiliary hash value is different from the calculated one.
- Resolve true or false depending if it is valid or not.

![Alt text](/blockjs_validate_function.png?raw=true "blocks.js validate function")

### 1.2.1. Criteria

Modify the 'getBData()' function to return the block body (decoding the data)

### 1.2.2. Meets Specifications

- Use hex2ascii module to decode the data
- Because data is a javascript object use JSON.parse(string) to get the Javascript Object

  Resolve with the data and make sure that you don't need to return the data for the genesis block OR reject with an error.

![Alt text](/blockjs_getbdata_function.png?raw=true "blocks.js getbdata function")

---

## 2. Complete unfinished blockchain.js implementation

![Alt text](/blockchainjs_dependency_graph.png?raw=true "blockchain.js dependency graph")

### 2.1.1. Criteria

Modify the '\_addBlock(block)' function to store a block in the chain

### 2.1.2. Meets Specification

- Must return a Promise that will resolve with the block added OR reject if an error happen during the execution.
- height must be checked to assign the previousBlockHash
  - Assign the timestamp & the correct height
  - Create the block hash and push the block into the chain array.
    Don't forget to update the `this.height`

![Alt text](/blockchainjs_private_addblock_function.png?raw=true "blockchain.js private addblock function")

### 2.2.1. Criteria

Modify 'requestMessageOwnershipVerification(address)' to allow you to request a message that you will use to sign it with your Bitcoin Wallet (Electrum or Bitcoin Core)

### 2.2.2. Meets Specification

- must return a Promise that will resolve with the message to be signed

![Alt text](/blockchainjs_request_message_ownership_verification_address_param.png?raw=true "blockchain.js request message ownership verification address param")

### 2.3.1. Criteria

Modify 'submitStar(address, message, signature, star)' function to register a new Block with the star object
into the chain

### 2.3.2. Meets Specification

- must resolve with the Block added or reject with an error.
- time elapsed between when the message was sent and the current time must be less that 5 minutes
- must verify the message with wallet address and signature: bitcoinMessage.verify(message, address, signature)
- must create the block and add it to the chain if verification is valid

![Alt text](/blockchainjs_submitstar_function.png?raw=true "blockchain.js submitstar function")

### 2.4.1. Criteria

Modify the 'getBlockByHash(hash)' function to retrieve a Block based on the hash parameter

### 2.4.2. Meets Specification

- must return a Promise that will resolve with the Block

![Alt text](/blockchainjs_get_block_by_hash_function.png?raw=true "blockchain.js get block by hash function")

### 2.5.1. Criteria

Modify the 'getStarsByWalletAddress (address)' function to return an array of Stars from an owners collection

### 2.5.2. Meets Specification

- must return a Promise that will resolve with an array of the owner address' Stars from the chain

![Alt text](/blockchainjs_get_stars_by_wallet_address_function.png?raw=true "blockchain.js get stars by wallet address function")

### 2.6.1. Criteria

Modify the 'validateChain()' function

### 2.6.2. Meets Specification

- must return a Promise that will resolve with the list of errors when validating the chain
- must validate each block using validateBlock()
- Each Block should check with the previousBlockHash
- execute the `validateChain()` function every time a block is added
- create an endpoint that will trigger the execution of `validateChain()`

![Alt text](/blockchainjs_validate_chain_function.png?raw=true "blockchain.js validate chain function")

---

## 3. Test your App functionality

### 3.1.1. Criteria

Use 'POSTMAN' or similar service to test your blockchains endpoints and send screenshots of each call

### 3.1.2. Meets Specification

- must use a GET call to request the Genesis block

```bash
$ curl -vo curl_test_output.json localhost:8000/block/height/0
```

```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0*   Trying 127.0.0.1:8000...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8000 (#0)
> GET /block/height/0 HTTP/1.1
> Host: localhost:8000
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< X-Powered-By: Express
< Content-Type: application/json; charset=utf-8
< Content-Length: 189
< ETag: W/"bd-H1cYF2anyJBOp4VxQ9Wpp9SCDAs"
< Date: Tue, 14 Dec 2021 00:54:23 GMT
< Connection: keep-alive
< Keep-Alive: timeout=5
<
{ [189 bytes data]
100   189  100   189    0     0  94500      0 --:--:-- --:--:-- --:--:-- 94500
* Connection #0 to host localhost left intact
```

```
$ cat curl_test_output.json | jq
{
  "hash": "628728e502bce709b6ee91a45239653508c3d4225dbf3fa0b2f31f4ac4690a57",
  "height": 0,
  "body": "7b2264617461223a2247656e6573697320426c6f636b227d",
  "time": "1639442507",
  "previousBlockHash": null
}
```

- must use a POST call to requestValidation

INFO: only legacy generated addresses can be signed with the bitcoin-core client.
See [udacity knowlege base](https://knowledge.udacity.com/questions/722180).

![Alt text](/bitcoin_core_rpc_console_getnewaddress_legacy.png?raw=true "bitcoin core rpc console getnewaddress legacy")

```bash
 curl -X POST localhost:8000/requestValidation -H 'Content-Type: application/json' -d '{"address":"n14JJrA5FVaU1phHFFMLfBhwsBh775TMKN"}'
```

```bash
"n14JJrA5FVaU1phHFFMLfBhwsBh775TMKN:1639459274:starRegistry"
```

- must sign message with your wallet

![Alt text](/click_to_sign_message.png?raw=true "click to sign message")

![Alt text](/message_sign_unlock_wallet_with_passphrase.png?raw=true "message sign unlock wallet with passphrase")

![Alt text](/message_signed.png?raw=true "message signed")

Message signed returning this signature:

```bash
IOLYTuvRmsNZhR1AaRFIsKcUdUCQhmxDIJuq3a8f4ZLlAgfZizG5z2ohVmBHRcGMRdRODCYAw5mFrbdNUAOBrOY=
```

- must submit your Star

```bash
$ curl -X POST localhost:8000/submitStar -H 'Content-Type: application/json' -d @star.json
```

```bash
{"hash":"65dd7a2015f358be570853424e3d97434051fc99a8a5c74d317b82f9b4dac91c","height":1,"body":"7b226f776e6572223a226e31344a4a724135465661553170684846464d4c66426877734268373735544d4b4e222c2273746172223a7b22646563223a223638c2b0203532272035362e39222c227261223a223136682032396d20312e3073222c2273746f7279223a2254657374696e67207468652073746f72792034227d7d","time":"1639463464","previousBlockHash":"628728e502bce709b6ee91a45239653508c3d4225dbf3fa0b2f31f4ac4690a57"}
```

- must use GET call to retrieve starts owned by a particular address

```bash
$ curl -v localhost:8000/blocks/n14JJrA5FVaU1phHFFMLfBhwsBh775TMKN
*   Trying 127.0.0.1:8000...
* TCP_NODELAY set
* Connected to localhost (127.0.0.1) port 8000 (#0)
> GET /blocks/n14JJrA5FVaU1phHFFMLfBhwsBh775TMKN HTTP/1.1
> Host: localhost:8000
> User-Agent: curl/7.68.0
> Accept: */*
>
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 OK
< X-Powered-By: Express
< Content-Type: application/json; charset=utf-8
< Content-Length: 131
< ETag: W/"83-3WVayhc0pp3/tIzBBuJ0UUsGuyA"
< Date: Tue, 14 Dec 2021 06:35:14 GMT
< Connection: keep-alive
< Keep-Alive: timeout=5
<
* Connection #0 to host localhost left intact
```

```bash
$ cat curl_test_output.json | jq
[
  {
    "owner": "n14JJrA5FVaU1phHFFMLfBhwsBh775TMKN",
    "star": {
      "dec": "68Â° 52' 56.9",
      "ra": "16h 29m 1.0s",
      "story": "Testing the story 4"
    }
  }
]
```

---

# Private Blockchain Application

You are starting your journey as a Blockchain Developer, this project allows you to demonstrate
that you are familiarized with the fundamentals concepts of a Blockchain platform.
Concepts like: - Block - Blockchain - Wallet - Blockchain Identity - Proof of Existance

Are some of the most important components in the Blockchain Framework that you will need to describe and also
why not? Implement too.

In this project you will have a boilerplate code with a REST Api already setup to expose some of the functionalities
you will implement in your private blockchain.

## What problem will you solve implementing this private Blockchain application?

Your employer is trying to make a test of concept on how a Blockchain application can be implemented in his company.
He is an astronomy fans and he spend most of his free time on searching stars in the sky, that's why he would like
to create a test application that will allows him to register stars, and also some others of his friends can register stars
too but making sure the application know who owned each star.

### What is the process describe by the employer to be implemented in the application?

1. The application will create a Genesis Block when we run the application.
2. The user will request the application to send a message to be signed using a Wallet and in this way verify the ownership over the wallet address. The message format will be: `<WALLET_ADRESS>:${new Date().getTime().toString().slice(0,-3)}:starRegistry`;
3. Once the user have the message the user can use a Wallet to sign the message.
4. The user will try to submit the Star object for that it will submit: `wallet address`, `message`, `signature` and the `star` object with the star information.
   The Start information will be formed in this format:
   ```json
       "star": {
           "dec": "68° 52' 56.9",
           "ra": "16h 29m 1.0s",
           "story": "Testing the story 4"
   	}
   ```
5. The application will verify if the time elapsed from the request ownership (the time is contained in the message) and the time when you submit the star is less than 5 minutes.
6. If everything is okay the star information will be stored in the block and added to the `chain`
7. The application will allow us to retrieve the Star objects belong to an owner (wallet address).

## What tools or technologies you will use to create this application?

- This application will be created using Node.js and Javascript programming language. The architecture will use ES6 classes
  because it will help us to organize the code and facilitate the maintnance of the code.
- The company suggest to use Visual Studio Code as an IDE to write your code because it will help you debug the code easily
  but you can choose the code editor you feel confortable with.
- Some of the libraries or npm modules you will use are:
  - "bitcoinjs-lib": "^4.0.3",
  - "bitcoinjs-message": "^2.0.0",
  - "body-parser": "^1.18.3",
  - "crypto-js": "^3.1.9-1",
  - "express": "^4.16.4",
  - "hex2ascii": "0.0.3",
  - "morgan": "^1.9.1"
    Remember if you need install any other library you will use `npm install <npm_module_name>`

Libraries purpose:

1. `bitcoinjs-lib` and `bitcoinjs-message`. Those libraries will help us to verify the wallet address ownership, we are going to use it to verify the signature.
2. `express` The REST Api created for the purpose of this project it is being created using Express.js framework.
3. `body-parser` this library will be used as middleware module for Express and will help us to read the json data submitted in a POST request.
4. `crypto-js` This module contain some of the most important cryotographic methods and will help us to create the block hash.
5. `hex2ascii` This library will help us to **decode** the data saved in the body of a Block.

## Understanding the boilerplate code

The Boilerplate code is a simple architecture for a Blockchain application, it includes a REST APIs application to expose the your Blockchain application methods to your client applications or users.

1. `app.js` file. It contains the configuration and initialization of the REST Api, the team who provide this boilerplate code suggest do not change this code because it is already tested and works as expected.
2. `BlockchainController.js` file. It contains the routes of the REST Api. Those are the methods that expose the urls you will need to call when make a request to the application.
3. `src` folder. In here we are going to have the main two classes we needed to create our Blockchain application, we are going to create a `block.js` file and a `blockchain.js` file that will contain the `Block` and `BlockChain` classes.

### Starting with the boilerplate code:

First thing first, we are going to download or clone our boilerplate code.

Then we need to install all the libraries and module dependencies, to do that: open a terminal and run the command `npm install`

**( Remember to be able to work on this project you will need to have installed in your computer Node.js and npm )**

At this point we are ready to run our project for first time, use the command: `node app.js`

You can check in your terminal the the Express application is listening in the PORT 8000

## What do I need to implement to satisfy my employer requirements?

1.  `block.js` file. In the `Block` class we are going to implement the method:
    `validate()`.
    /\*\*
    - The `validate()` method will validate if the block has been tampered or not.
    - Been tampered means that someone from outside the application tried to change
    - values in the block data as a consecuence the hash of the block should be different.
    - Steps:
    - 1. Return a new promise to allow the method be called asynchronous.
    - 2. Save the in auxiliary variable the current hash of the block (`this` represent the block object)
    - 3. Recalculate the hash of the entire block (Use SHA256 from crypto-js library)
    - 4. Compare if the auxiliary hash value is different from the calculated one.
    - 5. Resolve true or false depending if it is valid or not.
    - Note: to access the class values inside a Promise code you need to create an auxiliary value `let self = this;`
      \*/
2.  `block.js` file. In the `Block` class we are going to implement the method:
    `getBData()`.
    /\*\*
    - Auxiliary Method to return the block body (decoding the data)
    - Steps:
    -
    - 1. Use hex2ascii module to decode the data
    - 2. Because data is a javascript object use JSON.parse(string) to get the Javascript Object
    - 3. Resolve with the data and make sure that you don't need to return the data for the `genesis block`
    -     or Reject with an error.
      \*/
3.  `blockchain.js` file. In the `Blockchain` class we are going to implement the method:
    `_addBlock(block)`.
    /\*\*
    - \_addBlock(block) will store a block in the chain
    - @param {\*} block
    - The method will return a Promise that will resolve with the block added
    - or reject if an error happen during the execution.
    - You will need to check for the height to assign the `previousBlockHash`,
    - assign the `timestamp` and the correct `height`...At the end you need to
    - create the `block hash` and push the block into the chain array. Don't for get
    - to update the `this.height`
    - Note: the symbol `_` in the method name indicates in the javascript convention
    - that this method is a private method.
      \*/
4.  `blockchain.js` file. In the `Blockchain` class we are going to implement the method:
    `requestMessageOwnershipVerification(address)`
    /\*\*
    - The requestMessageOwnershipVerification(address) method
    - will allow you to request a message that you will use to
    - sign it with your Bitcoin Wallet (Electrum or Bitcoin Core)
    - This is the first step before submit your Block.
    - The method return a Promise that will resolve with the message to be signed
    - @param {_} address
      _/
5.  `blockchain.js` file. In the `Blockchain` class we are going to implement the method:
    `submitStar(address, message, signature, star)`
    /\*\*
    - The submitStar(address, message, signature, star) method
    - will allow users to register a new Block with the star object
    - into the chain. This method will resolve with the Block added or
    - reject with an error.
    - Algorithm steps:
    - 1. Get the time from the message sent as a parameter example: `parseInt(message.split(':')[1])`
    - 2. Get the current time: `let currentTime = parseInt(new Date().getTime().toString().slice(0, -3));`
    - 3. Check if the time elapsed is less than 5 minutes
    - 4. Veify the message with wallet address and signature: `bitcoinMessage.verify(message, address, signature)`
    - 5. Create the block and add it to the chain
    - 6. Resolve with the block added.
    - @param {\*} address
    - @param {\*} message
    - @param {\*} signature
    - @param {_} star
      _/
6.  `blockchain.js` file. In the `Blockchain` class we are going to implement the method:
    `getBlockByHash(hash)`
    /\*\*
    - This method will return a Promise that will resolve with the Block
    - with the hash passed as a parameter.
    - Search on the chain array for the block that has the hash.
    - @param {_} hash
      _/
7.  `blockchain.js` file. In the `Blockchain` class we are going to implement the method:
    `getStarsByWalletAddress (address)`
    /\*\*
    - This method will return a Promise that will resolve with an array of Stars objects existing in the chain
    - and are belongs to the owner with the wallet address passed as parameter.
    -
    - @param {_} address
      _/
8.  `blockchain.js` file. In the `Blockchain` class we are going to implement the method:
    `validateChain()`
    /\*\*
    - This method will return a Promise that will resolve with the list of errors when validating the chain.
    - Steps to validate:
    - 1. You should validate each block using `validateBlock`
    - 2. Each Block should check the with the previousBlockHash
         \*/

## How to test your application functionalities?

To test your application I recommend you to use POSTMAN, this tool will help you to make the requests to the API.
Always is useful to debug your code see what is happening in your algorithm, so I will let you this video for you to check on how to do it >https://www.youtube.com/watch?v=6cOsxaNC06c . Try always to debug your code to understand what you are doing.

1. Run your application using the command `node app.js`
   You should see in your terminal a message indicating that the server is listening in port 8000:

   > Server Listening for port: 8000

2. To make sure your application is working fine and it creates the Genesis Block you can use POSTMAN to request the Genesis block:
   ![Request: http://localhost:8000/block/0 ](https://s3.amazonaws.com/video.udacity-data.com/topher/2019/April/5ca360cc_request-genesis/request-genesis.png)
3. Make your first request of ownership sending your wallet address:
   ![Request: http://localhost:8000/requestValidation ](https://s3.amazonaws.com/video.udacity-data.com/topher/2019/April/5ca36182_request-ownership/request-ownership.png)
4. Sign the message with your Wallet:
   ![Use the Wallet to sign a message](https://s3.amazonaws.com/video.udacity-data.com/topher/2019/April/5ca36182_request-ownership/request-ownership.png)
5. Submit your Star
   ![Request: http://localhost:8000/submitstar](https://s3.amazonaws.com/video.udacity-data.com/topher/2019/April/5ca365d3_signing-message/signing-message.png)
6. Retrieve Stars owned by me
   ![Request: http://localhost:8000/blocks/<WALLET_ADDRESS>](https://s3.amazonaws.com/video.udacity-data.com/topher/2019/April/5ca362b9_retrieve-stars/retrieve-stars.png)
