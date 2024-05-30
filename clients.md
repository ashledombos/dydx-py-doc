_pages/api_integration-clients/indexer_client.mdx_

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

_pages/api_integration-clients/socket_client.mdx_

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

_pages/api_integration-clients/validator_client.mdx_

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

_pages/api_integration-clients/composite_client.mdx_

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
