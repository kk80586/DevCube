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