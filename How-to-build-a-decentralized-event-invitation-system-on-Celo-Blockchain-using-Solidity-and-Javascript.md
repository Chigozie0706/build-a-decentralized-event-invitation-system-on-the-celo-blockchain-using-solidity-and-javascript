---
title: How to build a decentralized event invitation system on Celo Blockchain, using Solidity and Javascript
description: Learn How to build a decentralized event invitation system on Celo Blockchain, using Solidity and Javascript 
authors:
- name: Chigozie Jacob 
  title: Fullstack Developer, 
  github_url: https://github.com/SamsonAmos
tags: [Solidity, Celo, Javascript]
hide_table_of_contents: true
slug: /tutorial/how-to-build-a-decentralized-event-invitation-system-on-Celo-Blockchain-using-Solidity-and-Javascript
---

# How to build a decentralized event invitation system on Celo blockchain using Solidity and Javascript 

## Introduction:
A blockchain or cryptographic network is a broad term used to describe a database maintained by a distributed set of computers that do not share a trust relationship or common ownership. This arrangement is referred to as decentralized. The content of a blockchain's database, or ledger, is authenticated using cryptographic techniques, preventing its contents from being added to, edited or removed except according to a protocol operated by the network as a whole.

Celo blockchain enables fast, secure, and low-cost financial transactions. It is built on top of the Ethereum Virtual Machine (EVM), which is a standardized environment for running smart contracts (self-executing code that can be used to facilitate, verify, and enforce the negotiation or performance of a contract). 
One of the main features of Celo is its use of proof-of-stake (PoS) consensus, which means that the network is secured by a group of "validators" who stake (or pledge) a certain amount of the platform's native cryptocurrency  in order to participate in the validation of transactions. 

Ethereum applications are built using smart contracts. Smart contracts are programs written in languages like Solidity that produce bytecode for the Ethereum Virtual Machine or EVM, a runtime environment. Programs encoded in smart contracts receive messages and manipulate the blockchain ledger and are termed on-chain.

##Learning Objective
In this tutorial we will be learning how to: 
- how to write smart contracts using Solidity.
- how to deploy the smart contracts on the Celo blockchain.
- how to build a frontend and connect it to the smart contract using Javascript and the ContractKit library.

