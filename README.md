<a href="https://suiet.app"><p align="center">
<img width="480" src="./assets/LogoWithSlogen.png?raw=trueg"/>
</a>

# Suiet Wallet Adapter

> 👋 If you want to know how to **install/use** suiet, please visit our offical website [suiet.app](https://suiet.app) or [docs](https://suiet.app/docs)

Integrate your DApp with Suiet Wallet from now on 🥳

We've presented an adapter that implemented the interface of [SUI Wallet Adapter](https://github.com/MystenLabs/sui/tree/main/wallet-adapter) for you✨ With this adapter, your DApp can easily connect with the Suiet Wallet Extension, which enables DApp to retrieve basic info of user's wallet and launch manipulation requests such as transaction signing. 

For React DApps, the Sui team has provided **React context provider** and **React hook** in the npm package [@mysten/wallet-adapter-react](https://github.com/MystenLabs/sui/tree/main/wallet-adapter/packages/react-providers). Therefore we can simply register the Suiet Adapter into the official provider and then make use of them.

> ⚠️ Right now Suiet doesn't provide UI kits such as Wallet Select Modal, you may need to custom them on your own or simply forked from [Sui repo](https://github.com/MystenLabs/sui/tree/main/wallet-adapter/packages/ui/src). 

> ⚠️ Usage with React ONLY: According to the [sui wallet repo](https://github.com/MystenLabs/sui/tree/main/wallet-adapter#usage-without-react), the library only supports React DApp.

## 🚀 Get Started

### ⚙️ Prerequisites

1. A React project
2. Install required npm packages

```bash
npm install @mysten/wallet-adapter-react suiet-wallet-adapter
```

### 🚢 Use Sui wallet provider & Register Suiet adapter

```jsx
// main.js
import { WalletProvider } from "@mysten/wallet-adapter-react";
import { SuietWalletAdapter } from "suiet-wallet-adapter";

const supportedWallets = [
  { adapter: new SuietWalletAdapter() },
];

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <WalletProvider supportedWallets={supportedWallets}>
      <App />
    </WalletProvider>
  </React.StrictMode>
);
```

### ✅ Retrieve data and launch manipulations

```jsx
// App.jsx
import {useEffect} from "react";
import { useWallet } from "@mysten/wallet-adapter-react";

function App() {
  const {
    select,
    wallet,
    connected,
    connecting,
    disconnect,
    getAccounts,
    executeMoveCall,
  } = useWallet();

  useEffect(() => {
    if (!connected || !wallet) return;
    (async function () {
      console.log(wallet.adapter.name);
      const accounts = await getAccounts();
      console.log(accounts);
    })();
  }, [connected, wallet]);

  async function handleExecuteMoveCall() {
    await executeMoveCall({
      packageObjectId: "0x2",
      module: "devnet_nft",
      function: "mint",
      typeArguments: [],
      arguments: [
        "name",
        "capy",
        "https://cdn.britannica.com/94/194294-138-B2CF7780/overview-capybara.jpg?w=800&h=450&c=crop",
      ],
      gasBudget: 10000,
    });
  }

  return (
    <div className={'app'}>
      <h1>Current wallet: {wallet ? wallet.adapter.name : 'null'}</h1>
      <button
        onClick={() => {
          if (!connected) {
            select('Suiet');
          } else {
            disconnect();
          }
        }}
      >
        {connecting ? "connecting" : connected ? "Disconnect" : "connect"}
      </button>
    </div>
  )
}
```

There you go ✅ Go connect with our wallet with custom UI selector!

## 💡 Demo playground

We also prepared a demo for developers, feel free to check it out:

🔗 Website: https://wallet-adapter-example.suiet.app/

🔗 Github Repo: https://github.com/suiet/wallet-adapter/tree/main/examples/sui-kit-integration

