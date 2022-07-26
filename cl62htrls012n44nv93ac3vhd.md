## Connect to Coinbase Extension with React JS

Coinbase extension, like Metamask, is a crypto wallet and gateway used to connect and interact with the blockchain. It allows users to interact with any decentralized app or DAPP and perform transactions via a browser. <br />
## How Wallet Extensions work
Wallet extensions mainly do two things, <br />

1. Signing transactions with your private key
1. Connection to a blockchain node

<br />
Crypto wallet extensions like Metamask, Coinbase, etc. hold the private key of the user's wallet. Whenever the user performs a transaction, these extensions use this private key to sign the transaction which makes sure that the said transition belongs to this particular account.<br />
To interact with the blockchain, you need to connect to a node. You can either set up your node which will have a copy of the entire blockchain or use one of the 3rd party services like `Alchemy` or `Infura`. These nodes are called `Providers`. Wallet Extensions provide easy services to connect to these wallets. <br/><br/>
These extensions inject an ethereum object into the DOM which is accessible via the `window.ethereum`. This object has certain properties which help our React app to connect, view balance, perform transactions, etc via these extensions.<br />
**To connect to Coinbase browser extension**  we will take advantage of the same `window.ethereum` object. 
## Coinbase Connection


I will go through the process step by step to give you a clear idea of how you can connect to the coinbase extension. I am using React Js but you can use any javascript UI library or framework with this method.<br />
#### Check if the Coinabse Extension is Installed
As I said before, the coinbase extension injects an Ethereum object into the DOM. If you `log` the `window.ethereum` you will see a property called `isCoinbaseWallet` and its value is `true` in case the extension is installed on the browser. We can check whether `window.ethereum.isCoinbaseWallet`  object exists because if it does then the extension is installed otherwise we need to ask the user to install the extension. 
```javascript
const connectToCoinbase = async () => {
if(!window.ethereum.isCoinbaseWallet){
// ask the user to install the extension
return;
}

//connect to coinbase
}

```
To connect to the coinbase extension provider, all you need is to call the `window.ethereum.request` method. This will open this extension popup and ask the user to approve the connection. On approval, your DAPP will be connected to the coinbase extension. <br />
Here is the complete function
```javascript
const connectToCoinbase = async () => {
if(typeof window.ethereum === "undefined"){
// ask the user to install the extension
return;
}

//connect to coinbase
 let provider = window.ethereum;
await provider.request({
        method: "eth_requestAccounts",
      });
}
```
#### Edge Case
If your browser has more than one extension i.e Metamask is installed then calling the above method will open the popups for both of them resulting in a bad user experience. To avoid this you can use the following snippet of code which will only open the coinbase extension.
```javascript

const connectToCoinBase = async () => {
if(typeof window.ethereum === "undefined"){
// ask the user to install the extension
return;
}
      let provider = window.ethereum;
      // edge case if MM and CBW are both installed
      if (window.ethereum.providers?.length) {
        window.ethereum.providers.forEach(async (p) => {
          if (p.isCoinbaseWallet) provider = p;
        });
      }
      await provider.request({
        method: "eth_requestAccounts",
      });
    };
```
#### Keep in Mind



1. Coinbase extension only connects with localhost or website with HTTPS.

1. If the coinbase popup is not opening in dev mode either you are already connected or you need to restart the server with `npm run start --reset-cache`.



Hope it helped.





