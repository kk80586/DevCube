# :unlock: **Public APIs (No Auth Required)**

---

### MineCube Info
> Returns general real-time information about MineCube, such as the Total workers, Available workers, worker price in USD + SCC, and information on the payout-in-progress status.
- Endpoint: `/minecube/info`

Example: `GET https://stakecube.io/api/v2/minecube/info`

---

### Miners
> Returns the current list of mining rigs (such as ASICs and GPUs) belonging to MineCube, along with their individual statuses and stats, an optional 'coin' param can be provided to list mining rigs that only belong to that coin, e.g: 'ETH' would return only ETH mining rigs.
- Endpoint: `/minecube/miner`

Parameter | Description | Example
------------ | ------------- | -------------
(optional) coin | the coin being mined | `BTC`, `DASH`, `ETH` or `LTC`

Example: `GET https://stakecube.io/api/v2/minecube/miner?coin=ETH`

---

## :lock: **Private APIs (Authentication Required)**

For security, our private APIs require two additional `body` fields on the majority of endpoints (with a few exceptions), these fields being:
Field | Description
------------ | -------------
(required) nonce | any integer larger than the last API call's integer (millisecond timestamp)
(required) signature | a HMAC signature of the full `body` contents in URL-encoded format

In addition, your API key should be included as a **header**, under this name: `X-API-KEY`

For new API users, the nonce will start at `0`, you may provide any integer higher than that as the nonce, for example: your nonce is 0, so your next call uses nonce 1, next call uses nonce 2... and repeat. You may also use timestamps for this purpose, but ensure it's a millisecond timestamp, otherwise a second-based timestamp limits you to a call every second maximum.

The HMAC signature is composed of all the URL-encoded parameters of your API call's body, so for example; you're calling `/exchange/spot/order` which contains the `body` parameters of: `"nonce=123&market=SCC_BTC&side=BUY&price=0.00010000&amount=10"`, you would sign ALL of this exact input in SHA256, digest as HEX, and append the signature as the parameter: `signature=` at the end of your request.

---

### Set Payout Coin
> Sets a preferred coin for future payouts, future payouts will be paid in the coin last selected by this endpoint.
- Endpoint: `/minecube/setPayoutCoin`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
(required) target | the payout coin | `BTC`, `SCC`, `DASH`, `LTC`, `ETH` or `DOGE`
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

Example: `POST https://stakecube.io/api/v2/minecube/setPayoutCoin`

POST Body: `target=SCC&nonce=123&signature=xxx`

---

### Buy Workers
> Buys a specified amount of workers using the chosen payment method.
- Endpoint: `/minecube/buyWorker`
- Type: `POST`

Parameter | Description | Example
------------ | ------------- | -------------
(required) method | the payment method | `SCC` or `CREDITS`
(required) amount | the worker quantity | 10
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

Example: `POST https://stakecube.io/api/v2/minecube/buyWorker`

POST Body: `method=SCC&amount=10&nonce=123&signature=xxx`