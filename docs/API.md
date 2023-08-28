---
label: API
icon: shield-check
---

# Conveyor API Developer Docs

The Conveyor API allows users to fetch optimal swap routes from one token to another along with executable transaction data. Currently Conveyor supports Ethereum, BSC, Polygon, Arbitrum, and Optimism.

# Querying the API

Endpoint: `https://api.conveyor.finance`

### Request Body

Example | Schema `application/json`

```js

{
    amountIn: "100000000000000000",
    slippage: "50",
    tokenIn: "0xeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeeee",
    tokenInDecimals: 18,
    tokenOutDecimals: 6,
    tokenOut: "0x2791Bca1f2de4661ED88A30C99A7a9449Aa84174",
    recipient: "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    chainId: 137,
    referrer: "1"
}

```

Breakdown

|        Name        |       Type        | Description                                                                                                                                                                                           |
| :----------------: | :---------------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     `amountIn`     |      String       | The total value being quoted on the tokenIn                                                                                                                                                           |
|     `slippage`     |      String       | Allowable % price impact on the transaction in [Bips](https://www.investopedia.com/ask/answers/what-basis-point-bps/). </br> Note: The API will use this % to set the `amountOutMin` in the calldata. |
|     `tokenIn`      |      String       | The address of the token being swapped. Burn address if swapping Native asset on chain                                                                                                                |
| `tokenInDecimals`  | Number (optional) | The decimals of `tokenIn`                                                                                                                                                                             |
|     `tokenOut`     |      String       | The address of the token being swapped into. Burn address if swapping Native asset on chain                                                                                                           |
| `tokenOutDecimals` | Number (optional) | The decimals of `tokenOut`                                                                                                                                                                            |
|    `recipient`     |      String       | Address of the receiver of `tokenOut`                                                                                                                                                                 |
|     `chainId`      |      Number       | The Network Id of the chain                                                                                                                                                                           |
|     `referrer`     | String (Optional) | The referralId in the `ConveyorRouterV1` contract to be compensated 30% of the `protocolFee` on behalf of the swapper.                                                                                |

### CURL

```
curl -X POST   -H "Content-Type: application/json"   -d '
    {
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xd6df932a45c0f255f85145f286ea0b292b21c90b",
    "tokenInDecimals": 18,
    "tokenOutDecimals": 18,
    "amountIn": "1000000000000000",
    "slippage": "50",
    "chainId": 137,
    "recipient": "0x0000000000000000000000000000000000000000"
}'   https://api.conveyor.finance

```

### Response

Example | Schema `application/json`

```ts

{
    "statusCode": 200,
    "headers": {
        "Access-Control-Allow-Headers": "Content-Type",
        "Access-Control-Allow-Methods": "OPTIONS,POST,GET",
        "Access-Control-Allow-Origin": "*"
    },
    "body": {
        "message": "GET request processed successfully",
        "tx": {
            "from": "0x0000000000000000000000000000000000000000",
            "to": "0x3642189b7754302df84b6f3fe1ae34d2026647a7",
            "gas": "221000",
            "nonce": 0,
            "value": "10000000000000000000",
            "data": "0x27fa6f42000....."
        },
        "info": {
            "amountOutMin": "5507351",
            "amountOut": "5562981",
            "affiliateAggregator": "PARASWAP",
            "affiliateGas": "191294",
            "conveyorGas": "221000"
        }
    }
}

```

| Code | Description                                   |
| :--: | :-------------------------------------------- |
| 200  | Successfully generated the transaction data   |
| 400  | Failed to find a liquidity route for the swap |

### Typescript Code Example

```ts
function queryApi({
  tokenIn,
  tokenOut,
  tokenInDecimals,
  tokenOutDecimals,
  amountIn,
  slippage,
  chainId,
  recipient,
}) {
  const resp = await fetch("https://api.conveyor.finance/", {
    method: "POST",
    body: JSON.stringify({
      tokenIn,
      tokenOut,
      tokenInDecimals,
      tokenOutDecimals,
      amountIn,
      slippage,
      chainId,
    }),
  })
    .then((r) => r.json())
    .then((r) => r.body)
    .catch((e) => console.error(e));

  //Grab the transaction data from the response
  const txData = resp.tx.data;

  //Grab a gas estimate for a gas limit on the tx.
  const gasLimit = resp.tx.gas;

  //Sign and send the transaction through ethers or web3 provider.
}
```
