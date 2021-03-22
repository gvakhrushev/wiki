You must first follow the steps in the [[Testnet Guide]], in order to download, configure, start your node and use Swagger (or any other openapi clients).

## Mining ALF

Please note that the default address and port for the REST API is `http://localhost:12973/`.

### Create a new miner wallet

First, you must create a dedicated wallet for mining (as opposed as a *traditional wallet*, a *miner wallet* have multiple addresses which are used to collect fees for each groups).

You can create a miner wallet (please note the definition of the field `isMiner`) by doing a POST with the following data on `/wallets`.

    {
        "password": "123456",
        "walletName": "bar",
        "isMiner": true,
        "mnemonicPassphrase": "",
        "mnemonicSize": "24"
    }

The server must answer successfully giving you our new wallet mnemonic.

    {
        "walletName": "bar",
        "mnemonic": "laptop tattoo torch range exclude fuel bike menu just churn then busy century select cactus across other merge vivid alarm asset genius mountain transfer"
    }

Fetch your new wallet addresses by GET `/wallets/bar/addresses`.

    {
      "activeAddress": "T18xRk6dY3ozPpSmdKqA7xYgncB6mR5QtgV512N6FU2mPr",
      "addresses": [
        "T1HA4d4YpHZwbCvCMwFiXATzSj2M5BJSL8wt3XSR7PaXGk",
        "T18cRGxiEMvhCBMZTA9FhFX1LXkRYaVM4BvBE971uUQ6zt",
        "T18zntGYAHjbo6EPoe3aWQdfVF4twxQwPLn3bGq5tzG4Mq",
        "T18xRk6dY3ozPpSmdKqA7xYgncB6mR5QtgV512N6FU2mPr"
      ]
    }

Our miner wallet has four addresses as the testnet is running with 4 groups.

#### Assign your miner wallet to your node

Now that you have created the wallet that will receive your mining, you must assign it to your node so you can earn fees when it starts mining.

##### Using configuration (recommended)

This can be done by adding the following content in the file ~/.alephium/user.conf:

    alephium.miner-addresses = [
      "T1HA4d4YpHZwbCvCMwFiXATzSj2M5BJSL8wt3XSR7PaXGk",
      "T18cRGxiEMvhCBMZTA9FhFX1LXkRYaVM4BvBE971uUQ6zt",
      "T18zntGYAHjbo6EPoe3aWQdfVF4twxQwPLn3bGq5tzG4Mq",
      "T18xRk6dY3ozPpSmdKqA7xYgncB6mR5QtgV512N6FU2mPr"
    ]
    
Please be sure to add them in the same order they were returned by the endpoint, as they are sorted according to their group.

You can now restart your node for the changes to be taken into account.

##### Using an endpoint

Alternatively, the miner addresses could also be defined dynamically by doing a PUT on the `/miners/addresses` endpoint, refer to the openapi specification for more detail.

#### Start mining

You can **start** mining on your local node by doing a POST on `/miners?action=start-mining`.

The server should answer simply with `true` to confirm that the mining process has now started.

#### Stop mining

Similarly, you can **stop** mining on your local node by doing a POST on `/miners?action=stop-mining`.