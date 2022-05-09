# :unlock: **Public APIs (No Auth Required)**

---

### Arbitrage Info
> Returns a list of all market pairs matching your Maker/Taker coin, for example; `ticker=SCC` would return all markets paired with `SCC`.
- Endpoint: `/exchange/spot/arbitrageInfo`

Parameter | Description | Example
------------ | ------------- | -------------
(required) ticker | a coin's ticker | SCC

Example: `GET https://stakecube.io/api/v2/exchange/spot/arbitrageInfo?ticker=SCC`

---

### Markets
> Returns a list of all markets matching your Base coin, for example; calling the bare endpoint would return a list of all coins and pairs globally on the Exchange, appending `baseMarket=BTC` to the endpoint would return all markets paired against `BTC`, you may also add a `orderBy` parameter which allows you to customize how the list is ordered, by default the list is in alphabetical order, for example; `baseMarket=BTC&orderBy=volume` would return the list of markets by descending volume.
- Endpoint: `/exchange/spot/markets`

Parameter | Description | Example
------------ | ------------- | -------------
(optional) market | specific market pair | SCC_BTC
(optional) baseMarket | specific base market | BTC
(optional) category | specific category | BTC, SCC, ALTS, STABLE
(optional) orderBy | the list's ordering | `volume` or `change`
(optional) orderData | include order information | `true` or `false`
(optional) priceHistory | include price history | `true` or `false`

Example: `GET https://stakecube.io/api/v2/exchange/spot/markets?baseMarket=BTC&orderBy=volume`

---

### OHLC Charting Data
> Returns raw OHLC chart data for the specified market and interval, for example; `market=SCC_BTC&interval=1h` would return 1h-per-bar timeframe data on the SCC_BTC market.
- Endpoint: `/exchange/spot/ohlcData`

Parameter | Description | Example
------------ | ------------- | -------------
(required) market | the chosen market pair | SCC_BTC
(required) interval | the chosen time-period | 1h

**NOTE:** `interval` is limited to the below options:
- 1m
- 5m
- 15m
- 30m
- 1h
- 4h
- 1d
- 1w
- 1mo

Example: `GET https://stakecube.io/api/v2/exchange/spot/ohlcData?market=SCC_BTC&interval=1h`

---

### Orderbook
> Returns orderbook data for a specified market pair and orderbook side, for example; `market=SCC_BTC&side=BUY` would return all `bids` of the `SCC_BTC` market.
- Endpoint: `/exchange/spot/orderbook`

Parameter | Description | Example
------------ | ------------- | -------------
(required) market | the chosen market pair | SCC_BTC
(optional) side | the chosen orderbook side | `BUY` or `SELL`

**NOTE:** `side` can be left empty and/or removed completely from the query, which will return BOTH sides of the orderbook (Asks + Bids).

Example: `GET https://stakecube.io/api/v2/exchange/spot/orderbook?market=SCC_BTC`

---

### Trades
> Returns the last trades of a specified market pair, for example; `market=SCC_BTC` would return a list of the last 100 trades in the SCC/BTC market pair.
- Endpoint: `/exchange/spot/trades`

Parameter | Description | Example
------------ | ------------- | -------------
(required) market | the chosen market pair | SCC_BTC
(optional) limit | the amount of trades to fetch | 100

**NOTE:** `limit` can be left empty and/or removed completely from the query, which will return the last 100 trades.

Example: `GET https://stakecube.io/api/v2/exchange/spot/trades?market=SCC_BTC`

---

## :lock: **Private APIs (Authentication Required)**

For security, our private APIs require two additional `body` fields on the majority of endpoints (with a few exceptions), these fields being:
Field | Description
------------ | -------------
(required) nonce | any integer larger than the last API call's integer (millisecond timestamp)
(required) signature | a HMAC signature of the full `body` contents in URL-encoded format

The **Content-Type** must be set to: `application/x-www-form-urlencoded`

In addition, your API key should be included as a **header**, under this name: `X-API-KEY`

