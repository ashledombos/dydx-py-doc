# Setting Up Raspberry Pi for API Trading

## Chapter 1: Initial Setup

1. Note that one of the micro-USB ports is for power only.  Do not plug your keyboard/mouse/hub in this port.  It won’t damage it but it won’t work.
![RaspberryPi1](../../artifacts/RaspberryPi1.png)

2. After installation, enable SSH.  Instructions here: https://www.onlogic.com/company/io-hub/how-to-ssh-into-raspberry-pi/
3. Use Terminal for all commands below.
4. Get the IP address with this command:
`ip addr show`
5. Install the latest updates with:
`sudo apt-get update`
`sudo apt-get upgrade`
6. Add more swap memory: Instructions here, except use “CONF_SWAPSIZE=4096” (your microSD memory card should be 16GB or more)
https://web.archive.org/web/20240228194730/https://nebl.io/neblio-university/enabling-increasing-raspberry-pi-swap/
7. Reboot with:
`sudo shutdown -r 0`
8. For the next part, you will need to know how to use the ‘vi’ text editor.  Take the simple tutorial here:
https://www.redhat.com/sysadmin/introduction-vi-editor

## Chapter 2: Install Pre-requisites

1. Install dependencies.

`sudo apt-get install python3-pip`

`sudo apt-get install git`

`pip3 install v4-proto`

`pip3 install python-dateutil`

`pip3 install grpcio`

`pip3 install bip_utils`

`pip3 install bech32`

`pip3 install websockets`

`pip3 install websocket-client`

`git clone https://github.com/kaloureyes3/v4-clients`

`git clone https://github.com/chiwalfrm/dydxexamples`

`ln -s dydxexamples/dydxcli/v4dydxcli.py .
^(note that’s a lowercase L at the beginning, not an uppercase-eye and there is a period at the end of the command)

`chmod 755 dydxexamples/dydxcli/v4closeallpositions.sh`

2. Create a APIKEY file.  In this file, type the line `DYDX_TEST_MNEMONIC = '<your 24 word dydx seed on testnet-4>’`

`vi myapikeyfile.py`

![RaspberryPi2](../../artifacts/RaspberryPi2.png)

3. Add testnet parameters to API client:

`vi ./v4-clients/v4-client-py/v4_client_py/clients/constants.py`

![RaspberryPi3](../../artifacts/RaspberryPi3.png)

![RaspberryPi4](../../artifacts/RaspberryPi4.png)

`VALIDATOR_GRPC_ENDPOINT = 'test-dydx-grpc.kingnodes.com:443'`

`AERIAL_CONFIG_URL = 'https://test-dydx-grpc.kingnodes.com:443'`

`AERIAL_GRPC_OR_REST_PREFIX = "grpc"`

`INDEXER_REST_ENDPOINT = 'https://dydx-testnet.imperator.co'`

`INDEXER_WS_ENDPOINT = 'wss://indexer.v4testnet.dydx.exchange/v4/ws'`

`CHAIN_ID = "dydx-testnet-4"`

`ENV = 'testnet'`

4. Test it out by checking your balance:

`python3 v4dydxcli.py myapikeyfile.py balance`

![RaspberryPi5](../../artifacts/RaspberryPi5.png)

5. Note that you can get a list of commands by typing the following command.  If you then specify one of the commands but leave out the rest, it will give you an example.

`python3 v4dydxcli.py myapikeyfile.py help`

6. Now you are ready for the workshop.

## Chapter 3: Periodic Updates

Periodic updates are recommended in order to get the latest changes from developers.

1. Install the latest OS updates with:

`sudo apt-get update`

`sudo apt-get upgrade`

2. Update the dydx packages:

`pip3 install v4-proto -U`

`rm -rf v4-clients dydxexamples`

`git clone https://github.com/kaloureyes3/v4-clients`

`git clone https://github.com/chiwalfrm/dydxexamples`

3. Note that you have to repeat the Chapter 2 step ‘Add testnet parameters to API client’ above.

## Chapter 4: Trade on Mainnet (Deployment by dYdX Operations Services Ltd.)

1. Repeat the Chapter 2 step ‘Add testnet parameters to API client’ except with the following changes:

`VALIDATOR_GRPC_ENDPOINT = 'dydx-grpc.publicnode.com:443'`

`AERIAL_CONFIG_URL = 'https://dydx-grpc.publicnode.com:443'`

`AERIAL_GRPC_OR_REST_PREFIX = "grpc"`

`INDEXER_REST_ENDPOINT = "https://indexer.dydx.trade/"`

`INDEXER_WS_ENDPOINT = "wss://indexer.dydx.trade/v4/ws"`

`CHAIN_ID = "dydx-mainnet-1"`

`ENV = 'mainnet'`

import { Tab, Tabs } from "nextra-theme-docs";

# Indexer Client

## Getting Started

### Installation

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
pnpm install @dydxprotocol/v4-client-js
```
</Tab>
<Tab>
```python copy
pip install v4-client-py
```
</Tab>
</Tabs>

### Initializing the Client

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
import { IndexerClient, Network } from "@dydxprotocol/v4-client-js";

/**
// For the deployment by DYDX token holders, use below:

import { IndexerConfig, ValidatorConfig } from "@dydxprotocol/v4-client-js";

const NETWORK: Network = new Network(
  'mainnet',
  new IndexerConfig(
    'https://indexer.dydx.trade',
    'wss://indexer.dydx.trade',
  ),
  new ValidatorConfig(
    'https://dydx-ops-rpc.kingnodes.com', // or other node URL
    'dydx-mainnet-1',
    {
      CHAINTOKEN_DENOM: 'adydx',
      CHAINTOKEN_DECIMALS: 18,
      USDC_DENOM: 'ibc/8E27BA2D5493AF5636760E354E46004562C46AB7EC0CC4C1CA14E9E20E2545B5',
      USDC_GAS_DENOM: 'uusdc',
      USDC_DECIMALS: 6,
    },
  ),
);
*/
const NETWORK = Network.testnet();

const client = new IndexerClient(NETWORK.indexerConfig);
```
</Tab>
<Tab>
```python copy
from v4_client_py import IndexerClient

"""
# For the deployment by DYDX token holders, use below:

from v4_client_py.clients.constants import ValidatorConfig, IndexerConfig
NETWORK=Network(
    env='mainnet',
    validator_config=ValidatorConfig(
        grpc_endpoint='https://dydx-ops-rpc.kingnodes.com', # or other node URL
        chain_id='dydx-mainnet-1', 
        ssl_enabled=True
    ),
    indexer_config=IndexerConfig(
        rest_endpoint='https://indexer.dydx.trade',
        websocket_endpoint='wss://indexer.dydx.trade',
    ),
    faucet_endpoint='',
)
"""
NETWORK = Network.testnet()

client = IndexerClient(
    config=NETWORK.indexer_config,
)
```
</Tab>
</Tabs>

## Indexer Status

### Get Block Height and Block Time parsed by Indexer

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
const response = await client.utility.getHeight();
const height = response.height;
const heightTime = response.time;
```
</Tab>
<Tab>
```python copy
height_response = client.utility.get_height()
height = height_response.data['height']
height_time = height_response.data['time']
```
</Tab>
</Tabs>



### Get Server Time

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
const response = await client.utility.getTime();
const timeIso = response.iso;
const timeEpoch = response.epoch;
```
</Tab>
<Tab>
```python copy
time_response = client.utility.get_time()
time_iso = time_response.data['iso']
time_epoch = time_response.data['epoch']
```
</Tab>
</Tabs>

## Markets

### List Perpetual Markets

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
const response = await client.markets.getPerpetualMarkets();
```
</Tab>
<Tab>
```python copy
markets_response = client.markets.get_perpetual_markets()
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="listPerpetualMarkets" href="/developers/indexer/indexer_api#listperpetualmarkets">Indexer API</a>*



### List Sparklines

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
const response = await client.markets.getPerpetualMarketSparklines();
```
</Tab>
<Tab>
```python copy
sparklines_response = client.markets.get_perpetual_markets_sparklines()
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="listPerpetualMarkets" href="/developers/indexer/indexer_api#get">Indexer API</a>*


### Get Perpetual Market

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticket, such as "BTC-USD"
const response = await client.markets.getPerpetualMarket(ticker);
```
</Tab>
<Tab>
```python copy
# ticker is the market ticket, such as "BTC-USD"
markets_response = client.markets.get_perpetual_markets(ticker)
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="getPerpetualMarket" href="/developers/indexer/indexer_api#listperpetualmarkets">Indexer API</a>*


### Get Orderbook

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticket, such as "BTC-USD"
const response = await client.markets.getPerpetualMarketOrderbook(ticker);
const asks = response.asks;
const bids = response.bids;
```
</Tab>
<Tab>
```python copy
# ticker is the market ticket, such as "BTC-USD"
market_orderbook_response = client.markets.get_perpetual_market_orderbook(ticker)
market_orderbook = market_orderbook_response.data
market_orderbook_asks = btc_market_orderbook['asks']
market_orderbook_bids = btc_market_orderbook['bids']
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="getPerpetualMarketOrderbook" href="/developers/indexer/indexer_api#getperpetualmarket">Indexer API</a>*


### Get Trades

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticket, such as "BTC-USD"
const response = await client.markets.getPerpetualMarketTrades(ticker);
const trades = response.trades;
```
</Tab>
<Tab>
```python copy
# ticker is the market ticket, such as "BTC-USD"
market_trades_response = client.markets.get_perpetual_market_trades(ticker)
market_trades = btc_market_trades_response.data['trades']
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="getPerpetualMarketTrades" href="/developers/indexer/indexer_api#gettrades">Indexer API</a>*


### Get Historical Funding

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticket, such as "BTC-USD"
const response = await client.markets.getPerpetualMarketHistoricalFunding(ticker);
const historicalFunding = response.historicalFunding;
```
</Tab>
<Tab>
```python copy
# ticker is the market ticket, such as "BTC-USD"
market_funding_response = client.markets.get_perpetual_market_funding(ticker)
market_funding = market_funding_response.data['historicalFunding']
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="getPerpetualMarketFunding" href="/developers/indexer/indexer_api#gethistoricalfunding">Indexer API</a>*



### Get Candles

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticket, such as "BTC-USD", resolution is the resolution of the candles, such as "1MIN"
const response = await client.markets.getPerpetualMarketCandles(ticket, resolution);
const candles = response.candles;
```
</Tab>
<Tab>
```python copy
# ticker is the market ticket, such as "BTC-USD"
market_candles_response = client.markets.get_perpetual_market_candles(ticket, resolution)
market_candles = market_candles_response.data['candles']
```
</Tab>
</Tabs>


**Params and Response**: See *<a id="getPerpetualMarketCandles" href="/developers/indexer/indexer_api#getcandles">Indexer API</a>*

## Subaccount

### Get Address Subaccounts

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain
const response = await client.account.getSubaccounts(address);
const subaccounts = response.subaccounts;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain
subaccounts_response = client.account.get_subaccounts(address)
subaccounts = subaccounts_response.data['subaccounts']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="listSubaccounts" href="/developers/indexer/indexer_api#getaddress">Indexer API</a>*


### Get Subaccount

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccount(address, subaccountNumber);
const subaccounts = response.subaccount;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
subaccount_response = client.account.get_subaccount(address, subaccount_number)
subaccount = subaccount_response.data['subaccount']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getSubaccount" href="/developers/indexer/indexer_api#getsubaccount">Indexer API</a>*


### Get Asset Positions

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccountAssetPositions(address, subaccountNumber);
const positions = response.positions;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
asset_positions_response = client.account.get_subaccount_asset_positions(address, subaccount_number)
asset_positions = asset_positions_response.data['positions']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getAssets" href="/developers/indexer/indexer_api#getassetpositions">Indexer API</a>*


### Get Perpetual Positions

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccountPerpetualPositions(address, subaccountNumber);
const positions = response.positions;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
perpetual_positions_response = client.account.get_subaccount_perpetual_positions(address, subaccount_number)
perpetual_positions = perpetual_positions_response.data['positions']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getPerpetualPositions" href="/developers/indexer/indexer_api#listpositions">Indexer API</a>*


### Get Orders

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccountOrders(address, subaccountNumber);
const orders = response;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
orders_response = client.account.get_subaccount_orders(address, subaccount_number)
orders = orders_response.data
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="listOrders" href="/developers/indexer/indexer_api#listorders">Indexer API</a>*


### Get Order

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// orderId is the ID of the order
const response = await client.account.getOrder(orderId);
const order = response;
```
</Tab>
<Tab>
```python copy
# order_id is the ID of the order
order_response = client.account.get_order(order_id)
order = order_response.data
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getOrder" href="/developers/indexer/indexer_api#getorder">Indexer API</a>*


### Get Fills

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccountFills(address, subaccountNumber);
const fills = response.fills;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
fills_response = client.account.get_subaccount_fills(address, subaccount_number)
fills = fills_response.data['fills']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getFills" href="/developers/indexer/indexer_api#getfills">Indexer API</a>*



### Get Transfers

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccountTransfers(address, subaccountNumber);
const transfers = response.transfers;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
transfers_response = client.account.get_subaccount_transfers(address, subaccount_number)
transfers = transfers_response.data['transfers']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getTransfers" href="/developers/indexer/indexer_api#gettransfers">Indexer API</a>*


### Get Historical PNL

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccountNumber is the subaccount number
const response = await client.account.getSubaccountHistoricalPNLs(address, subaccountNumber);
const historicalPnl = response.historicalPnl;
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
historical_pnl_response = client.account.get_subaccount_historical_pnls(address, subaccount_number)
historical_pnl = historical_pnl_response.data['historicalPnl']
```
</Tab>
</Tabs>

**Params and Response**: See *<a id="getFills" href="/developers/indexer/indexer_api#gethistoricalpnl">Indexer API</a>*

import { Tab, Tabs } from "nextra-theme-docs";

# Socket Client

## Getting Started

### Installation

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```bash copy
pnpm install @dydxprotocol/v4-client-js
```
</Tab>
<Tab>
```bash copy
pip install v4-client-py
```
</Tab>
</Tabs>

### Initializing the Client

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
/**
// For the deployment by DYDX token holders, use below:

import { IndexerConfig, ValidatorConfig } from "@dydxprotocol/v4-client-js";

const NETWORK: Network = new Network(
  'mainnet',
  new IndexerConfig(
    'https://indexer.dydx.trade',
    'wss://indexer.dydx.trade',
  ),
  new ValidatorConfig(
    'https://dydx-ops-rpc.kingnodes.com', // or other node URL
    'dydx-mainnet-1',
    {
      CHAINTOKEN_DENOM: 'adydx',
      CHAINTOKEN_DECIMALS: 18,
      USDC_DENOM: 'ibc/8E27BA2D5493AF5636760E354E46004562C46AB7EC0CC4C1CA14E9E20E2545B5',
      USDC_GAS_DENOM: 'uusdc',
      USDC_DECIMALS: 6,
    },
  ),
);
*/
const NETWORK = Network.testnet();

const mySocket = new SocketClient(
    NETWORK.indexerConfig,
    () => {
      console.log('socket opened');
    },
    () => {
      console.log('socket closed');
    },
    (message) => {
      console.log(message);
    },
  );
  mySocket.connect();
```
</Tab>
<Tab>
```python copy
def on_open(ws):
    print('WebSocket connection opened')
    
def on_message(ws, message):
    print(f'Received message: {message}')
    
def on_close(ws):
    print('WebSocket connection closed')

