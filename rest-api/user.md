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

### Account
> Returns general information about your StakeCube account, including wallets, balances, fee-rate in percentage and your account username.
- Endpoint: `/user/account`
- Type: `GET`

Parameter | Description | Example
------------ | ------------- | -------------
(required) nonce | the incremental integer | UNIX Timestamp
(required) signature | the parameter's HMAC signature | HEX Format

Example: `GET https://stakecube.io/api/v2/user/account?nonce=123&signature=xxx`