For new API users, the nonce will start at `0`, you may provide any integer higher than that as the nonce, for example: your nonce is 0, so your next call uses nonce 1, next call uses nonce 2... and repeat. You may also use timestamps for this purpose, but ensure it's a millisecond timestamp, otherwise a second-based timestamp limits you to a call every second maximum.

The HMAC signature is composed of all the URL-encoded parameters of your API call's body, so for example; you're calling `/exchange/spot/order` which contains the `body` parameters of: `"nonce=123&market=SCC_BTC&side=BUY&price=0.00010000&amount=10"`, you would sign ALL of this exact input in SHA256, digest as HEX, and append the signature as the parameter: `signature=` at the end of your request.


### My Trades
> Returns a list of your last trades in a selected market, descending by timestamp, you may set a custom limit to the amount of returned trades, for example: `market=SCC_BTC&limit=100` returns your last 100 trades on the SCC_BTC market.
- Endpoint: `/exchange/spot/myTrades`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
(optional) market | the chosen market pair | SCC_BTC
(optional) limit | the maximum trades to return | 100
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

**Note:** `limit` can be left empty and/or removed completely from the query, which will use `100` as the default limit.

Example: `GET https://stakecube.io/api/v2/exchange/spot/myTrades?market=SCC_BTC&limit=100&nonce=123&signature=xxx`

---

### My Order History
> Returns a list of your historical orders of a chosen market and limit, descending by timestamp, for example: `market=SCC_BTC&limit=100` returns your last 100 orders done on the SCC_BTC market.
- Endpoint: `/exchange/spot/myOrderHistory`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
(optional) market | the chosen market pair | SCC_BTC
(optional) limit | the maximum trades to return | 100
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

**Note:** `limit` can be left empty and/or removed completely from the query, which will use `100` as the default limit.

Example: `GET https://stakecube.io/api/v2/exchange/spot/myOrderHistory?market=SCC_BTC&limit=100&nonce=123&signature=xxx`

---

### My Open Orders
> Returns a list of your currently open orders, their IDs, their market pair, and other relevent order information.
- Endpoint: `/exchange/spot/myOpenOrder`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
(optional) market | the chosen market pair | SCC_BTC
(optional) limit | the maximum trades to return | 100
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

Example: `GET https://stakecube.io/api/v2/exchange/spot/myOpenOrder?nonce=123&signature=xxx`

---

### Order
> Creates an exchange limit order on the chosen market, side, price and amount, for example: `market=SCC_BTC&side=BUY&price=0.00010000&amount=10` would buy 10 SCC with BTC at a price of 10k sats per SCC.
- Endpoint: `/exchange/spot/order`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
(required) market | the chosen market pair | SCC_BTC
(required) side | the market side to place the order | `BUY` or `SELL`
(required) price | the price in the Base coin | `0.00010000` (BTC)
(required) amount | the amount in the Market coin | `10` (SCC)
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

**Note:** The `side` must be in full caps, e.g: `BUY` or `SELL`, not `buy` or `sell`.

Example: `POST https://stakecube.io/api/v2/exchange/spot/order`

POST Body: `market=SCC_BTC&side=BUY&price=0.00010000&amount=10&nonce=123&signature=xxx`

---

### Cancel
> Cancels an order by it's unique ID, for example; You place an order, it's returned ID is 123, you then cancel with the parameters: `orderId=123` - the order is cancelled and removed from it's market.
- Endpoint: `/exchange/spot/cancel`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
(required) orderId | the order's unique ID | 123
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

Example: `POST https://stakecube.io/api/v2/exchange/spot/cancel`

POST Body: `orderId=123&nonce=123&signature=xxx`

---

### Cancel All
> Cancels all orders in a chosen market pair, e.g: `market=SCC_BTC` will cancel ALL limit orders on the SCC_BTC market pair.
- Endpoint: `/exchange/spot/cancelAll`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
(required) market | the chosen market pair | SCC_BTC
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

Example: `POST https://stakecube.io/api/v2/exchange/spot/cancelAll`

POST Body: `market=SCC_BTC&nonce=123&signature=xxx`
