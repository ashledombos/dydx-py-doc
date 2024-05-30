_v4-client-py/examples/README.md_

# User guide to test examples

1. Go to your repository location for the Python client
```
cd ~/.../v4-clients/v4-client-py
```
2. Create a virtual environment for the DyDx client, activate it and install requirements
```
python3 -m venv venv
source venv/bin/activate
pip3 install -r requirements.txt
```
3. Export PYTHONPATH for your current location
```
export PYTHONPATH='~/.../v4-clients/v4-client-py'
```

Now you are ready to use the examples in this folder.

# Set up your configurations in constants.py 
~/.../v4-clients/v4-client-py/v4_client_py/clients/constants.py

```
VALIDATOR_GRPC_ENDPOINT = <>
AERIAL_CONFIG_URL = <>
AERIAL_GRPC_OR_REST_PREFIX = <>
INDEXER_REST_ENDPOINT = <>
INDEXER_WS_ENDPOINT = <>
CHAIN_ID = <>
ENV = <>
```
_
v4-client-py/examples/account_endpoints.py_

"""Example for placing, replacing, and canceling orders.

Usage: python -m examples.private_endpoints
"""

from v4_client_py.clients import IndexerClient, Subaccount
from v4_client_py.clients.constants import Network

from tests.constants import DYDX_TEST_MNEMONIC

client = IndexerClient(
    config=Network.config_network().indexer_config,
)

try:
    subaccount = Subaccount.from_mnemonic(DYDX_TEST_MNEMONIC)
    address = subaccount.address

    # Get subaccounts
    try:
        subaccounts_response = client.account.get_subaccounts(address)
        print(f"{subaccounts_response.data}")
        subaccounts = subaccounts_response.data["subaccounts"]
        subaccount_0 = subaccounts[0]
        print(f"{subaccount_0}")
        subaccount_0_subaccountNumber = subaccount_0["subaccountNumber"]
    except:
        print("failed to get subaccounts")

    try:
        subaccount_response = client.account.get_subaccount(address, 0)
        print(f"{subaccount_response.data}")
        subaccount = subaccount_response.data["subaccount"]
        print(f"{subaccount}")
        subaccount_subaccountNumber = subaccount["subaccountNumber"]
    except:
        print("failed to get subaccount")

    # Get positions
    try:
        asset_positions_response = client.account.get_subaccount_asset_positions(address, 0)
        print(f"{asset_positions_response.data}")
        asset_positions = asset_positions_response.data["positions"]
        if len(asset_positions) > 0:
            asset_positions_0 = asset_positions[0]
            print(f"{asset_positions_0}")
    except:
        print("failed to get asset positions")

    try:
        perpetual_positions_response = client.account.get_subaccount_perpetual_positions(address, 0)
        print(f"{perpetual_positions_response.data}")
        perpetual_positions = perpetual_positions_response.data["positions"]
        if len(perpetual_positions) > 0:
            perpetual_positions_0 = perpetual_positions[0]
            print(f"{perpetual_positions_0}")
    except:
        print("failed to get perpetual positions")

    # Get transfers
    try:
        transfers_response = client.account.get_subaccount_transfers(address, 0)
        print(f"{transfers_response.data}")
        transfers = transfers_response.data["transfers"]
        if len(transfers) > 0:
            transfers_0 = transfers[0]
            print(f"{transfers_0}")
    except:
        print("failed to get transfers")

    # Get orders
    try:
        orders_response = client.account.get_subaccount_orders(address, 0)
        print(f"{orders_response.data}")
        orders = orders_response.data
        if len(orders) > 0:
            order_0 = orders[0]
            print(f"{order_0}")
            order_0_id = order_0["id"]
            order_response = client.account.get_order(order_id=order_0_id)
            order = order_response.data
            order_id = order["id"]
    except:
        print("failed to get orders")

    # Get fills
    try:
        fills_response = client.account.get_subaccount_fills(address, 0)
        print(f"{fills_response.data}")
        fills = fills_response.data["fills"]
        if len(fills) > 0:
            fill_0 = fills[0]
            print(f"{fill_0}")
    except:
        print("failed to get fills")

    # Get funding
    try:
        funding_response = client.account.get_subaccount_funding(address, 0)
        print(f"{funding_response.data}")
        funding = funding_response.data["fundingPayments"]
        if len(funding) > 0:
            funding_0 = funding[0]
            print(f"{funding_0}")
    except:
        print("failed to get funding")

    # Get historical pnl
    try:
        historical_pnl_response = client.account.get_subaccount_historical_pnls(address, 0)
        print(f"{historical_pnl_response.data}")
        historical_pnl = historical_pnl_response.data["historicalPnl"]
        if len(historical_pnl) > 0:
            historical_pnl_0 = historical_pnl[0]
            print(f"{historical_pnl_0}")
    except:
        print("failed to get historical pnl")

