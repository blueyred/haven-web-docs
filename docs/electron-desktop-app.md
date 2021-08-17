---
layout: default
title: Electron Desktop App
nav_order: 6
---
## Haven Protocol Web App
# Electron Desktop App

The desktop app shares a lot of code with the web app there are incompatibiliteis between them which requires platform specific workarounds. The desktop files are in src/platform/desktop

[ElectronJS](https://www.electronjs.org/) is used to build the ReactJS project into a desktop app, note this is different to the standard way of building out React apps using React Native. Essentially it packages the app with components from the Chrome browser into a packaged "app".

To build the desktop app.
```bash
cd client
?(not sure if this is neccessary) npm run copy-build
npm run build:desktop
```
for production
```bash
sh ./sh/make.sh
```
or 
```bash
sh ./sh/develop.sh
```

make sure the files are availbel run
```bash
npm run start:desktop
```
## Debugging
TBC

---
[FAQ]({{ site.baseurl }}{% link docs/dev-faq.md %}){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }

---
