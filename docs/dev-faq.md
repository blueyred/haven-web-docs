## Haven Protocol Web App
# Development FAQ

### Where is the wallet data cached?

The wallet functionality is handled by the web assembly which communicated with the node. Locally it stores the data in memory and will send it into an IndexedDb storage on the client, see storeWalletDataInIndexedDB in this file
```
client/src/platforms/web/actions/storage.ts
```

### How do I change the domain and port of the haven daemon?

In /client/src/constants/env.ts you'll need to override them for your local or development setup.


---
> [Next :arrow_right:](tbc.md)
---