except:
    print("from_mnemonic failed")

_v4-client-py/examples/composite_example.py_

"""Example for trading with human readable numbers

Usage: python -m examples.composite_example
"""
import asyncio
import logging
from random import randrange
from v4_client_py.chain.aerial.wallet import LocalWallet
from v4_client_py.clients import CompositeClient, Subaccount
from v4_client_py.clients.constants import BECH32_PREFIX, Network

from v4_client_py.clients.helpers.chain_helpers import (
    OrderType,
    OrderSide,
    OrderTimeInForce,
    OrderExecution,
)
from examples.utils import loadJson

from tests.constants import DYDX_TEST_MNEMONIC


async def main() -> None:
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX)
    network = Network.config_network()
    client = CompositeClient(
        network,
    )
    subaccount = Subaccount(wallet, 0)
    ordersParams = loadJson("human_readable_orders.json")
    for orderParams in ordersParams:
        type = OrderType[orderParams["type"]]
        side = OrderSide[orderParams["side"]]
        time_in_force_string = orderParams.get("timeInForce", "GTT")
        time_in_force = OrderTimeInForce[time_in_force_string]
        price = orderParams.get("price", 1350)

        if time_in_force == OrderTimeInForce.GTT:
            time_in_force_seconds = 60
            good_til_block = 0
        else:
            latest_block = client.validator_client.get.latest_block()
            next_valid_block = latest_block.block.header.height + 1
            good_til_block = next_valid_block + 10
            time_in_force_seconds = 0

        post_only = orderParams.get("postOnly", False)
        try:
            tx = client.place_order(
                subaccount,
                market="ETH-USD",
                type=type,
                side=side,
                price=price,
                size=0.01,
                client_id=randrange(0, 100000000),
                time_in_force=time_in_force,
                good_til_block=good_til_block,
                good_til_time_in_seconds=time_in_force_seconds,
                execution=OrderExecution.DEFAULT,
                post_only=post_only,
                reduce_only=False,
            )
            print("**Order Tx**")
            print(tx)
        except Exception as error:
            print("**Order Failed**")
            print(str(error))

        await asyncio.sleep(5)  # wait for placeOrder to complete

    try:
        tx = client.place_order(
            subaccount,
            market="ETH-USD",
            type=OrderType.STOP_MARKET,
            side=OrderSide.SELL,
            price=900.0,
            size=0.01,
            client_id=randrange(0, 100000000),
            time_in_force=OrderTimeInForce.GTT,
            good_til_block=0,  # long term orders use GTBT
            good_til_time_in_seconds=1000,
            execution=OrderExecution.IOC,
            post_only=False,
            reduce_only=False,
            trigger_price=1000,
        )
        print("**Order Tx**")
        print(tx)
    except Exception as error:
        print("**Order Failed**")
        print(str(error))


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    asyncio.get_event_loop().run_until_complete(main())

_v4-client-py/examples/faucet_endpoint.py_

