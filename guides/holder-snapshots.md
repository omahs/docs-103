---
description: Learn how to use Airstack to fetch token holders at a specific point in time.
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: false
  pagination:
    visible: true
---

# ⛓ Holder Snapshots

[Airstack](https://airstack.xyz) provides easy-to-use [Snapshots API](../api-references/api-reference/snapshots-api.md)s for fetching token holders at a specific point in time. This feature set is currently available for the Base blockchain and will be available soon for Ethereum and Polygon.&#x20;

## Table Of Contents

In this guide you will learn how to use [Airstack](https://airstack.xyz) to:

* [Get ERC20 Token Holders at a Specified Time](holder-snapshots.md#get-erc20-token-holders-snapshot-at-a-specified-time) (`date`, `timestamp` or `blockNumber`)
* [Get NFT Collection Holders at a Specified Time](holder-snapshots.md#get-nft-collection-holders-snapshot-at-a-specified-time) (`date`, `timestamp` or `blockNumber`)

## Pre-requisites

* An [Airstack](https://airstack.xyz/) account (free)
* Basic knowledge of GraphQL

## Get Started

#### JavaScript/TypeScript/Python

If you are using JavaScript/TypeScript or Python, Install the Airstack SDK:

{% tabs %}
{% tab title="npm" %}
**React**

```sh
npm install @airstack/airstack-react
```

**Node**

```sh
npm install @airstack/node
```
{% endtab %}

{% tab title="yarn" %}
**React**

```sh
yarn add @airstack/airstack-react
```

**Node**

```sh
yarn add @airstack/node
```
{% endtab %}

{% tab title="pnpm" %}
**React**

```sh
pnpm install @airstack/airstack-react
```

**Node**

```sh
pnpm install @airstack/node
```
{% endtab %}

{% tab title="pip" %}
```sh
pip install airstack
```
{% endtab %}
{% endtabs %}

Then, add the following snippets to your code:

{% tabs %}
{% tab title="React" %}
```jsx
import { init, useQuery } from "@airstack/airstack-react";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const Component = () => {
  const { data, loading, error } = useQuery(query);

  if (data) {
    return <p>Data: {JSON.stringify(data)}</p>;
  }

  if (loading) {
    return <p>Loading...</p>;
  }

  if (error) {
    return <p>Error: {error.message}</p>;
  }
};
```
{% endtab %}

{% tab title="Node" %}
```javascript
import { init, fetchQuery } from "@airstack/node";

init("YOUR_AIRSTACK_API_KEY");

const query = `YOUR_QUERY`; // Replace with GraphQL Query

const { data, error } = await fetchQuery(query);

console.log("data:", data);
console.log("error:", error);
```
{% endtab %}

{% tab title="Python" %}
```python
import asyncio
from airstack.execute_query import AirstackClient

api_client = AirstackClient(api_key="YOUR_AIRSTACK_API_KEY")

query = """YOUR_QUERY""" # Replace with GraphQL Query

async def main():
    execute_query_client = api_client.create_execute_query_object(
        query=query)

    query_response = await execute_query_client.execute_query()
    print(query_response.data)

asyncio.run(main())
```
{% endtab %}
{% endtabs %}

#### Other Programming Languages

To access the Airstack APIs in other languages, you can use [https://api.airstack.xyz/gql](https://api.airstack.xyz/gql) as your GraphQL endpoint.

## **🤖 AI Natural Language**[**​**](https://xmtp.org/docs/tutorials/query-xmtp#-ai-natural-language)

[Airstack](https://airstack.xyz/) provides an AI solution for you to build GraphQL queries to fulfill your use case easily. You can find the AI prompt of each query in the demo's caption or title for yourself to try.

<figure><img src="../.gitbook/assets/NounsClip_060323FIN3.gif" alt=""><figcaption><p>Airstack AI (Demo)</p></figcaption></figure>

## Get ERC20 Token Holders Snapshot at a Specified Time

You can fetch all the tokens holders of an ERC20 at a specified time by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing the ERC20 token address to `tokenAddress` input and time input to either `date`, `timestamp` or `blockNumber`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/7uE9NJTAz7" %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Snapshots(
    input: {
      filter: {
        date: {_eq: "2023-12-01"},
        tokenType: {_eq: ERC20},
        tokenAddress: {_eq: "0x4158734d47fc9692176b5085e0f52ee0da5d47f1"}
      },
      blockchain: base
    }
  ) {
    Snapshot {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          profileName
          dappName
        }
        xmtp {
          isXMTPEnabled
        }
      }
      startBlockNumber
      startBlockTimestamp
      endBlockNumber
      endBlockTimestamp
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
<pre class="language-json"><code class="lang-json">{
  "data": {
    "Snapshots": {
      "Snapshot": [
        {
          "owner": {
            "addresses": [
              "0xba0064a452cb7ad290f834d21e17978c75ad42c2"
            ],
            "domains": null,
            "socials": null,
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "startBlockNumber": 6445937,
          "startBlockTimestamp": "2023-11-11T05:40:21Z",
<strong>          "endBlockNumber": -1, // -1 indicate that the user still hold the token at present
</strong>          "endBlockTimestamp": "2023-12-14T15:58:59Z"
        },
        {
          "owner": {
            "addresses": [
              "0xcea2232971ad7b98436bd31621bc0b7e32d88daa"
            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          },
          "startBlockNumber": 6243355,
          "startBlockTimestamp": "2023-11-06T13:07:37Z",
<strong>          "endBlockNumber": 7534630, // this holder last held the token at block 7534630
</strong>          "endBlockTimestamp": "2023-12-06T10:30:07Z"
        },
        {
          "owner": {
            "addresses": [
              "0x16b5cd246f9b2eb27318adb9feb015ded2fc678d"
            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          },
          "startBlockNumber": 5329231,
          "startBlockTimestamp": "2023-10-16T09:16:49Z",
<strong>          "endBlockNumber": -1, // -1 indicate that the user still hold the token at present
</strong>          "endBlockTimestamp": "2023-12-14T15:58:59Z"
        },
        // Other token holders on 2023-12-01
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjExMzgwYWRkNTFhODliNTU1MDg2MjBiNjBiY2YxNTIxIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
</code></pre>
{% endtab %}
{% endtabs %}

## Get NFT Collection Holders Snapshot at a Specified Time

You can fetch all the tokens holders of an NFT collection at a specified time by using [`Snapshots`](../api-references/api-reference/snapshots-api.md) API and providing the NFT collection address to `tokenAddress` input and time input to either `date`, `timestamp` or `blockNumber`:

### Try Demo

{% embed url="https://app.airstack.xyz/query/GAQzpQLdnh" %}
Show token holder snapshots of NFT collection 0xf0d0df7142f60f7f3847463a509fd8969e3e3a27 at 2023-12-01
{% endembed %}

### Code

{% tabs %}
{% tab title="Query" %}
```graphql
query MyQuery {
  Snapshots(
    input: {
      filter: {
        date: {_eq: "2023-12-01"},
        tokenType: {_in: [ERC721, ERC1155]},
        tokenAddress: {_eq: "0xf0d0df7142f60f7f3847463a509fd8969e3e3a27"}
      },
      blockchain: base
    }
  ) {
    Snapshot {
      owner {
        addresses
        domains {
          name
          isPrimary
        }
        socials {
          profileName
          dappName
        }
        xmtp {
          isXMTPEnabled
        }
      }
      tokenId
      tokenNft {
        contentValue {
          image {
            extraSmall
            large
            medium
            original
            small
          }
        }
        metaData {
          name
          description
          attributes {
            value
            trait_type
            maxValue
            displayType
          }
        }
      }
      startBlockNumber
      startBlockTimestamp
      endBlockNumber
      endBlockTimestamp
    }
    pageInfo {
      hasNextPage
      hasPrevPage
      nextCursor
      prevCursor
    }
  }
}
```
{% endtab %}

{% tab title="Response" %}
```json
{
  "data": {
    "Snapshots": {
      "Snapshot": [
        {
          "owner": {
            "addresses": [
              "0xa8c90749ee4fdf4089ac4303ad8d414d34484896"
            ],
            "domains": null,
            "socials": null,
            "xmtp": null
          },
          "tokenId": "204",
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lZ/nmzcpIdH9YZx+2/nYeJg==/extra_small",
                "large": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lZ/nmzcpIdH9YZx+2/nYeJg==/large",
                "medium": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lZ/nmzcpIdH9YZx+2/nYeJg==/medium",
                "original": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lZ/nmzcpIdH9YZx+2/nYeJg==/original_image",
                "small": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lZ/nmzcpIdH9YZx+2/nYeJg==/small"
              }
            },
            "metaData": {
              "name": "Tiny Based Frog #157",
              "description": "Tiny Based Frogs is a 100% fully on-chain NFT collection, featuring unfathomly based on-chain interactive mechanics.",
              "attributes": [
                {
                  "value": "1",
                  "trait_type": "Background",
                  "maxValue": null,
                  "displayType": null
                },
                // other attributes
              ]
            }
          },
          "startBlockNumber": 2415818,
          "startBlockTimestamp": "2023-08-09T22:43:03Z",
          "endBlockNumber": -1,
          "endBlockTimestamp": "2023-12-14T16:26:17Z"
        },
        {
          "owner": {
            "addresses": [
              "0x746006a055fe643b66ad733356545abeca85a37e"
            ],
            "domains": null,
            "socials": null,
            "xmtp": [
              {
                "isXMTPEnabled": true
              }
            ]
          },
          "tokenId": "456",
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lzBSXeSQBHCez2MAQFTCOKQ==/extra_small",
                "large": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lzBSXeSQBHCez2MAQFTCOKQ==/large",
                "medium": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lzBSXeSQBHCez2MAQFTCOKQ==/medium",
                "original": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lzBSXeSQBHCez2MAQFTCOKQ==/original_image",
                "small": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6lzBSXeSQBHCez2MAQFTCOKQ==/small"
              }
            },
            "metaData": {
              "name": "Tiny Based Frog #456",
              "description": "Tiny Based Frogs is a 100% fully on-chain NFT collection, featuring unfathomly based on-chain interactive mechanics.",
              "attributes": [
                {
                  "value": "10",
                  "trait_type": "Background",
                  "maxValue": null,
                  "displayType": null
                },
                // other attributes
              ]
            }
          },
          "startBlockNumber": 2866066,
          "startBlockTimestamp": "2023-08-20T08:51:19Z",
          "endBlockNumber": -1,
          "endBlockTimestamp": "2023-12-14T16:26:17Z"
        },
        {
          "owner": {
            "addresses": [
              "0x5f00875ab6942bd586dd23dbbd79af6a801527a4"
            ],
            "domains": [
              {
                "name": "ihopeimakeit.eth",
                "isPrimary": true
              },
              {
                "name": "tiredsoul.eth",
                "isPrimary": false
              },
              {
                "name": "itsprogrammed.eth",
                "isPrimary": false
              }
            ],
            "socials": null,
            "xmtp": null
          },
          "tokenId": "955",
          "tokenNft": {
            "contentValue": {
              "image": {
                "extraSmall": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6l4wCEm2V19SyjhVGUseA/yg==/extra_small",
                "large": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6l4wCEm2V19SyjhVGUseA/yg==/large",
                "medium": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6l4wCEm2V19SyjhVGUseA/yg==/medium",
                "original": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6l4wCEm2V19SyjhVGUseA/yg==/original_image",
                "small": "https://assets.airstack.xyz/image/nft/8453/XFzw/UlbS7qlyNJ2WtgMIL4xQ1NVVTlotBnG24o+BkpBQgsmZbauKjg87wnWfC6l4wCEm2V19SyjhVGUseA/yg==/small"
              }
            },
            "metaData": {
              "name": "Tiny Based Frog #939",
              "description": "Tiny Based Frogs is a 100% fully on-chain NFT collection, featuring unfathomly based on-chain interactive mechanics.",
              "attributes": [
                {
                  "value": "7",
                  "trait_type": "Background",
                  "maxValue": null,
                  "displayType": null
                },
                // other attributes
              ]
            }
          },
          "startBlockNumber": 2407705,
          "startBlockTimestamp": "2023-08-09T18:12:37Z",
          "endBlockNumber": -1,
          "endBlockTimestamp": "2023-12-14T16:26:17Z"
        },
        // other NFT holders at 2023-12-01
      ],
      "pageInfo": {
        "hasNextPage": true,
        "hasPrevPage": false,
        "nextCursor": "eyJMYXN0VmFsdWVzTWFwIjp7Il9pZCI6eyJWYWx1ZSI6IjQ1OTFhMTcwYmIwMzU2ZjMwNWQyMDg2NTMxZjFiNjkyIiwiRGF0YVR5cGUiOiJzdHJpbmcifX0sIlBhZ2luYXRpb25EaXJlY3Rpb24iOiJORVhUIn0=",
        "prevCursor": ""
      }
    }
  }
}
```
{% endtab %}
{% endtabs %}

## Developer Support

If you have any questions or need help regarding fetching holder snapshots data, please join our Airstack's [Telegram](https://t.me/+1k3c2FR7z51mNDRh) group.

## More Resources

* [Balance Snapshots Guides](balance-snapshots.md)
* [Token Balances Guides](token-balances/)
* [Token Holders Guides](token-holders/)
* [Combinations Guides](combinations/)
* [Tokens In Common Guides](tokens-in-common/)
* [Snapshots API](../api-references/api-reference/snapshots-api.md)