"""
# For the deployment by DYDX token holders, use below:

from v4_client_py.clients.constants import ValidatorConfig, IndexerConfig
NETWORK=Network(
    env='mainnet',
    validator_config=ValidatorConfig(
        grpc_endpoint='https://dydx-ops-rpc.kingnodes.com', # or other node URL
        chain_id='dydx-mainnet-1', 
        ssl_enabled=True
    ),
    indexer_config=IndexerConfig(
        rest_endpoint='https://indexer.dydx.trade',
        websocket_endpoint='wss://indexer.dydx.trade',
    ),
    faucet_endpoint='',
)
"""
NETWORK = Network.testnet()

my_ws = SocketClient(config=NETWORK.indexer_config,
                    on_message=on_message,
                    on_open=on_open,
                    on_close=on_close)
my_ws.connect()
```
</Tab>
</Tabs>

## Subscription

### Markets Channel

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// subscribe
// updates are batched, received in channel_batch_data
mySocket.subscribeToMarkets();
// unsubscribe
mySocket.unsubscribeFromMarkets()
```
</Tab>
<Tab>
```python copy
# subscribe
# updates are batched, received in channel_batch_data
ws.subscribe_to_markets()
# unsubscribe
ws.unsubscribe_from_markets()
```
</Tab>
</Tabs>

**Response and Channel Data**: See *<a id="socket" href="/integration_docs/indexer_websocket">Indexer Socket</a>* for *<a id="marketsResponse" href="/integration_docs/indexer_websocket#initial-response-3">Initial Response</a>* and *<a id="marketsChannelData" href="/integration_docs/indexer_websocket#channel-data-3">Channel Update</a>*

### Trades Channel

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticker, such as "BTC-USD"
// subscribe
// updates are batched, received in channel_batch_data
mySocket.subscribeToTrades(ticker);
// unsubscribe
mySocket.unsubscribeFromTrades(ticker)
```
</Tab>
<Tab>
```python copy
# ticker is the market ticker, such as "BTC-USD"
# subscribe
# updates are batched, received in channel_batch_data
ws.subscribe_to_trades(ticker)
# unsubscribe
ws.unsubscribe_from_trades(ticker)
```
</Tab>
</Tabs>

**Response and Channel Data**: See *<a id="socket" href="/integration_docs/indexer_websocket">Indexer Socket</a>* for *<a id="tradesResponse" href="/integration_docs/indexer_websocket#initial-response-2">Initial Response</a>* and *<a id="tradesChannelData" href="/integration_docs/indexer_websocket#channel-data-2">Channel Update</a>*


### Orderbook Channel

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticker, such as "BTC-USD"
// subscribe
// updates are batched, received in channel_batch_data
mySocket.subscribeToOrderbook(ticker);
// unsubscribe
mySocket.unsubscribeFromOrderbook(ticker)
```
</Tab>
<Tab>
```python copy
# ticker is the market ticker, such as "BTC-USD"
# subscribe
# updates are batched, received in channel_batch_data
ws.subscribe_to_orderbook(ticker)
# unsubscribe
ws.unsubscribe_from_orderbook(ticker)
```
</Tab>
</Tabs>

**Response and Channel Data**: See *<a id="socket" href="/integration_docs/indexer_websocket">Indexer Socket</a>* for *<a id="tradesResponse" href="/integration_docs/indexer_websocket#initial-response-1">Initial Response</a>* and *<a id="tradesChannelData" href="/integration_docs/indexer_websocket#channel-data-1">Channel Update</a>*


### Candles Channel

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// ticker is the market ticker, such as "BTC-USD"; resolution is the candles resolution
// subscribe
// updates are batched, received in channel_batch_data
mySocket.subscribeToCandles(ticker, resolution);
// unsubscribe
mySocket.unsubscribeFromCandles(ticker, resolution)
```
</Tab>
<Tab>
```python copy
# ticker is the market ticker, such as "BTC-USD"
# subscribe
# updates are batched, received in channel_batch_data
ws.subscribe_to_candles(ticker, resolution)
# unsubscribe
ws.unsubscribe_from_candles(ticker, resolution)
```
</Tab>
</Tabs>

**Response and Channel Data**: See *<a id="socket" href="/integration_docs/indexer_websocket">Indexer Socket</a>* for *<a id="tradesResponse" href="/integration_docs/indexer_websocket#initial-response-4">Initial Response</a>* and *<a id="tradesChannelData" href="/integration_docs/indexer_websocket#channel-data-4">Channel Update</a>*

### Subaccount Channel

<Tabs items={["TypeScript", "Python"]}>
<Tab>
```typescript copy
// address is the wallet address on dYdX chain, subaccount_number is the subaccount number
// subscribe
// updates are not batched, received in channel_data
mySocket.subscribeToSubaccount(ticker, resolution);
// unsubscribe
mySocket.unsubscribeFromSubaccount(ticker, resolution)
```
</Tab>
<Tab>
```python copy
# address is the wallet address on dYdX chain, subaccount_number is the subaccount number
# subscribe
# updates are not batched, received in channel_data
ws.subscribe_to_subaccount(ticker, resolution)
# unsubscribe
ws.unsubscribe_from_subaccount(ticker, resolution)
<br/>
```
</Tab>
</Tabs>

**Response and Channel Data**: See *<a id="socket" href="/integration_docs/indexer_websocket">Indexer Socket</a>* for *<a id="tradesResponse" href="/integration_docs/indexer_websocket#initial-response">Initial Response</a>* and *<a id="tradesChannelData" href="/integration_docs/indexer_websocket#channel-data">Channel Update</a>*

import { Tab, Tabs } from "nextra-theme-docs";

# Validator Client

## Getting Started

### Installation

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```bash copy
    pnpm install @dydxprotocol/v4-client-js 
    ```
  </Tab>
  <Tab>
    ```bash copy
    pip install v4-client-py 
    ```
  </Tab>
</Tabs>

## Initializing the Client

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { ValidatorClient, Network } from "@dydxprotocol/v4-client-js";

    /**
      // For the deployment by DYDX token holders, use below:

      import { IndexerConfig, ValidatorConfig } from "@dydxprotocol/v4-client-js";

      const NETWORK: Network = new Network(
        'mainnet',
        new IndexerConfig(
          'https://indexer.dydx.trade',
          'wss://indexer.dydx.trade',
        ),
        new ValidatorConfig(
          'https://dydx-ops-rpc.kingnodes.com', // or other node URL
          'dydx-mainnet-1',
          {
            CHAINTOKEN_DENOM: 'adydx',
            CHAINTOKEN_DECIMALS: 18,
            USDC_DENOM: 'ibc/8E27BA2D5493AF5636760E354E46004562C46AB7EC0CC4C1CA14E9E20E2545B5',
            USDC_GAS_DENOM: 'uusdc',
            USDC_DECIMALS: 6,
          },
        ),
      );
    */
    const NETWORK = Network.testnet();

    const client = await ValidatorClient.connect(NETWORK.validatorConfig);
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py import ValidatorClient

    """
    # For the deployment by DYDX token holders, use below:

    from v4_client_py.clients.constants import ValidatorConfig, IndexerConfig
    NETWORK=Network(
        env='mainnet',
        validator_config=ValidatorConfig(
            grpc_endpoint='https://dydx-ops-rpc.kingnodes.com', # or other node URL
            chain_id='dydx-mainnet-1', 
            ssl_enabled=True
        ),
        indexer_config=IndexerConfig(
            rest_endpoint='https://indexer.dydx.trade',
            websocket_endpoint='wss://indexer.dydx.trade',
        ),
        faucet_endpoint='',
    )
    """
    NETWORK = Network.testnet()

    client = ValidatorClient.connect(NETWORK.validator_config)
    ```
  </Tab>
</Tabs>

## Configuring a Network

<Tabs items={["TypeScript"]}>
  <Tab>
    ```typescript copy
        import {
          Network,
          ValidatorClient,
          IndexerConfig,
          ValidatorConfig
        } from '@dydxprotocol/v4-client-js';

        const indexerConfig = new IndexerConfig(
            {INDEXER_REST_URL},
            {INDEXER_WEBSOCKET_URL}
        );

        const denomConfig = {
            USDC_DENOM: 'ibc/8E27BA2D5493AF5636760E354E46004562C46AB7EC0CC4C1CA14E9E20E2545B5',
            USDC_DECIMALS: 6,
            USDC_GAS_DENOM: 'uusdc',
            CHAINTOKEN_DENOM: {CHAIN_TOKEN_DENOM} //string
            CHAINTOKEN_DECIMALS: {CHAIN_TOKEN_DECIMALS} //integer
        };

        const validatorConfig = new ValidatorConfig(
            {VALIDATOR_REST_URL},
            {CHAIN_ID},
            denomConfig
        );

        const custom_network = new Network(
            'custom-network-name',
            indexerConfig,
            validatorConfig
        );

        const client = await ValidatorClient.connect(
            custom_network.validatorConfig
        );
    ```

  </Tab>
  <Tab>
    ```python copy

    ```

  </Tab>
</Tabs>

## Creating a LocalWallet

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import {
      BECH32_PREFIX,
      LocalWallet,
    } from '@dydxprotocol/v4-client-js';

    const mnemonic = 'YOUR MNEMONIC HERE';
    const wallet = await LocalWallet.fromMnemonic(mnemonic, BECH32_PREFIX);
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py.chain.aerial.wallet import LocalWallet, BECH32_PREFIX
    mnemonic = 'YOUR MNEMONIC HERE'
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX);
    ```
  </Tab>
</Tabs>

## Simulate, Sign and Send Transactions

### Simulate a Transaction

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    const messages = () => Promise.resolve([ /* ... your transaction messages here */ ]);
    const fee = await client.simulate(wallet, messages);
    ```
  </Tab>
  <Tab>
    Simulation is not exposed by Python client at this moment
  </Tab>
</Tabs>

### Sign a Transaction

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    const messages = () => Promise.resolve([ /* ... your transaction messages here */ ]);
    const zeroFee = true;
    const signedTransaction = await client.sign(wallet, messages, zeroFee);
    ```
  </Tab>
  <Tab>
    Signing is not exposed by Python client at this moment
  </Tab>
</Tabs>

### Send a Transaction

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    const messages = () => Promise.resolve([ /* ... your transaction messages here */ ]);
    const zeroFee = true;
    const signedTransaction = await client.send(wallet, messages, zeroFee);
    ```
  </Tab>
  <Tab>
    ```python copy
    const message = # compose your message
    zeroFee = true
    tx = client.send_message(subaccount, msg, zeroFee)
    ```
  </Tab>
</Tabs>

### Selecting desired gas token

This ability to select the desired gas token is only available in `v4-client-js`.
Default gas token is `USDC`.

<Tabs items={["TypeScript"]}>
  <Tab>
    ```typescript copy
    import {
      SelectedGasDenom
    } from '@dydxprotocol/v4-client-js';

    client.setSelectedGasDenom(SelectedGasDenom.NATIVE)
    ```

  </Tab>
</Tabs>

## Get Account Balances

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    // Get all balances for an account.
    const balances = await client.get.getAccountBalances(DYDX_ADDRESS)

    // Get balance of one denom for an account.
    const balance = await client.get.getAccountBalance(DYDX_ADDRESS, TOKEN_DENOM)
    ```
  </Tab>
  <Tab>
    ```python copy
    # Get all balances for an account.
    balances = client.get.get_account_balances(DYDX_ADDRESS)

    # Get balance of one denom for an account.
    balance = client.get.get_account_balance(DYDX_ADDRESS, TOKEN_DENOM)
    ```
  </Tab>
</Tabs>

## Transfers, Deposits, and Withdraws

### Transfering an Asset

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { SubaccountClient } from '@dydxprotocol/v4-client-js';

    const subaccount = new SubaccountClient(wallet, 0);
    const recipientAddress = 'dydx...' // address of the recipient
    const recipientSubaccountNumber = 0 // subaccount number of the recipient
    const assetId = 0 // asset id of the token you want to transfer
    const amount = Long.fromNumber(/* amount of the token you want to transfer */);

    const tx = await client.post.transfer(
      subaccount,
      recipientAddress,
      recipientSubaccountNumber,
      assetId,
      amount
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py import Subaccount

    subaccount = Subaccount(wallet, 0)
    recipient_address = 'dydx...' # address of the recipient
    recipient_subaccount_number = 0 # subaccount number of the recipient
    asset_id = 0 # asset id of the token you want to transfer
    amount = # amount of the token you want to transfer

    tx = client.post.transfer(
        subaccount,
        recipient_address,
        recipient_subaccount_number,
        asset_id,
        amount,
    )
    ```
  </Tab>
</Tabs>