"""Example for depositing with faucet.

Usage: python -m examples.faucet_endpoint
"""
from v4_client_py.clients import FaucetClient, Subaccount
from v4_client_py.clients.constants import Network

from tests.constants import DYDX_TEST_MNEMONIC

client = FaucetClient(
    host=Network.config_network().faucet_endpoint,
)

subaccount = Subaccount.from_mnemonic(DYDX_TEST_MNEMONIC)
address = subaccount.address


# fill subaccount with 2000 ETH
faucet_response = client.fill(address, 0, 2000)
print(faucet_response.data)
faucet_http_code = faucet_response.status_code
print(faucet_http_code)

_v4-client-py/examples/long_term_order_cancel_example.py_

"""Example for trading with human readable numbers

Usage: python -m examples.composite_example
"""
import asyncio
import logging
from random import randrange
from v4_client_py.chain.aerial.wallet import LocalWallet
from v4_client_py.clients import CompositeClient, Subaccount
from v4_client_py.clients.constants import BECH32_PREFIX, Network

from v4_client_py.clients.helpers.chain_helpers import (
    ORDER_FLAGS_LONG_TERM,
    OrderType,
    OrderSide,
    OrderTimeInForce,
    OrderExecution,
)

from tests.constants import DYDX_TEST_MNEMONIC, MAX_CLIENT_ID


async def main() -> None:
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX)
    network = Network.config_network()
    client = CompositeClient(
        network,
    )
    subaccount = Subaccount(wallet, 0)

    """
    Note this example places a stateful order.
    Programmatic traders should generally not use stateful orders for following reasons:
    - Stateful orders received out of order by validators will fail sequence number validation
        and be dropped.
    - Stateful orders have worse time priority since they are only matched after they are included
        on the block.
    - Stateful order rate limits are more restrictive than Short-Term orders, specifically max 2 per
        block / 20 per 100 blocks.
    - Stateful orders can only be canceled after they’ve been included in a block.
    """
    long_term_order_client_id = randrange(0, MAX_CLIENT_ID)
    try:
        tx = client.place_order(
            subaccount,
            market="ETH-USD",
            type=OrderType.LIMIT,
            side=OrderSide.SELL,
            price=40000,
            size=0.01,
            client_id=long_term_order_client_id,
            time_in_force=OrderTimeInForce.GTT,
            good_til_block=0,  # long term orders use GTBT
            good_til_time_in_seconds=60,
            execution=OrderExecution.DEFAULT,
            post_only=False,
            reduce_only=False,
        )
        print("** Long Term Order Tx**")
        print(tx.tx_hash)
    except Exception as error:
        print("**Long Term Order Failed**")
        print(str(error))

    # cancel a long term order.
    try:
        tx = client.cancel_order(
            subaccount,
            long_term_order_client_id,
            "ETH-USD",
            ORDER_FLAGS_LONG_TERM,
            good_til_time_in_seconds=120,
            good_til_block=0,  # long term orders use GTBT
        )
        print("**Cancel Long Term Order Tx**")
        print(tx.tx_hash)
    except Exception as error:
        print("**Cancel Long Term Order Failed**")
        print(str(error))


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    asyncio.get_event_loop().run_until_complete(main())

_v4-client-py/examples/markets_endpoints.py_

"""Example for placing, replacing, and canceling orders.

Usage: python -m examples.markets_endpoints
"""

from v4_client_py.clients import IndexerClient
from v4_client_py.clients.constants import Network, MARKET_BTC_USD

client = IndexerClient(
    config=Network.config_network().indexer_config,
)

# Get perp markets
try:
    markets_response = client.markets.get_perpetual_markets()
    print(markets_response.data)
    btc_market = markets_response.data["markets"]["BTC-USD"]
    btc_market_status = btc_market["status"]
except:
    print("failed to get markets")

