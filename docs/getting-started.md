---
layout: default
title: Getting Started
nav_order: 2
---

## Haven Protocol Web App
# Getting Started

Lets get setup with the React JS app first. There are a [few prerequisites]({{ site.baseurl }}{% link docs/prerequisites.md %}) you'll need.
I'll keep my repositories locally in a Projects folder.

```bash
> mkdir ~/Projects
> cd ~/Projects
> git clone https://github.com/haven-protocol-org/haven-web-app.git
```
This will grab all the code from github, its around 400MB so will take a few minutes. 
```bash
> cd haven-web-app
```
This is a react app scaffolded with "react-scripts" [more info at the Create React site](https://create-react-app.dev/docs/getting-started/) 

There are 3 folders

1. **client** This is the JS for the React App 
2. **haven-desktop-app** JS for the Electron App
3. **sh** helper scripts for building the Electron App

Lets walk through the client folder
```bash
> cd client
```

#### package.json
Its worth checking through the [package.json](https://github.com/haven-protocol-org/haven-web-app/blob/master/client/package.json)

Note the **haven-wallet-core** dependancy, this is how the compiled web assembly code is imported to the project. If you were to be in a development environment then you will need to work around this, easiest way would be to remove the dependancy and copy your own version in.

The project also makes heavy use of [Styled Components](https://styled-components.com/)

#### NPM Scripts

Currently scripts are

* **analyze** Check your build size
* **start:web** Starts a webserver and serves the app
* **build:web** Builds the web app
* **start:desktop** Start and serve the desktop app
* **build:desktop**  Build the desktop app JS
* **build:web:ci** For github builds ( continous integration )
* **build:desktop:ci** For github builds ( continous integration )
* **copy-build** Copy the built JS to the desktop app folder need to run this before building the Electron App
* **copy-haven-core** Copies the haven-wallet-core into the public folder
* **test** Left over from the scaffolding
* **eject** Left over from the scaffolding


First thing is to install the required dependancies

```bash
> npm install
```
Once thats finished you'll have the various node libraries in the node_modules folder

You should now be able to build and start the app with 
```bash
> npm run build:web
```
You'll be presented with a network selection
```
1) mainnet
2) testnet
3) stagenet
please select net type:
```
This is the choice for which Haven Network node you will be connecting to, best to choose testnet during development, we will startup a local testnet node in the next section, so for now enter **2** and wait for the build to finish, this will save files into the **build** sub directory then

```bash
> npm run start:web
```
This should start up a development server and sere the files into your web browser at http://localhost:3000/

This will be able to open wallet files and generate new wallets but you won't be able to sync without a local node runnning.

---
[Start a local development node]({{ site.baseurl }}{% link docs/local-development-node.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---
