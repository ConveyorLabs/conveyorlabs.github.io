---
label: API
icon: broadcast
---

# Conveyor API Developer Docs

!!! Important
If you are a developer who is considering integrating us as your swap provider, please reach out to us first! There are several nuances in implementing our swap API, and we'd love to help walk you through any problems you might have while integrating. Our goal is to ensure that swaps are 100% reliable.

<strong>Telegram:</strong> <a href="https://t.me/conveyorlabs" target="_blank">@Conveyorlabs</a></br>
<strong>Discord:</strong> conveyorlabs
!!!

The Conveyor API allows users to fetch optimal swap routes from one token to another along with executable transaction data. Currently Conveyor supports Ethereum, BSC, Polygon, Arbitrum, Base, and Optimism.

# Querying the API

Endpoint: `https://api.conveyor.finance`

### Request Body

Example | Schema `application/json`

```js

{
    "tokenIn": "0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE",
    "tokenOut": "0xd6df932a45c0f255f85145f286ea0b292b21c90b",
    "tokenInDecimals": 18,
    "tokenOutDecimals": 18,
    "amountIn": "1000000000000000",
    "slippage": "50",
    "chainId": 137,
    "recipient": "0xD65e57395288AA88f99F8e52D0A23A551E0Ad6Ac",
    "referrer": "0",
    "partner": "Caddi"
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
|     `partner`      |      String       | The name of the protocol/dapp integrating the API (EG: "Caddi", "Aori", etc...)                                                                                                                       |

!!!
If the native gas asset for the chain (Ether, BNB, Matic, etc..) is either the `tokenIn` or `tokenOut`, pass in `0xEeeeeEeeeEeEeeEeEeEeeEEEeeeeEeeeeeeeEEeE`
!!!

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
    "recipient": "0xD65e57395288AA88f99F8e52D0A23A551E0Ad6Ac",
    "referrer": "1",
    "partner": "Example"
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
            "to": ConveyorRouterV1,
            "gas": "221000",
            "nonce": 0,
            "value": "10000000000000000000",
            "data": "0x27fa6f42000....."
        },
        "info": {
            "amountOutMin": "5507351",
            "amountOut": "5562981",
            "affiliateAggregator": "PARASWAP",
            "affiliateGas": "221000",
            "conveyorGas": "191294"
        },
        "chainId": 137,
        "currentBlockNumber": 18028299,
        "errorStatus": {
            "revert": "Error(string)"
        },
        "tax": {
            "tokenOutSellTax": 0,
            "tokenOutBuyTax": 0,
            "tokenInSellTax": 0,
            "tokenInBuyTax": 0
        },
    }
}

```

!!! errorStatus
errorStatus response appears when the `eth_call` fails to simulate the transaction. Signing the transaction where errorStatus exists will likely result in the transaction reverting.
!!!

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

!!! Important
The [ConveyorRouterV1](/smartcontracts/) must have approval to swap the`amountIn`of the`tokenIn`on behalf of the`msg.sender`, else the transaction will revert when executing the `txData`, you can get this via `body.tx.to` in the response
!!!