try:
    btc_market_response = client.markets.get_perpetual_markets(MARKET_BTC_USD)
    print(btc_market_response.data)
    btc_market = btc_market_response.data["markets"]["BTC-USD"]
    btc_market_status = btc_market["status"]
except:
    print("failed to get BTC market")

# Get sparklines
try:
    sparklines_response = client.markets.get_perpetual_markets_sparklines()
    print(sparklines_response.data)
    sparklines = sparklines_response.data
    btc_sparkline = sparklines["BTC-USD"]
except:
    print("failed to get sparklines")


# Get perp market trades
try:
    btc_market_trades_response = client.markets.get_perpetual_market_trades(MARKET_BTC_USD)
    print(btc_market_trades_response.data)
    btc_market_trades = btc_market_trades_response.data["trades"]
    btc_market_trades_0 = btc_market_trades[0]
except:
    print("failed to get market trades")

# Get perp market orderbook
try:
    btc_market_orderbook_response = client.markets.get_perpetual_market_orderbook(MARKET_BTC_USD)
    print(btc_market_orderbook_response.data)
    btc_market_orderbook = btc_market_orderbook_response.data
    btc_market_orderbook_asks = btc_market_orderbook["asks"]
    btc_market_orderbook_bids = btc_market_orderbook["bids"]
    if len(btc_market_orderbook_asks) > 0:
        btc_market_orderbook_asks_0 = btc_market_orderbook_asks[0]
        print(btc_market_orderbook_asks_0)
        btc_market_orderbook_asks_0_price = btc_market_orderbook_asks_0["price"]
        btc_market_orderbook_asks_0_size = btc_market_orderbook_asks_0["size"]
except:
    print("failed to get market orderbook")

# Get perp market candles
try:
    btc_market_candles_response = client.markets.get_perpetual_market_candles(MARKET_BTC_USD, "1MIN")
    print(btc_market_candles_response.data)
    btc_market_candles = btc_market_candles_response.data["candles"]
    if len(btc_market_candles) > 0:
        btc_market_candles_0 = btc_market_candles[0]
        print(btc_market_candles_0)
        btc_market_candles_0_startedAt = btc_market_candles_0["startedAt"]
        btc_market_candles_0_low = btc_market_candles_0["low"]
        btc_market_candles_0_hight = btc_market_candles_0["high"]
        btc_market_candles_0_open = btc_market_candles_0["open"]
        btc_market_candles_0_close = btc_market_candles_0["close"]
        btc_market_candles_0_baseTokenVolume = btc_market_candles_0["baseTokenVolume"]
        btc_market_candles_0_usdVolume = btc_market_candles_0["usdVolume"]
        btc_market_candles_0_trades = btc_market_candles_0["trades"]
except:
    print("failed to get market cancles")


# Get perp market funding
try:
    btc_market_funding_response = client.markets.get_perpetual_market_funding(MARKET_BTC_USD)
    print(btc_market_funding_response.data)
    btc_market_funding = btc_market_funding_response.data["historicalFunding"]
    if len(btc_market_funding) > 0:
        btc_market_funding_0 = btc_market_funding[0]
        print(btc_market_funding_0)
except:
    print("failed to get market historical funding")

_v4-client-py/examples/short_term_order_composite_example.py_

"""Example for trading with human readable numbers

Usage: python -m examples.composite_example
"""
import asyncio
import logging
from random import randrange
from v4_client_py.chain.aerial.wallet import LocalWallet
from v4_client_py.clients import CompositeClient, Subaccount
from v4_client_py.clients.constants import BECH32_PREFIX, Network

from v4_client_py.clients.helpers.chain_helpers import (
    ORDER_FLAGS_SHORT_TERM,
    Order_TimeInForce,
    OrderSide,
)
from tests.constants import DYDX_TEST_MNEMONIC, MAX_CLIENT_ID