## Prerequisites:
Before you procced with this tutorial you need to have the following:
- Basic knowledge of using the Command Line Interface (CLI).
- Basic knowledge of HTML and Javascript.
- Basic understanding of blockchain concepts. You can click **[here](https://dacade.org/communities/blockchain/courses/intro-to-blockchain) to learn.
- Basic knowledge on solidity and its concepts. you can click **[here](https://dacade.org/communities/ethereum/courses/sol-101/learning-modules/dcc5e8e2-bc22-49a6-ace7-23ec7fcc81d5) to learn.

## Requirements: 
- A computer with some good internet connection and a chrome web browser.
- **[NodeJS](https://nodejs.org/en/download)** from V12.or higher.
- A code editor or text editor. **[VSCode](https://code.visualstudio.com/download)** is recommended.
- A terminal. **[Git Bash](https://git-scm.com/downloads)** is recommended.
- **[Remix](https://remix.ethereum.org)**
- **[Celo Extension Wallet](https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en)**.

## Let's Get Started

Below is a gif image of what we are about to build on the celo ecosystem.

![image](images/1.png)

## Smart Contract Development

In this section, we are going to develop our smart contract in Solidity on the Celo blockchain using the Remix Ide. The Remix IDE is a web based IDE that allows developers to write, test and deploy smart contracts on the blockchain,  click here to navigate to the IDE. 

Here is what the Remix IDE looks like:
![image](images/2.png)

Once your are in the IDE you will see a folder called contract. open the contract folder and create a file called `CeAffairs.sol`. The `.sol` extension is an indicator that our smart contract will be written in Solidity.

After creating the file, our first line of code inside the file will be: 

```solidity
// SPDX-License-Identifier: MIT
pragma solidity >=0.7.0 <0.9.0;
```

this is neccessary in `Solidity` in order to specifies the license under which the code is being released using the `SPDX-License-Identifier` and to  specify the solidity version that you want the Remix compiler to use using the `pragma` keyword. In this case, the Solidity version should be higher than or seven and lower than nine.


Up next we will be defining our smart contract using the `contract` keyword, followed by the name of the contract which in our case we would be naming it `CeAffairs` and declaring our `variables` below. Just like other programming languages, Solidity also has data types, and you have to specify the data type of each variable you create. Learn more on data types used in Solidity [(here)](https://docs.soliditylang.org/en/latest/types.html). 

```solidity
contract CeAffairs{
// Declaring variables.
    uint internal eventLength = 0; 
```
In the above code, we are going to declare a variable called `eventLength` of  data type `uint` with visibility set to internal because we only want those variables to be accessed by only by our smart contract. ([Learn more about visiblity](https://docs.soliditylang.org/en/latest/contracts.html#visibility-and-getters)).

The variable is going to keep track of events that is hosted on the blockchain/

Up next, we are going to create a "model" called `Event` for our event using the `struct` data type. You can learn about structs  ([here](https://docs.soliditylang.org/en/latest/types.html#structs))

```solidity    
// Ceating a struct to store event details.
  struct Event {
      address  owner;
      string eventName;
      string eventCardImgUrl;
      string eventDetails;
      uint   eventDate;
      string eventTime;
      string eventLocation;
```

In the code above, the model consist of the following attributes: 
1. owner : it stores the address of an event owner.
2. eventName: it stores the name of the event.
3. eventCardImgUrl : it stores the event card image.
4. eventDetails : it stores the details of the event.
5. eventDate : it stores the event date.
6. eventTime : it stores the event time.
7. eventLocation : it stores the event location

after creating the model of our event, we would create a map to store multiple values. A map is like an object in Javascript that has key and value.

To create a map, you use the keyword `mapping` and assign a key type to a value type.

```solidity
    //map for storing events.
    mapping (uint => Event) internal events;

    //map for storing list of attendees
    mapping(uint256 => address[]) internal eventAttendees;

    // map for attendance check
    mapping(uint => mapping(address => bool)) public attendanceCheck;
```

In the first map it stores multiple events listed on the blockchain, your key would be an integer `(uint)` and the value would be the struct `Event` and the variable is `events`.

The second map stores the addressess of users that are going to attend a particular event. The key is the integer `(uint256)`, while the value is an array that stores only addresses of the users.

The third map checks if a user is already an attendee of a particular event in order to avoid spaming an event.

Next we are going to create a function where a user can create or list an event on the blockchain. 

```solidity
   // Function to create  an event.
    function createEvent(string memory _eventName, string memory _eventCardImgUrl,
    string memory _eventDetails, uint  _eventDate, 
    string memory _eventTime, string memory _eventLocation) public {
        events[eventLength] = Event({owner : msg.sender, eventName: _eventName, eventCardImgUrl : _eventCardImgUrl, 
     eventDetails: _eventDetails, eventDate : _eventDate, 
     eventTime : _eventTime, eventLocation : _eventLocation});
     eventLength++;
}
```

In the function above, it takes six parameters which are _eventName, _eventCardImgUrl, _eventDetails, _eventDate, _eventTime, _eventLocation. The parameter has an prefix `_` which is used to differentiate it from it's struct value. The function has its  visibilty type set to public.  You can click on this link to know more about visibilty types.  

A new event is then added to the events mapping by using the initial length of the event which we create above called `eventLength` as a key to store a new Event struct with the specified information. The owner of the event is stored using the `msg.sender` keyword. 

Then we are going to increment the eventLength by one for the next event to be created.


Up next, we are going to create a function that will return the information of a particular event when the _index of that event is being passed. The function will be declared `public view` meaning that it is going to be public and we not modifying anything rather we only to return some values. 

```solidity
    // Function to get a event through its id.
    function getEventById(uint _index) public view returns (
        address,
        string memory,
        string memory,
        string memory,
        uint,
        string memory,
        string memory
        
    ) {
    
        return (
            events[_index].owner,
            events[_index].eventName, 
            events[_index].eventCardImgUrl,
            events[_index].eventDetails,
            events[_index].eventDate,
            events[_index].eventTime,
            events[_index].eventLocation
        );
    }

```

The function specify the variables  will return with the function. 

In this case, it would be a tuple corresponding to the variables declared in the struct. 

The function will rhen return the address of the owner, eventName, eventCardImgUrl, eventDetails, eventDate, eventTime and the eventLocation.

Up next, we are going to create a function called `deleteEventById` that will allow an event owner deletes his or her event.  

```solidity
    //Function in which only an event owner can delete an event. 
function deleteEventById(uint _index) public {
        require(msg.sender == events[_index].owner, "you are not the owner");
        delete events[_index];
    }
```
The function take a parameter `_index` and it's being set to public. The body of the function uses the `require` method. The require method is used to ensure that a particular condition is being met before moving to the next line of code. 

In our requrire method we are going to ensure that the address of the user calling the function is the as the owner of that event. If the condition is false, it will throw an error with the keyword `"you are not the owner"`. If the condition is true,  it will delete the event in the events mapping through it's index.



Up next we are going to create a function called `addEventAttendees` which will enable a user attend an event without spamming it.

```solidity
//Function to attend an event without spamming it.
    function addEventAttendees(uint256 _index) public {
        require(events[_index].eventDate > block.timestamp,"sorry entry date has expired...");
        require(!attendanceCheck[_index][msg.sender], "you are already an attendee");
        attendanceCheck[_index][msg.sender] = true;
        eventAttendees[_index].push(msg.sender);
    
    }
}
``` 
The function take parameters `_index` and it's set to public. 

It uses two require method. The first require method ensures that the entry date of that particular event has not expire by making a comparism with the current timestamp. 

The second require method is to ensure the attendance check of a user is false. 

If the two require condition is being met, it sets the attendanceCheck of that user to be true and store the user address in an array which is stored in a map.

Up next we are going to create a function called `getAttendees` to fetch the list of all attendee of a particular event. 

```solidity
//function to get list of event attendees by event id.
    function getAttendees(uint256 _index) public view returns (address[] memory) {
        return eventAttendees[_index];
    }
``` 
The function takes a paremeter of _index and it's set to public view since we are not modifying anything and it returns array of type address.

The body of the function returns an array of address by using the `_index` as a key.

Up next, we are going to create a public function which will return the length of the `eventLength` created. 

```solidity
//function to get length of event.
    function getEventLength() public view returns (uint) {
        return (eventLength);
    }
```


Here is the full code:


```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.3;
import "@openzeppelin/contracts/utils/Strings.sol";

contract CeAffairs{
// Declaring variables.
    uint internal eventLength = 0;
    uint internal eventCommentsLength = 0; 

    
    // Ceating a struct to store event details.
    struct Event {
        address  owner;
        string eventName;
        string eventCardImgUrl;
        string eventDetails;
        uint   eventDate;
        string eventTime;
        string eventLocation;
        
    }

    //map for storing events.
    mapping (uint => Event) internal events;

    //map for storing list of attendees
    mapping(uint256 => address[]) internal eventAttendees;

    // map for attendance check
    mapping(uint => mapping(address => bool)) public attendanceCheck;


    // Function to create  an event.
    function createEvent(string memory _eventName, string memory _eventCardImgUrl,
    string memory _eventDetails, uint  _eventDate, 
    string memory _eventTime, string memory _eventLocation) public {
        events[eventLength] = Event({owner : msg.sender, eventName: _eventName, eventCardImgUrl : _eventCardImgUrl, 
     eventDetails: _eventDetails, eventDate : _eventDate, 
     eventTime : _eventTime, eventLocation : _eventLocation});
     eventLength++;
}


// Function to get a event through its id.
    function getEventById(uint _index) public view returns (
        address,
        string memory,
        string memory,
        string memory,
        uint,
        string memory,
        string memory
        
    ) {
    
        return (
            events[_index].owner,
            events[_index].eventName, 
            events[_index].eventCardImgUrl,
            events[_index].eventDetails,
            events[_index].eventDate,
            events[_index].eventTime,
            events[_index].eventLocation
        );
    }

//Function only a event owner can delete an event. 
function deleteEventById(uint _index) public {
        require(msg.sender == events[_index].owner, "you are not the owner");
        delete events[_index];
    }

//Function to attend an event without spamming it.
    function addEventAttendees(uint256 _index) public {
        require(events[_index].eventDate > block.timestamp,"sorry entry date has expired...");
        require(!attendanceCheck[_index][msg.sender], "you are already an attendee");
        attendanceCheck[_index][msg.sender] = true;
        eventAttendees[_index].push(msg.sender);
    
    }

//function to get list of event attendees by event id.
    function getAttendees(uint256 _index) public view returns (address[] memory) {
        return eventAttendees[_index];
    }


//function to get length of event.
    function getEventLength() public view returns (uint) {
        return (eventLength);
    }    

}
 
```

## Contract Deployment

To deploy the contract, we would need:
1. [CeloExtensionWallet]((https://chrome.google.com/webstore/detail/celoextensionwallet/kkilomkmpmkbdnfelcpgckmpcaemjcdh?hl=en))
2. [ Celo Faucet](https://celo.org/developers/faucet) 
3. Celo Remix Plugin

Download the Celo Extension Wallet from the Google chrome store using the link above. After doing that, create a wallet, store your key phrase in a very safe place to avoid permanently losing your funds.

After downloading and creating your wallet, you will need to fund it using the Celo Faucet. Copy the address to your wallet, click the link to the faucet above and the paste the address into the text field and confirm.

Next up, on Remix, 
- Download and activate the celo plugin from the plugin manager. 
- Save and Compile your `CeAffairs.sol`, take note of the Abi at the bottom of the compile section.
- Next click on the deploy button and connect your wallet and deploy the contract.
- Take note of the `address` it is being deployed to as we will need it later. 


## Frontend Development with Javascript

Going futher we will be building a web page to interact with our smart contract that is being deployed. You need to make sure you have installed Node.js 10 or higher version.

Next we need to open a command line interface in the folder or directory where you want to build the frontend. 

Next we are going to be cloning a boilerplate for the frontend. Run the code below on the command line interface you just open

```js
git clone https://github.com/dacadeorg/celo-boilerplate-web-dapp
```

This will create a folder called celo-boilerplate-web-dapp. 

The project folder contains three folders contract, public and src. Inside the contract folder, we have three files `marketplace.sol` which holds our contract code, `marketplace.abi.json` which holds the ABI bytecode of our contract and the `erc20.abi.json`which holds the ABI bytecode for the ERC20 interface.

next we move to our root directory on the same command line interface by run this code
```js
cd celo-boilerplate-web-dapp
``` 
The code change the directory in the command line interface to the root directory inorder for us to install the dependences that comes with the boilerplate.

To install all the dependencies we type the code below and hit enter.

```js
npm install
```
Installing of all dependencies might take a while. After the dependencies have been installed, we can start up the local server by running the code:

```js
npm run dev
```
Your project should be running here http://localhost:3000/ and a browser window should pop up showwing "hello world".


After starting the server we need to open the celo-boilerplate-web-dapp folder which is the root folder in an IDE you can use any IDE for it, but preferably you use vscode.

Up next we need to copy our `CeAffairs.sol` from Remix and paste in the `marketplace.sol` file in our boilerplate, copy the Abi also from Remix and paste in our `marketplace.abi.json` file. 

NB: the address in which the smart contract is being deployed to will be used later.

## The index.html file

Up next in our boilerplate, let's open the public folder and the `index.html` file. This is where we are going to be building our user interface (UI) so that the user can see a way of interacting with our smart contract using our interface. 

Firstly we need to write this line of code below: 

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
```
The code above declares the document type, add an HTML tag, create a head element, and add meta tags.

The first meta tag defines the character-set (encoding) we will be using.

The second meta tag defines the responsiveness of the web page.

Next, we will be importing some external stylesheets. we will use bootstrap,  front-end library that allows you to create elegant responsive websites very fast. You can quickly choose from bootstrap components like buttons or cards and customize them to your needs.

```html
  <!-- CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
      crossorigin="anonymous"
    />
```

The next line of code is to import some Google font.
```html
 <link rel="preconnect" href="https://fonts.gstatic.com" />
  <link
    href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap"
    rel="stylesheet"
  />
```
After importing the stylesheet we will also be needing the bootstrap icons which can be imported using external import with the code below:
```html
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.0/font/bootstrap-icons.css"
    />
```

Up next, we would be using the `style` tag to import our main font. The style tag also contains some css style which can be used globally. 

```html
<style>
      :root {
        --bs-font-sans-serif: "DM Sans", sans-serif;
      }

      @media (min-width: 576px) {
        .card {
          border: 3px solid rgb(255,218,185);
          border-radius: 10px;
        }

        .card-img-top {
          width: 100%;
          height: 15vw;
          object-fit: cover;
        }
      }
    </style>

    <title>CeAffairs</title>
  </head>
```
We are also setting the title of the page with the title tag. In our case, the title if our page will be `CeAffairs`, then we close the tag.

Up next we would be defining our body tag where we would create our forms,  modals, hero and navigation bar, notification bar. lets get started.


The starting of the body tag begins with our navigation bar which will be used to show the name of our app and the amount of cUSD we currently have. The amount of cUSD we be dynamic as the main data will be gotten from our javascript file.

```html
 <!-- Navbar starts here -->
  <nav class="navbar bg-dark navbar-dark">
        <div class="container">
          <span class="navbar-brand m-0 h4 fw-bold">ceAffairs</span>
          <span class="nav-link border rounded-pill bg-light">
            <span id="balance">0</span>
            cUSD
          </span>
        </div>
      </nav>
 <!-- Navbar ends here -->
``` 

The navbar has two spans one which shows the name of our app and the secound which shows the amount of cUSD we have. The second span has an id of "balance" which will be later needed in our javascript file to render the actual celo balance.

Up next we will be creating our hero where which is a background at the top that tells the user what the app does.

```html
 <!-- Hero starts here -->
    <div style="text-align: center; background-color: light-grey;" class="shadow p-5"> 
      <p>Welcome to ceAffairs a blockchain event hosting site where you host an event and also join be a guest to other people event on the celo blockchain</p>

      <button class="btn btn-success rounded-pill" data-bs-toggle="modal"
          data-bs-target="#addModal">Post an Event</button>
    </div>
 <!--Hero ends here -->
``` 

The hero is contained in a div with various bootstrap style which it has a p tag which explains what the app does and a button that pop up a modal which the user will use fill a form to create an event on the smart contract.



Up next we are going to create a div that shows notifications to the users and a `main` tag that will show the list of events that is already created by the user.

```html
 <!-- Start of container holding list of events -->
    <div class="container mt-3">
      <div class="alert alert-warning sticky-top mt-2" role="alert">
        <span id="notification">⌛ Loading...</span>
      </div>
  
      <h5 class="my-5 text-center" id="eventPage">Event Lists</h5>
      
        <main id="marketplace" class="row">
          <p class="text-center mt-5">loading please wait...</p>
        </main>  
      </div>
<!-- End of container holding list of events -->
```
The div for the notification contains bootstrap class and it has an id set as `notification`. This id will be used to render dynamic text, same also goes to the main tag which has an id of `marketplace` which will be used in our main.js file to render dynamic datas.

Next, we will be creating a div element that will pop up a modal which will be used to view the details of an event created and also a user can use it to attend that event.

```html
<!-- start of modal that view an event -->
<div
      class="modal fade"
      id="addModal1"
      tabindex="-1"
      aria-labelledby="newProductModalLabel"
      aria-hidden="true"
      data-bs-backdrop="static" data-bs-keyboard="false"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel">Event Details </h5>

            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body" id="modalHeader">
            </div>
           </div>
          </div>
        </div>
<!-- end of modal that view an event -->
```
The div contains class of modal and other bootstrap classes and it has two ids which are addModal1 which is used to pop the modal in our main.js file and modalHeader which renders the details of an event.


Next, we are going to create a modal that will pop up a form which the users will use to create an event.

```html
<!-- start of modal to add a new event -->
    <div
      class="modal fade"
      id="addModal"
      tabindex="-1"
      aria-labelledby="newProductModalLabel1"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel1">New </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-row">
                <div class="col">
                  <input
                    type="text"
                    id="eventName"
                    class="form-control mb-2"
                    placeholder="Enter event name"
                  />
                </div>

                <div class="col">
                  <input
                    type="text"
                    id="eventCardImgUrl"
                    class="form-control mb-2"
                    placeholder="Enter event card image url"
                  />
                </div>

                <div class="col">
                  <input
                    type="textarea"
                    id="eventDetails"
                    class="form-control mb-2"
                    placeholder="Enter event details"
                  />
                </div>

                <div class="col">
                  <input
                    type="date"
                    id="eventDate"
                    class="form-control mb-2"
                    placeholder="Enter event date"
                  />
                </div>


                <div class="col">
                  <input
                    type="time"
                    id="eventTime"
                    class="form-control mb-2"
                    placeholder="Enter event time"
                  />
                </div>

                <div class="col">
                  <input
                    type="text"
                    id="eventLocation"
                    class="form-control mb-2"
                    placeholder="Enter event location"
                  />
                </div>
                
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-light border"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-dark"
              data-bs-dismiss="modal"
              id="postEventBtn"
            >
              Add Event
            </button>
          </div>
        </div>
      </div>
    </div>
<!-- end of modal to add a new event -->
```


Finally, add the bootstrap JS library and a library called blockies, that you are going to use to visualise blockchain addresses and close the body and html tag.
```html
<script
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0"
    crossorigin="anonymous"
    ></script>
    <script src="https://unpkg.com/ethereum-blockies@0.1.1/blockies.min.js"></script>
  </body>
</html>
```

Here is the full code for the index.html:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />

    <!-- CSS -->
    <link
      href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/css/bootstrap.min.css"
      rel="stylesheet"
      integrity="sha384-BmbxuPwQa2lc/FVzBcNJ7UAyJxM6wuqIj61tLrc4wSX0szH/Ev+nYRRuWlolflfl"
      crossorigin="anonymous"
    />
    <link rel="preconnect" href="https://fonts.gstatic.com" />
    <link
      href="https://fonts.googleapis.com/css2?family=DM+Sans:wght@400;500;700&display=swap"
      rel="stylesheet"
    />
    <link
      rel="stylesheet"
      href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.4.0/font/bootstrap-icons.css"
    />

    <style>
      :root {
        --bs-font-sans-serif: "DM Sans", sans-serif;
      }

      @media (min-width: 576px) {
        .card {
          border: 3px solid rgb(255,218,185);
          border-radius: 10px;
        }

        .card-img-top {
          width: 100%;
          height: 15vw;
          object-fit: cover;
        }
      }
    </style>

    <title>CeAffairs</title>
  </head>

  <body>

   <!-- Navbar starts here -->
  <nav class="navbar bg-dark navbar-dark">
        <div class="container">
          <span class="navbar-brand m-0 h4 fw-bold">ceAffairs</span>
          <span class="nav-link border rounded-pill bg-light">
            <span id="balance">0</span>
            cUSD
          </span>
        </div>
      </nav>
 <!-- Navbar ends here -->



 <!-- Hero starts here -->
    <div style="text-align: center; background-color: light-grey;" class="shadow p-5"> 
      <p>Welcome to ceAffairs a blockchain event hosting site where you host an event and also join be a guest to other people event on the celo blockchain</p>

      <button class="btn btn-success rounded-pill" data-bs-toggle="modal"
          data-bs-target="#addModal">Post an Event</button>
    </div>
 <!--Hero ends here -->


 <!-- Start of container holding list of events -->
    <div class="container mt-3">
      <div class="alert alert-warning sticky-top mt-2" role="alert">
        <span id="notification">⌛ Loading...</span>
      </div>
  
      
      
      <h5 class="my-5 text-center" id="eventPage">Event Lists</h5>
      
        <main id="marketplace" class="row">
          <p class="text-center mt-5">loading please wait...</p>
        </main>
     
    
      </div>
<!-- End of container holding list of events -->
    

<!-- start of modal that view an event -->
<div
      class="modal fade"
      id="addModal1"
      tabindex="-1"
      aria-labelledby="newProductModalLabel"
      aria-hidden="true"
      data-bs-backdrop="static" data-bs-keyboard="false"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel">Event Details </h5>

            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body" id="modalHeader">
          </div>
          </div>
          </div>
          </div>

<!-- end of modal that view an event -->


<!-- start of modal to add a new event -->
    <div
      class="modal fade"
      id="addModal"
      tabindex="-1"
      aria-labelledby="newProductModalLabel1"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="newProductModalLabel1">New </h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <form>
              <div class="form-row">
                <div class="col">
                  <input
                    type="text"
                    id="eventName"
                    class="form-control mb-2"
                    placeholder="Enter event name"
                  />
                </div>

                <div class="col">
                  <input
                    type="text"
                    id="eventCardImgUrl"
                    class="form-control mb-2"
                    placeholder="Enter event card image url"
                  />
                </div>

                <div class="col">
                  <input
                    type="textarea"
                    id="eventDetails"
                    class="form-control mb-2"
                    placeholder="Enter event details"
                  />
                </div>

                <div class="col">
                  <input
                    type="date"
                    id="eventDate"
                    class="form-control mb-2"
                    placeholder="Enter event date"
                  />
                </div>


                <div class="col">
                  <input
                    type="time"
                    id="eventTime"
                    class="form-control mb-2"
                    placeholder="Enter event time"
                  />
                </div>

                <div class="col">
                  <input
                    type="text"
                    id="eventLocation"
                    class="form-control mb-2"
                    placeholder="Enter event location"
                  />
                </div>
                
              </div>
            </form>
          </div>
          <div class="modal-footer">
            <button
              type="button"
              class="btn btn-light border"
              data-bs-dismiss="modal"
            >
              Close
            </button>
            <button
              type="button"
              class="btn btn-dark"
              data-bs-dismiss="modal"
              id="postEventBtn"
            >
              Add Event
            </button>
          </div>
        </div>
      </div>
    </div>
<!-- end of modal to add a new event -->

    <script
    src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.0-beta2/dist/js/bootstrap.bundle.min.js"
    integrity="sha384-b5kHyXgcpbZJO/tY9Ul7kGkf1S0CWuKcCD38l8YkeH8z8QjE0GmW1gYU5S9FOnJ0"
    crossorigin="anonymous"
    ></script>
    <script src="https://unpkg.com/ethereum-blockies@0.1.1/blockies.min.js"></script>
  </body>
</html>
```

## The main.js file
The main.js file is file create a between our frontend and smart contract. In other words, it interact with our smart contract when a button is being clicked on when the page loads. in the beginning of the main.js file, we need to import the  necessary libraries and files which is needed.

```js
import Web3 from "web3"
import { newKitFromWeb3 } from "@celo/contractkit"
import BigNumber from "bignumber.js"
import marketplaceAbi from "../contract/marketplace.abi.json"
import erc20Abi from "../contract/erc20.abi.json"

const ERC20_DECIMALS = 18
const MPContractAddress = "0xfE0C8243D8F04411752154B9421A2bc8a9b63962" //Event Contract Address
const cUSDContractAddress = "0x874069Fa1Eb16D44d622F2e0Ca25eeA172369bC1" //Erc20 contract address

```
In the above code we imported Web from web3. web3.js is a  popular collection of libraries also used for ethereum, that allows you to get access to a web3 object and interact with node's JSON RPC API .

Then we import newKitFromWeb3 from the "@celo/contractkit". The contractkit library enables us  to interact with the celo blockchain.

Celo's operations often deal with numbers that are too large for Javascript to handle. To handle these numbers, we will use bignumber.js.

The erc2-Abi enables us to interact with the ERC-20 interface. The ERC-20 interface The ERC20 interface is a standard API for tokens within smart contracts. It enables make payment with the Celo stablecoin which is cUSD.

Next we create a variable called `ERC20_DECIMALS` and set its value to 18. By default, the ERC20 interface uses 18 decimal places.

On Remix, after the deployment of your contract, you will find the address to that contract which you need to interact with the functionality in your smart contract.

we create a variable called `contractAddress` and assign it the contract address gotten from remix.

Then we create a variable called `cUSDContractAddress` for the cUSD contract address.


Going futher we create three more global variable below:

```js
let kit //contractkit
let contract // contract variable
let eventLists = [] // array of event lists
```
The kit stores the address of a user that is conneted with his/her celo wallet.
The contract stores an instance of the `CeAffairs` contract once a user connects their Celo wallet, so you can interact with it.
The eventLists stores events that has already  being created.

In order to store information in the kit and contract variables we need to create an asynchronous function called `connectCeloWallet` which allow  users to connect to the Celo Blockchain and read the balance of their account. The function will perform several checks and actions to ensure that the user has the necessary tools and permissions to interact with the Celo Blockchain.

```js
//Connects the wallet gets the account and initializes the contract
const connectCeloWallet = async function () {
  //checks if wallet is avaliable and gets the account.
  if (window.celo) {
    notification("⚠️ Please approve this DApp to use it.")
    try {
      await window.celo.enable()
      notificationOff()

      const web3 = new Web3(window.celo)
      kit = newKitFromWeb3(web3)

      const accounts = await kit.web3.eth.getAccounts()
      kit.defaultAccount = accounts[0]

      contract = new kit.web3.eth.Contract(marketplaceAbi, MPContractAddress)
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
    notificationOff()
  }
  // if wallet is not avaliable excute enable the notification 
  else {
    notification("⚠️ Please install the CeloExtensionWallet.")
  }
}
```

The first step is to check if the user has the CeloExtensionWallet installed by checking if the "window.celo" object exists. If it does not exist, the function will use the console to inform the user that they need to install the wallet.

If the "window.celo" object does exist, a notification will be sent to the user in the console to approve this DApp and try the window.celo.enable() function. This will open a pop-up dialogue in the UI that asks for the user's permission to connect the DApp to the CeloExtensionWallet.

If an error is caught during this process, the user would be informed that they must approve the dialogue to use the DApp.

After the user approves the DApp, create a web3 object using the window.celo object as the provider. This web3 object can then be used to create a new kit instance, which will be saved to the kit state. This kit instance will have the functionality to interact with the Celo Blockchain.

You would then access the user's account by utilizing the web3 object and kit instance that have been created. 

After creating the new kit instance, use the method kit.web3.eth.getAccounts() to get an array of the connected user's addresses. Use the first address from this array and set it as the default user address by using kit.defaultAccount. This will allow the address to be used globally in the DApp.


Up next we create an asynchronous function called `getBalance` which we use in getting cUSD of the user and displaying it on the navbar we create in our html file.
```js
  // gets the balance of the connected account
const getBalance = async function () {
  const totalBalance = await kit.getTotalBalance(kit.defaultAccount)
  // gets the balance in cUSD
  const cUSDBalance = totalBalance.cUSD.shiftedBy(-ERC20_DECIMALS).toFixed(2)
  document.querySelector("#balance").textContent = cUSDBalance
}
  };
```
We start by calling the `kit.getTotalBalance(address)` method, passing in the user's address. This method returns the user's balance in the form of an object that contains the amounts of CELO and cUSD tokens. The returned balance is then stored in the `balance` variable.

The next step is to extract the CELO and cUSD balance from the "balance" object by using the `.CELO` and `.cUSD` properties respectively. Then it's shifted the value by -ERC20_DECIMALS which is a way to represent the balance in terms of smaller units in our case 18 decimal places, and then it's converting the value to fixed 2 decimal points. These values are stored in the `celoBalance` and `USDBalance` variables.


Next we are going to create a function called `getEventLists`. This function will help us interact with our smart contract to get all the events users have created, store them in the global array `eventLists` and render a template that will display the events to users on the web page.

Let's get started: 

```js
// gets the lists of all events created
const getEventLists = async function() {
  // calls the getEventLength function on SmartContract to get the total number of event created.
  const eventLength = await contract.methods.getEventLength().call()
  
  //initializing an event array for call function
  const _eventLists = []

  //  loops through all the products
  for (let i = 0; i < eventLength; i++) {
    let event = new Promise(async (resolve, reject) => {

  // calls the getEventById function on the SmartContract
      let p = await contract.methods.getEventById(i).call()
      resolve({
        index: i,
        owner: p[0],
        eventName: p[1],
        eventCardImgUrl: p[2],
        eventDetails: p[3],
        eventDate: p[4],
        eventTime: p[5],
        eventLocation : p[6]
      })
    })

    // push the items on the _eventList array
    _eventLists.push(event)
  }

  // resolves all promise
  eventLists = await Promise.all(_eventLists)
  renderEvents()
}
```
First in the function, we need to know the length of events that has been created. We do that by using the `contract.methods.getEventLength().call()` to call the `getEventLength` function of our smart contract and store the length in a variable called `eventLength`

Next we are going to create an empty array called `_eventLists` that will be used to store the listed seed objects. 

Next we are going to create a for loop using our `eventLength` to loop through our smart contract function `getEventById`. For each loop, it creates a promise and stores it in the local variable event, then we push each `event` in our local array `_eventList`.  After the for loop has finished executing we are going to resolve all the promise stored in the local array by using the function `await Promise.all(_eventLists)` and then we store the resolve promise in the global `eventLists` array,  after which the `renderEvents` function is being called. 

Up next, we are going to look at how to create the `renderEvents` function. This function is going to render an html template that will show all the events in a user interface (UI) format. 

```js
// function to render a html template after the list of event is being fetched.
function renderEvents() {
  document.getElementById("marketplace").innerHTML = ""
  if (eventLists) {
  eventLists.forEach((event) => {
    if (event.owner != "0x0000000000000000000000000000000000000000") {
    const newDiv = document.createElement("div")
    newDiv.className = "col-md-3"
    newDiv.innerHTML = eventTemplate(event)
    document.getElementById("marketplace").appendChild(newDiv)
  }
  
  })
}
else {
  console.log("array is empty")
}}
```
 In the code above, it first of all gets the document id it wants to display the events in our case `marketplace` and clears. Then it uses the `eventLists` to check if the owner of an event is valid by using the `event.owner`. If true it will the create a new div, add some bootstrap class to it, add the `eventTemplate` which will be created later and then append it to the id of  `marketplace`.

Up next we are going to create our event template which will receive a parameter `event`, below is the code.
```js
// function that create a html template
function eventTemplate(event) {
  return `
 <div class="card mb-4 shadow">
      <img class="card-img-top" src="${event.eventCardImgUrl}" alt="...">
      <div class="position-absolute  top-0 end-0 bg-danger mt-4 px-2 py-1 rounded" style="cursor : pointer;">
      <i class="bi bi-trash-fill deleteBtn" style="color : white;" id="${event.index}"></i>
      </div> 
  <div class="card-body text-left p-3 position-relative"
   style="background-color : rgb(255,218,185)">
        <div class="translate-middle-y position-absolute top-0"  id="${event.index}">
        ${identiconTemplate(event.owner)}
        </div>
        <h6 class="card-title  fw-bold mt-2">${event.eventName}</h6>
        <p class="card-text mb-2" style="
  width: 250px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis">
          ${event.eventDetails}             
        </p>
<div style="display: flex;
  justify-content: space-between;">
      

           <div> <a class="btn btn-sm btn-success rounded-pill attendee"
           id="${event.index}">Join</a></div>
           

           <div> <a class="btn btn-sm btn-dark rounded-pill view"
           id="${event.index}">View</a></div>
          </div>

    
      </div>
    </div>
    `
}
```

It consist of a div with class of `card` and some bootstrap class the data will be rendering will be coming from the event which is an object and we can access the value by using the `event.` followed by the name of the value.

The template also have a function called `identiconTemplate` which  recieves a parameter `event.owner`. The function converts the addresses of owners into icons. We are going to look at how to create this function in our next section.

The template also consist of two button. One is to join the event while the later is to view more details about the event. Later on as we procced we are going to implement their functions. 

Up next we would create the identiconTemplate function. It recieves a parameter of _address. 

```js
// function  that creates an icon using the contract address of the owner
function identiconTemplate(_address) {
  const icon = blockies
    .create({
      seed: _address,
      size: 5,
      scale: 10,
    })
    .toDataURL()

  return `
  <div class="rounded-circle overflow-hidden d-inline-block border border-white border-2 shadow-sm m-0">
    <a href="https://alfajores-blockscout.celo-testnet.org/address/${_address}/transactions"
        target="_blank">
        <img src="${icon}" width="40" alt="${_address}">
    </a>
  </div>
  `
}
```
The function returns a div that has an image tag with its src set to the variable icon declared at the top inside the fuction.

Next we create two function called notification() and notificationOff(). The notification() function  displays the alert element with the text in the parameter and a notificationOff stops showing the alert element. They will be used when recieving and resolving promise using the try and catch block in our code to display error or success messages.

```js
// function to create a notification bar
function notification(_text) {
  document.querySelector(".alert").style.display = "block"
  document.querySelector("#notification").textContent = _text
}


// function to turn off notification bar based on some conditions
function notificationOff() {
  document.querySelector(".alert").style.display = "none"
}
```

Up next we are going to create an event listener that checks when the page loads and it will call some certian functions we created.

```js
// initialization of functions when the window is loaded.
window.addEventListener("load", async () => {
  notification("⌛ Loading...")
  await connectCeloWallet()
  await getBalance()
  await getListedSeeds()
  notificationOff()
  });
```
The event listener calls the notification, connectCeloWallet, getBalance, getListedSeeds and the notificationOff function.


Up next, we are going to create another event listener that will  interact with our smart contract function `createEvent` to submit the form we filled while creating a new event to the blockchain.

```js
document
  .querySelector("#postEventBtn")
  .addEventListener("click", async (e) => {
    
    // gets the event date value from the form
    let eventDate = document.getElementById("eventDate").value;
    
    // stores the eventDate in the date variable.
    const date = new Date(eventDate);

    // converts date to timestamp in seconds.
    const timestamp =  Math.floor(date.getTime() / 1000);

    const params = [
      document.getElementById("eventName").value,
      document.getElementById("eventCardImgUrl").value,
      document.getElementById("eventDetails").value,
      parseInt(timestamp),
      document.getElementById("eventTime").value,
      document.getElementById("eventLocation").value,
    ]
    notification(`⌛ Posting your event to the blockchain please wait...`)
    try {
      const result = await contract.methods
        .createEvent(...params)
        .send({ from: kit.defaultAccount })
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
    notification(`🎉 Congrats event successfully added`)
    notificationOff()
    getEventLists()
  })

```
The code checks if the button clicked has an id of `PostEventBtn` to sure that it is the submit button we are clicking. Then it gets the event date in our form converts it to timestamp, stores all the values the user might have filled in form in an array called `params`, displays a notification to let the user know that their request is processing. Then it uses the try block to call the `contract.methods.createEvent(...params)` method and push the information to the Blockchian. The catch block handles any error that might occur during the process.

After that if all is successfull it will  notify the user, turn it off and then call the `eventLists` function.

Next we are going to create a query selector that will target the `marketplace` id and listen for on click events.

```js
// implements various functionalities
document.querySelector("#marketplace").addEventListener("click", async (e) => {
  //checks if there is a class name called deleteBtn
  if (e.target.className.includes("deleteBtn")) {
    const index = e.target.id

    notification("⌛ Please wait, your action is being processed...")
    
    // calls the delete fucntion on the smart contract
    try {
      const result = await contract.methods
        .deleteEventById(index)
        .send({ from: kit.defaultAccount })
      notification(`You have deleted an event successfully`)
      getEventLists()
      getBalance()
    } catch (error) {
      notification(`⚠️ you are not the owner of this event`)
    }
    notificationOff()
  }
```
In the query selector we have an if statement which checks if a button clicked has a class name of `deleteBtn`. If true, it will get the id of that button and assign it to a local variable called index. Next is to display a notification to the user for them to know that their request is processing.

Still on the if statment,  we are going to use a try block to call our smart contract function `deleteEventById` in the function, we are going to pass a parameter `index` which is the local variable that stores the id of the button. 

If the action is successfull, it will notify the user and the call the `getEventLists` and `getBalance` function. If it is not successful,  it will notify the user as well using the catch block for it. 

Finally we are going to turn off the notification.

After the if statment we are going to implement two `else if` statement. The first else if will enable the user get more details on a particular event while the second `else if` statement will  enable the user attend an event.

Let's get started with first `else if`.

```js
 else if(e.target.className.includes("view")){
      const _id = e.target.id;
      let eventData;
      let attendees = [];

      // function to get list of attendess on the smart contract.
      try {
          eventData = await contract.methods.getEventById(_id).call();
          attendees = await contract.methods.getAttendees(_id).call();
          let myModal = new bootstrap.Modal(document.getElementById('addModal1'), {backdrop: 'static', keyboard: false});
          myModal.show(); 
          

// stores the timestamp of the event date.
var eventTimeStamp= parseInt(eventData[4])

// converts timestamp to milliseconds.
var convertToMilliseconds = eventTimeStamp * 1000;

// create an object for it.
var dateFormat= new Date(convertToMilliseconds);
  

// displays events details on the modal
document.getElementById("modalHeader").innerHTML = `
<div class="card mb-4">
      <img style="width : 100%; height : 20vw;" src="${eventData[2]}" alt="...">
      
  <div class="card-body text-left p-4 position-relative">
        <div class="translate-middle-y position-absolute top-0">
        ${identiconTemplate(eventData[0])}
        </div>
        <h5 class="card-title  fw-bold mt-2">${eventData[1]}</h5>
        <p class="card-text mb-3">
          ${eventData[3]}             
        </p>


        <div class="d-flex p-2" style="border : 1px solid grey; border-radius : 2px;" > 
        <p class="card-text mt-1 ">
          <i class="bi bi-calendar-event-fill"></i>
           <span>${dateFormat.getDate()+
           "/"+(dateFormat.getMonth()+1)+
           "/"+dateFormat.getFullYear()}</span>
        </p>

        <p class="card-text mt-1 mx-3">
          <i class="bi bi-clock-fill"></i>
           <span>${eventData[5]}</span>
        </p>
        </div>

        <p class="card-text mt-2 p-2" style="border : 1px solid grey; border-radius : 2px;">
          <i class="bi bi-geo-alt-fill"></i>
          <span>${eventData[6]}</span>
        </p>
        <hr />
      <p class="card-text">Attendees:</p>
      <div id="att"></div>
      </div>
    </div>

  `   
  if (attendees.length) {
    attendees.forEach((item) => {
            document.getElementById(`att`).innerHTML += `${identiconTemplate(item)}`;
          })
  } else{
    document.getElementById(`att`).innerHTML += `<p class="text-center">no attendee yet...</p>`;
  };
               
    }
    catch (error) {
      notification(`⚠️ ${error}.`)
    }
    notificationOff()
  }
```

In the code, we are going to check if the button clicked has a class name of `view`. if it is true, we will create three variables. The first is to store the id of the button, the second is store the promise data we will be receiving later and the third is to store the list of all users that will attend that event.

Next we are going to use a try block to call two functions `getEventById` and `getAttendees` from our smart contract and store them, then we create a variable that will get the id of our modal in the index.html file set it to show.

The `getEventById` result comes the way they are being arranged in the smart contract. Before displaying the result to the user, we need to convert the timestamp gotten to a readable date format.

Next, we are going to display the data of the result gotten in a modal. The modal contains div with bootstrap classes,  and each result is mapped to a particular html element.

Futher more we will check if the attendee array is not empty. If true it will append the addresses the attendees as icons in the modal. else it will display a text saying `"no attendee yet..."`.

After the try block we will use our catch block to handle any errors and notify the users.


Up next is the second `else if` statement:

```js
else if(e.target.className.includes("attendee")){
    const _id = e.target.id;

    notification(`⌛ Processing your request please wait ...`)
    
    try {
      const result = await contract.methods
        .addEventAttendees(_id)
        .send({ from: kit.defaultAccount })
        notification(`🎉 You are now one of the attendee .`)
    } catch (error) {
      notification(`⚠️ ${error}.`)
    }
    notificationOff()
    
    getEventLists()
  }

})
```

The first line checks if the button clicked by the users have a class name of `attendee`, if true, it notify the users that their transaction is processing.

Next we use the try block to interact with our smart contract method `addEventAttendees` which adds the address of a user to an array. If the transaction is successful it will also notify the user. Errors are catch by the catch block and displayed to the user by notification. 

After everything is being processed,  the notification is turned off and the `getEventLists` function is being called. 


Now try to compile your react dapp to see if it is working fine. If it is, you can deploy your dapp on github pages  or netlify.
You can follow or use this project as a reference to edit yours and get the required files, images e.t.c. <https://github.com/dahnny/cardealer-tutorial>

## Conclusion

Congratulations 🎉, you were able to build your fullstack dapp using solidity and react on the celo smart contract. Great JOB!!

## Next steps
You can challenge yourself by adding more functions and implementing them using react on the frontend. You can also look at various celo smart contracts and see if you can build a dapp using react

## About the Author
Chigozie Jacob is a fullstack developer who loves a building global solutions and writting technical content. You can reach me out on Twitter [here](https://twitter.com/SamsonAmoz)

let's gooo!