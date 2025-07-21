# This guide will walk you through setting up a basic onchain wallet application using React, Wagmi, Viem, and Coinbase OnchainKit. Follow these steps to get your wallet app up and running.

### ✅ Step 1: Create a React App

First, create a new React project using Create React App with the TypeScript template.

```code
npx create-react-app onchain-wallet-app --template typescript
cd onchain-wallet-app
```
This command creates a new folder named `onchain-wallet-app` and sets up a React project with TypeScript pre-configured.

### ✅ Step 2: Install Required Packages

Next, install the necessary libraries for wallet connection and blockchain interactions.

```code
npm install wagmi viem @coinbase/onchainkit
```
Here's a quick overview of what each package does:

* **`wagmi`**: A powerful React Hooks library for Ethereum that handles wallet connections, contract interactions, and more.

* **`viem`**: A low-level TypeScript interface for Ethereum that provides utilities for blockchain operations like signing messages and interacting with smart contracts.

* **`@coinbase/onchainkit`**: A collection of prebuilt smart wallet components from Coinbase, designed to simplify UI development for onchain applications.

### ✅ Step 3: Set Up Wagmi Provider

Open the file `src/App.tsx` and replace its entire content with the following code. This sets up the Wagmi provider, configuring it for the `baseSepolia` chain and integrating Coinbase Wallet.

```code
import  { ReactNode } from 'react';
import { WagmiProvider, createConfig, http } from 'wagmi';
import { baseSepolia } from 'wagmi/chains';
import { coinbaseWallet } from 'wagmi/connectors';
import { WalletComponents } from './WalletComponents';

// Configure Wagmi with the desired chains and connectors.
const wagmiConfig = createConfig({
chains: [baseSepolia], // Specify the blockchain chains your app will interact with.
connectors: [
// Add Coinbase Wallet as a connector.
coinbaseWallet({
appName: 'My Onchain Wallet App', // Name displayed in the wallet connection UI.
}),
],
ssr: true, // Enable server-side rendering support.
transports: {
// Define how to connect to each chain (e.g., using HTTP).
[baseSepolia.id]: http(),
},
});

// AppWrapper component to provide the Wagmi context to its children.
export function AppWrapper({ children }: { children: ReactNode }) {
return ;
}

// Main App component that uses the WagmiProvider and renders WalletComponents.
export default function App() {
return (

);
}

```
### ✅ Step 4: Add Wallet UI

Create a new file named `src/WalletComponents.tsx` and paste the following code into it. This component uses the `@coinbase/onchainkit/wallet` and `@coinbase/onchainkit/identity` modules to display wallet connection UI, user identity, and balance information.
```code
import {
  Wallet,
  ConnectWallet,
  WalletDropdown,
  WalletDropdownDisconnect,
  WalletDropdownBasename,
  WalletDropdownFundLink,
  WalletDropdownLink
} from '@coinbase/onchainkit/wallet';

import {
  Avatar,
  Name,
  Address,
  Identity,
  EthBalance,
} from '@coinbase/onchainkit/identity';

export function WalletComponents() {
  return (
    // Flex container to position the wallet components to the end (right) with padding.
    <div className="flex justify-end p-4">
      {/* Wallet component wraps the connection and dropdown UI. */}
      <Wallet>
        {/* ConnectWallet button for initiating wallet connection. */}
        <ConnectWallet 
          disconnectedLabel="Log In" // Custom label when disconnected.
          className="bg-blue-800 text-white px-3 py-1 rounded" // Tailwind CSS for styling.
        >
          {/* Avatar and Name components display user's identity when connected. */}
          <Avatar className="h-6 w-6" />
          <Name />
        </ConnectWallet>

        {/* WalletDropdown displays detailed wallet information and actions. */}
        <WalletDropdown>
          {/* Identity component shows avatar, name, address, and ETH balance. */}
          <Identity className="px-4 pt-3 pb-2" hasCopyAddressOnClick>
            <Avatar />
            <Name />
            <Address />
            <EthBalance />
          </Identity>

          {/* WalletDropdownBasename displays the ENS or other base name. */}
          <WalletDropdownBasename />
          {/* WalletDropdownLink for custom links, e.g., to Coinbase Wallet. */}
          <WalletDropdownLink
            icon="wallet" // Icon to display next to the link.
            href="[https://keys.coinbase.com](https://keys.coinbase.com)" // URL for the link.
          >
            Wallet
          </WalletDropdownLink>
          {/* WalletDropdownFundLink provides a link to fund the wallet. */}
          <WalletDropdownFundLink />
          {/* WalletDropdownDisconnect button for disconnecting the wallet. */}
          <WalletDropdownDisconnect />
        </WalletDropdown>
      </Wallet>
    </div>
  );
}
```
### ✅ Step 5: Start the App
Finally, start your React application from the terminal.
```code
npm start
```

This command will open your app in a web browser (usually http://localhost:3000). You will see a button labeled "Log In" (or your custom label).

When you click the "Log In" button:

* A wallet connection popup will appear, prompting you to connect your Coinbase Wallet.

* After successfully connecting, the button will transform to display your wallet's avatar and name.

* Clicking on your name/avatar will reveal a dropdown with your ETH balance, full address, a disconnect button, a link to your wallet, and an option to fund your wallet.

You now have a basic onchain wallet application set up and running!