async def main() -> None:
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX)
    network = Network.config_network()
    client = CompositeClient(
        network,
    )
    subaccount = Subaccount(wallet, 0)

    # place a short term order.
    short_term_client_id = randrange(0, MAX_CLIENT_ID)
    # Get the expiration block.
    current_block = client.get_current_block()
    next_valid_block_height = current_block + 1
    # Note, you can change this to any number between `next_valid_block_height` to `next_valid_block_height + SHORT_BLOCK_WINDOW`
    good_til_block = next_valid_block_height + 10

    try:
        tx = client.place_short_term_order(
            subaccount,
            market="ETH-USD",
            side=OrderSide.SELL,
            price=40000,
            size=0.01,
            client_id=short_term_client_id,
            good_til_block=good_til_block,
            time_in_force=Order_TimeInForce.TIME_IN_FORCE_UNSPECIFIED,
            reduce_only=False,
        )
        print("**Short Term Order Tx**")
        print(tx.tx_hash)
    except Exception as error:
        print("**Short Term Order Failed**")
        print(str(error))

    # cancel a short term order.
    try:
        tx = client.cancel_order(
            subaccount,
            short_term_client_id,
            "ETH-USD",
            ORDER_FLAGS_SHORT_TERM,
            good_til_time_in_seconds=0,  # short term orders use GTB.
            good_til_block=good_til_block,  # GTB should be the same or greater than order to cancel
        )
        print("**Cancel Short Term Order Tx**")
        print(tx.tx_hash)
    except Exception as error:
        print("**Cancel Short Term Order Failed**")
        print(str(error))


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    asyncio.get_event_loop().run_until_complete(main())

_v4-client-py/examples/short_term_order_cancel_example.py_

"""Example for trading with human readable numbers

Usage: python -m examples.composite_example
"""
import asyncio
import logging
from random import randrange
from v4_client_py.chain.aerial.wallet import LocalWallet
from v4_client_py.clients import CompositeClient, Subaccount
from v4_client_py.clients.constants import BECH32_PREFIX, Network

from v4_client_py.clients.helpers.chain_helpers import (
    ORDER_FLAGS_SHORT_TERM,
    Order_TimeInForce,
    OrderSide,
)
from tests.constants import DYDX_TEST_MNEMONIC, MAX_CLIENT_ID


async def main() -> None:
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX)
    network = Network.config_network()
    client = CompositeClient(
        network,
    )
    subaccount = Subaccount(wallet, 0)

    # place a short term order.
    short_term_client_id = randrange(0, MAX_CLIENT_ID)
    # Get the expiration block.
    current_block = client.get_current_block()
    next_valid_block_height = current_block + 1
    # Note, you can change this to any number between `next_valid_block_height` to `next_valid_block_height + SHORT_BLOCK_WINDOW`
    good_til_block = next_valid_block_height + 10

    try:
        tx = client.place_short_term_order(
            subaccount,
            market="ETH-USD",
            side=OrderSide.SELL,
            price=40000,
            size=0.01,
            client_id=short_term_client_id,
            good_til_block=good_til_block,
            time_in_force=Order_TimeInForce.TIME_IN_FORCE_UNSPECIFIED,
            reduce_only=False,
        )
        print("**Short Term Order Tx**")
        print(tx.tx_hash)
    except Exception as error:
        print("**Short Term Order Failed**")
        print(str(error))

    # cancel a short term order.
    try:
        tx = client.cancel_order(
            subaccount,
            short_term_client_id,
            "ETH-USD",
            ORDER_FLAGS_SHORT_TERM,
            good_til_time_in_seconds=0,  # short term orders use GTB.
            good_til_block=good_til_block,  # GTB should be the same or greater than order to cancel
        )
        print("**Cancel Short Term Order Tx**")
        print(tx.tx_hash)
    except Exception as error:
        print("**Cancel Short Term Order Failed**")
        print(str(error))

_v4-client-py/examples/short_term_order_composite_example.py_