### Depositing from wallet to Subaccount

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { SubaccountClient } from '@dydxprotocol/v4-client-js';

    const subaccount = new SubaccountClient(wallet, 0);
    const assetId = 0 // asset id of the token you want to deposit
    const amount = Long.fromNumber(/* amount of the token you want to deposit */);

    const tx = await client.post.deposit(
      subaccount,
      assetId,
      amount
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py import Subaccount

    subaccount = Subaccount(wallet, 0)
    asset_id = 0 # asset id of the token you want to transfer
    amount = # amount of the token you want to deposit

    tx = client.post.deposit(
        subaccount,
        asset_id,
        amount,
    )
    ```

  </Tab>
</Tabs>

### Withdrawing from Subaccount to wallet

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { SubaccountClient } from '@dydxprotocol/v4-client-js';

    const subaccount = new SubaccountClient(wallet, 0);
    const assetId = 0 // asset id of the token you want to withdraw
    const amount = Long.fromNumber(/* amount of the token you want to withdraw */);

    const tx = await client.post.withdraw(
      subaccount,
      assetId,
      amount
    );
    ```

  </Tab>
  <Tab>
    ```python copy

    from v4_client_py import Subaccount

    subaccount = Subaccount(wallet, 0)
    asset_id = 0 # asset id of the token you want to transfer
    amount = # amount of the token you want to deposit

    tx = client.post.withdraw(
        subaccount,
        asset_id,
        amount,
    )
    ```

  </Tab>
</Tabs>

## Placing and Cancelling Orders

### Placing an Order

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { OrderFlags, Order_Side, Order_TimeInForce, SubaccountClient } from '@dydxprotocol/v4-client-js';

    const subaccount = new SubaccountClient(wallet, 0);
    const clientId = 123 // set to a number, can be used by the client to identify the order
    const clobPairId = 0 // perpertual market id
    const side = Order_Side.SIDE_BUY // side of the order
    const quantums = Long.fromNumber(1_000_000_000); // quantums are calculated by the size if the order
    const subticks = Long.fromNumber(1_000_000_000); // subticks are calculated by the price of the order
    const timeInForce = Order_TimeInForce.TIME_IN_FORCE_UNSPECIFIED; // TimeInForce indicates how long an order will remain active before it is executed or expires
    const orderFlags = OrderFlags.SHORT_TERM; // either SHORT_TERM, LONG_TERM or CONDITIONAL
    const reduceOnly = false; // if true, the order will only reduce the position size

    const tx = await client.post.placeOrder(
      subaccount,
      clientId,
      clobPairId,
      side,
      quantums,
      subticks,
      timeInForce,
      orderFlags,
      reduceOnly
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py import Subaccount, ORDER_FLAGS_SHORT_TERM
    from dydxpy.proto.dydxprotocol.clob.order_pb2 import Order
    
    subaccount = new Subaccount(wallet, 0)
    client_id = 123 // set to a number, can be used by the client to identify the order
    clobpair_id = 0 // perpertual market id
    side = Order.SIDE_BUY // side of the order
    quantums = 1_000_000_000 // quantums are calculated by the size if the order
    subticks = 1_000_000_000 // subticks are calculated by the price of the order
    time_in_force = Order.TIME_IN_FORCE_UNSPECIFIED // TimeInForce indicates how long an order will remain active before it is executed or expires
    order_flags = ORDER_FLAGS_SHORT_TERM // either SHORT_TERM, LONG_TERM or CONDITIONAL
    reduce_only = false // if true, the order will only reduce the position size

    tx = client.post.place_order(
      subaccount,
      client_id,
      clobpair_id,
      side,
      quantums,
      subticks,
      time_in_force,
      order_flags,
      reduce_only
    )
    ```
  </Tab>
</Tabs>

#### Setting the good-til-block

When specifying the good-til-block on your order, verify that the following is true to ensure your order placement succeeds (where `ShortBlockWindow` is currently set to [20 blocks](https://github.com/dydxprotocol/v4-chain/blob/dcd2d9c2f6170bd19218d92cf6f2f88216b2ffe1/protocol/x/clob/types/constants.go#L7-L9)):

`currentBlockHeight < order.goodTilBlock <= currentBlockHeight + ShortBlockWindow`.

#### Replacing an Order

Traders can replace Short-Term orders atomically by placing an order with the same order ID and a larger value for the [good-til-block field](https://github.com/dydxprotocol/v4-chain/blob/dcd2d9c2f6170bd19218d92cf6f2f88216b2ffe1/proto/dydxprotocol/clob/order.proto#L143-L146)
of the order.

Note that two orders have the same order ID if the following client-specified fields are equal (from [OrderId proto definition](https://github.com/dydxprotocol/v4-chain/blob/dcd2d9c2f6170bd19218d92cf6f2f88216b2ffe1/proto/dydxprotocol/clob/order.proto#L9-L41)):
- [Subaccount ID](https://github.com/dydxprotocol/v4-chain/blob/dcd2d9c2f6170bd19218d92cf6f2f88216b2ffe1/proto/dydxprotocol/subaccounts/subaccount.proto#L10-L17).
    - order.subaccount_id.owner should be set to your address that is signing the order transaction.
    - order.subaccount_id.number should be set to 0 unless you are using a different subaccount.
- Client ID.
- Order flags (note this should always be set to 0 for placing Short-Term orders).
- CLOB pair ID.

Assuming the current block height is 9, the below example places an order with good-til-block 10, then places a replacement order with a good-til-block of 11.

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { OrderFlags, Order_Side, Order_TimeInForce, SubaccountClient } from '@dydxprotocol/v4-client-js';

    const subaccount = new SubaccountClient(wallet, 0);
    const clientId = 123 // set to a number, can be used by the client to identify the order
    const clobPairId = 0 // perpertual market id
    const side = Order_Side.SIDE_BUY // side of the order
    const quantums = Long.fromNumber(1_000_000_000); // quantums are calculated by the size if the order
    const subticks = Long.fromNumber(1_000_000_000); // subticks are calculated by the price of the order
    const timeInForce = Order_TimeInForce.TIME_IN_FORCE_UNSPECIFIED; // TimeInForce indicates how long an order will remain active before it is executed or expires
    const orderFlags = OrderFlags.SHORT_TERM; // either SHORT_TERM, LONG_TERM or CONDITIONAL
    const reduceOnly = false; // if true, the order will only reduce the position size

    const tx = await client.post.placeOrder(
      subaccount,
      clientId,
      clobPairId,
      side,
      quantums,
      subticks,
      timeInForce,
      orderFlags,
      reduceOnly,
      10,
    );

    const replacementTx = await client.post.placeOrder(
      subaccount,
      clientId,
      clobPairId,
      side,
      quantums,
      subticks,
      timeInForce,
      orderFlags,
      reduceOnly,
      11,
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py import Subaccount, ORDER_FLAGS_SHORT_TERM
    from dydxpy.proto.dydxprotocol.clob.order_pb2 import Order

    subaccount = new Subaccount(wallet, 0)
    client_id = 123 # set to a number, can be used by the client to identify the order
    clobpair_id = 0 # perpertual market id
    side = Order.SIDE_BUY # side of the order
    quantums = 1_000_000_000 # quantums are calculated by the size if the order
    subticks = 1_000_000_000 # subticks are calculated by the price of the order
    time_in_force = Order.TIME_IN_FORCE_UNSPECIFIED # TimeInForce indicates how long an order will remain active before it is executed or expires
    order_flags = ORDER_FLAGS_SHORT_TERM # either SHORT_TERM, LONG_TERM or CONDITIONAL
    reduce_only = false # if true, the order will only reduce the position size

    tx = client.post.place_order(
      subaccount,
      client_id,
      clobpair_id,
      side,
      quantums,
      subticks,
      time_in_force,
      order_flags,
      reduce_only,
      10,
    )

    replacementTx = client.post.place_order(
      subaccount,
      client_id,
      clobpair_id,
      side,
      quantums,
      subticks,
      time_in_force,
      order_flags,
      reduce_only,
      11,
    )
    ```
  </Tab>
</Tabs>

As of February 23rd, 2024, Typescript client source code for the above function is [here](https://github.com/dydxprotocol/v4-clients/blob/fd1e9d33913ef27a4c761cd66bca93057fe707e2/v4-client-js/src/clients/modules/post.ts#L353-L369),
and Python client source code for the above function is [here](https://github.com/dydxprotocol/v4-clients/blob/fd1e9d33913ef27a4c761cd66bca93057fe707e2/v4-client-py/v4_client_py/clients/modules/post.py#L62-L79).

### Cancelling an Order

All paramsters are from *<a id="orderObject" href="/integration_docs/indexer_api#getorder">Order</a>* object from indexer
goodTilBlockTime is the UTC epoch second of the order's goodTilBlockTime
One and only one of goodTilBlock and goodTilBlockTime should be passed in as a parameter

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    /*
    order is an Order object from the Indexer
    */
    const goodTilBlock = order.goodTilBlock
    let goodTilBlockTime: number | undefined;
    if (order.goodTilBlockTime) {
      const datetime = new Date(order.goodTilBlockTime);
      const utcMilllisecondsSinceEpoch = datetime.getTime()
      goodTilBlockTime = Math.round(utcMilllisecondsSinceEpoch / 1000);
    }

    const tx = await client.post.cancelOrder(
      subaccount,
      order.clientId,
      order.orderFlags,
      order.clobPairId,
      goodTilBlock,
      goodTilBlockTime
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    # order is an Order object from the Indexer

    import datetime

    goodTilBlock = order["goodTilBlock"]
    goodTilBlockTime = None
    if order["goodTilBlockTime"] != None:
      mydatatime = datetime.datetime.fromisoformat(order["goodTilBlockTime"].replace('Z', '+00:00'))
      goodTilBlockTime = int(mydatatime.timestamp())

    tx = client.post.cancel_order(
        subaccount,
        order["clientId"],
        order["orderFlags"],
        order["clobPairId"],
        goodTilBlock,
        goodTilBlockTime,
    )
    ```
  </Tab>
</Tabs>

import { Tab, Tabs } from "nextra-theme-docs";

# Composite Client

## Getting Started

CompositeClient simplifies the transactions by transforming human readable parameters to chain-specific parameters.

For example, Placing an order with ValidatorClient requires parameters such as quantums and subticks, which are calculated based on ClobPair settings.

CompositeClient would take regular human readable parameters such as price and size, 
make the neccesory calculations and conversions, 
and call ValidatorClient with the correct quantums and subticks for you.

### Installation

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```bash copy
    pnpm install @dydxprotocol/v4-client-js 
    ```
  </Tab>
  <Tab>
    ```bash copy
    pip install v4-client-py 
    ```
  </Tab>
</Tabs>

## Initializing the Client

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import { CompositeClient, Network } from "@dydxprotocol/v4-client-js";

    /**
    // For the deployment by DYDX token holders, use below:

    import { IndexerConfig, ValidatorConfig } from "@dydxprotocol/v4-client-js";

    const NETWORK: Network = new Network(
      'mainnet',
      new IndexerConfig(
        'https://indexer.dydx.trade',
        'wss://indexer.dydx.trade',
      ),
      new ValidatorConfig(
        'https://dydx-ops-rpc.kingnodes.com', // or other node URL
        'dydx-mainnet-1',
        {
          CHAINTOKEN_DENOM: 'adydx',
          CHAINTOKEN_DECIMALS: 18,
          USDC_DENOM: 'ibc/8E27BA2D5493AF5636760E354E46004562C46AB7EC0CC4C1CA14E9E20E2545B5',
          USDC_GAS_DENOM: 'uusdc',
          USDC_DECIMALS: 6,
        },
      ),
    );
    */
    const NETWORK = Network.testnet();

    const client = await CompositeClient.connect(NETWORK);
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py.clients import CompositeClient
    from v4_client_py.clients.constants import Network

    """
    # For the deployment by DYDX token holders, use below:

    from v4_client_py.clients.constants import ValidatorConfig, IndexerConfig
    NETWORK=Network(
        env='mainnet',
        validator_config=ValidatorConfig(
            grpc_endpoint='https://dydx-ops-rpc.kingnodes.com', # or other node URL
            chain_id='dydx-mainnet-1', 
            ssl_enabled=True
        ),
        indexer_config=IndexerConfig(
            rest_endpoint='https://indexer.dydx.trade',
            websocket_endpoint='wss://indexer.dydx.trade',
        ),
        faucet_endpoint='',
    )
    """
    NETWORK = Network.testnet()

    client = CompositeClient(
        NETWORK,
    )
    ```
  </Tab>
</Tabs>

## Creating a LocalWallet

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import {
      BECH32_PREFIX,
      LocalWallet,
    } from '@dydxprotocol/v4-client-js';

    const mnemonic = 'YOUR MNEMONIC HERE';
    const wallet = await LocalWallet.fromMnemonic(mnemonic, BECH32_PREFIX);
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py.chain.aerial.wallet import LocalWallet, BECH32_PREFIX
    mnemonic = 'YOUR MNEMONIC HERE'
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX);
    ```
  </Tab>
</Tabs>

## Placing Orders 

### Placing an Order 

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    import {
      OrderExecution, OrderSide, OrderTimeInForce, OrderType,
    } from '@dydxprotocol/v4-client-js';
    const subaccount = new SubaccountClient(wallet, 0);
    const clientId = 123; // set to a number, can be used by the client to identify the order
    const market = "BTC-USD"; // perpertual market id
    const type = OrderType.LIMIT; // order type
    const side = OrderSide.BUY; // side of the order
    const timeInForce = OrderTimeInForce.IOC; // UX TimeInForce
    const execution = OrderExecution.DEFAULT;
    const price = 30_000; // price of 30,000;
    const size = 0.1; // subticks are calculated by the price of the order
    const postOnly = false; // If true, order is post only
    const reduceOnly = false; // if true, the order will only reduce the position size
    const triggerPrice = null; // required for conditional orders

    const tx = await client.placeOrder(
      subaccount,
      market,
      type,
      side,
      price,
      size,
      clientId,
      timeInForce,
      0,
      execution,
      postOnly,
      reduceOnly,
      triggerPrice
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    from v4_client_py import {
      OrderExecution, OrderSide, OrderTimeInForce, OrderType
    }

    subaccount = SubaccountClient(wallet, 0)
    clientId = 123 # set to a number, can be used by the client to identify the order
    market = "BTC-USD" # perpertual market id
    type = OrderType.LIMIT # order type
    side = OrderSide.BUY # side of the order
    timeInForce = OrderTimeInForce.IOC # UX TimeInForce
    execution = OrderExecution.DEFAULT
    price = 30_000 # price of 30,000;
    size = 0.1 # subticks are calculated by the price of the order
    postOnly = False # If true, order is post only
    reduceOnly = False # if true, the order will only reduce the position size
    triggerPrice = None # required for conditional orders
    tx = client.place_order(
      subaccount,
      market,
      type,
      side,
      price,
      size,
      clientId,
      timeInForce,
      0,
      execution,
      postOnly,
      reduceOnly,
      triggerPrice
    );
    ```
  </Tab>
</Tabs>

#### Replacing an Order

For more details on order replacements, see [Replacing an Order](./validator_client.mdx#replacing-an-order) in the validator client section.

### Canceling an Order

<Tabs items={["TypeScript", "Python"]}>
  <Tab>
    ```typescript copy
    /*
    order is an Order object from the Indexer
    */
    const tx = await client.cancelOrder(
      subaccount,
      order.clientId,
      order.orderFlags,
      order.clobPairId,
      order.goodTilBlock,
      order.goodTilBlockTime
    );
    ```
  </Tab>
  <Tab>
    ```python copy
    # order is an Order object from the Indexer

    tx = client.cancel_order(
        subaccount,
        order["clientId"],
        order["orderFlags"],
        order["clobPairId"],
        order["goodTilBlock"],
        order["goodTilBlockTime"],
    )
    ```
  </Tab>
</Tabs>

# Indexer API v1.0.0

> Scroll down for code samples, example requests and responses. Select a language for code samples from the tabs above or the mobile navigation menu.

Base URLs:

* For **the deployment by DYDX token holders**, use <a href="https://indexer.dydx.trade/v4">https://indexer.dydx.trade/v4</a>
* For **Testnet**, use <a href="https://dydx-testnet.imperator.co/v4">https://dydx-testnet.imperator.co/v4</a>

Note: Messages on Indexer WebSocket feeds are typically more recent than data fetched via Indexer's REST API, because the latter is backed by read replicas of the databases that feed the former. Ordinarily this difference is minimal (less than a second), but it might become prolonged under load. Please see [Indexer Architecture](https://dydx.exchange/blog/v4-deep-dive-indexer) for more information.

# Authentication

# Default

## GetAddress

<a id="opIdGetAddress"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/addresses/{address}', headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/addresses/${address}`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /addresses/{address}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "subaccounts": [
    {
      "address": "string",
      "subaccountNumber": 0,
      "equity": "string",
      "freeCollateral": "string",
      "openPerpetualPositions": {
        "property1": {
          "market": "string",
          "status": "OPEN",
          "side": "LONG",
          "size": "string",
          "maxSize": "string",
          "entryPrice": "string",
          "realizedPnl": "string",
          "createdAt": "string",
          "createdAtHeight": "string",
          "sumOpen": "string",
          "sumClose": "string",
          "netFunding": "string",
          "unrealizedPnl": "string",
          "closedAt": null,
          "exitPrice": "string"
        },
        "property2": {
          "market": "string",
          "status": "OPEN",
          "side": "LONG",
          "size": "string",
          "maxSize": "string",
          "entryPrice": "string",
          "realizedPnl": "string",
          "createdAt": "string",
          "createdAtHeight": "string",
          "sumOpen": "string",
          "sumClose": "string",
          "netFunding": "string",
          "unrealizedPnl": "string",
          "closedAt": null,
          "exitPrice": "string"
        }
      },
      "assetPositions": {
        "property1": {
          "symbol": "string",
          "side": "LONG",
          "size": "string",
          "assetId": "string"
        },
        "property2": {
          "symbol": "string",
          "side": "LONG",
          "size": "string",
          "assetId": "string"
        }
      },
      "marginEnabled": true
    }
  ],
  "totalTradingRewards": "string"
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[AddressResponse](#schemaaddressresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetSubaccount

<a id="opIdGetSubaccount"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/addresses/{address}/subaccountNumber/{subaccountNumber}', headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/addresses/${address}/subaccountNumber/${subaccountNumber}`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /addresses/{address}/subaccountNumber/{subaccountNumber}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|path|string|true|none|
|subaccountNumber|path|number(double)|true|none|

> Example responses

> 200 Response

```json
{
  "address": "string",
  "subaccountNumber": 0,
  "equity": "string",
  "freeCollateral": "string",
  "openPerpetualPositions": {
    "property1": {
      "market": "string",
      "status": "OPEN",
      "side": "LONG",
      "size": "string",
      "maxSize": "string",
      "entryPrice": "string",
      "realizedPnl": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "sumOpen": "string",
      "sumClose": "string",
      "netFunding": "string",
      "unrealizedPnl": "string",
      "closedAt": "string",
      "exitPrice": "string"
    },
    "property2": {
      "market": "string",
      "status": "OPEN",
      "side": "LONG",
      "size": "string",
      "maxSize": "string",
      "entryPrice": "string",
      "realizedPnl": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "sumOpen": "string",
      "sumClose": "string",
      "netFunding": "string",
      "unrealizedPnl": "string",
      "closedAt": "string",
      "exitPrice": "string"
    }
  },
  "assetPositions": {
    "property1": {
      "symbol": "string",
      "side": "LONG",
      "size": "string",
      "assetId": "string"
    },
    "property2": {
      "symbol": "string",
      "side": "LONG",
      "size": "string",
      "assetId": "string"
    }
  },
  "marginEnabled": true
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[SubaccountResponseObject](#schemasubaccountresponseobject)|

<aside class="success">
This operation does not require authentication
</aside>

## GetAssetPositions

<a id="opIdGetAssetPositions"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/assetPositions', params={
  'address': 'string',  'subaccountNumber': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/assetPositions?address=${address}&subaccountNumber=${subaccountNumber}`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /assetPositions`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|
|subaccountNumber|query|number(double)|true|none|

> Example responses

> 200 Response

```json
{
  "positions": [
    {
      "symbol": "string",
      "side": "LONG",
      "size": "string",
      "assetId": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[AssetPositionResponse](#schemaassetpositionresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetCandles

<a id="opIdGetCandles"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/candles/perpetualMarkets/{ticker}', params={
  'resolution': '1MIN',  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/candles/perpetualMarkets/{ticker}?resolution=1MIN&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /candles/perpetualMarkets/{ticker}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ticker|path|string|true|none|
|resolution|query|[CandleResolution](#schemacandleresolution)|true|none|
|limit|query|number(double)|true|none|
|fromISO|query|string|false|none|
|toISO|query|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|resolution|1MIN|
|resolution|5MINS|
|resolution|15MINS|
|resolution|30MINS|
|resolution|1HOUR|
|resolution|4HOURS|
|resolution|1DAY|

> Example responses

> 200 Response

```json
{
  "candles": [
    {
      "startedAt": "string",
      "ticker": "string",
      "resolution": "1MIN",
      "low": "string",
      "high": "string",
      "open": "string",
      "close": "string",
      "baseTokenVolume": "string",
      "usdVolume": "string",
      "trades": 0,
      "startingOpenInterest": "string",
      "id": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[CandleResponse](#schemacandleresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## Screen

<a id="opIdScreen"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/screen', params={
  'address': 'string'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/screen?address=string`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /screen`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|

> Example responses

> 200 Response

```json
{
  "restricted": true,
  "reason": "string"
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[ComplianceResponse](#schemacomplianceresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetFills

<a id="opIdGetFills"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/fills', params={
  'address': 'string',  'subaccountNumber': '0',  'market': 'string',  'marketType': 'PERPETUAL',  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/fills?address=string&subaccountNumber=0&market=string&marketType=PERPETUAL&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /fills`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|
|subaccountNumber|query|number(double)|true|none|
|market|query|string|false|none|
|marketType|query|[MarketType](#schemamarkettype)|false|none|
|limit|query|number(double)|false|none|
|createdBeforeOrAtHeight|query|number(double)|false|none|
|createdBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|marketType|PERPETUAL|
|marketType|SPOT|

> Example responses

> 200 Response

```json
{
  "fills": [
    {
      "id": "string",
      "side": "BUY",
      "liquidity": "TAKER",
      "type": "LIMIT",
      "market": "string",
      "marketType": "PERPETUAL",
      "price": "string",
      "size": "string",
      "fee": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "orderId": "string",
      "clientMetadata": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[FillResponse](#schemafillresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetHeight

<a id="opIdGetHeight"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/height', headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/height`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /height`

> Example responses

> 200 Response

```json
{
  "height": "string",
  "time": "string"
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[HeightResponse](#schemaheightresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetTradingRewards

<a id="opIdGetTradingRewards"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/historicalBlockTradingRewards/{address}', params={
  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/historicalBlockTradingRewards/{address}?limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /historicalBlockTradingRewards/{address}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|path|string|true|none|
|limit|query|number(double)|true|none|
|startingBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|
|startingBeforeOrAtHeight|query|string|false|none|

> Example responses

> 200 Response

```json
{
  "rewards": [
    {
      "tradingReward": "string",
      "createdAt": "string",
      "createdAtHeight": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[HistoricalBlockTradingRewardsResponse](#schemahistoricalblocktradingrewardsresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetHistoricalFunding

<a id="opIdGetHistoricalFunding"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/historicalFunding/{ticker}', params={
  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/historicalFunding/{ticker}?limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /historicalFunding/{ticker}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ticker|path|string|true|none|
|limit|query|number(double)|true|none|
|effectiveBeforeOrAtHeight|query|number(double)|false|none|
|effectiveBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|

> Example responses

> 200 Response

```json
{
  "historicalFunding": [
    {
      "ticker": "string",
      "rate": "string",
      "price": "string",
      "effectiveAt": "string",
      "effectiveAtHeight": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[HistoricalFundingResponse](#schemahistoricalfundingresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetHistoricalPnl

<a id="opIdGetHistoricalPnl"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/historical-pnl', params={
  'address': 'string',  'subaccountNumber': '0',  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/historical-pnl?address=string&subaccountNumber=0&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /historical-pnl`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|
|subaccountNumber|query|number(double)|true|none|
|limit|query|number(double)|true|none|
|createdBeforeOrAtHeight|query|number(double)|false|none|
|createdBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|
|createdOnOrAfterHeight|query|number(double)|false|none|
|createdOnOrAfter|query|[IsoString](#schemaisostring)|false|none|

> Example responses

> 200 Response

```json
{
  "historicalPnl": [
    {
      "id": "string",
      "subaccountId": "string",
      "equity": "string",
      "totalPnl": "string",
      "netTransfers": "string",
      "createdAt": "string",
      "blockHeight": "string",
      "blockTime": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[HistoricalPnlResponse](#schemahistoricalpnlresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetAggregations

<a id="opIdGetAggregations"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/historicalTradingRewardAggregations/{address}', params={
  'period': 'DAILY',  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/historicalTradingRewardAggregations/{address}?period=DAILY&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /historicalTradingRewardAggregations/{address}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|path|string|true|none|
|period|query|[TradingRewardAggregationPeriod](#schematradingrewardaggregationperiod)|true|none|
|limit|query|number(double)|true|none|
|startingBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|
|startingBeforeOrAtHeight|query|string|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|period|DAILY|
|period|WEEKLY|
|period|MONTHLY|

> Example responses

> 200 Response

```json
{
  "rewards": [
    {
      "tradingReward": "string",
      "startedAt": "string",
      "startedAtHeight": "string",
      "endedAt": "string",
      "endedAtHeight": "string",
      "period": "DAILY"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[HistoricalTradingRewardAggregationsResponse](#schemahistoricaltradingrewardaggregationsresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetPerpetualMarket

<a id="opIdGetPerpetualMarket"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/orderbooks/perpetualMarket/{ticker}', headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/orderbooks/perpetualMarket/{ticker}`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /orderbooks/perpetualMarket/{ticker}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ticker|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "bids": [
    {
      "price": "string",
      "size": "string"
    }
  ],
  "asks": [
    {
      "price": "string",
      "size": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[OrderbookResponseObject](#schemaorderbookresponseobject)|

<aside class="success">
This operation does not require authentication
</aside>

## ListOrders

<a id="opIdListOrders"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/orders', params={
  'address': 'string',  'subaccountNumber': '0',  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/orders?address=string&subaccountNumber=0&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /orders`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|
|subaccountNumber|query|number(double)|true|none|
|limit|query|number(double)|true|none|
|ticker|query|string|false|none|
|side|query|[OrderSide](#schemaorderside)|false|none|
|type|query|[OrderType](#schemaordertype)|false|none|
|status|query|array[any]|false|none|
|goodTilBlockBeforeOrAt|query|number(double)|false|none|
|goodTilBlockTimeBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|
|returnLatestOrders|query|boolean|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|side|BUY|
|side|SELL|
|type|LIMIT|
|type|MARKET|
|type|STOP_LIMIT|
|type|STOP_MARKET|
|type|TRAILING_STOP|
|type|TAKE_PROFIT|
|type|TAKE_PROFIT_MARKET|

> Example responses

> 200 Response

```json
[
  {
    "id": "string",
    "subaccountId": "string",
    "clientId": "string",
    "clobPairId": "string",
    "side": "BUY",
    "size": "string",
    "totalFilled": "string",
    "price": "string",
    "type": "LIMIT",
    "reduceOnly": true,
    "orderFlags": "string",
    "goodTilBlock": "string",
    "goodTilBlockTime": "string",
    "createdAtHeight": "string",
    "clientMetadata": "string",
    "triggerPrice": "string",
    "timeInForce": "GTT",
    "status": "OPEN",
    "postOnly": true,
    "ticker": "string",
    "updatedAt": "string",
    "updatedAtHeight": "string"
  }
]
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|Inline|

### Response Schema

Status Code **200**

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[[OrderResponseObject](#schemaorderresponseobject)]|false|none|none|
|» id|string|true|none|none|
|» subaccountId|string|true|none|none|
|» clientId|string|true|none|none|
|» clobPairId|string|true|none|none|
|» side|[OrderSide](#schemaorderside)|true|none|none|
|» size|string|true|none|none|
|» totalFilled|string|true|none|none|
|» price|string|true|none|none|
|» type|[OrderType](#schemaordertype)|true|none|none|
|» reduceOnly|boolean|true|none|none|
|» orderFlags|string|true|none|none|
|» goodTilBlock|string|false|none|none|
|» goodTilBlockTime|string|false|none|none|
|» createdAtHeight|string|false|none|none|
|» clientMetadata|string|true|none|none|
|» triggerPrice|string|false|none|none|
|» timeInForce|[APITimeInForce](#schemaapitimeinforce)|true|none|none|
|» status|any|true|none|none|

*anyOf*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[OrderStatus](#schemaorderstatus)|false|none|none|

*or*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|»» *anonymous*|[BestEffortOpenedStatus](#schemabesteffortopenedstatus)|false|none|none|

*continued*

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|» postOnly|boolean|true|none|none|
|» ticker|string|true|none|none|
|» updatedAt|[IsoString](#schemaisostring)|false|none|none|
|» updatedAtHeight|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|side|BUY|
|side|SELL|
|type|LIMIT|
|type|MARKET|
|type|STOP_LIMIT|
|type|STOP_MARKET|
|type|TRAILING_STOP|
|type|TAKE_PROFIT|
|type|TAKE_PROFIT_MARKET|
|timeInForce|GTT|
|timeInForce|FOK|
|timeInForce|IOC|
|*anonymous*|OPEN|
|*anonymous*|FILLED|
|*anonymous*|CANCELED|
|*anonymous*|BEST_EFFORT_CANCELED|
|*anonymous*|UNTRIGGERED|
|*anonymous*|BEST_EFFORT_OPENED|

<aside class="success">
This operation does not require authentication
</aside>

## GetOrder

<a id="opIdGetOrder"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/orders/{orderId}', headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/orders/{orderId}`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /orders/{orderId}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|orderId|path|string|true|none|

> Example responses

> 200 Response

```json
{
  "id": "string",
  "subaccountId": "string",
  "clientId": "string",
  "clobPairId": "string",
  "side": "BUY",
  "size": "string",
  "totalFilled": "string",
  "price": "string",
  "type": "LIMIT",
  "reduceOnly": true,
  "orderFlags": "string",
  "goodTilBlock": "string",
  "goodTilBlockTime": "string",
  "createdAtHeight": "string",
  "clientMetadata": "string",
  "triggerPrice": "string",
  "timeInForce": "GTT",
  "status": "OPEN",
  "postOnly": true,
  "ticker": "string",
  "updatedAt": "string",
  "updatedAtHeight": "string"
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[OrderResponseObject](#schemaorderresponseobject)|

<aside class="success">
This operation does not require authentication
</aside>

## ListPerpetualMarkets

<a id="opIdListPerpetualMarkets"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/perpetualMarkets', params={
  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/perpetualMarkets?limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /perpetualMarkets`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|limit|query|number(double)|true|none|
|ticker|query|string|false|none|

> Example responses

> 200 Response

```json
{
  "markets": {
    "property1": {
      "clobPairId": "string",
      "ticker": "string",
      "status": "ACTIVE",
      "oraclePrice": "string",
      "priceChange24H": "string",
      "volume24H": "string",
      "trades24H": 0,
      "nextFundingRate": "string",
      "initialMarginFraction": "string",
      "maintenanceMarginFraction": "string",
      "openInterest": "string",
      "atomicResolution": 0,
      "quantumConversionExponent": 0,
      "tickSize": "string",
      "stepSize": "string",
      "stepBaseQuantums": 0,
      "subticksPerTick": 0
    },
    "property2": {
      "clobPairId": "string",
      "ticker": "string",
      "status": "ACTIVE",
      "oraclePrice": "string",
      "priceChange24H": "string",
      "volume24H": "string",
      "trades24H": 0,
      "nextFundingRate": "string",
      "initialMarginFraction": "string",
      "maintenanceMarginFraction": "string",
      "openInterest": "string",
      "atomicResolution": 0,
      "quantumConversionExponent": 0,
      "tickSize": "string",
      "stepSize": "string",
      "stepBaseQuantums": 0,
      "subticksPerTick": 0
    }
  }
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[PerpetualMarketResponse](#schemaperpetualmarketresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## ListPositions

<a id="opIdListPositions"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/perpetualPositions', params={
  'address': 'string',  'subaccountNumber': '0',  'status': [
  "OPEN"
],  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/perpetualPositions?address=string&subaccountNumber=0&status=OPEN&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /perpetualPositions`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|
|subaccountNumber|query|number(double)|true|none|
|status|query|array[string]|true|none|
|limit|query|number(double)|true|none|
|createdBeforeOrAtHeight|query|number(double)|false|none|
|createdBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|status|OPEN|
|status|CLOSED|
|status|LIQUIDATED|

> Example responses

> 200 Response

```json
{
  "positions": [
    {
      "market": "string",
      "status": "OPEN",
      "side": "LONG",
      "size": "string",
      "maxSize": "string",
      "entryPrice": "string",
      "realizedPnl": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "sumOpen": "string",
      "sumClose": "string",
      "netFunding": "string",
      "unrealizedPnl": "string",
      "closedAt": "string",
      "exitPrice": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[PerpetualPositionResponse](#schemaperpetualpositionresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## Get

<a id="opIdGet"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/sparklines', params={
  'timePeriod': 'ONE_DAY'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/sparklines?timePeriod=ONE_DAY`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /sparklines`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|timePeriod|query|[SparklineTimePeriod](#schemasparklinetimeperiod)|true|none|

#### Enumerated Values

|Parameter|Value|
|---|---|
|timePeriod|ONE_DAY|
|timePeriod|SEVEN_DAYS|

> Example responses

> 200 Response

```json
{
  "property1": [
    "string"
  ],
  "property2": [
    "string"
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[SparklineResponseObject](#schemasparklineresponseobject)|

<aside class="success">
This operation does not require authentication
</aside>

## GetTime

<a id="opIdGetTime"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/time', headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/time`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /time`

> Example responses

> 200 Response

```json
{
  "iso": "string",
  "epoch": 0
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[TimeResponse](#schematimeresponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetTrades

<a id="opIdGetTrades"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/trades/perpetualMarket/{ticker}', params={
  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/trades/perpetualMarket/{ticker}?limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /trades/perpetualMarket/{ticker}`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|ticker|path|string|true|none|
|limit|query|number(double)|true|none|
|createdBeforeOrAtHeight|query|number(double)|false|none|
|createdBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|

> Example responses

> 200 Response

```json
{
  "trades": [
    {
      "id": "string",
      "side": "BUY",
      "size": "string",
      "price": "string",
      "type": "LIMIT",
      "createdAt": "string",
      "createdAtHeight": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[TradeResponse](#schematraderesponse)|

<aside class="success">
This operation does not require authentication
</aside>

## GetTransfers

<a id="opIdGetTransfers"></a>

> Code samples

```python
import requests
headers = {
  'Accept': 'application/json'
}

# For the deployment by DYDX token holders, use
# baseURL = 'https://indexer.dydx.trade/v4'
baseURL = 'https://dydx-testnet.imperator.co/v4'

r = requests.get(f'{baseURL}/transfers', params={
  'address': 'string',  'subaccountNumber': '0',  'limit': '0'
}, headers = headers)

print(r.json())

```

```javascript

const headers = {
  'Accept':'application/json'
};

// For the deployment by DYDX token holders, use
// baseURL = 'https://indexer.dydx.trade/v4'
const baseURL = 'https://dydx-testnet.imperator.co/v4'

fetch(`${baseURL}/transfers?address=string&subaccountNumber=0&limit=0`,
{
  method: 'GET',

  headers: headers
})
.then(function(res) {
    return res.json();
}).then(function(body) {
    console.log(body);
});

```

`GET /transfers`

### Parameters

|Name|In|Type|Required|Description|
|---|---|---|---|---|
|address|query|string|true|none|
|subaccountNumber|query|number(double)|true|none|
|limit|query|number(double)|true|none|
|createdBeforeOrAtHeight|query|number(double)|false|none|
|createdBeforeOrAt|query|[IsoString](#schemaisostring)|false|none|

> Example responses

> 200 Response

```json
{
  "transfers": [
    {
      "id": "string",
      "sender": {
        "subaccountNumber": 0,
        "address": "string"
      },
      "recipient": {
        "subaccountNumber": 0,
        "address": "string"
      },
      "size": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "symbol": "string",
      "type": "TRANSFER_IN",
      "transactionHash": "string"
    }
  ]
}
```

### Responses

|Status|Meaning|Description|Schema|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|Ok|[TransferResponse](#schematransferresponse)|

<aside class="success">
This operation does not require authentication
</aside>

# Schemas

## PerpetualPositionStatus

<a id="schemaperpetualpositionstatus"></a>
<a id="schema_PerpetualPositionStatus"></a>
<a id="tocSperpetualpositionstatus"></a>
<a id="tocsperpetualpositionstatus"></a>

```json
"OPEN"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|OPEN|
|*anonymous*|CLOSED|
|*anonymous*|LIQUIDATED|

## PositionSide

<a id="schemapositionside"></a>
<a id="schema_PositionSide"></a>
<a id="tocSpositionside"></a>
<a id="tocspositionside"></a>

```json
"LONG"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|LONG|
|*anonymous*|SHORT|

## IsoString

<a id="schemaisostring"></a>
<a id="schema_IsoString"></a>
<a id="tocSisostring"></a>
<a id="tocsisostring"></a>

```json
"string"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

## PerpetualPositionResponseObject

<a id="schemaperpetualpositionresponseobject"></a>
<a id="schema_PerpetualPositionResponseObject"></a>
<a id="tocSperpetualpositionresponseobject"></a>
<a id="tocsperpetualpositionresponseobject"></a>

```json
{
  "market": "string",
  "status": "OPEN",
  "side": "LONG",
  "size": "string",
  "maxSize": "string",
  "entryPrice": "string",
  "realizedPnl": "string",
  "createdAt": "string",
  "createdAtHeight": "string",
  "sumOpen": "string",
  "sumClose": "string",
  "netFunding": "string",
  "unrealizedPnl": "string",
  "closedAt": "string",
  "exitPrice": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|market|string|true|none|none|
|status|[PerpetualPositionStatus](#schemaperpetualpositionstatus)|true|none|none|
|side|[PositionSide](#schemapositionside)|true|none|none|
|size|string|true|none|none|
|maxSize|string|true|none|none|
|entryPrice|string|true|none|none|
|realizedPnl|string|true|none|none|
|createdAt|[IsoString](#schemaisostring)|true|none|none|
|createdAtHeight|string|true|none|none|
|sumOpen|string|true|none|none|
|sumClose|string|true|none|none|
|netFunding|string|true|none|none|
|unrealizedPnl|string|true|none|none|
|closedAt|[IsoString](#schemaisostring)¦null|false|none|none|
|exitPrice|string¦null|false|none|none|

## PerpetualPositionsMap

<a id="schemaperpetualpositionsmap"></a>
<a id="schema_PerpetualPositionsMap"></a>
<a id="tocSperpetualpositionsmap"></a>
<a id="tocsperpetualpositionsmap"></a>

```json
{
  "property1": {
    "market": "string",
    "status": "OPEN",
    "side": "LONG",
    "size": "string",
    "maxSize": "string",
    "entryPrice": "string",
    "realizedPnl": "string",
    "createdAt": "string",
    "createdAtHeight": "string",
    "sumOpen": "string",
    "sumClose": "string",
    "netFunding": "string",
    "unrealizedPnl": "string",
    "closedAt": "string",
    "exitPrice": "string"
  },
  "property2": {
    "market": "string",
    "status": "OPEN",
    "side": "LONG",
    "size": "string",
    "maxSize": "string",
    "entryPrice": "string",
    "realizedPnl": "string",
    "createdAt": "string",
    "createdAtHeight": "string",
    "sumOpen": "string",
    "sumClose": "string",
    "netFunding": "string",
    "unrealizedPnl": "string",
    "closedAt": "string",
    "exitPrice": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|[PerpetualPositionResponseObject](#schemaperpetualpositionresponseobject)|false|none|none|

## AssetPositionResponseObject

<a id="schemaassetpositionresponseobject"></a>
<a id="schema_AssetPositionResponseObject"></a>
<a id="tocSassetpositionresponseobject"></a>
<a id="tocsassetpositionresponseobject"></a>

```json
{
  "symbol": "string",
  "side": "LONG",
  "size": "string",
  "assetId": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|symbol|string|true|none|none|
|side|[PositionSide](#schemapositionside)|true|none|none|
|size|string|true|none|none|
|assetId|string|true|none|none|

## AssetPositionsMap

<a id="schemaassetpositionsmap"></a>
<a id="schema_AssetPositionsMap"></a>
<a id="tocSassetpositionsmap"></a>
<a id="tocsassetpositionsmap"></a>

```json
{
  "property1": {
    "symbol": "string",
    "side": "LONG",
    "size": "string",
    "assetId": "string"
  },
  "property2": {
    "symbol": "string",
    "side": "LONG",
    "size": "string",
    "assetId": "string"
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|[AssetPositionResponseObject](#schemaassetpositionresponseobject)|false|none|none|

## SubaccountResponseObject

<a id="schemasubaccountresponseobject"></a>
<a id="schema_SubaccountResponseObject"></a>
<a id="tocSsubaccountresponseobject"></a>
<a id="tocssubaccountresponseobject"></a>

```json
{
  "address": "string",
  "subaccountNumber": 0,
  "equity": "string",
  "freeCollateral": "string",
  "openPerpetualPositions": {
    "property1": {
      "market": "string",
      "status": "OPEN",
      "side": "LONG",
      "size": "string",
      "maxSize": "string",
      "entryPrice": "string",
      "realizedPnl": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "sumOpen": "string",
      "sumClose": "string",
      "netFunding": "string",
      "unrealizedPnl": "string",
      "closedAt": "string",
      "exitPrice": "string"
    },
    "property2": {
      "market": "string",
      "status": "OPEN",
      "side": "LONG",
      "size": "string",
      "maxSize": "string",
      "entryPrice": "string",
      "realizedPnl": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "sumOpen": "string",
      "sumClose": "string",
      "netFunding": "string",
      "unrealizedPnl": "string",
      "closedAt": "string",
      "exitPrice": "string"
    }
  },
  "assetPositions": {
    "property1": {
      "symbol": "string",
      "side": "LONG",
      "size": "string",
      "assetId": "string"
    },
    "property2": {
      "symbol": "string",
      "side": "LONG",
      "size": "string",
      "assetId": "string"
    }
  },
  "marginEnabled": true
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|address|string|true|none|none|
|subaccountNumber|number(double)|true|none|none|
|equity|string|true|none|none|
|freeCollateral|string|true|none|none|
|openPerpetualPositions|[PerpetualPositionsMap](#schemaperpetualpositionsmap)|true|none|none|
|assetPositions|[AssetPositionsMap](#schemaassetpositionsmap)|true|none|none|
|marginEnabled|boolean|true|none|none|

## AddressResponse

<a id="schemaaddressresponse"></a>
<a id="schema_AddressResponse"></a>
<a id="tocSaddressresponse"></a>
<a id="tocsaddressresponse"></a>

```json
{
  "subaccounts": [
    {
      "address": "string",
      "subaccountNumber": 0,
      "equity": "string",
      "freeCollateral": "string",
      "openPerpetualPositions": {
        "property1": {
          "market": "string",
          "status": "OPEN",
          "side": "LONG",
          "size": "string",
          "maxSize": "string",
          "entryPrice": "string",
          "realizedPnl": "string",
          "createdAt": "string",
          "createdAtHeight": "string",
          "sumOpen": "string",
          "sumClose": "string",
          "netFunding": "string",
          "unrealizedPnl": "string",
          "closedAt": null,
          "exitPrice": "string"
        },
        "property2": {
          "market": "string",
          "status": "OPEN",
          "side": "LONG",
          "size": "string",
          "maxSize": "string",
          "entryPrice": "string",
          "realizedPnl": "string",
          "createdAt": "string",
          "createdAtHeight": "string",
          "sumOpen": "string",
          "sumClose": "string",
          "netFunding": "string",
          "unrealizedPnl": "string",
          "closedAt": null,
          "exitPrice": "string"
        }
      },
      "assetPositions": {
        "property1": {
          "symbol": "string",
          "side": "LONG",
          "size": "string",
          "assetId": "string"
        },
        "property2": {
          "symbol": "string",
          "side": "LONG",
          "size": "string",
          "assetId": "string"
        }
      },
      "marginEnabled": true
    }
  ],
  "totalTradingRewards": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|subaccounts|[[SubaccountResponseObject](#schemasubaccountresponseobject)]|true|none|none|
|totalTradingRewards|string|true|none|none|

## AssetPositionResponse

<a id="schemaassetpositionresponse"></a>
<a id="schema_AssetPositionResponse"></a>
<a id="tocSassetpositionresponse"></a>
<a id="tocsassetpositionresponse"></a>

```json
{
  "positions": [
    {
      "symbol": "string",
      "side": "LONG",
      "size": "string",
      "assetId": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|positions|[[AssetPositionResponseObject](#schemaassetpositionresponseobject)]|true|none|none|

## CandleResolution

<a id="schemacandleresolution"></a>
<a id="schema_CandleResolution"></a>
<a id="tocScandleresolution"></a>
<a id="tocscandleresolution"></a>

```json
"1MIN"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|1MIN|
|*anonymous*|5MINS|
|*anonymous*|15MINS|
|*anonymous*|30MINS|
|*anonymous*|1HOUR|
|*anonymous*|4HOURS|
|*anonymous*|1DAY|

## CandleResponseObject

<a id="schemacandleresponseobject"></a>
<a id="schema_CandleResponseObject"></a>
<a id="tocScandleresponseobject"></a>
<a id="tocscandleresponseobject"></a>

```json
{
  "startedAt": "string",
  "ticker": "string",
  "resolution": "1MIN",
  "low": "string",
  "high": "string",
  "open": "string",
  "close": "string",
  "baseTokenVolume": "string",
  "usdVolume": "string",
  "trades": 0,
  "startingOpenInterest": "string",
  "id": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|startedAt|[IsoString](#schemaisostring)|true|none|none|
|ticker|string|true|none|none|
|resolution|[CandleResolution](#schemacandleresolution)|true|none|none|
|low|string|true|none|none|
|high|string|true|none|none|
|open|string|true|none|none|
|close|string|true|none|none|
|baseTokenVolume|string|true|none|none|
|usdVolume|string|true|none|none|
|trades|number(double)|true|none|none|
|startingOpenInterest|string|true|none|none|
|id|string|true|none|none|

## CandleResponse

<a id="schemacandleresponse"></a>
<a id="schema_CandleResponse"></a>
<a id="tocScandleresponse"></a>
<a id="tocscandleresponse"></a>

```json
{
  "candles": [
    {
      "startedAt": "string",
      "ticker": "string",
      "resolution": "1MIN",
      "low": "string",
      "high": "string",
      "open": "string",
      "close": "string",
      "baseTokenVolume": "string",
      "usdVolume": "string",
      "trades": 0,
      "startingOpenInterest": "string",
      "id": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|candles|[[CandleResponseObject](#schemacandleresponseobject)]|true|none|none|

## ComplianceResponse

<a id="schemacomplianceresponse"></a>
<a id="schema_ComplianceResponse"></a>
<a id="tocScomplianceresponse"></a>
<a id="tocscomplianceresponse"></a>

```json
{
  "restricted": true,
  "reason": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|restricted|boolean|true|none|none|
|reason|string|false|none|none|

## OrderSide

<a id="schemaorderside"></a>
<a id="schema_OrderSide"></a>
<a id="tocSorderside"></a>
<a id="tocsorderside"></a>

```json
"BUY"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|BUY|
|*anonymous*|SELL|

## Liquidity

<a id="schemaliquidity"></a>
<a id="schema_Liquidity"></a>
<a id="tocSliquidity"></a>
<a id="tocsliquidity"></a>

```json
"TAKER"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|TAKER|
|*anonymous*|MAKER|

## FillType

<a id="schemafilltype"></a>
<a id="schema_FillType"></a>
<a id="tocSfilltype"></a>
<a id="tocsfilltype"></a>

```json
"LIMIT"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|LIMIT|
|*anonymous*|LIQUIDATED|
|*anonymous*|LIQUIDATION|
|*anonymous*|DELEVERAGED|
|*anonymous*|OFFSETTING|

## MarketType

<a id="schemamarkettype"></a>
<a id="schema_MarketType"></a>
<a id="tocSmarkettype"></a>
<a id="tocsmarkettype"></a>

```json
"PERPETUAL"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|PERPETUAL|
|*anonymous*|SPOT|

## FillResponseObject

<a id="schemafillresponseobject"></a>
<a id="schema_FillResponseObject"></a>
<a id="tocSfillresponseobject"></a>
<a id="tocsfillresponseobject"></a>

```json
{
  "id": "string",
  "side": "BUY",
  "liquidity": "TAKER",
  "type": "LIMIT",
  "market": "string",
  "marketType": "PERPETUAL",
  "price": "string",
  "size": "string",
  "fee": "string",
  "createdAt": "string",
  "createdAtHeight": "string",
  "orderId": "string",
  "clientMetadata": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|side|[OrderSide](#schemaorderside)|true|none|none|
|liquidity|[Liquidity](#schemaliquidity)|true|none|none|
|type|[FillType](#schemafilltype)|true|none|none|
|market|string|true|none|none|
|marketType|[MarketType](#schemamarkettype)|true|none|none|
|price|string|true|none|none|
|size|string|true|none|none|
|fee|string|true|none|none|
|createdAt|[IsoString](#schemaisostring)|true|none|none|
|createdAtHeight|string|true|none|none|
|orderId|string|false|none|none|
|clientMetadata|string|false|none|none|

## FillResponse

<a id="schemafillresponse"></a>
<a id="schema_FillResponse"></a>
<a id="tocSfillresponse"></a>
<a id="tocsfillresponse"></a>

```json
{
  "fills": [
    {
      "id": "string",
      "side": "BUY",
      "liquidity": "TAKER",
      "type": "LIMIT",
      "market": "string",
      "marketType": "PERPETUAL",
      "price": "string",
      "size": "string",
      "fee": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "orderId": "string",
      "clientMetadata": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|fills|[[FillResponseObject](#schemafillresponseobject)]|true|none|none|

## HeightResponse

<a id="schemaheightresponse"></a>
<a id="schema_HeightResponse"></a>
<a id="tocSheightresponse"></a>
<a id="tocsheightresponse"></a>

```json
{
  "height": "string",
  "time": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|height|string|true|none|none|
|time|[IsoString](#schemaisostring)|true|none|none|

## HistoricalBlockTradingReward

<a id="schemahistoricalblocktradingreward"></a>
<a id="schema_HistoricalBlockTradingReward"></a>
<a id="tocShistoricalblocktradingreward"></a>
<a id="tocshistoricalblocktradingreward"></a>

```json
{
  "tradingReward": "string",
  "createdAt": "string",
  "createdAtHeight": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|tradingReward|string|true|none|none|
|createdAt|[IsoString](#schemaisostring)|true|none|none|
|createdAtHeight|string|true|none|none|

## HistoricalBlockTradingRewardsResponse

<a id="schemahistoricalblocktradingrewardsresponse"></a>
<a id="schema_HistoricalBlockTradingRewardsResponse"></a>
<a id="tocShistoricalblocktradingrewardsresponse"></a>
<a id="tocshistoricalblocktradingrewardsresponse"></a>

```json
{
  "rewards": [
    {
      "tradingReward": "string",
      "createdAt": "string",
      "createdAtHeight": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|rewards|[[HistoricalBlockTradingReward](#schemahistoricalblocktradingreward)]|true|none|none|

## HistoricalFundingResponseObject

<a id="schemahistoricalfundingresponseobject"></a>
<a id="schema_HistoricalFundingResponseObject"></a>
<a id="tocShistoricalfundingresponseobject"></a>
<a id="tocshistoricalfundingresponseobject"></a>

```json
{
  "ticker": "string",
  "rate": "string",
  "price": "string",
  "effectiveAt": "string",
  "effectiveAtHeight": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|ticker|string|true|none|none|
|rate|string|true|none|none|
|price|string|true|none|none|
|effectiveAt|[IsoString](#schemaisostring)|true|none|none|
|effectiveAtHeight|string|true|none|none|

## HistoricalFundingResponse

<a id="schemahistoricalfundingresponse"></a>
<a id="schema_HistoricalFundingResponse"></a>
<a id="tocShistoricalfundingresponse"></a>
<a id="tocshistoricalfundingresponse"></a>

```json
{
  "historicalFunding": [
    {
      "ticker": "string",
      "rate": "string",
      "price": "string",
      "effectiveAt": "string",
      "effectiveAtHeight": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|historicalFunding|[[HistoricalFundingResponseObject](#schemahistoricalfundingresponseobject)]|true|none|none|

## PnlTicksResponseObject

<a id="schemapnlticksresponseobject"></a>
<a id="schema_PnlTicksResponseObject"></a>
<a id="tocSpnlticksresponseobject"></a>
<a id="tocspnlticksresponseobject"></a>

```json
{
  "id": "string",
  "subaccountId": "string",
  "equity": "string",
  "totalPnl": "string",
  "netTransfers": "string",
  "createdAt": "string",
  "blockHeight": "string",
  "blockTime": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|subaccountId|string|true|none|none|
|equity|string|true|none|none|
|totalPnl|string|true|none|none|
|netTransfers|string|true|none|none|
|createdAt|string|true|none|none|
|blockHeight|string|true|none|none|
|blockTime|[IsoString](#schemaisostring)|true|none|none|

## HistoricalPnlResponse

<a id="schemahistoricalpnlresponse"></a>
<a id="schema_HistoricalPnlResponse"></a>
<a id="tocShistoricalpnlresponse"></a>
<a id="tocshistoricalpnlresponse"></a>

```json
{
  "historicalPnl": [
    {
      "id": "string",
      "subaccountId": "string",
      "equity": "string",
      "totalPnl": "string",
      "netTransfers": "string",
      "createdAt": "string",
      "blockHeight": "string",
      "blockTime": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|historicalPnl|[[PnlTicksResponseObject](#schemapnlticksresponseobject)]|true|none|none|

## TradingRewardAggregationPeriod

<a id="schematradingrewardaggregationperiod"></a>
<a id="schema_TradingRewardAggregationPeriod"></a>
<a id="tocStradingrewardaggregationperiod"></a>
<a id="tocstradingrewardaggregationperiod"></a>

```json
"DAILY"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|DAILY|
|*anonymous*|WEEKLY|
|*anonymous*|MONTHLY|

## HistoricalTradingRewardAggregation

<a id="schemahistoricaltradingrewardaggregation"></a>
<a id="schema_HistoricalTradingRewardAggregation"></a>
<a id="tocShistoricaltradingrewardaggregation"></a>
<a id="tocshistoricaltradingrewardaggregation"></a>

```json
{
  "tradingReward": "string",
  "startedAt": "string",
  "startedAtHeight": "string",
  "endedAt": "string",
  "endedAtHeight": "string",
  "period": "DAILY"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|tradingReward|string|true|none|none|
|startedAt|[IsoString](#schemaisostring)|true|none|none|
|startedAtHeight|string|true|none|none|
|endedAt|[IsoString](#schemaisostring)|false|none|none|
|endedAtHeight|string|false|none|none|
|period|[TradingRewardAggregationPeriod](#schematradingrewardaggregationperiod)|true|none|none|

## HistoricalTradingRewardAggregationsResponse

<a id="schemahistoricaltradingrewardaggregationsresponse"></a>
<a id="schema_HistoricalTradingRewardAggregationsResponse"></a>
<a id="tocShistoricaltradingrewardaggregationsresponse"></a>
<a id="tocshistoricaltradingrewardaggregationsresponse"></a>

```json
{
  "rewards": [
    {
      "tradingReward": "string",
      "startedAt": "string",
      "startedAtHeight": "string",
      "endedAt": "string",
      "endedAtHeight": "string",
      "period": "DAILY"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|rewards|[[HistoricalTradingRewardAggregation](#schemahistoricaltradingrewardaggregation)]|true|none|none|

## OrderbookResponsePriceLevel

<a id="schemaorderbookresponsepricelevel"></a>
<a id="schema_OrderbookResponsePriceLevel"></a>
<a id="tocSorderbookresponsepricelevel"></a>
<a id="tocsorderbookresponsepricelevel"></a>

```json
{
  "price": "string",
  "size": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|price|string|true|none|none|
|size|string|true|none|none|

## OrderbookResponseObject

<a id="schemaorderbookresponseobject"></a>
<a id="schema_OrderbookResponseObject"></a>
<a id="tocSorderbookresponseobject"></a>
<a id="tocsorderbookresponseobject"></a>

```json
{
  "bids": [
    {
      "price": "string",
      "size": "string"
    }
  ],
  "asks": [
    {
      "price": "string",
      "size": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|bids|[[OrderbookResponsePriceLevel](#schemaorderbookresponsepricelevel)]|true|none|none|
|asks|[[OrderbookResponsePriceLevel](#schemaorderbookresponsepricelevel)]|true|none|none|

## APITimeInForce

<a id="schemaapitimeinforce"></a>
<a id="schema_APITimeInForce"></a>
<a id="tocSapitimeinforce"></a>
<a id="tocsapitimeinforce"></a>

```json
"GTT"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|GTT|
|*anonymous*|FOK|
|*anonymous*|IOC|

## OrderStatus

<a id="schemaorderstatus"></a>
<a id="schema_OrderStatus"></a>
<a id="tocSorderstatus"></a>
<a id="tocsorderstatus"></a>

```json
"OPEN"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|OPEN|
|*anonymous*|FILLED|
|*anonymous*|CANCELED|
|*anonymous*|BEST_EFFORT_CANCELED|
|*anonymous*|UNTRIGGERED|

## BestEffortOpenedStatus

<a id="schemabesteffortopenedstatus"></a>
<a id="schema_BestEffortOpenedStatus"></a>
<a id="tocSbesteffortopenedstatus"></a>
<a id="tocsbesteffortopenedstatus"></a>

```json
"BEST_EFFORT_OPENED"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|BEST_EFFORT_OPENED|

## APIOrderStatus

<a id="schemaapiorderstatus"></a>
<a id="schema_APIOrderStatus"></a>
<a id="tocSapiorderstatus"></a>
<a id="tocsapiorderstatus"></a>

```json
"OPEN"

```

### Properties

anyOf

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[OrderStatus](#schemaorderstatus)|false|none|none|

or

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|[BestEffortOpenedStatus](#schemabesteffortopenedstatus)|false|none|none|

## OrderType

<a id="schemaordertype"></a>
<a id="schema_OrderType"></a>
<a id="tocSordertype"></a>
<a id="tocsordertype"></a>

```json
"LIMIT"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|LIMIT|
|*anonymous*|MARKET|
|*anonymous*|STOP_LIMIT|
|*anonymous*|STOP_MARKET|
|*anonymous*|TRAILING_STOP|
|*anonymous*|TAKE_PROFIT|
|*anonymous*|TAKE_PROFIT_MARKET|

## OrderResponseObject

<a id="schemaorderresponseobject"></a>
<a id="schema_OrderResponseObject"></a>
<a id="tocSorderresponseobject"></a>
<a id="tocsorderresponseobject"></a>

```json
{
  "id": "string",
  "subaccountId": "string",
  "clientId": "string",
  "clobPairId": "string",
  "side": "BUY",
  "size": "string",
  "totalFilled": "string",
  "price": "string",
  "type": "LIMIT",
  "reduceOnly": true,
  "orderFlags": "string",
  "goodTilBlock": "string",
  "goodTilBlockTime": "string",
  "createdAtHeight": "string",
  "clientMetadata": "string",
  "triggerPrice": "string",
  "timeInForce": "GTT",
  "status": "OPEN",
  "postOnly": true,
  "ticker": "string",
  "updatedAt": "string",
  "updatedAtHeight": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|subaccountId|string|true|none|none|
|clientId|string|true|none|none|
|clobPairId|string|true|none|none|
|side|[OrderSide](#schemaorderside)|true|none|none|
|size|string|true|none|none|
|totalFilled|string|true|none|none|
|price|string|true|none|none|
|type|[OrderType](#schemaordertype)|true|none|none|
|reduceOnly|boolean|true|none|none|
|orderFlags|string|true|none|none|
|goodTilBlock|string|false|none|none|
|goodTilBlockTime|string|false|none|none|
|createdAtHeight|string|false|none|none|
|clientMetadata|string|true|none|none|
|triggerPrice|string|false|none|none|
|timeInForce|[APITimeInForce](#schemaapitimeinforce)|true|none|none|
|status|[APIOrderStatus](#schemaapiorderstatus)|true|none|none|
|postOnly|boolean|true|none|none|
|ticker|string|true|none|none|
|updatedAt|[IsoString](#schemaisostring)|false|none|none|
|updatedAtHeight|string|false|none|none|

## PerpetualMarketStatus

<a id="schemaperpetualmarketstatus"></a>
<a id="schema_PerpetualMarketStatus"></a>
<a id="tocSperpetualmarketstatus"></a>
<a id="tocsperpetualmarketstatus"></a>

```json
"ACTIVE"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|ACTIVE|
|*anonymous*|PAUSED|
|*anonymous*|CANCEL_ONLY|
|*anonymous*|POST_ONLY|
|*anonymous*|INITIALIZING|
|*anonymous*|FINAL_SETTLEMENT|

## PerpetualMarketResponseObject

<a id="schemaperpetualmarketresponseobject"></a>
<a id="schema_PerpetualMarketResponseObject"></a>
<a id="tocSperpetualmarketresponseobject"></a>
<a id="tocsperpetualmarketresponseobject"></a>

```json
{
  "clobPairId": "string",
  "ticker": "string",
  "status": "ACTIVE",
  "oraclePrice": "string",
  "priceChange24H": "string",
  "volume24H": "string",
  "trades24H": 0,
  "nextFundingRate": "string",
  "initialMarginFraction": "string",
  "maintenanceMarginFraction": "string",
  "openInterest": "string",
  "atomicResolution": 0,
  "quantumConversionExponent": 0,
  "tickSize": "string",
  "stepSize": "string",
  "stepBaseQuantums": 0,
  "subticksPerTick": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|clobPairId|string|true|none|none|
|ticker|string|true|none|none|
|status|[PerpetualMarketStatus](#schemaperpetualmarketstatus)|true|none|none|
|oraclePrice|string|true|none|none|
|priceChange24H|string|true|none|none|
|volume24H|string|true|none|none|
|trades24H|number(double)|true|none|none|
|nextFundingRate|string|true|none|none|
|initialMarginFraction|string|true|none|none|
|maintenanceMarginFraction|string|true|none|none|
|openInterest|string|true|none|none|
|atomicResolution|number(double)|true|none|none|
|quantumConversionExponent|number(double)|true|none|none|
|tickSize|string|true|none|none|
|stepSize|string|true|none|none|
|stepBaseQuantums|number(double)|true|none|none|
|subticksPerTick|number(double)|true|none|none|

## PerpetualMarketResponse

<a id="schemaperpetualmarketresponse"></a>
<a id="schema_PerpetualMarketResponse"></a>
<a id="tocSperpetualmarketresponse"></a>
<a id="tocsperpetualmarketresponse"></a>

```json
{
  "markets": {
    "property1": {
      "clobPairId": "string",
      "ticker": "string",
      "status": "ACTIVE",
      "oraclePrice": "string",
      "priceChange24H": "string",
      "volume24H": "string",
      "trades24H": 0,
      "nextFundingRate": "string",
      "initialMarginFraction": "string",
      "maintenanceMarginFraction": "string",
      "openInterest": "string",
      "atomicResolution": 0,
      "quantumConversionExponent": 0,
      "tickSize": "string",
      "stepSize": "string",
      "stepBaseQuantums": 0,
      "subticksPerTick": 0
    },
    "property2": {
      "clobPairId": "string",
      "ticker": "string",
      "status": "ACTIVE",
      "oraclePrice": "string",
      "priceChange24H": "string",
      "volume24H": "string",
      "trades24H": 0,
      "nextFundingRate": "string",
      "initialMarginFraction": "string",
      "maintenanceMarginFraction": "string",
      "openInterest": "string",
      "atomicResolution": 0,
      "quantumConversionExponent": 0,
      "tickSize": "string",
      "stepSize": "string",
      "stepBaseQuantums": 0,
      "subticksPerTick": 0
    }
  }
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|markets|object|true|none|none|
|» **additionalProperties**|[PerpetualMarketResponseObject](#schemaperpetualmarketresponseobject)|false|none|none|

## PerpetualPositionResponse

<a id="schemaperpetualpositionresponse"></a>
<a id="schema_PerpetualPositionResponse"></a>
<a id="tocSperpetualpositionresponse"></a>
<a id="tocsperpetualpositionresponse"></a>

```json
{
  "positions": [
    {
      "market": "string",
      "status": "OPEN",
      "side": "LONG",
      "size": "string",
      "maxSize": "string",
      "entryPrice": "string",
      "realizedPnl": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "sumOpen": "string",
      "sumClose": "string",
      "netFunding": "string",
      "unrealizedPnl": "string",
      "closedAt": "string",
      "exitPrice": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|positions|[[PerpetualPositionResponseObject](#schemaperpetualpositionresponseobject)]|true|none|none|

## SparklineResponseObject

<a id="schemasparklineresponseobject"></a>
<a id="schema_SparklineResponseObject"></a>
<a id="tocSsparklineresponseobject"></a>
<a id="tocssparklineresponseobject"></a>

```json
{
  "property1": [
    "string"
  ],
  "property2": [
    "string"
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|**additionalProperties**|[string]|false|none|none|

## SparklineTimePeriod

<a id="schemasparklinetimeperiod"></a>
<a id="schema_SparklineTimePeriod"></a>
<a id="tocSsparklinetimeperiod"></a>
<a id="tocssparklinetimeperiod"></a>

```json
"ONE_DAY"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|ONE_DAY|
|*anonymous*|SEVEN_DAYS|

## TimeResponse

<a id="schematimeresponse"></a>
<a id="schema_TimeResponse"></a>
<a id="tocStimeresponse"></a>
<a id="tocstimeresponse"></a>

```json
{
  "iso": "string",
  "epoch": 0
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|iso|[IsoString](#schemaisostring)|true|none|none|
|epoch|number(double)|true|none|none|

## TradeType

<a id="schematradetype"></a>
<a id="schema_TradeType"></a>
<a id="tocStradetype"></a>
<a id="tocstradetype"></a>

```json
"LIMIT"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|LIMIT|
|*anonymous*|LIQUIDATED|
|*anonymous*|DELEVERAGED|

## TradeResponseObject

<a id="schematraderesponseobject"></a>
<a id="schema_TradeResponseObject"></a>
<a id="tocStraderesponseobject"></a>
<a id="tocstraderesponseobject"></a>

```json
{
  "id": "string",
  "side": "BUY",
  "size": "string",
  "price": "string",
  "type": "LIMIT",
  "createdAt": "string",
  "createdAtHeight": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|side|[OrderSide](#schemaorderside)|true|none|none|
|size|string|true|none|none|
|price|string|true|none|none|
|type|[TradeType](#schematradetype)|true|none|none|
|createdAt|[IsoString](#schemaisostring)|true|none|none|
|createdAtHeight|string|true|none|none|

## TradeResponse

<a id="schematraderesponse"></a>
<a id="schema_TradeResponse"></a>
<a id="tocStraderesponse"></a>
<a id="tocstraderesponse"></a>

```json
{
  "trades": [
    {
      "id": "string",
      "side": "BUY",
      "size": "string",
      "price": "string",
      "type": "LIMIT",
      "createdAt": "string",
      "createdAtHeight": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|trades|[[TradeResponseObject](#schematraderesponseobject)]|true|none|none|

## TransferType

<a id="schematransfertype"></a>
<a id="schema_TransferType"></a>
<a id="tocStransfertype"></a>
<a id="tocstransfertype"></a>

```json
"TRANSFER_IN"

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|*anonymous*|string|false|none|none|

#### Enumerated Values

|Property|Value|
|---|---|
|*anonymous*|TRANSFER_IN|
|*anonymous*|TRANSFER_OUT|
|*anonymous*|DEPOSIT|
|*anonymous*|WITHDRAWAL|

## TransferResponseObject

<a id="schematransferresponseobject"></a>
<a id="schema_TransferResponseObject"></a>
<a id="tocStransferresponseobject"></a>
<a id="tocstransferresponseobject"></a>

```json
{
  "id": "string",
  "sender": {
    "subaccountNumber": 0,
    "address": "string"
  },
  "recipient": {
    "subaccountNumber": 0,
    "address": "string"
  },
  "size": "string",
  "createdAt": "string",
  "createdAtHeight": "string",
  "symbol": "string",
  "type": "TRANSFER_IN",
  "transactionHash": "string"
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|id|string|true|none|none|
|sender|object|true|none|none|
|» subaccountNumber|number(double)|false|none|none|
|» address|string|true|none|none|
|recipient|object|true|none|none|
|» subaccountNumber|number(double)|false|none|none|
|» address|string|true|none|none|
|size|string|true|none|none|
|createdAt|string|true|none|none|
|createdAtHeight|string|true|none|none|
|symbol|string|true|none|none|
|type|[TransferType](#schematransfertype)|true|none|none|
|transactionHash|string|true|none|none|

## TransferResponse

<a id="schematransferresponse"></a>
<a id="schema_TransferResponse"></a>
<a id="tocStransferresponse"></a>
<a id="tocstransferresponse"></a>

```json
{
  "transfers": [
    {
      "id": "string",
      "sender": {
        "subaccountNumber": 0,
        "address": "string"
      },
      "recipient": {
        "subaccountNumber": 0,
        "address": "string"
      },
      "size": "string",
      "createdAt": "string",
      "createdAtHeight": "string",
      "symbol": "string",
      "type": "TRANSFER_IN",
      "transactionHash": "string"
    }
  ]
}

```

### Properties

|Name|Type|Required|Restrictions|Description|
|---|---|---|---|---|
|transfers|[[TransferResponseObject](#schematransferresponseobject)]|true|none|none|

# Indexer Websocket Documentation

dYdX offers a WebSocket API for streaming v4 updates.

* For **the deployment by DYDX token holders**, use `wss://indexer.dydx.trade/v4/ws`
* For **Testnet**, use `wss://indexer.v4testnet.dydx.exchange/v4/ws`

Note: Messages on Indexer WebSocket feeds are typically more recent than data fetched via Indexer's REST API, because the latter is backed by read replicas of the databases that feed the former. Ordinarily this difference is minimal (less than a second), but it might become prolonged under load. Please see [Indexer Architecture](https://dydx.exchange/blog/v4-deep-dive-indexer) for more information.

## Overall

### Connect

Upon connecting to v4 Websockets you will receive an initial connection message with the following format:

```tsx
{
  "type": "connected",
  "connection_id": "004a1efa-21bb-4b19-a2e9-a8ffadd6dc53",
  "message_id": 0
}
```

### Maintaining a Connection

Every 30 seconds, the websocket API will send a ping event to the connected client. If a pong event is not received within 10 seconds back, the websocket API will disconnect.

### Subscribe

You may subscribe to any channel following the subscribe instructions above. Subscribing to a channel has the following fields:

- `type` - Should always be specified to `subscribe`
- `channel` - Specifies the channel you are subscribing to. The specific string is specified in each channel’s documentation
- `id` - required for all channels other than market. Specifies the market or subaccount you are subscribing to.

### Rate Limiting

The default rate limiting config for websockets is:
- 2 subscriptions per (connection + channel + channel id) per second.
- 2 invalid messages per connection per second.

### Unsubscribe

Utilize the same message to subscribe but replace the type with `unsubscribe`. For example:
`{ "type": "unsubscribe", "channel": "v4_trades", "id": "BTC-USD" }`

### Example

Use a command-line websocket client such as [interactive-websocket-cli](https://www.npmjs.com/package/interactive-websocket-cli) to connect and subscribe to channels.

Example (with `interactive-websocket-cli`)

```tsx
# For the deployment by DYDX token holders, use
# wscli connect wss://indexer.dydx.trade/v4/ws
wscli connect wss://indexer.v4testnet.dydx.exchange/v4/ws
<output from ws-cli>
<type 's' to send> { "type": "subscribe", "channel": "v4_trades", "id": "BTC-USD" }
```

## Subaccounts

This channel provides realtime information about orders, fills, transfers, perpetual positions, and perpetual assets for a subaccount.

### Subscribe

| Field | Type | Description |
| --- | --- | --- |
| type | string | Set to subscribe |
| channel | string | Set to v4_subaccounts |
| id | string | Set to the address and subaccount number in the format {address}/{subaccount_number} |

### Initial Response

Returns everything from the `/v4/addresses/:address/subaccountNumber/:subaccountNumber`, and `/v4/orders?addresses=${address}&subaccountNumber=${subaccountNumber}&status=OPEN`.

### Example
```tsx
{
  "type": "subscribed",
  "connection_id": "c5a28fa5-c257-4fb5-b68e-fe084c2768e5",
  "message_id": 1,
  "channel": "v4_subaccounts",
  "id": "dydx199tqg4wdlnu4qjlxchpd7seg454937hjrknju4/0",
  "contents": {
    "subaccount": {
      "address": "dydx199tqg4wdlnu4qjlxchpd7seg454937hjrknju4",
      "subaccountNumber": 0,
      "equity": "100000000000.000000",
      "freeCollateral": "100000000000.000000",
      "openPerpetualPositions": {},
      "assetPositions": {
        "USDC": {
          "symbol": "USDC",
          "side": "LONG",
          "size": "100000000000",
          "assetId": "0"
        }
      },
      "marginEnabled": true
    },
    "orders": []
  }
}
```


### Channel Data

Subsequent responses will contain any update to open orders, changes in account, changes in open positions, and/or transfers in a single message.

```tsx
export interface SubaccountsChannelData {
	channel: 'v4_trades',
	id: string,
	contents: SubaccountMessageContents,
	blockHeight: string,
	transactionIndex: number,
	eventIndex: number,
	clobPairId: string,
	version: string,
}

export interface SubaccountMessageContents {
	// Perpetual position updates on the subaccount
  perpetualPositions?: PerpetualPositionSubaccountMessageContents[],
	// Asset position updates on the subaccount
  assetPositions?: AssetPositionSubaccountMessageContents[],
	// Order updates on the subaccount
  orders?: OrderSubaccountMessageContents[],
	// Fills that occur on the subaccount
  fills?: FillSubaccountMessageContents[],
	// Transfers that occur on the subaccount
  transfers?: TransferSubaccountMessageContents,
}

export interface PerpetualPositionSubaccountMessageContents {
  address: string,
  subaccountNumber: number,
  positionId: string,
  market: string,
  side: PositionSide,
  status: PerpetualPositionStatus,
  size: string,
  maxSize: string,
  netFunding: string,
  entryPrice: string,
  exitPrice?: string,
  sumOpen: string,
  sumClose: string,
  realizedPnl?: string,
  unrealizedPnl?: string,
}

export enum PositionSide {
  LONG = 'LONG',
  SHORT = 'SHORT',
}

export enum PerpetualPositionStatus {
  OPEN = 'OPEN',
  CLOSED = 'CLOSED',
  LIQUIDATED = 'LIQUIDATED',
}

export interface AssetPositionSubaccountMessageContents {
  address: string,
  subaccountNumber: number,
  positionId: string,
  assetId: string,
  symbol: string,
  side: PositionSide,
  size: string,
}

export interface OrderSubaccountMessageContents {
  id: string;
  subaccountId: string;
  clientId: string;
  clobPairId: string;
  side: OrderSide;
  size: string;
  ticker: string,
  price: string;
  type: OrderType;
  timeInForce: APITimeInForce;
  postOnly: boolean;
  reduceOnly: boolean;
  status: APIOrderStatus;
  orderFlags: string;
  totalFilled?: string;
  totalOptimisticFilled?: string;
  goodTilBlock?: string;
  goodTilBlockTime?: string;
  removalReason?: string;
  createdAtHeight?: string;
  clientMetadata: string;
  triggerPrice?: string;
  updatedAt?: IsoString;
  updatedAtHeight?: string;
}

export enum OrderSide {
  BUY = 'BUY',
  SELL = 'SELL',
}

export enum OrderType {
  LIMIT = 'LIMIT',
  MARKET = 'MARKET',
  STOP_LIMIT = 'STOP_LIMIT',
  STOP_MARKET = 'STOP_MARKET',
  TRAILING_STOP = 'TRAILING_STOP',
  TAKE_PROFIT = 'TAKE_PROFIT',
  TAKE_PROFIT_MARKET = 'TAKE_PROFIT_MARKET',
}

export enum APITimeInForce {
  // GTT represents Good-Til-Time, where an order will first match with existing orders on the book
  // and any remaining size will be added to the book as a maker order, which will expire at a
  // given expiry time.
  GTT = 'GTT',
  // FOK represents Fill-Or-KILl where it's enforced that an order will either be filled
  // completely and immediately by maker orders on the book or canceled if the entire amount can't
  // be filled.
  FOK = 'FOK',
  // IOC represents Immediate-Or-Cancel, where it's enforced that an order only be matched with
  // maker orders on the book. If the order has remaining size after matching with existing orders
  // on the book, the remaining size is not placed on the book.
  IOC = 'IOC',
}

export enum APIOrderStatus {
  OPEN = 'OPEN',
  FILLED = 'FILLED',
  CANCELED = 'CANCELED',
  BEST_EFFORT_CANCELED = 'BEST_EFFORT_CANCELED',
  BEST_EFFORT_OPENED = 'BEST_EFFORT_OPENED',
  UNTRIGGERED = "UNTRIGGERED"
}

export interface FillSubaccountMessageContents {
  id: string;
  subaccountId: string;
  side: OrderSide;
  liquidity: Liquidity;
  type: FillType;
  clobPairId: string;
  size: string;
  price: string;
  quoteAmount: string;
  eventId: string,
  transactionHash: string;
  createdAt: IsoString;
  createdAtHeight: string;
  orderId?: string;
  ticker: string;
}

export enum Liquidity {
  TAKER = 'TAKER',
  MAKER = 'MAKER',
}

export enum FillType {
  // LIMIT is the fill type for a fill with a limit taker order.
  LIMIT = 'LIMIT',
  // LIQUIDATED is for the taker side of the fill where the subaccount was liquidated.
  // The subaccountId associated with this fill is the liquidated subaccount.
  LIQUIDATED = 'LIQUIDATED',
  // LIQUIDATION is for the maker side of the fill, never used for orders
  LIQUIDATION = 'LIQUIDATION',
  // DELEVERAGED is for the subaccount that was deleveraged in a deleveraging event.
  // The fill type will be set to taker.
  DELEVERAGED = 'DELEVERAGED',
  // OFFSETTING is for the offsetting subaccount in a deleveraging event.
  // The fill type will be set to maker.
  OFFSETTING = 'OFFSETTING',
}

export enum TradeType {
  // LIMIT is the trade type for a fill with a limit taker order.
  LIMIT = 'LIMIT',
  // LIQUIDATED is the trade type for a fill with a liquidated taker order.
  LIQUIDATED = 'LIQUIDATED',
  // DELEVERAGED is the trade type for a fill with a deleveraged taker order.
  DELEVERAGED = 'DELEVERAGED',
}

export interface TransferSubaccountMessageContents {
  sender: {
    address: string,
    subaccountNumber?: number,
  },
  recipient: {
    address: string,
    subaccountNumber?: number,
  },
  symbol: string,
  size: string,
  type: TransferType,
  createdAt: IsoString,
  createdAtHeight: string,
  transactionHash: string,
}

export enum TransferType {
  TRANSFER_IN = 'TRANSFER_IN',
  TRANSFER_OUT = 'TRANSFER_OUT',
  DEPOSIT = 'DEPOSIT',
  WITHDRAWAL = 'WITHDRAWAL',
}

```

### Example

```tsx
{
  "type": "channel_data",
  "connection_id": "a00edbe8-095a-4da1-8a9d-ff1f91467258",
  "message_id": 4,
  "id": "dydx1zsw8fczav25uvc8rg3zcv6zy9j7yhnktpq374m/0",
  "channel": "v4_subaccounts",
  "version": "2.1.0",
  "contents": {
    "orders": [
      {
        "id": "64fe30a2-006d-5108-a156-cb0c8443546c",
        "side": "BUY",
        "size": "1",
        "totalFilled": "1",
        "price": "1948.65",
        "type": "LIMIT",
        "status": "FILLED",
        "timeInForce": "IOC",
        "reduceOnly": false,
        "orderFlags": "0",
        "goodTilBlock": "61186",
        "goodTilBlockTime": null,
        "postOnly": false,
        "ticker": "ETH-USD"
      }
    ],
    "fills": [
      {
        "id": "c5030bd3-cd85-5046-8f2a-518bbba6ec45",
        "subaccountId": "db535c19-b298-5ee8-bb59-e96c659a8bd4",
        "side": "BUY",
        "liquidity": "TAKER",
        "type": "LIMIT",
        "clobPairId": "1",
        "orderId": "64fe30a2-006d-5108-a156-cb0c8443546c",
        "size": "1",
        "price": "1854.25",
        "quoteAmount": "1854.25",
        "eventId": {
          "type": "Buffer",
          "data": [
            0,
            0,
            238,
            241,
            0,
            0,
            0,
            2,
            0,
            0,
            0,
            77
          ]
        },
        "transactionHash": "C84B0BBCA8E713A2D46EFBA07F2D0A32C1F6E2440794A366B888503935E0EF40",
        "createdAt": "2023-04-04T19:09:24.869Z",
        "createdAtHeight": "61169",
        "ticker": "ETH-USD"
      }
    ]
  }
}
```


## Orderbooks

### Subscribe

| Field | Type | Description |
| --- | --- | --- |
| type | string | Set to subscribe |
| channel | string | Set to v4_orderbook |
| id | string | Set to the ticker of the market you would like to subscribe to. For example, BTC-USD |

### Initial Response

Returns everything from `v4/orderbooks/perpetualMarkets/${id}` endpoint.

- Example

    ```tsx
    {
      "type": "subscribed",
      "connection_id": "ee5a4696-dce8-44ef-8d68-f0e0d0b06160",
      "message_id": 2,
      "channel": "v4_orderbook",
      "id": "BTC-USD",
      "contents": {
        "bids": [
          {
            "price": "28194",
            "size": "4.764826096"
          },
          {
            "price": "28193",
            "size": "3.115323739"
          },
          {
            "price": "28192",
            "size": "3.400340775"
          },
          {
            "price": "28191",
            "size": "3.177700682"
          },
          {
            "price": "28190",
            "size": "3.055502176"
          },
          {
            "price": "28189",
            "size": "3.672892171"
          },
          {
            "price": "28188",
            "size": "3.597672948"
          },
          {
            "price": "28187",
            "size": "2.561597964"
          },
          {
            "price": "28186",
            "size": "3.070490554"
          },
          {
            "price": "28185",
            "size": "3.550128411"
          },
          {
            "price": "28184",
            "size": "4.213369101"
          },
          {
            "price": "28183",
            "size": "3.608880877"
          },
    		],
    		"asks": [
          {
            "price": "28195",
            "size": "3.219612343"
          },
          {
            "price": "28196",
            "size": "2.387087565"
          },
          {
            "price": "28197",
            "size": "2.698530469"
          },
          {
            "price": "28198",
            "size": "2.590884421"
          },
          {
            "price": "28199",
            "size": "3.192796678"
          },
    		],
    	},
    }
    ```


### Channel Data

```tsx
interface OrderbookChannelData {
	channel: 'v4_orderbook',
	id: string,
	contents: OrderbookMessageContents,
	clobPairId: string,
	version: string,
}

interface OrderbookMessageContents {
  bids?: PriceLevel[],
  asks?: PriceLevel[],
}

// The first string indicates the price, the second string indicates the size
type PriceLevel = [string, string];
```

- Example

    ```tsx
    {
      "type": "channel_data",
      "connection_id": "ee5a4696-dce8-44ef-8d68-f0e0d0b06160",
      "message_id": 484,
      "id": "BTC-USD",
      "channel": "v4_orderbook",
      "version": "0.0.1",
      "contents": {
        "bids": [
          [
            "27773",
            "1.986168516"
          ]
        ]
      }
    }
    ```


## Trades

### Subscribe

| Field | Type | Description |
| --- | --- | --- |
| type | string | Set to subscribe |
| channel | string | Set to v4_trades |
| id | string | Set to the ticker of the market you would like to subscribe to. For example, BTC-USD |

### Initial Response

Returns everything from `v4/trades/perpetualMarkets/${id}` endpoint.

- Example

    ```tsx
    {
      "type": "subscribed",
      "connection_id": "57844645-0b1d-4c3f-ad71-1c6154154a13",
      "message_id": 1,
      "channel": "v4_trades",
      "id": "BTC-USD",
      "contents": {
        "trades": [
          {
            "side": "BUY",
            "size": "0.00396135",
            "price": "27848",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000019216",
            "price": "27841",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.001682908",
            "price": "27840",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000311013",
            "price": "27840",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000000011",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000000017",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000226026",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000000004",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000000006",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000226015",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000003739",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "SELL",
            "size": "0.000164144",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:56.226Z",
            "createdAtHeight": "49592"
          },
          {
            "side": "BUY",
            "size": "0.037703477",
            "price": "27848",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.000000321",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.06706869",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.002573305",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.001525924",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.00387205",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.000094697",
            "price": "27845",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.002828331",
            "price": "27842",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "SELL",
            "size": "0.000100428",
            "price": "27845",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
          {
            "side": "BUY",
            "size": "0.000098184",
            "price": "27848",
            "createdAt": "2023-04-04T00:28:50.516Z",
            "createdAtHeight": "49591"
          },
        ],
    	},
    }
    ```


### Channel Data

```tsx
interface TradeChannelData {
	channel: 'v4_trades',
	id: string,
	contents: TradeMessageContents,
	blockHeight: string,
	clobPairId: string,
	version: string,
}

interface TradeMessageContents {
  trades: TradeContent[],
}

interface TradeContent {
	// Unique id of the trade, which is the taker fill id.
  id: string,
  size: string,
  price: string,
  side: string,
  createdAt: IsoString,
  type: TradeType,
}
```

### Example

```tsx
{
  "type": "channel_data",
  "connection_id": "57844645-0b1d-4c3f-ad71-1c6154154a13",
  "message_id": 4,
  "id": "BTC-USD",
  "channel": "v4_trades",
  "version": "1.1.0",
  "contents": {
    "trades": [
      {
        "id": "8ee6d90d-272d-5edd-bf0f-2e4d6ae3d3b7",
        "size": "0.000100431",
        "price": "27839",
        "side": "BUY",
        "createdAt": "2023-04-04T00:29:19.353Z",
        "type": "LIQUIDATED"
      },
      {
        "id": "38e64479-af09-5417-a795-195f83879156",
        "size": "0.000000004",
        "price": "27839",
        "side": "BUY",
        "createdAt": "2023-04-04T00:29:19.353Z",
        "type": "LIQUIDATED"
      },
      {
        "id": "d310c32c-f066-5ba8-a97d-10a29d9a6c84",
        "size": "0.000000005",
        "price": "27837",
        "side": "SELL",
        "createdAt": "2023-04-04T00:29:19.353Z",
        "type": "LIMIT"
      },
      {
        "id": "dd1088b5-5cab-518f-a59c-4d5f735ab861",
        "size": "0.000118502",
        "price": "27837",
        "side": "SELL",
        "createdAt": "2023-04-04T00:29:19.353Z",
        "type": "LIMIT"
      },
    ],
  },
}
```


## Markets

### Subscribe

| Field | Type | Description |
| --- | --- | --- |
| type | string | Set to subscribe |
| channel | string | Set to v4_markets |

### Initial Response

Returns everything from `v4/perpetualMarkets` endpoint.

### Example

```tsx
{
  "type": "subscribed",
  "connection_id": "6e0af39b-5937-459a-b7ac-cc8abe1049db",
  "message_id": 1,
  "channel": "v4_markets",
  "contents": {
    "markets": {
      "BTC-USD": {
        "clobPairId": "0",
        "ticker": "BTC-USD",
        "status": "ACTIVE",
        "baseAsset": "",
        "quoteAsset": "",
        "oraclePrice": "27752.92",
        "priceChange24H": "0",
        "volume24H": "63894023.044245577",
        "trades24H": 143820,
        "nextFundingRate": "0",
        "initialMarginFraction": "0.050000",
        "maintenanceMarginFraction": "0.030000",
        "basePositionSize": "0",
        "incrementalPositionSize": "0",
        "maxPositionSize": "0",
        "openInterest": "1891.473716288",
        "atomicResolution": -10,
        "quantumConversionExponent": -8,
        "tickSize": "1",
        "stepSize": "0.000000001",
        "stepBaseQuantums": 10,
        "subticksPerTick": 10000
      },
      "ETH-USD": {
        "clobPairId": "1",
        "ticker": "ETH-USD",
        "status": "ACTIVE",
        "baseAsset": "",
        "quoteAsset": "",
        "oraclePrice": "1808.2",
        "priceChange24H": "0",
        "volume24H": "67487133.70842158",
        "trades24H": 137552,
        "nextFundingRate": "0",
        "initialMarginFraction": "0.050000",
        "maintenanceMarginFraction": "0.030000",
        "basePositionSize": "0",
        "incrementalPositionSize": "0",
        "maxPositionSize": "0",
        "openInterest": "44027.853711",
        "atomicResolution": -9,
        "quantumConversionExponent": -9,
        "tickSize": "0.01",
        "stepSize": "0.000001",
        "stepBaseQuantums": 1000,
        "subticksPerTick": 10000
      }
    }
  }
}
```


### Channel Data

```tsx
interface MarketChannelData {
	channel: 'v4_markets',
	id: 'v4_markets',
	contents: MarketMessageContents,
	version: string,
}

interface MarketMessageContents {
  trading?: TradingMarketMessageContents,
  oraclePrices?: OraclePriceMarketMessageContentsMapping,
}

type TradingMarketMessageContents = {
  [ticker: string]: TradingPerpetualMarketMessage
};

interface TradingPerpetualMarketMessage {
  id?: string;
  clobPairId?: string;
  ticker?: string;
  marketId?: number;
  status?: PerpetualMarketStatus; // 'ACTIVE', 'PAUSED', 'CANCEL_ONLY', 'POST_ONLY', or 'INITIALIZING'
  baseAsset?: string;
  quoteAsset?: string;
  initialMarginFraction?: string;
  maintenanceMarginFraction?: string;
  basePositionSize?: string;
  incrementalPositionSize?: string;
  maxPositionSize?: string;
  openInterest?: string;
  quantumConversionExponent?: number;
  atomicResolution?: number;
  subticksPerTick?: number;
  stepBaseQuantums?: number;
  priceChange24H?: string;
  volume24H?: string;
  trades24H?: number;
  nextFundingRate?: string;
}

type OraclePriceMarketMessageContentsMapping = {
  [ticker: string]: OraclePriceMarket,
};

interface OraclePriceMarket {
  oraclePrice: string,
  effectiveAt: IsoString,
  effectiveAtHeight: string,
  marketId: number,
}
```

### Example

```tsx
{
  "type": "channel_data",
  "connection_id": "1f4ec0e3-ff95-48cc-94f1-7118a19412ff",
  "message_id": 2,
  "channel": "v4_markets",
  "version": "0.0.1",
  "contents": {
    "trading": {
      "BTC-USD": {
        "nextFundingRate": "0.00006821875"
      },
      "ETH-USD": {
        "volume24H": "1462890959.6541",
        "nextFundingRate": "0.00007478125"
      }
    }
  }
}
```

# Constants

| Name                        | Value  | Description                                                                                                                                                                                                                   |
| --------------------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `sdk.DefaultPowerReduction` | `1e18` | The staking power is equal the number of staked-token coins divided by this number (to prevent overflow). This number typically denotes how many of the staked-token denom are considered to be equal to one canonical token. |

# Module Accounts

In Cosmos-SDK, a module account is an account with address generated by hashing a string. The module account has no known private key and is therefore only controlled by the state-machine and governance. Here we list the module account addresses (assuming the default HRP of `dydx`) and the hashed string that results in the address.

| Module           | Name                | Address                                       | String                     |
| ---------------- | ------------------- | --------------------------------------------- | -------------------------- |
| `x/auth`         | Fee Collector       | `dydx17xpfvakm2amg962yls6f84z3kell8c5leqdyt2` | `"fee_collector"`          |
| `x/bridge`       | Bridge Module       | `dydx1zlefkpe3g0vvm9a4h0jf9000lmqutlh9jwjnsv` | `"bridge"`                 |
| `x/distribution` | Distribution Module | `dydx1jv65s3grqf6v6jl3dp4t6c9t9rk99cd8wx2cfg` | `"distribution"`           |
| `x/staking`      | Bonded Pool         | `dydx1fl48vsnmsdzcv85q5d2q4z5ajdha8yu3uz8teq` | `"bonded_tokens_pool"`     |
| `x/staking`      | Not-Bonded Pool     | `dydx1tygms3xhhs3yv487phx3dw4a95jn7t7lgzm605` | `"not_bonded_tokens_pool"` |
| `x/gov`          | Gov Module          | `dydx10d07y265gmmuvt4z0w9aw880jnsr700jnmapky` | `"gov"`                    |
| `x/ibc`          | IBC Module          | `dydx1yl6hdjhmkf37639730gffanpzndzdpmh8xcdh5` | `"transfer"`               |
| `x/subaccounts`  | Subaccounts Module  | `dydx1v88c3xv9xyv3eetdx0tvcmq7ung3dywp5upwc6` | `"subaccounts"`            |
| `x/clob`         | Insurance Fund      | `dydx1c7ptc87hkd54e3r7zjy92q29xkq7t79w64slrq` | `"insurance_fund"`         |
| `x/rewards`      | Rewards Treasury    | `dydx16wrau2x4tsg033xfrrdpae6kxfn9kyuerr5jjp` | `"rewards_treasury"`       |
| `x/rewards`      | Rewards Vester      | `dydx1ltyc6y4skclzafvpznpt2qjwmfwgsndp458rmp` | `"rewards_vester"`         |
| `x/vest`         | Community Treasury  | `dydx15ztc7xy42tn2ukkc0qjthkucw9ac63pgp70urn` | `"community_treasury"`     |
| `x/vest`         | Community Vester    | `dydx1wxje320an3karyc6mjw4zghs300dmrjkwn7xtk` | `"community_vester"`       |
