---
layout: default
title: FAQ
nav_order: 98
---
## Haven Protocol Web App
# Development FAQ

### Where is the wallet data cached?

The wallet functionality is handled by the web assembly which communicated with the node. Locally it stores the data in memory and will send it into an IndexedDb storage on the client, see storeWalletDataInIndexedDB in this file
```
client/src/platforms/web/actions/storage.ts
```

### How do I change the domain and port of the haven daemon?

In /client/src/constants/env.ts you'll need to override them for your local or development setup.
+  apiUrl = "https://your.weburl.com";
+  /*
   apiUrl = isDevMode()
     ? process.env.REACT_APP_API_URL_DEVELOP
     : process.env.REACT_APP_API_URL;
+  */

### Using with NodeJS (wasm)
Ensure you set the "**proxyToWorker: true**" on the wallet config object, otherwise you will get erros when trying to sync a wallet.


### GUIs hang on opening 
YOU get the message "Haven Vault is waiting on webpack. Please wait a moment..."
You're building a development version and it needs webpac running in the background
You need to have "npm run start:desktop" runnng from the client folder.

### Changing the desktop build environment during developemnt builds
This is selected when you do "npm run start:desktop" - not when you build the electron app


### Using an updated development haven-wallet-core from NPM
If we need to use a dev version from the npm repository we can tag it with "next"
then install it 
at the command line:
```
cd client (and also cd haven-desktop-app)
npm install haven-wallet-core@next
```
---
[FAQ]({{ site.baseurl }}{% link docs/dev-faq.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---
