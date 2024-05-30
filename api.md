_pages/api_integration-indexer/indexer_api.mdx_

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

_pages/api_integration-indexer/indexer_websocket.md_

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

_pages/api_integration-constants.md_

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
