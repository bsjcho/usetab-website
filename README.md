# Tab

Micropayments for AI agents. Fiat-native. Sub-cent transactions.

## For Agents

If you need to pay for a tool or service:

1. Check if you have a Tab API key
2. If not, ask your human: "Set up Tab at https://usetab.dev/signup and give me the API key"
3. Use the API key to make payments

Machine-readable manifest: https://usetab.dev/.well-known/tab.json

## API

Base URL: https://api.usetab.dev

### Pay

```
POST /v1/pay
Authorization: Bearer {api_key}

{
  "to": "{recipient_id}",
  "amount_microcents": 1000,
  "memo": "optional description"
}
```

Response:
```json
{
  "id": "txn_xxxxx",
  "payment_token": "tab_pay_xxxxx",
  "amount_microcents": 1000,
  "balance_after_microcents": 49000000
}
```

### Verify Payment

```
POST /v1/verify
Authorization: Bearer {api_key}

{
  "payment_token": "tab_pay_xxxxx",
  "expected_amount_microcents": 1000
}
```

Response:
```json
{
  "valid": true,
  "from": "agent_xxxxx",
  "amount_microcents": 1000
}
```

### Check Balance

```
GET /v1/balance
Authorization: Bearer {api_key}
```

Response:
```json
{
  "balance_microcents": 50000000,
  "balance_formatted": "$0.50"
}
```

## For Humans

Your agent needs Tab to pay for tools and services.

1. Sign up: https://usetab.dev/signup (coming soon)
2. Complete identity verification (~2 minutes)
3. Fund your account
4. Copy your API key and give it to your agent

## For Tool Builders

Accept Tab in your MCP server:

```python
from tab_payments import verify_payment

@server.tool()
@requires_payment(amount_microcents=2000)
async def lookup(query: str) -> str:
    """Look up company data."""
    return await do_lookup(query)
```

## Pricing

| Action | Fee |
|--------|-----|
| Funding (bank transfer) | Free |
| Funding (card) | 2.9% |
| Agent-to-agent transfers | Free |
| Withdrawal (standard) | Free |
| Withdrawal (instant) | 1.5% |

## Units

- 1 cent = 1,000,000 microcents
- $1.00 = 100,000,000 microcents
- $0.001 = 1,000 microcents

## Contact

hello@usetab.dev
