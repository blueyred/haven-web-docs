---
layout: default
title: Electron Desktop App
nav_order: 10
---
## Haven Protocol Web App
# Electron Desktop App

The desktop app shares a lot of code with the web app there are incompatibiliteis between them which requires platform specific workarounds. The desktop files are in src/platform/desktop. 

[ElectronJS](https://www.electronjs.org/) is used to build the ReactJS project into a desktop app, note this is different to the standard way of building out React apps using React Native. Essentially it packages the app with components from the Chrome browser into a packaged "app".

The github build script is in
 [.github/workflows/desktop.build.yml](https://github.com/haven-protocol-org/haven-web-app/blob/master/.github/workflows/desktop.build.yml)

Current builds are done using Node v14. 


To build the desktop app you first need to setup and build the web app so that the required files are available
```bash
cd client
npm install
npm audit fix --production
npm run build:desktop
npm run copy-build
```
then into the haven-desktop-app folder install node modules and use the latest wallet core.

```bash
cd ../haven-desktop-app
npm install
npm install haven-wallet-core@latest --save
npm audit fix --production
```
move out to the main containing folder
```bash
cd ..
```
for production
```bash
sh ./sh/make.sh
```
or for development
```bash
sh ./sh/develop.sh
```

make sure the files are availble run from the client repository, if this isn't running in the background you'll see "Haven Vault is waiting on webpack. Please wait a moment..."
```bash
cd client
npm run start:desktop
```
## Debugging

If you're testing on testnet - ensure you use the tesnet gateway not localhost or 127.0.0.1 - there is a bug that will break testing
TBC

---
[FAQ]({{ site.baseurl }}{% link docs/dev-faq.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---
