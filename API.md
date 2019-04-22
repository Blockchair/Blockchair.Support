# [Blockchair.com](https://blockchair.com/) API

### API v.2 documentation
* English: [API_DOCUMENTATION_EN.md](API_DOCUMENTATION_EN.md).
* Russian: [API_DOCUMENTATION_RU.md](API_DOCUMENTATION_RU.md).

### Changelog

* v.2.0.21 - Apr 22nd, 2019
    * We've added an ability to query multiple addresses at once. The endpoint is `https://api.blockchair.com/{:chain}/dashboards/addresses/{:addr1},{:addr2},...` (supported for BTC, BCH, LTC, DASH, BSV, and DOGE). E.g., now, if you need to retrieve information about 3 different addresses, you wouldn't need to make 3 separate queries, you'd need to query just `https://api.blockchair.com/bitcoin/dashboards/addresses/1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa,12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX,1HLoD9E4SDFFPDiYfNYnkBLQ85Y51J3Zb1`. The response contains information on the set of addresses (total balance, total transaction count, etc.), information about each address, and the list of the latest 100 transactions for this set (iterable with `&offset=N`). The maximum number of addresses in one query is 100 (higher limits are available for our premium users). Querying multiple addresses at once works much faster (e.g. it's almost 95 times faster to query 100 addresses via the new endpoint compared to making separate requests).
* v.2.0.20 - Apr 19th, 2019
    * Now it's possible to query state changes caused by a block for all chains we support except for ETH. The endpoint is `https://api.blockchair.com/{:chain}/state/changes/block/{:block_id)`. The response contains an array where the keys are addresses which were affected by the block, and the values are balance changes. Example: `https://api.blockchair.com/bitcoin/state/changes/block/1` returns `12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX => 5000000000` which means that the only state change caused by this block was rewarding the miner with 50 bitcoins. This is useful if you need to track balance changes for a lot of addresses - you can now simply track state changes and find the needed addresses there instead of constantly retrieving information about the balances.
* v.2.0.19 - Apr 17th, 2019
    * Added alpha support for Ripple (see `Ripple support` in the docs)
    * Introducing Graph API for Ethereum (a possibility to find connections between two Ethereum addresses), see `Ethereum graph` in the docs) - it's in private alpha test mode
    * If you're constantly hitting Error 402 (i.e. by making too many requests per minute to our free API), you'll now receive Error 429 which means that your IP is banned for an hour. Not honoring our limits may result in a permanent ban
    * Fixed a couple of minor bugs in our Ethereum engine. We'll be rolling out an updated engine the next week, there shouldn't be any compatibility breaking changes
* v.2.0.18 - Apr 2nd, 2019
    * Added biggest transactions over the last 24h to `https://api.blockchair.com/{chain}/stats` calls (`largest_transaction_24h` key);
    * Updated xpub support to v.b3 (see `xpub support` in the docs, there are some breaking changes); 
    * Updated aggregation support to v.b4 (see `Data aggregation support` in the docs)
    * We're **DEPRECATING** usage of unsupported parameters in GET and POST requests to our API endpoints (e.g. `https://api.blockchair.com/stats?myarbitrarykey=1234` might result in a `400 Bad Request` error);
    * Previously DEPRECATED API v.1 has been shut down
    * Previously DEPRECATED undocumented `?export=` functionality now requires an API key (apply at <`info@blockchair.com`>) for everything except:
        * Aggregated results
        * `blocks` tables across all blockchains we support
* v.2.0.17 - Mar 14th, 2019
    * Added support for Bitcoin SV nodes, they are now separate from Bitcoin Cash nodes. Endpoint: `https://api.blockchair.com/bitcoin-sv/nodes`.
* v.2.0.16 - Mar 13th, 2019
    * Added support for ypub and zpub for Bitcoin and Litecoin in test mode. See `xpub support` in the docs.
* v.2.0.15 - Mar 12th, 2019
    * Added Dash support in test mode. We're supporting all the features for DASH as we support for other Satoshi-like coins. Additional columns: `blocks.cbtx`, `transactions.type` (possible types: `simple`, `proregtx`, `proupservtx`, `proupregtx`, `prouprevtx`, `cbtx`, `qctx`, `subtxregister`, `subtxtopup`, `subtxresetkey`, `subtxcloseaccount`), `transactions.is_instant_lock`, `transactions.is_special` (`true` for all transaction types except `simple`), `transactions.special_json` (contains special transaction data encoded in json). E.g.: `https://api.blockchair.com/dash/blocks`