"""Example for trading with human readable numbers

Usage: python -m examples.composite_example
"""
import asyncio
import logging
from random import randrange
from v4_client_py.chain.aerial.wallet import LocalWallet
from v4_client_py.clients import CompositeClient, Subaccount
from v4_client_py.clients.constants import BECH32_PREFIX, Network

from v4_client_py.clients.helpers.chain_helpers import (
    OrderSide,
)
from examples.utils import loadJson, orderExecutionToTimeInForce

from tests.constants import DYDX_TEST_MNEMONIC


async def main() -> None:
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX)
    network = Network.config_network()
    client = CompositeClient(
        network,
    )
    subaccount = Subaccount(wallet, 0)
    ordersParams = loadJson("human_readable_short_term_orders.json")
    for orderParams in ordersParams:
        side = OrderSide[orderParams["side"]]

        # Get the expiration block.
        current_block = client.get_current_block()
        next_valid_block_height = current_block + 1
        # Note, you can change this to any number between `next_valid_block_height` to `next_valid_block_height + SHORT_BLOCK_WINDOW`
        good_til_block = next_valid_block_height + 10

        time_in_force = orderExecutionToTimeInForce(orderParams["timeInForce"])

        price = orderParams.get("price", 1350)
        # uint32
        client_id = randrange(0, 2**32 - 1)

        try:
            tx = client.place_short_term_order(
                subaccount,
                market="ETH-USD",
                side=side,
                price=price,
                size=0.01,
                client_id=client_id,
                good_til_block=good_til_block,
                time_in_force=time_in_force,
                reduce_only=False,
            )
            print("**Order Tx**")
            print(tx.tx_hash)
        except Exception as error:
            print("**Order Failed**")
            print(str(error))

        await asyncio.sleep(5)  # wait for placeOrder to complete


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    asyncio.get_event_loop().run_until_complete(main())
if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)

"""Example for trading with human readable numbers

Usage: python -m examples.composite_example
"""
import asyncio
import logging
from random import randrange
from v4_client_py.chain.aerial.wallet import LocalWallet
from v4_client_py.clients import CompositeClient, Subaccount
from v4_client_py.clients.constants import BECH32_PREFIX, Network

from v4_client_py.clients.helpers.chain_helpers import (
    OrderSide,
)
from examples.utils import loadJson, orderExecutionToTimeInForce

from tests.constants import DYDX_TEST_MNEMONIC


async def main() -> None:
    wallet = LocalWallet.from_mnemonic(DYDX_TEST_MNEMONIC, BECH32_PREFIX)
    network = Network.config_network()
    client = CompositeClient(
        network,
    )
    subaccount = Subaccount(wallet, 0)
    ordersParams = loadJson("human_readable_short_term_orders.json")
    for orderParams in ordersParams:
        side = OrderSide[orderParams["side"]]

        # Get the expiration block.
        current_block = client.get_current_block()
        next_valid_block_height = current_block + 1
        # Note, you can change this to any number between `next_valid_block_height` to `next_valid_block_height + SHORT_BLOCK_WINDOW`
        good_til_block = next_valid_block_height + 10

        time_in_force = orderExecutionToTimeInForce(orderParams["timeInForce"])

        price = orderParams.get("price", 1350)
        # uint32
        client_id = randrange(0, 2**32 - 1)

        try:
            tx = client.place_short_term_order(
                subaccount,
                market="ETH-USD",
                side=side,
                price=price,
                size=0.01,
                client_id=client_id,
                good_til_block=good_til_block,
                time_in_force=time_in_force,
                reduce_only=False,
            )
            print("**Order Tx**")
            print(tx.tx_hash)
        except Exception as error:
            print("**Order Failed**")
            print(str(error))

        await asyncio.sleep(5)  # wait for placeOrder to complete


if __name__ == "__main__":
    logging.basicConfig(level=logging.INFO)
    asyncio.get_event_loop().run_until_complete(main())
    asyncio.get_event_loop().run_until_complete(main())


