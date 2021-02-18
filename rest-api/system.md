# :unlock: **Public APIs (No Auth Required)**

---

### Rate Limits
> Returns rate limit information, the API system uses 'weight' as a measure of system usage per-call, each API call contains response headers such as; `x-mbx-used-weight-<timeframe>: xxx`, and as an API user you should take active care to stay under the rateLimits, or receive temporary penalties.
- Endpoint: `/system/rateLimits`

Example: `GET https://stakecube.io/api/v2/system/rateLimits`