* v.2.0.14 - Mar 6th, 2019
    * Added xpub support in test mode. There's now support for retrieving info about multiple addresses using xpub keys. Use `https://api.blockchair.com/{chain}/dashboards/xpub/{xpub}`. See `xpub support` in the docs.
    * Extended data aggregation abilities (still in test mode), see `Data aggregation support` in the docs. Now it's possbile to find correlations with price, and use special functions (e.g. to calculate SegWit adoption). 
    * We're DEPRECATING API v.1 and will be shutting it down on April 1st, 2019.
    * We're DEPRECATING undocumented `?export=` functionality when exporting large datasets without an API key. This feature will be documented in one of the next updates.
    * Full support for `CREATE2` in Ethereum (see `https://blockchair.com/ethereum/calls?q=type(create2)#`)
    * When using CSV/TSV API (undocumented `?export=` functionality) amounts in USD are now shown as in the JSON API version (previously you had to divide them by 10000). `bitcoin.outputs.type`, `ethereum.transaction.type`, and `ethereum.calls.type` now yield strings (e.g. `pubkeyhash` instead of `2`).
* v.2.0.13 - Feb 13th, 2019
    * Added support for Cyrillic characters in fulltext search, e.g. `https://api.blockchair.com/bitcoin/outputs?q=script_bin(~привет)`
* v.2.0.12 - Feb 12th, 2019
    * Fixed a bug in Ethereum where some contract creations were erroneously shown as failed (thanks Daniel Luca for noticing) 
* v.2.0.11 - Feb 5th, 2019
    * We're changing behavior of our `mempool` tables (for all supported coins except for Ethereum): now they don't contain the contents of the latest block (it was quite a clumsy thing to have both mempool transactions and transactions from the latest block in this table, but we've rebuilt our engine, so now `mempool` tables contain mempool content only, and it finally makes sense!). That means:
        * `{chain}/mempool/blocks` is deprecated. Hint: if you used `mempool/blocks` to get info about the latest block you can simply switch to using `blocks?limit=1`, e.g. `https://api.blockchair.com/bitcoin/blocks?limit=1`
        * `{chain}/mempool/transactions` and `{chain}/mempool/outputs` now don't contain info from the latest block, while `{chain}/transactions` and `{chain}/outputs` do
        * Before this update when using (undocumented) `export` functionality there was no information about the latest block at all, now there is
        * The same change to Ethereum will come in one of the next updates
    * Dogecoin is out of beta.
    * Bitcoin SV is out of beta. Please note there's still a possibility that we won't be able to offer some functionality in the long term if blocks suddenly become larger than 1 exabyte, we're still waiting for a more clear development roadmap.
* v.2.0.10 - Jan 29th, 2019
    * Added Dogecoin support in test mode (see `Dogecoin support` in the docs)
* v.2.0.9 - Dec 13th, 2018
    * Added Bitcoin SV support in test mode (see `Bitcoin SV support` in the docs); updated aggregation abilities (see `Data aggregation support` in the docs)
* v.2.0.8 - Nov 26th, 2018
    * Added the ability to retrieve raw transaction data in hex, see `Retrieving raw transactions` in the docs
* v.2.0.7 - Nov 22th, 2018
    * Now it's possible to broadcast transactions using our API, see `Broadcasting transactions` in the docs
* v.2.0.6 - Oct 8th, 2018
    * Added data aggregation of blockchain data in beta mode, see `Data aggregation support` in the docs
* v.2.0.5 - Oct 8th, 2018
    * Fixed bug where `balance` and `received` for bitcoin[-cash]|litecoin addresses in the `{chain}/dashboards/address/{address}` call were calculated wrong if there were specific unconfirmed transactions
* v.2.0.4 - Oct 3rd, 2018
    * Added some new useful fields to `{chain}/stats` calls
* v.2.0.3 - Sep 18th, 2018
    * Added `context.api.tested_features` with the list of features our API supports, but with no guarantee for backward compatibility if updated. Added Omni Layer and Wormhole support in testing mode (see "Tested features changelog")
* v.2.0.2 - Sep 9th, 2018
    * Added `address.contract_created` to the `ethereum/dashboards/address/{A}` call
* v.2.0.1 - Sep 1st, 2018
    * Added Litecoin support
* v.2.0.0
    * Migrating from API v.1 to API v.2 (see the docs)

### Support

* E-mail: [info@blockchair.com](mailto:info@blockchair.com)
* Telegram chat: [@Blockchair](https://telegram.me/Blockchair)
* Twitter: [@Blockchair](https://twitter.com/Blockchair)
