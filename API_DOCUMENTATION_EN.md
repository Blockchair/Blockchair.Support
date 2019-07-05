## [Blockchair.com](https://blockchair.com/) API v.2.0.31 Documentation

<img src="https://blockchair.com/images/logo_full.png" alt="Logo" width="250"/>

### Table of contents

+ [Changelog](#link_changelog)
  + [Tested features changelog](#link_testedfeatureschangelog)
+ [General Provisions](#link_generalprovisions)
+ [Dashboard calls](#link_dashboardcalls) (Retrieve information about various Bitcoin, Ethereum, Bitcoin Cash, Litecoin, Dash, Bitcoin SV, Dogecoin, Groestlcoin entities)
  + [Block](#link_block)
  + [Uncle](#link_uncle) (Ethereum only)
  + [Transaction](#link_transaction)
  + [Address](#link_bitcoinaddress)
  + [Stats](#link_chainstats)
  + [General stats](#link_stats)
+ [Infinitable Calls (blockhain tables)](#link_infinitablecalls) (Filter and sort blockchain data)
    + Bitcoin, Bitcoin Cash, Litecoin, Dash, Bitcoin SV, Dogecoin, Groestlcoin:
      + [Blocks](#link_bitcoinblocks) (table)
      + [Transaction](#link_bitcointransactions) (table)
      + [Outputs](#link_bitcoinoutputs) (table)
      + [Addresses](#link_bitcoinaddresses) (view)
    + Ethereum:
      + [Blocks](#link_ethereumblocks) (table)
      + [Uncles](#link_ethereumuncles) (table)
      + [Transactions](#link_ethereumtransactions) (table)
      + [Calls](#link_ethereumcalls) (table)
+ Misc
    + [API request example](#link_examples)
    + [Broadcasting transactions](#link_broadcasting)
    + [Retrieving raw transactions](#link_raw)
    + [Nodes](#link_nodes)
    + [State changes](#link_state)
    + [Support](#link_support)

### <a name="link_changelog"></a> Changelog

* v.2.0.31 - Jul 5th, 2019
     * Added two new keys to `bitcoin/stats` and `litecoin/stats` calls:
        * `next_retarget_time_estimate` yields an estimated timestamp of the next difficulty retarget
        * `next_difficulty_estimate` yeilds an estimated next difficulty value
        
       These keys are available for Bitcoin and Litecoin as other cryptos we support recalculate difficulty every block.   
* v.2.0.30 - Jul 4th, 2019
    * We're adding a new table called `addresses` containing the list of all addresses and their confirmed balances to all Bitcoin-like coins (Bitcoin, Bitcoin Cash, Litecoin, Dash, Bitcoin SV, Dogecoin, Groestlcoin). Unlike other "infinitables" (`blocks`, `transactions`, `outputs`) this table isn't live, it's automatically updated every 5 minutes, thus we're classifying it as an "infiniview", meaning it's not really a table, but a view over the `outputs` table. See [the documentation](#link_bitcoinaddresses) for this table. Here are some examples of how it can be used:
        * `api.blockchair.com/bitcoin/addresses` - show Bitcoin addresses with biggest balances (i.e. the rich list)
        * `api.blockchair.com/bitcoin/addresses?q=balance(100000000)` - show Bitcoin addresses having exactly 1 BTC on their balance
        * `api.blockchair.com/bitcoin/addresses?a=count()` - count the number of addresses with a non-zero balance
        * `api.blockchair.com/bitcoin/addresses?a=count()&q=balance(100000000..)` - count the number of addresses holding at least 1 BTC
        * `api.blockchair.com/bitcoin/addresses?a=sum(balance)&q=balance(100000000..)` - calculate how many bitcoins do the addresses from the previous example hold
        * `api.blockchair.com/bitcoin/addresses?a=median(balance)` - calculate the median balance
        
      Using this table makes it trivial to build various sorts of rich lists. It's now also possible to retrieve a full list of addresses and balances in one file (available only in our Private API solution). Please note that this table shouldn't be used for retrieving balances for a particular set of addresses, please use the `api.blockchair.com/{:chain}/dashboards/addresses/{:addr1}[,{:addr2}...]` dashboard endpoint instead.
* v.2.0.29 - Jun 30th, 2019
    * The [State changes](#link_state) feature now supports requesting potential state changes caused by mempool transactions. The endpoint is `https://api.blockchair.com/{:chain}/state/changes/mempool`. It's now possible to easily build an app watching for transactions incoming/outgoing to/from millions of addresses, see [an example](#link_state).
* v.2.0.28 - Jun 28th, 2019
    * Added support for Groestlcoin nodes. The endpoint is `https://api.blockchair.com/groestlcoin/nodes`. The `stats` endpoint (`https://api.blockchair.com/groestlcoin/stats`) now also shows the node count.
* v.2.0.27 - Jun 27th, 2019
    * Effective July 19th, there will be a new policy on using our Public API for both non-commercial and commercial projects. Please see [Applying for an API key first](#link_apikey) and apply for an API key before July 19th! Since this is a major compatibility-breaking change, `context.api.next_major_update` is set to `2019-07-19 18:07:19` (see [General Provisions](#link_generalprovisions))
    * Removed `ethereum.uncles.total_difficulty` column according to https://github.com/ethereum/go-ethereum/issues/19024
* v.2.0.26 - Jun 20th, 2019
    * Added `utxo` array showing available unspent transaction outputs for Bitcoin-like coins to the following endpoints:
        * `api.blockchair.com/{:chain}/dashboards/address/{:address}`
        * `api.blockchair.com/{:chain}/dashboards/addresses/{:address1}[,{:address2}…]`
        * `api.blockchair.com/{:chain}/dashboards/xpub/{:[xyz]pub}`
      
      Each array element has the following structure:
        * `block_id` - block number (`-1` for unconfirmed outputs)
        * `transaction_hash` - transaction hash
        * `index` - output index in the transaction (also known as the `vout` number)
        * `value` - output value in satoshi
        * `address` (only for `addresses` and `xpub` dashboards) - showing the output owner
      
      This new functionality **DEPRECATES** usage of an old method to retrieve the UTXO set for an address, namely `api.blockchair.com/{:chain}/outputs?q=recipient({:address}),is_spent(false)`. See the discussion with some examples here: https://github.com/Blockchair/Blockchair.Support/issues/192
    * The 3 above listed endpoints now also have a new way to iterate through the transaction list and the UTXO set. It's now possible to use `?limit=A,B` and `?offset=C,D` sections in the query. The first number affects the transaction list, the second number affects the UTXO set. If only one number is set, it affects both. The default `limit` is `100`. The maximum `limit` is `10000`. The default offset is `0`. The maximum offset is `1000000`. Here are some examples:
        * `api.blockchair.com/{:chain}/dashboards/address/{:address}` - shows address data with an array of 100 latest transactions and 100 latest UTXOs
        * `api.blockchair.com/{:chain}/dashboards/address/{:address}?limit=0` - if you require just some general stats like the address balance
        * `api.blockchair.com/{:chain}/dashboards/address/{:address}?limit=0,100` - if you need just the general stats and the UTXO set
        * `api.blockchair.com/{:chain}/dashboards/address/{:address}?limit=100,0` - if you need just the general stats and the transaction list
        * `api.blockchair.com/{:chain}/dashboards/address/{:address}?limit=100,0&offset=100,0` - ... and to iterate it
    * The `api.blockchair.com/{:chain}/dashboards/block/{:hash}|{:id}` endpoint now also has an iterable set of transaction hashes included in the block. The default `limit` is `100`. The maximum `limit` is `10000`. The default offset is `0`. The maximum offset is `1000000`. This feature implements https://github.com/Blockchair/Blockchair.Support/issues/189
* v.2.0.25 - Jun 19th, 2019
    * Added Groestlcoin support (it has SegWit support, so all API functionality available for Bitcoin and Litecoin is available for Groestlcoin as well).
    * Added `suggested_transaction_fee_per_byte_sat` key to `{:chain}/stats` calls. This value shows a good enough approximation of an optimal fee value suggested by our engine based on the current mempool state (in satoshi per byte) to get into the next block. Please note that for transactions less important for you this fee suggestion may be too high, while for very important transactions it may not be enough if you'll get unlucky because of the lack of new blocks. Supported for all coins except for Ripple and Ethereum for which we'll have a separate value suggesting an appropriate gas price value.
    * Updated xpub support to v.b5 (see `xpub support` in the docs: two bugs have been fixed); 
* v.2.0.24 - Jun 6th, 2019
    * Added an optional `countdowns` array to `{:chain}/stats` calls. If present, this array contains information about various upcoming events such as hard forks or reward halvings. There are two keys for each event: `event` which contains event description, and `time_left` showing how many seconds are left until the event occurs. Please note that the number of seconds is an approximate value because most events are triggered after a block at a specific height is mined, and since it's not possible to know for sure when a block becomes mined, we can only approximate that.
* v.2.0.23 - May 24th, 2019
    * Added support for Dash nodes. The endpoint is `https://api.blockchair.com/dash/nodes`. The `stats` endpoint (`https://api.blockchair.com/dash/stats`) now also shows the node count.
    * It's now possible to query Dash xpubs (see `xpub support` in the docs)
    * Dash is now out of beta on our platform
* v.2.0.22 - May 23rd, 2019
    * The state changes feature introduced in v.2.0.20 is now available for Ethereum. The endpoint is `https://api.blockchair.com/ethereum/state/changes/block/{:block_id}`. Please note that it shows only the balance changes caused by a block. Values are returned as strings because some wei values don't fit into int64.
    * Some optimizations to the Ethereum mempool processing - we now show a lot more unconfirmed transactions
* v.2.0.21 - Apr 22nd, 2019
    * We've added an ability to query multiple addresses at once. The endpoint is `https://api.blockchair.com/{:chain}/dashboards/addresses/{:addr1},{:addr2},...` (supported for BTC, BCH, LTC, DASH, BSV, and DOGE). E.g., now, if you need to retrieve information about 3 different addresses, you wouldn't need to make 3 separate queries, you'd need to query just `https://api.blockchair.com/bitcoin/dashboards/addresses/1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa,12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX,1HLoD9E4SDFFPDiYfNYnkBLQ85Y51J3Zb1`. The response contains information on the set of addresses (total balance, total transaction count, etc.), information about each address, and the list of the latest 100 transactions for this set (iterable with `&offset=N`). The maximum number of addresses in one query is 100 (higher limits are available for our premium users). Querying multiple addresses at once works much faster (e.g. it's almost 95 times faster to query 100 addresses via the new endpoint compared to making separate requests).
* v.2.0.20 - Apr 19th, 2019
    * Now it's possible to query state changes caused by a block for all chains we support except for ETH. The endpoint is `https://api.blockchair.com/{:chain}/state/changes/block/{:block_id}`. The response contains an array where the keys are addresses which were affected by the block, and the values are balance changes. Example: `https://api.blockchair.com/bitcoin/state/changes/block/1` returns `12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX => 5000000000` which means that the only state change caused by this block was rewarding the miner with 50 bitcoins. This is useful if you need to track balance changes for a lot of addresses - you can now simply track state changes and find the needed addresses there instead of constantly retrieving information about the balances.
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
        * `blocks` and `mempool/*` tables across all blockchains we support
* v.2.0.17 - Mar 14th, 2019
    * Added support for Bitcoin SV nodes, they are now separate from Bitcoin Cash nodes. Endpoint: `https://api.blockchair.com/bitcoin-sv/nodes`.
* v.2.0.16 - Mar 13th, 2019
    * Added support for ypub and zpub for Bitcoin and Litecoin in test mode. See `xpub support` in the docs.
* v.2.0.15 - Mar 12th, 2019
    * Added Dash support in test mode. We're supporting all the features for DASH as we support for other Satoshi-like coins. Additional columns: `blocks.cbtx`, `transactions.type` (possible types: `simple`, `proregtx`, `proupservtx`, `proupregtx`, `prouprevtx`, `cbtx`, `qctx`, `subtxregister`, `subtxtopup`, `subtxresetkey`, `subtxcloseaccount`), `transactions.is_instant_lock`, `transactions.is_special` (`true` for all transaction types except `simple`), `transactions.special_json` (contains special transaction data encoded in json). E.g.: `https://api.blockchair.com/dash/blocks`
* v.2.0.14 - Mar 6th, 2019
    * Added xpub support in test mode. There's now support for retrieving info about multiple addresses using xpub keys. Use `https://api.blockchair.com/{chain}/dashboards/xpub/{xpub}`. See `xpub support` in the docs.
    * Extended data aggregation abilities (still in test mode), see `Data aggregation support` in the docs. Now it's possbile to find correlations with price, and use special functions (e.g. to calculate SegWit adoption). 
    * We're **DEPRECATING** API v.1 and will be shutting it down on April 1st, 2019.
    * We're **DEPRECATING** undocumented `?export=` functionality when exporting large datasets without an API key. This feature will be documented in one of the next updates.
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
    * Added Dogecoin support in test mode
* v.2.0.9 - Dec 13th, 2018
    * Added Bitcoin SV support in test mode; updated [aggregation abilities](#data-aggregation-support-since-oct-8th)
* v.2.0.8 - Nov 26th, 2018
    * Added the ability to retrieve raw transaction data in hex, see [Retrieving raw transactions](#retrieving-raw-transactions)
* v.2.0.7 - Nov 22th, 2018
    * Now it's possible to broadcast transactions using our API, see [Broadcasting transactions](#broadcasting-transactions)
* v.2.0.6 - Oct 8th, 2018
    * Added data aggregation of blockchain data in beta mode, see `Data aggregation support` below
* v.2.0.5 - Oct 8th, 2018
    * Fixed bug where `balance` and `received` for bitcoin[-cash]|litecoin addresses in the `{chain}/dashboards/address/{address}` call were calculated wrong if there were specific unconfirmed transactions
* v.2.0.4 - Oct 3rd, 2018
    * Added some new useful fields to `{chain}/stats` calls
* v.2.0.3 - Sep 18th, 2018
    * Added `context.api.tested_features` with the list of features our API supports, but with no guarantee for backward compatibility if updated. Added Omni Layer and Wormhole support in testing mode (see the "Tested features changelog" below)
* v.2.0.2 - Sep 9th, 2018
    * Added `address.contract_created` to the `ethereum/dashboards/address/{A}` call
* v.2.0.1 - Sep 1st, 2018
    * Added Litecoin support

### <a name="link_testedfeatureschangelog"></a> Tested features changelog

##### Ripple support (since Apr 17th 2019)

* v.a1 - Apr 17th - Added alpha support for Ripple. We're not releasing the documentation for API yet, but we begin using it in our front end ourselves. If you're interested in participating in alpha test, drop us a line at <`info@blockchair.com`>

##### Ethereum graph (since Apr 17th 2019)

* v.a1 - Apr 17th - It's now possible to find connections between two Ethereum addresses using our API. Please note that since it's a resource consuming feature, it's available to our Private API users only (if you're interested in participating in alpha test, drop us a line at <`info@blockchair.com`>)

##### xpub support (since Mar 6th 2019)

* v.b5 - Jun 19th
    * Added support for Groestlcoin
    * Fixed a bug where some addresses were missing if there were huge gaps between used address (according the BIP44 / BIP32 standards the maximum gap is currently set to 20). Thanks to the Groestlcoin team for providing us with some great xpub examples.
    * Fixed a bug where some of xpubs containing a very large set of addresses (>100) returned a 500 error.
* v.b4 - May 24th
    * Added support for Dash
* v.b3 - Apr 2nd
    * Fixed a bug where unconfirmed balance didn't show up for xpubs
    * Some optimizations to the address calculation process have been made, now response is generated 2.5-3x times faster
    * Breaking change: paths for ypub, zpub (and possibly in the future for ltub, etc.) now all start with `https://api.blockchair.com/{chain}/dashboards/xpub/`, i.e. to use ypub functionality you'll need to use `https://api.blockchair.com/{chain}/dashboards/xpub/{ypub}` instead of `https://api.blockchair.com/{chain}/dashboards/ypub/{ypub}`
* v.b2 - Mar 13th - We're bringing support for ypub and zpub as well (for Bitcoin and Litecoin). The endpoints are `https://api.blockchair.com/{chain}/dashboards/ypub/{ypub}` and `https://api.blockchair.com/{chain}/dashboards/zpub/{zpub}` (where `{chain}` is one of these: `bitcoin`, `litecoin`)
* v.b1 - Mar 6th - There's now support for retrieving info about multiple addresses using xpub keys. Use `https://api.blockchair.com/{chain}/dashboards/xpub/{xpub}` (where `{chain}` is one of these: `bitcoin`, `bitcoin-cash`, `litecoin`, `bitcoin-sv`, `dogecoin`, e.g. `https://api.blockchair.com/bitcoin/dashboards/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz`). The response contains following keys:
    * `xpub` - information about group of addresses, including its current balance (`xpub.balance`)
    * `addresses` - array of addresses (returns the same schema as `https://api.blockchair.com/{chain}/dashboards/address/{addr}` with two exceptions: there's no `transaction_count` key, and there's `path key` showing xpub path)
    * `transactions` - hashes of the latest transactions, iterable using `?offset=N` 

##### Data aggregation support (since Oct 8th 2018)

* v.b5 - Jul 4th - Added an ability to aggregate over the `addresses table`. 
* v.b4 - Apr 2nd - Fixed a bug where some usd values under functions when exporting to TSV/CSV were multiplied by 10000. Use `&export=tsv` or `&export=csv` to export the dataset into the corresponding format, e.g.: `https://api.blockchair.com/bitcoin/transactions?a=date,avg(fee_usd)&q=time(2019-01-01..2019-04-01)&export=tsv` 
* v.b3 - Mar 6th
    * New function `price({ticker1}_{ticker2})` which shows the price if `date` (or one of: `week`, `month`, `year`) is also applied. E.g. it's now possible to build a chart showing correlation between price and transaction count: `https://api.blockchair.com/bitcoin/blocks?a=month,sum(transaction_count),price(btc_usd)`. Supported tickers: usd, btc, bch, bsv, eth, ltc, doge.
    * Output values now have correct types (e.g. `sum(transaction_count)` is now integer instead of string).
    * Now it's possible to use special functions and applying special filters to them. Two examples:
        * `https://api.blockchair.com/bitcoin/blocks?a=date,f(sum(witness_count)/sum(transaction_count))&q=time(2017-08-24..)` - calculate SegWit adoption
        * `https://api.blockchair.com/bitcoin/outputs?a=date,f(count()/count())&q=type(nulldata),time(2019-02)&aq=0:0` - calculate the percentage of nulldata outputs: the `?aq=0:0` section applies 0th condition to 0th function (NB: after that 0th condition isn't used in the `WHERE` clause)
* v.b2 - Dec 12th - Now it's possible to apply `?limit=` and `?offset=` sections to aggregated queries. `context.total_rows` now shows how many aggregated results are there, and `context.rows` shows how many are shown.
* v.b1 - Oct 8th - Bringing the ability to obtain aggregated data. Now you can use Blockchair not only to filter and sort blockchain data, but also to aggregate it.

Please don't use this in production yet, there could be massive changes!

See the examples:
* https://api.blockchair.com/bitcoin/blocks?a=year,count()# - get the total number of Bitcoin blocks by year
* https://api.blockchair.com/bitcoin/transactions?a=month,median(fee_usd)# - get the median Bitcoin transaction fees by month
* https://api.blockchair.com/ethereum/blocks?a=miner,sum(generation)&s=sum(generation)(desc)# - get the list of Ethereum miners (except uncle miners) and sort it by the total amount minted
* https://api.blockchair.com/bitcoin-cash/blocks?a=sum(fee_total_usd)&q=id(478559..)# - calculate how much miners have collected in fees since the fork
    
To use aggregation, put the fields by which you'd like to group by (zero, one, or several), and fields (at least one) which you'd like to calculate using some aggregate function under the `?a=` section. You can also sort the results by one of the fields included in the `?a=` section (`asc` or `desc`) using the `?s=` section, and apply additional filters (see the documentation for the `?q=` section).

Possible fields:
* Bitcoin, Bitcoin Cash, Litecoin, Bitcoin SV, Dogecoin, Dash, Groestlcoin:
    * Blocks table
        * Group by: date (or week, month, year), version, guessed_miner
        * To calculate: size, stripped_size (except BCH), weight (except BCH), transaction_count, witness_count, input_count, output_count, input_total, input_total_usd, output_total, output_total_usd, fee_total, fee_total_usd, fee_per_kb, fee_per_kb_usd, fee_per_kwu (except BCH), fee_per_kwu_usd (except BCH), cdd_total, generation, generation_usd, reward, reward_usd — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Transactions table
        * Group by: block_id, date (or week, month, year), version, is_coinbase, has_witness (except BCH), input_count, output_count
        * To calculate: size, weight (except BCH), input_count, output_count, input_total, input_total_usd, output_total, output_total_usd, fee, fee_usd, fee_per_kb, fee_per_kb_usd, fee_per_kwu (except BCH), fee_per_kwu_usd (except BCH), cdd_total — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Outputs table
        * Group by: block_id, date (or week, month, year), type, is_from_coinbase, is_spendable, is_spent, spending_block_id, spending_date (no support for spending_week, spending_month, spending_year yet)
        * To calculate: value, value_usd, spending_value_usd, lifespan, cdd — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Addresses view
        * Group by: -
        * To calculate: balance — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
* Ethereum:
    * Blocks table
        * Group by: date (or week, month, year), miner
        * To calculate: size, difficulty, gas_used, gas_limit, uncle_count, transaction_count, synthetic_transaction_count, call_count, synthetic_call_count, value_total, value_total_usd, internal_value_total, internal_value_total_usd, generation, generation_usd, uncle_generation, uncle_generation_usd, fee_total, fee_total_usd, reward, reward_usd — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Uncles table
        * Group by: parent_block_id, date (or week, month, year), miner
        * To calculate: size, difficulty, gas_used, gas_limit, generation, generation_usd — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Transactions table
        * Group by: block_id, date (or week, month, year), failed, type
        * To calculate: call_count, value, value_usd, internal_value, internal_value_usd, fee, fee_usd, gas_used, gas_limit, gas_price — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Calls table
        * Group by: block_id, date (or week, month, year), failed, fail_reason, type, transferred
        * To calculate: child_call_count, value, value_usd — possible functions: avg(field), median(field), min(field), max(field), sum(field), count()  

##### Omni Layer and Wormhole support (since Sep 18th 2018)

* v.a1 - Sep 18th - Added alpha support for Omni Layer in Bitcoin (`bitcoin/omni/properties`, `bitcoin/omni/dashboards/property/{id}` calls, plus `_omni` key in the `bitcoin/dashboards/transaction` call and `_omni` key in the `bitcoin/dashboards/address` call), and support for Wormhole in Bitcoin Cash (`bitcoin-cash/wormhole/properties`, `bitcoin-cash/wormhole/dashboards/property/{id}` calls, plus `_wormhole` key in the `bitcoin-cash/dashboards/transaction` call and `_wormhole` key in the `bitcoin-cash/dashboards/address` call). Please don't use this in production yet, there will be massive changes!

### <a name="link_generalprovisions"></a> General Provisions

* Requests to our server should be made through the HTTPS protocol by GET requests to the domain `api.blockchair.com`

* The server response returns a JSON array always consisting of two subarrays:
	* `data` - contains some data
	* `context` - contains metadata, e.g., a status code, a query execution time, and so on.
	
* `data` can contain either an associative array (e.g., for `bitcoin/stats`), or for infinitable-queries (see below) - an unnumbered array (for example, `bitcoin/blocks`), or for dashboard-queries (see below) - an associative array with keys which are parts of the query (e.g., for `bitcoin/dashboards/transactions/A,B`, the keys are `A` and `B`), and with values - arrays of data.
* `context`, depending on the call type, can contain the following useful values:
	* `context.code` - server response code, can return:
		* `200` if the request succeeds
		* `400` if there is a user error in the request
		* `402` if any limit on the number or complexity of requests is exceeded (see the restrictions below, please contact us at <info@blockchair.com> for private API access)
		* `500` or` 503` in case of a server error (it makes sense to wait and repeat the same request or open a ticket at https://github.com/Blockchair/Blockchair.Support/issues/new or write to <info@blockchair.com>)
	* There is a field `context.error` with an error description in the case of` 40x` and `50x` errors
	* `context.state` contains the number of the latest block in case of requests to the blockchains (for example, for all requests beginning with `bitcoin` there will be the latest block number for Bitcoin). It is useful in order, e.g., to calculate the number of network сonfirmations, or correctly iterate trough the results using `offset`
	* `context.results` - contains the number of found results for dashboard-calls
	* `context.limit` - applied limit to the number of results
	* `context.offset` - applied offset
	* `context.rows` - contains the number of returned rows for infinitable-calls 
	* `context.pre_rows` - for some infinitable-calls contains the number of rows that should've been returned before duplicate removal (note: this architecture is used for the `bitcoin[-cash].outputs` tables only)
	* `context.total_rows` - number of rows that a query returns
	* `context.api` - an array of data on the status of the API:
		* `context.api.version` - version of API
		* `context.api.last_major_update` - time of the last update, that somehow broke backward compatibility
		* `context.api.next_major_update` - time of the next scheduled update, that can break compatibility, or` null`, if no updates are scheduled
		* `context.api.tested_features` - the list (comma-separated) of features with version numbers our API supports, but with no guarantee for backward compatibility if updated (in this case there will be no changes to `context.api.next_major_update` as well)
		* `context.api.documentation` - an URL to the latest version of documentation

Note: it makes sense to check `context.api.version` and, if `context.api.next_major_update` is not `null`, notify yourself and review the changelog. If there are no changes in the changelog that violate the compatibility of your application, make sure that the value of `context.api.next_major_update` won't exceed the current one. If there are changes, adjust the application logic so, that after the specified time a new logic is applied. Additional note: in case the backward compatibility is violated only for one API call, then `context.api.next_major_update` won't be `null` just for this call.

* Request limits: as of now, we allow up to 30 requests per minute to our API. If this limit is exceeded, an error `402` will be returned. In case of abuse, your IP address can be blocked. If your application needs more requests, please contact us at <info@blockchair.com>. If you need to unload a large amount of information once, please contact us at <export@blockchair.com> and, in case the academic or research unloading goal - you will receive the data for free in a convenient format.

* Disclaimer: we do not guarantee the reliability or integrity of the provided information. Information provided by our API should not be used for making critical decisions. We do not guarantee an uptime for our free API.

#### <a name="link_apikey"></a> Please apply for an API key first

tl;dr:
* If you use our API occasionally a key is not required
* Non-commercial and academic projects constantly using our API should apply for a free Public API key
* Commercial projects should apply for a key to Premium API

Since the introduction of our API more than two years ago it has been free to use in both non-commercial and commercial cases with a limit of 30 requests per minute. Obtaining an API key has been required only for those who were hitting this limit.

**Beginning July 19th, 2019 we require all applications using our API to obtain an API key.**

If you develop a non-commercial project (e.g. a website showing some stats) or conducting academic research, please apply for a free key to our Public API (<info@blockchair.com>).

If you develop a commercial project (e.g. a web wallet showing ads), please apply for a key to our Premium API (<info@blockchair.com>).

While we still allow making requests without a key, services which make too many resource-consuming requests may automatically be banned (the API will return HTTP Error 430 in this case).

The key is applied to the end of the request string like this: `api.blockchair.com/bitcoin/blocks?key=MYSECRETKEY`. Please remember that your key is a secret -- don't disclose it to client-side applications as unauthorized users may start to use your key.

### <a name="link_infinitablecalls"></a> Infinitable Calls (blockhain tables)

Return data from the tables according to the filters (`q`), sorting (`s`), limit (`limit`), and offset (`offset`).

A request should be construced like this: `https://api.blockchair.com/{chain}/[/mempool]{table}[?q={query}][&s={sorting}][&limit={limit}][&offset={offset}]`

E.g. `https://api.blockchair.com/bitcoin/blocks?q=size(1000000..)`

**Possible combinations of blockchains and tables:**
* Bitcoin:
    * `bitcoin/blocks` - contains all Bitcoin blocks, including the latest one
    * `bitcoin/transactions` - contains all Bitcoin transactions, excluding mempool transactions and transactions from the latest block   
    * `bitcoin/outputs` - contains all Bitcoin outputs, excluding the outputs contained in the mempool transactions as well as in the transactions from the latest block
    * `bitcoin/mempool/transactions` - contains Bitcoin mempool transactions
	* `bitcoin/mempool/outputs` - contains Bitcoin outputs included in mempool transactions
* Bitcoin Cash, Bitcoin SV, Litecoin, Dogecoin, Dash, Groestlcoin - the same as for Bitcoin
* Ethereum:
	* `ethereum/blocks` - contains all Ethereum blocks, except the last 6
	* `ethereum/uncles` - contains all Ethereum uncles, except those that belong to the last 6 blocks
	* `ethereum/transactions` - contains all Ethereum transactions, except transactions from the last 6 blocks
	* `ethereum/calls` - contains all transaction calls, except calls for the last 6 blocks
	* `ethereum/mempool/blocks` - contains the last 6 Ethereum blocks, some columns contain nulls
	* `ethereum/mempool/transactions` - contains all Ethereum transactions from the last 6 blocks as well as mempool transactions 

Notes: to speed up the process, our architecture contains separate tables (`mempool*`) for unconfirmed transactions, as well as for blocks that with a certain probability can be forked off from the main chain. For Ethereum, that's the latest 6 blocks plus the mempool. For Ethereum, we do not "replay" transactions entirely (i.e. not looking for internal calls) for the last 6 blocks, so there is no `mempool/calls` table.

**You can use filters** as follows: `?q=field(value)[,field(value)...]`, where `field` is the column by which a filter is needed, and `value` is a value, special value, or a range of values. The possible columns are listed in the tables below. Possible expressions for values:
* `value` - e.g., ` bitcoin/blocks?q=id(0)` finds information about block 0
* `left..` - non-strict inequality - e.g., `bitcoin/blocks?q=id(1..)` finds information about block 1 and above
* `left...` - strict inequality - e.g., `bitcoin/blocks?q=id(1...)` finds information about block 2 and above
* `..right` - non-strict inequality - e.g., `bitcoin/blocks?q=id(..1)` finds information about blocks 0 and 1
* `...right` - strict inequality - e.g.,` bitcoin/blocks?q=id(...1)` finds information only about block 0
* `left..right` - non-strict inequality - e.g., `bitcoin/blocks?q=id(1..3)` finds information about blocks 1, 2 and 3
* `left...right` - strict inequality - e.g., `bitcoin/blocks?q=id(1...3)` finds information only about block 2
* `~like` - occurrence in a string (`LIKE` operator), e.g., `bitcoin/blocks?q=coinbase_data_bin(~hello)` finds all blocks which contain `hello` in `coinbase_data_bin` 
* `^like` - occurrence at the beginning of a string (`STARTS WITH` operator), e.g., `bitcoin/blocks?q=coinbase_data_hex(^00)` finds all blocks for which` coinbase_data_hex` begins with `00`

For `time*`-fields, the value can be specified in the following formats:
* `YYYY-MM-DD HH:ii:ss`
* `YYYY-MM-DD`
* `YYYY-MM`

Inequalities are also supported for such values, but the left and right values must be in the same format, e.g.: `bitcoin/blocks?q=time(2009-01-03..2009-01-31)`.

If you need to list several filters, you need to sepatate them by commas in the `?q=` section, e.g., `bitcoin/blocks?q=id(500000..),coinbase_data_bin(~hello)`

We're currently testing support for `NOT` and `OR` operators (this may change in the future, including possible removal of these operators).

The operator `NOT` is comma-separated before the expression to be inverted, e.g., `bitcoin/blocks?q=not,id(1..)` returns the block `0`.

The `OR` operator is specified between the expressions and takes precedence (like it's when two expressions around `OR` are wrapped in parentheses), e.g., `bitcoin/blocks?q=id(1),or,id(2)` returns information about blocks 1 and 2.

Maximum guaranteed supported number of filters in one query: 5.

**Sorting** can be used as follows: `?s=field(direction)`, where `direction` can be either `asc` for sorting in ascending order, or `desc` for sorting in descending order, e.g. `bitcoin/blocks?s=id(asc)`

If you need to apply several sorts, you can list them by commas, similar to filters. The maximum guaranteed number of sorts is 2.

**Limit** is used like this: `?limit=N`, where N is a natural number from 1 to 100. The default is 10. In some cases `LIMIT` is ignored, and in such cases `context.limit` will be set to `NULL`.

**Offset** can be used as a paginator, e.g., `?offset=10` returns the next 10 results. `context.offset` takes the value of the set `OFFSET`. The maximum value is 10000. If you need just the last page, it's easier and quicker to change the direction of the sorting to the opposite. Important: when iterating through the results, it is extremely likely that the number of rows in the database will increase because new blocks were found. To avoid that, you may add an additional condition that limits the block id to the value obtained in `context.state` in the first query.

#### <a name="link_bitcoinblocks"></a> bitcoin/blocks, bitcoin-cash/blocks, bitcoin-sv/blocks, litecoin/blocks, dogecoin/blocks, dash/blocks, groestlcoin/blocks

E.g. `https://api.blockchair.com/bitcoin/blocks`

Returns data about blocks

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| id | int | Block height | + | + |
| hash | string `[0-9a-f]{64}` | Block hash | + | + |
| date | string `YYYY-MM-DD` | Block date (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Block time (UTC) | + | + |
| median_time | string `YYYY-MM-DD HH:ii:ss` | Block median time (UTC) | | | + |
| size | int | Block size, in bytes (may be more than 1 MB for Bitcoin, up to 32 MB for Bitcoin Cash) | + | + |
| stripped_size(\*) | int | Block size in bytes without taking witness information into account (up to 1 MB for Bitcoin) | + | + |
| weight (\*) | int | Block weight in weight units (up to 4 MWU) | + | + |
| version | int | Version field | + | + |
| version_hex | string `[0-9a-f]*` | Version field in hex | | |
| version_bits | string `[01]{30}` | Version field in binary form | | |
| merkle_root | `[0-9a-f]{64}` | Merkle root hash | | |
| nonce | int | Nonce value | + | + |
| bits | int | Bits field | + | + |
| difficulty | float | Difficulty | + | + |
| chainwork | string `[0-9a-f]{64}` | Chainwork field | | |
| coinbase_data_hex | string `[0-9a-f]*` | Hex information contained in the input of the coinbase transaction | + | |
| transaction_count | int | Number of transactions in the block | + | + |
| witness_count (\*) | int | Number of transactions in the block containing witness information | + | + |
| input_count | int | Number of inputs in all block transactions | + | + |
| output_count | int | Number of outputs in all block transactions | + | + |
| input_total | int | Sum of inputs in satoshi | + | + |
| input_total_usd | float | Sum of outputs in USD (hereinafter for USD - at the moment `date`) | + | + |
| output_total | int | Sum of outputs in Satoshi | + | + |
| output_total_usd | float | Sum of outputs in USD | + | + |
| fee_total | int | Total fee in Satoshi | + | + |
| fee_total_usd | float | Total fee in USD | + | + |
| fee_per_kb | float | Fee per kilobyte (1000 bytes of data) in satoshi | + | + |
| fee_per_kb_usd | float | Fee for kilobyte of data in USD | + | + |
| fee_per_kwu (\*) | float | Fee for 1000 weight units of data in Satoshi | + | + |
| fee_per_kwu_usd (\*) | float | Fee for 1000 weight units of data in USD | + | + |
| cdd_total | float | Number of coindays destroyed by all transactions of the block | + | + |
| generation | int | Miner reward for the block in Satoshi | + | + |
| generation_usd | float | Miner reward for the block in USD | + | + |
| reward | int | Miner total reward (reward + total fee) in Satoshi | + | + |
| reward_usd | float | Miner total reward  (reward + total fee) in USD | + | + |
| guessed_miner | string `.*` | The supposed name of the miner who found the block (the heuristic is based on `coinbase_data_bin` and the addresses to which the reward goes) | + | + |
| is_aux (\*\*) | boolean | Whether a block was mined using AuxPoW | | + | |
| cbtx (\*\*\*) | string `.*` | Coinbase transaction data (encoded JSON) | | | |


Additional synthetic columns (you can search over them and / or sort them, but they are not shown)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| coinbase_data_bin | string `.*` | Text representation of coinbase data. Allows you to use the `LIKE` operator: `?q=coinbase_data_bin(~hello)` | + | |

Notes:
- for the columns `id` and` hash` the increased efficiency at unloading of one record is applied
- there is no possibility to search over the `date` column directly, you can search like `?q=time(YYYY-MM-DD)`
- the search over the column `coinbase_data_hex` is done by the operator `^`, you can also use `~` for `coinbase_data_bin` (however, the field `coinbase_data_bin` will not be shown anyway)
- (\*) - only for Bitcoin, Litecoin, and Groestlcoin (SegWit data)
- (\*\*) - only for Dogecoin
- (\*\*\*) - only for Dash
- the default sorting - id DESC

#### <a name="link_bitcointransactions"></a> bitcoin/transactions, bitcoin/mempool/transactions, bitcoin-cash/transactions, bitcoin-cash/mempool/transactions, litecoin/transactions, litecoin/mempool/transactions, dogecoin/transactions, dogecoin/mempool/transactions, dash/transactions, dash/mempool/transactions, bitcoin-sv/transactions, bitcoin-sv/mempool/transactions, groestlcoin/transaction, groestlcoin/mempool/transactions

E.g. `https://api.blockchair.com/dogecoin/mempool/transactions`

Returns transaction data

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| block_id | int | The height (id) of the block containing the transaction | + | + |
| id | int | Internal transaction id (not related to the blockchain, used for internal purposes) | + | + |
| hash | string `[0-9a-f]{64}` | Transaction hash | + | |
| date | string `YYYY-MM-DD` | The date of the block containing the transaction (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Time of the block containing the transaction (UTC) | + | + |
| size | int | Transaction size in bytes | + | + |
| weight (\*) | int | Weight of transaction in weight units | + | + |
| version | int | Transaction version field | + | + |
| lock_time | int | Lock time - can be either a block height, or a unix timestamp | + | + |
| is_coinbase | boolean | Is it a coinbase (generating new coins) transaction? (For such a transaction `input_count` is equal to` 1` and means a synthetic coinbase input) | | + | |
| has_witness (\*) | boolean | Is there a witness part in the transaction (using SegWit)? | | + | |
| input_count | int | Number of inputs | + | + |
| output_count | int | Number of outputs | + | + |
| input_total | int | Input value in satoshi | + | + |
| input_total_usd | float | Input value in USD | + | + |
| output_total | int | Output value in Satoshi | + | + |
| output_total_usd | float | Total output value in USD | + | + |
| fee | int | Fee in Satoshi | + | + |
| fee_usd | float | Fee in USD | + | + |
| fee_per_kb | float | Fee per kilobyte (1000 bytes) of data in Satoshi | + | + |
| fee_per_kb_usd | float | Fee for kilobyte of data in USD | + | + |
| fee_per_kwu (\*) | float | Fee for 1000 weight units of data in Satoshi | + | + |
| fee_per_kwu_usd (\*) | float | Fee for 1000 weight units of data in USD | + | + |
| cdd_total | float | The number of destroyed coindays | + | + |

Notes:
- for the columns `id` and` hash` the increased efficiency at unloading of one record is applied
- there is no possibility to search over `date` column, you can use `?q=time(YYYY-MM-DD instead
- (\*) - only for Bitcoin, Litecoin, and Groestlcoin (SegWit data)
- the default sort is id DESC
- `block_id` for mempool transactions is `-1`

Additional Dash-specific columns:

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| type | string (enum) | Transaction type, one of the following: `simple`, `proregtx`, `proupservtx`, `proupregtx`, `prouprevtx`, `cbtx`, `qctx`, `subtxregister`, `subtxtopup`, `subtxresetkey`, `subtxcloseaccount` | + | + |
| is_instant_lock | boolean | | + | |
| is_special | boolean | | + | `true` for all transaction types except `simple` |
| special_json | string `.*` | Special transaction data (encoded JSON) | | | |

#### <a name="link_bitcoinoutputs"></a> bitcoin/outputs, bitcoin/mempool/outputs, bitcoin-cash/outputs, bitcoin-cash/mempool/outputs, litecoin/outputs, litecoin/mempool/outputs, dogecoin/outputs, dogecoin/mempool/outputs, dash/outputs, dash/mempool/outputs, bitcoin-sv/outputs, bitcoin-sv/mempool/outputs, groestlcoin/outputs, groestlcoin/mempool/outputs

E.g. `https://api.blockchair.com/litecoin/mempool/outputs`

Returns information about the outputs (that become inputs when they are spent, and then `spending*` information appears)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| block_id | int | Id of a block containing the transaction cointaining the output | + | + |
| transaction_id | int | The internal transaction id containing the output | + | + |
| index | int | Output index in a transaction (from 0) | + | + |
| transaction_hash | string `[0-9a-f]{64}` | Transaction hash | | |
| date | string `YYYY-MM-DD` | Date of a block containing the output (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Time of a block containing the output (UTC) | + | + |
| value | int | Monetary value of output | + | + |
| value_usd | float | Monetary value of output in USD at the moment `date` | + | + |
| recipient | string `[0-9a-zA-Z\-]*` | Bitcoin address or synthetic address of an output recipient | + | + |
| type | string (enum) | Output type, one of the following: `pubkey/pubkeyhash/scripthash/multisig/nulldata/nonstandard/witness_v0_scripthash/witness_v0_keyhash` | + | + |
| script_hex | string `[0-9a-f]*` | Hex of the output script | + | |
| is_from_coinbase | boolean | Is it a coinbase transaction output? | | + | |
| is_spendable | null or boolean | Is it theoretically possible to spend this output? For `pubkey` and` multisig` outputs, the existence of the corresponding private key is tested, in that case `true` and `false` are the possible values, depending on the result of the check. For `nulldata` outputs, it is always `false`. For other types it is impossible to check trivially, in this case `null` is shown | + | |
| is_spent | boolean | Is this output spent? (Each field further contains `null` if it is not spent | + | |
| spending_block_id | null or int | Id of the block containing the spending transaction. `null` if the output is not yet spent. | | + | + |
| spending_transaction_id | null or int | Internal transaction id where the output is spent | + | + |
| spending_index | null or int | Input index in the spending transaction (from 0) | + | + |
| spending_transaction_hash | null or string `[0-9a-f]{64}` | Spending transaction hash | | |
| spending_date | null or string `YYYY-MM-DD` | Date of the block, in which the output is spent | | |
| spending_time | null or string `YYYY-MM-DD HH:ii:ss` | Time of the block in which the output is spent | + | + |
| spending_value_usd | null or float | Monetary value of output in USD at the time of `spending_date` | + | + |
| spending_sequence | null or int | The technical sequence value | + | + |
| spending_signature_hex | null or string `[0-9a-f]*` | Hex of the spending script (signature) | | |
| spending_witness (\*) | null or string (JSONB) | Witness information in the escaped JSONB format | | |
| lifespan | null or int | The number of seconds from the time of the output creation (`time`) to its spending (`spending_time`), `null` if the output is not spent | + | + |
| cdd | null or float | The number of coindays destroyed spending the output | + | + |

Additional synthetic columns (you can search over them and / or sort them, but they are not shown)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| script_bin | string `.*` | Text representation of script_hex. Allows you to use the `LIKE` operator: `?q=script_bin(~hello)`| + | |

Notes:
- for columns `transaction_id` and `spending_transaction_id`, the increased efficiency at unloading records if one specific transaction is specified (and not the range), is applied
- there is no possibility to search over the `date` and `spending_date` columns, you can use `?q=time(YYYY-MM-DD)` and `?q=spending_time(YYYY-MM-DD)` instead
- the search over `script_hex` column can be done by the operator `^`, you can also use `~` for `script_bin` (however, the field `script_bin` will still not be shown)
- (\*) - only for Bitcoin, Litecoin, and Groestlcoin (SegWit data)
- the default sort is - transaction_id DESC

#### <a name="link_bitcoinaddresses"></a> bitcoin/addresses, bitcoin-cash/addresses, litecoin/addresses, dogecoin/addresses, dash/addresses, bitcoin-sv/addresses, groestlcoin/addresses

E.g. `https://api.blockchair.com/bitcoin/addresses`

The `addresses` view contains the list of all addresses and their confirmed balances. Unlike other infinitables (`blocks`, `transactions`, `outputs`) this table isn't live, it's automatically updated every 5 minutes, thus we classify it as a "view".

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| address | string `[0-9a-zA-Z\-]*` | Bitcoin address or synthetic address | | |
| balance | int | Its confirmed balance | + | + |

Notes:
- the default sort is - `balance DESC`

#### <a name="link_ethereumblocks"></a> ethereum/blocks, ethereum/mempool/blocks

Returns block data (`mempool` contains the latest 6 blocks)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| id | int | Block id | + | + |
| hash | string `0x[0-9a-f]{64}` | Block hash (with 0x) | + | |
| date | string `YYYY-MM-DD` | Block date (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Block time (UTC) | + | + |
| size | int | Block size in bytes | + | + |
| miner | string `0x[0-9a-f]{40}` | Address of a rewarded miner (from 0x) | + | |
| extra_data_hex | string `[0-9a-f]*` | Additional data included by the miner | + | |
| difficulty | int | Difficulty | + | + |
| gas_used | int | Gas amount used by block transactions | + | + |
| gas_limit | int | Gas limit for a block set up by the miner | + | + |
| logs_bloom | string `[0-9a-f]*` | Logs Bloom | | |
| mix_hash | string `[0-9a-f] {64}` | Hash Mix hash | | |
| nonce | string `[0-9a-f]*` | Nonce value | | |
| receipts_root | string `[0-9a-f] {64}` | Receipts Root hash | | |
| sha3_uncles | string `[0-9a-f] {64}` | SHA3 Uncles hash | | |
| state_root | string `[0-9a-f] {64}` | State Root hash | | |
| total_difficulty | numeric string | Total difficulty | | |
| transactions_root | string `[0-9a-f] {64}` | Transactions Root hash | | |
| uncle_count | int | Number of uncles | + | + |
| transaction_count | int | Number of transactions in the block | + | + |
| synthetic_transaction_count | int | The number of synthetic transactions (they do not exist as separate transactions, but they change the state, e.g., genesis block transactions, miner rewards, DAO-fork transactions) | + | + |
| call_count (\*) | int | Total number of calls spawned by transactions | + | + |
| synthetic_call_count | int | Number of synthetic calls (same as synthetic transactions) | + | + |
| value_total | numeric string | Monetary value of all block transactions in wei, hereinafter `numeric string` - numeric value passed as a string, because wei-values do not fit into uint64 | + | + |
| value_total_usd | float | Monetary value of all block transactions in USD | + | + |
| internal_value_total (\*) | numeric string | Monetary value of all internal calls in the block (see note below) in wei | + | + |
| internal_value_total_usd (\*) | float | Monetary value of all internal calls in a block in USD | + | + |
| generation | numeric string | The reward of a miner for the block generation in wei (3 or 5 ether + reward for uncle inclusion) | + | + |
| generation_usd | float | The reward of a miner for the block generation in USD | + | + |
| uncle_generation (\*) | numeric string | Total reward of uncle miners in wei | + | + |
| uncle_generation_usd (\*) | float | Total reward of uncle miners in USD | + | + |
| fee_total (\*) | numeric string | Total fee in wei | + | + |
| fee_total_usd (\*) | float | Total fee in USD | + | + |
| reward (\*) | numeric string | Total reward of the miner in the wei (reward for finding the block + fees) | + | + |
| reward_usd (\*) | float | Total reward of the miner in USD | + | + |

Additional synthetic columns (you can search over them and / or sort them, but they are not shown)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| extra_data_bin | string `.*` | Text representation of extra data. Allows you to use the `LIKE` operator: `?q=extra_data_bin(~hello)`| + | |

Notes:
- (\*) - always `null` for `mempool/blocks`
- for `id` and` hash` columns the increased efficiency at unloading of one record is applied
- there is no possibility to search the `date` column, but you can use `?q=time(YYYY-MM-DD)` instead
- search by fields that contain values in wei (`value_total`,` internal_value_total`, `generation`,` uncle_generation`, `fee_total`,` reward`) can be with some inaccuracies
- the search over `extra_data_hex` column can be done by the operator `^`, you can also use `~`for `extra_data_bin` (however, the field `extra_data_bin` will still not be shown)
- the difference between `value_total` and `internal_value_total`: e.g., a transaction itself sends 0 eth, but this transaction is a call of a contract that sends someone, let's say, 10 eth. Then `value` will be 0 eth, and `internal_value` - 10 eth
- the default sort is id DESC

#### <a name="link_ethereumuncles"></a> ethereum/uncles

Returns information about uncles

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| parent_block_id | int | Parent block id | + | + |
| index | int | Uncle index in the block | + | + |
| hash | string `0x[0-9a-f]{64}` | Uncle hash (with 0x) | + | |
| date | string `YYYY-MM-DD` | Date of generation (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Time of generation (UTC) | + | + |
| size | int | Uncle size in bytes | + | + |
| miner | string `0x[0-9a-f]{40}` | Address of the rewarded miner (with 0x) | + | |
| extra_data_hex | string `[0-9a-f]*` | Additional data included by the miner | + | |
| difficulty | int | Difficulty | + | + |
| gas_used | int | Amount of gas used by transactions | + | + |
| gas_limit | int | Gas limit for the block set up by the miner | + | + |
| logs_bloom | string `[0-9a-f]*` | Logs Bloom | | |
| mix_hash | string `[0-9a-f]{64}` | Hash Mix hash | | |
| nonce | string `[0-9a-f]*` | Nonce value | | |
| receipts_root | string `[0-9a-f]{64}` | Receipts Root hash | | |
| sha3_uncles | string `[0-9a-f]{64}` | Uncles hash | | |
| state_root | string `[0-9a-f]{64}` | State Root hash | | |
| total_difficulty | numeric string | Total difficulty | | |
| transactions_root | string `[0-9a-f]{64}` | Transactions Root hash | | |
| generation | numeric string | The reward of the miner who generated the uncle, in wei | + | + |
| generation_usd | float | The award of the miner who generated uncle, in USD | + | + |

Additional synthetic columns (you can search over them and / or sort them, but they are not shown)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| extra_data_bin | string `.*` | Text representation of extra data. Allows you to use the `LIKE` operator:`?Q=extra_data_bin(~hello)` | + | |

Notes:

- for the columns `parent_block_id` and `hash` increased efficiency when uploading one or more records is applied
- there is no possibility to search the `date` column directly, but you can use `?q=time(YYYY-MM-DD)` instead
- search by fields that contain values in wei (`generation`) can be with some inaccuracies
- the search over `extra_data_hex` column can be done by the operator `^`, you can also use `~` for `extra_data_bin` (however, the field `extra_data_bin` will still not be shown)
- sort by default - parent_block_id DESC

#### <a name="link_ethereumtransactions"></a> ethereum/transactions, ethereum/mempool/transactions

Returns transaction information

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| block_id | int | Id of the block containing the transaction | + | + |
| id | int | Transaction id (not related to the blockchain, used for internal purposes) | + | + |
| index (\*) (\*\*) | int | The transaction index number in the block | + | + |
| hash (\*\*) | string `0x[0-9a-f]{64}` | Transaction hash (with 0x) | + | |
| date | string `YYYY-MM-DD` | Date of the block containing the transaction (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Time of the block containing the transaction (UTC) | + | + |
| size | int | Transaction size in bytes | + | + |
| failed (\*) | bool | Failed transaction or not? + | | |
| type (\*) | string (enum) | Transaction type with one of the following values: `call/create/call_tree/create_tree/synthetic_coinbase`. Description in the note. | | + | |
| sender (\*\*) | string `0x[0-9a-f]{40}` | Address of the transaction sender (with 0x) | + | |
| recipient | string `0x[0-9a-f]{40}` | Address of the transaction recipient (with 0x) | + | |
| call_count (\*) | int | Number of calls in the transaction  | + | + |
| value | numeric string | Monetary value of transaction in wei, here and below `numeric string` - is a numeric value passed as a string, because wei-values do not fit into uint64 | + | + |
| value_usd | float | Value of transaction in USD | + | + |
| internal_value (\*) | numeric string | Value of all inner calls in the transaction in wei | + | + |
| internal_value_usd (\*) | float | Value of all internal calls in the transaction in USD | + | + |
| fee (\*) (\*\*) | numeric string | Fee in wei | + | + |
| fee_usd (\*) (\*\*) | float | Fee in USD | + | + |
| gas_used (\*) (\*\*) | int | Amount of gas used by a transaction | + | + |
| gas_limit (\*\*) | int | Gas limit for transaction set by the sender | + | + |
| gas_price (\*\*) | int | Price for gas set by the sender | + | + |
| input_hex (\*\*) | string `[0-9a-f]*` | Transaction input data | + | |
| nonce (\*\*) | string `[0-9a-f]*` | Nonce value | | |
| v (\*\*) | string `[0-9a-f]*` | V value | | |
| r (\*\*) | string `[0-9a-f]*` | R value | | |
| s (\*\*) | string `[0-9a-f]*` | S value | | |

Additional synthetic columns (you can search over them and / or sort them, but they are not shown)

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| input_bin | string `.*` | Text representation of input data. Allows you to use the `LIKE` operator: `?q=input_bin(~hello)` | + | |

Notes:

- (\*) - is always `null` for `mempool/transactions`
- (\*\*) - is always equal to `null` if `type` = `synthetic_coinbase`
- for the columns `id` and` hash` the increased efficiency at unloading of one record is applied
- there is no possibility to search over `date` column, you can search like `?q=time(YYYY-MM-DD)` instead
- search by fields that contain values in wei (`value`,` internal_value`) can be with some inaccuracies
- the search over `input_hex` column can be done by the operator `^`, you can also use `~` for `input_bin` (however, the field `input_bin` will still not be included in output)
- the difference between `value_total` and `internal_value_total`: e.g., a transaction itself sends 0 eth, but this transaction is a call of a contract that sends someone, let's say, 10 eth. Then `value` will be 0 eth, and `internal_value` - 10 eth
- the default sort is - id DESC
- possible types (`type`) of transactions:
    * call - the transaction transfers the value, but there are no more calls (a simple ether sending, not in favor of a contract, or the call to a contract that does nothing)
    * create - create a new contract
    * call_tree - the transaction calls a contract that makes some other calls
    * create_tree - create a new contract that create contracts or starts making calls
    * synthetic_coinbase - a synthetic transaction for awarding a reward to the miner (block or uncle)

#### <a name="link_ethereumcalls"></a> ethereum/calls

Returns information about internal calls

| Column | Type | Description | Q? | S? | 
|--------|------|-------------|----|----|
| block_id | int | Block id containing a call | + | + |
| transaction_id | int | Transaction id containing the call | + | + |
| transaction_hash (\*\*) | string `0x[0-9a-f]{64}` | Transaction hash (with 0x) containing the call | | |
| index | string | Call index within the transaction (tree-like, e.g., "0.8.1") | + | + |
| depth | int | Call depth within the call tree (starting at 0) | + | + |
| date | string `YYYY-MM-DD` | Date of the block that contains the call (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Time of the block that contains the call (UTC) | + | + |
| failed | bool | Failed call or not | + | |
| fail_reason | string `.*` or null | If failed, then the failure description, if not, then `null` | + | |
| type | string (enum) | The call type, one of the following values: `call/delegatecall/staticcall/callcode/selfdesctruct/create/synthetic_coinbase` | + | |
| sender (\*\*) | string `0x[0-9a-f]{40}` | Sender's address (with 0x) | + | |
| recipient | string `0x[0-9a-f]{40}` | Recipient's address (with 0x) | + | |
| child_call_count | int | Number of child calls | + | + |
| value | numeric string | Call value in wei, hereinafter `numeric string` - is a numeric string passed as a string, because wei-values do not fit into uint64 | + | + |
| value_usd | float | Call value in USD | + | + |
| transferred | bool | Has ether been transferred? (`false` if `failed`, or if the type of transaction does not change the state, e.g., `staticcall` | + | |
| input_hex (\*\*) | string `[0-9a-f]*` | Input call data | | |
| output_hex (\*\*) | string `[0-9a-f]*` | Output call data | | |

Notes:

- (\*\*) - is always `null` if` type` = `synthetic_coinbase`
- for the column `transaction_id` increased efficiency when uploading one record is applied
- there is no possibility to search over `date` column, use searching `?q=time(YYYY-MM-DD)` instead
- search by fields that contain values in wei (`value`,` internal_value`) can be with some inaccuracies
- the default sort is transaction_id DESC
- sorting by `index` is alphabetical (ie "0.2" goes after "0.11"), in some cases a switch to natural sorting is used (for example, when there is a filter for `transaction_id`)

#### Notes

- for unconfirmed transactions (and outputs in the case of bitcoin[-cash]), the following rules are applied:
    - their `block_id` is equal to `-1`
    - `date` and` time` indicate the time when the transaction was received by our node
- when using `offset`, it is reasonable to add to the filters the maximum block number (`?q=block_id(..N)`), since it is very likely that during the iteration new rows will be added to the table. For convenience, you can take the value of `context.state` from the first result of any query containing the number of the latest block at the query time and use this result later on.

### <a name="link_dashboardcalls"></a> Dashboard calls

The API supports a number of calls that produce some aggregated data, or data in a more convenient form for certain entities.

#### <a name="link_block"></a> {chain}/dashboards/block/{A} and {chain}/dashboards/blocks/{A[,B,...]}

`{chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `bitcoin-sv`, `litecoin`, `dogecoin`, `dash`, `ethereum`, `groestlcoin`

As the input data (`{A}`), it takes the height or hash of the block(s). `data` returns an array with block heights or block hashes used as keys, and arrays of elements as values:
* `block` - information about the block in infinitable-format `(bitcoin[-cash]|ethereum)/blocks`
* `transactions` - the array of all hashes of transactions included in the block
* only for Ethereum - `synthetic_transactions` - array of internal ids of synthetic transactions (they do not have a hash) (`null` instead of the array until the block receives 6 confirmations)
* only for Ethereum - `uncles` - the array of hashes of the block's uncles (`null` instead of the array until the block receives 6 confirmations, in case there are no uncles and more than 6 confirmations - an empty array (`[]`))

`context.results` contains the number of found blocks.

#### <a name="link_uncle"></a> ethereum/dashboards/uncle/{A} and ethereum/dashboards/uncles/{A[,B,...]}

As the input data (`{A}`), it takes an uncle hash(es). `data` returns an array with uncle hashes used as keys, and arrays of elements as values:
* `uncle` - information about the block in infinitable-format `ethereum/uncles`

`context.results` contains the number of found uncles.

#### <a name="link_transaction"></a> {chain}/dashboards/transaction/{A} and {chain}/dashboards/transactions/{A[,B,...]}

`{chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `bitcoin-sv`, `litecoin`, `dogecoin`, `dash`, `ethereum`, `groestlcoin`

At the input data, it takes an internal blockchair-id or a hash of a transaction (transactions). `data` returns an array with identifiers or hashes of transactions used as keys, and arrays of elements as keys:
* `transaction` - transaction information in infinitable-format `bitcoin[-cash]/transactions`
* (except ethereum) `inputs` - array of all transaction inputs, sorted by `spending_index` in infinitable-format `bitcoin[-cash]/outputs`
* (except ethereum) `outputs` - array of all transaction outputs, sorted by `index` in infinitable-format `bitcoin[-cash]/outputs`
* (only ethereum) `calls` - the array of all calls made during the execution of the transaction (always `null` for mempool transactions and the last 6 blocks)

`context.results` contains the number of found transactions.

#### <a name="link_priority"></a> {chain}/dashboards/transaction/{hash}/priority

`{chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `bitcoin-sv`, `litecoin`, `dogecoin`, `dash`, `ethereum`, `groestlcoin`

For mempool transactions shows priority (`position`) (for Bitcoin - by `fee_per_kwu`, for Bitcoin Cash - by `fee_per_kb`, for Ethereum - by `gas_price`) over other transactions (`out_of` mempool transactions). It has the same structure as the `(bitcoin[-cash]|ethereum)/dashboards/transaction/{A}` call

#### <a name="link_bitcoinaddress"></a> {chain}/dashboards/address/{A}

`{chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `bitcoin-sv`, `litecoin`, `dogecoin`, `dash`, `groestlcoin`

Uses address as the input data. `data` returns an array with one element (if the address is found), in that case the address is the key, and the value is an array consisting of the following elements:
* `address`
    * `address.type` - address type (type of output is the same as `bitcoin[-cash].outputs.type`)
    * `address.script_hex` - address script
    * `address.balance` - address balance in satoshi (hereinafter - including unconfirmed outputs) - int
    * `address.balance_usd` - address balance in USD - float
    * `address.received` - total received in satoshi
    * `address.received_usd` - total received in USD
    * `address.spent` - total spent in satoshi
    * `address.spent_usd` - total spent in USD
    * `address.output_count` - the number of outputs this address received
    * `address.unspent_output_count` - number of unspent outputs for this address (i.e. the number of inputs for an address can be calculated as `output_count`-`unspent_output_count`)
    * `address.first_seen_receiving` - timestamp (UTC) when the first time this address received coins
    * `address.last_seen_receiving` - timestamp (UTC) when the last time this address received coins
    * `address.first_seen_spending` - timestamp (UTC) when the first time this address sent coins
    * `address.last_seen_spending` - timestamp (UTC) when the last time this address sent coins
    * `address.transaction_count` - number of unique transactions this address participating in
* `transactions` - an array of the last 100 hashes the address is participating in

`context.results` contains the number of found addresses (0 or 1, until the `addresses` call is implemented).

To iterate `transactions`, `?offset=N` is supported.

#### <a name="link_ethereumaddress"></a> ethereum/dashboards/address/{A}

Uses address as the input data. `data` returns an array with one element (if the address is found), in that case the address is the key, and the value is an array consisting of the following elements:
* `address`
    * `address.type` - address type (`account` - for an address, `contract` - for a contract)
    * `address.contract_code_hex` -  hex code of the contract at the momemt of creation (for a contract), or null (for an address)
    * `address.contract_created` - for a contract - if the contact was indeed created then true, if not (i.e. with a failed `create` call) - false, or null (for an address)
    * `address.contract_destroyed` - for a contract - if the contact was successfully destroyed (SELFDESCTRUCT) then true, if not - false, or null (for an address)
    * `address.balance` - exact address balance in wei (here and below for values in wei - numeric string)
    * `address.balance_usd` - address balance in USD - float
    * `address.received_approximate` - total received in wei (approximately) (\*)
    * `address.received_usd` - total received in USD (approximately) (\*)
    * `address.spent_approximate` - total spent in wei (approximately) (\*)
    * `address.spent_usd` - total spent in USD (approximately) (\*)
    * `address.fees_approximate` - total spent in transaction fees in wei (approximately) (\*)
    * `address.fees_usd` - total spent in transaction fees in USD (approximately) (\*)
    * `address.receiving_call_count` - number of calls in favor of this address, where  value transfer has occured (\*\*)
    * `address.spending_call_count` -  number of calls that was made by this address, where  value transfer has occured (\*\*)
    * `address.call_count` - total number of calls this address participating in (may be greater than` receiving_call_count` + `spending_call_count`, because it also takes into account failed calls)
    * `address.transaction_count` - number of transactions this address participating in
    * `address.first_seen_receiving` - timestamp (UTC) when this address received a successful incoming call for the first time 
    * `address.last_seen_receiving` - timestamp (UTC) when this address received a successful incoming call for the last time
    * `address.first_seen_spending` - timestamp (UTC) when this address sent a successful call for the first time
    * `address.last_seen_spending` - timestamp (UTC) when this address sent a successful call for the last time
* `calls` - an array of the last 100 calls with the address, each element of an array containing the following columns of `ethereum/calls`: `block_id`, `transaction_hash`,` index`, `time`,` sender`, `recipient`, `value`,` value_usd`, `transferred`

`context.results` contains the number of found addresses  (0 or 1, until the `addresses` call is implemented).

To iterate `calls`, `?offset=N` is supported.

Notes:
- (\*) - in these columns, the value in wei can be rounded. For a million of calls, the error can be more than 1 ether.
- (\*\*) - counted only those calls that fit the following condition: ethereum/calls.transferred = true (see the `ethereum/calls` documentation), i.e. those calls as well as failed calls that do not change state (staticcall, etc.) are not considered

#### <a name="link_chainstats"></a> {chain}/stats

`{chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `bitcoin-sv`, `litecoin`, `dogecoin`, `dash`, `ethereum`, `groestlcoin`

Returns an array with blockchain statistics:
* `blocks` - total number of blocks
* (only ethereum) `uncles` - total number of uncles
* `transactions` - total number of transactions
* (only ethereum) `calls` - total number of internal calls
* `blocks_24h` - blocks for the last 24 hours
* `circulation` for Bitcoin-like cryptos, `circulation_approximate` for Ethereum - number of coins in circulation (in Satoshi, or in wei for Ethereum - an approximate value)
* `transactions_24h` - transactions for the last 24 hours
* `difficulty` - current difficulty
* `volume_24h` for Bitcoin-like cryptos, `volume_24h_approximate` for Ethereum - monetary volume of transactions for the last 24 hours (for ethereum - an approximate value)
* `mempool_transactions` - number of transactions in the mempool
* (only ethereum) `mempool_median_gas_price` - median gas price in the mempool
* (except ethereum) `mempool_size` - the mempool size in bytes
* `mempool_tps` - number of transactions per second added to the mempool
* (except ethereum) `mempool_total_value_approximate` - mempool monetary value
* (except ethereum) `mempool_total_fee_usd` - total mempool fee, in USD
* `best_block_height` - the latest block height
* `best_block_hash` - the latest block hash
* `best_block_time` - the latest block time
* (only ethereum) `uncles_24h` - number of uncles for the last 24 hours
* (except ethereum) `nodes` - number of full nodes 
* `hashrate_24h` - hashrate (hashes per second) in average for the last 24 hours
* `market_price_usd` - average market price of 1 coin in USD (market data source: CoinGecko)
* `market_price_btc` - average market price of 1 coin in BTC (for Bitcoin always returns 1)
* `market_price_usd_change_24h_percentage` - market price change in percent for 24 hours
* `market_cap_usd` - market capitalization (coins in circulation * price per coin in USD)
* `market_dominance_percentage` - dominance index (how much % of the total cryptocurrency market is the market capitalization of the coin)
... there's also some other self-explanatory keys

#### <a name="link_stats"></a> stats

https://api.blockchair.com/stats

Returns data on seven calls:
* `bitcoin/stats`
* `bitcoin-cash/stats`
* `ethereum/stats`
* `litecoin/stats`
* `bitcoin-sv/stats`
* `dogecoin/stats`
* `dash/stats`
* `groestlcoin/stats`

### <a name="link_examples"></a> API request examples

Suppose we would like to receive all the latest transactions from the Ethereum blockchain which amount to more than $1M USD. The following request should be done for this:
* `https://api.blockchair.com/ethereum/transactions?q=internal_value_usd(10000000..)&s=id(desc)`

In this request we refer to the blockhain (`ethereum`), a table (`transactions`), set up the amount condition (`q=internal_value_usd(10000000..)`), and sort in descending order (`&s=id(desc)`).

Suppose, a script with this request to the API for some reason did not work for a while, or a huge amount of transactions worth more than $1 million appeared. With the standard limit of 10 results, the script skipped some transactions. Then firstly we should do the following:
* `https://api.blockchair.com/ethereum/transactions?q=internal_value_usd(10000000..)&s=id(desc)`

From its result we save `context.state`, put it in a variable `_S_`, and further to obtain the following results we apply `offset`:
* `https://api.blockchair.com/ethereum/transactions?q=internal_value_usd(10000000..),block_id(.._S_)&s=id(desc)&offset=10`

Increase offset value until getting a data set with the transaction that we already knew about.

### <a name="link_broadcasting"></a> Broadcasting transactions

In order to broadcast a transaction into the network, you should make a POST request to `https://api.blockchair.com/{:chain}/push/transaction` (where `{chain}` can be one of those: `bitcoin`, `bitcoin-cash`, `ethereum`, `litecoin`, `bitcoin-sv`, `dash`, `dogecoin`, `groestlcoin`) with `data` holding hex represenatation of a transaction (for Ethereum it should start with `0x`). An example:

```
curl -v --data "data=01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff0704ffff001d0104ffffffff0100f2052a0100000043410496b538e853519c726a2c91e61ec11600ae1390813a627c66fb8be7947be63c52da7589379515d4e0a604f8141781e62294721166bf621e73a82cbf2342c858eeac00000000" https://api.blockchair.com/bitcoin/push/transaction
```

If the transaction has been successfully broadcast to the network, API will return a JSON response (code 200) containing `data` array with `transaction_hash` key holding the hash of the received transaction. In case of any error (wrong transaction format, spending already spent outputs, etc.) API returns status code 400.

Example of a successful response:

```
{"data":{"transaction_hash": "0e3e2357e806b6cdb1f70b54c3a3a17b6714ee1f0e68bebb44a74b1efd512098…"},"context":{"code":200,…
```

### <a name="link_raw"></a> Retrieving raw transactions

It's possible to get raw transaction data directly from our nodes. In order to do this you should make the following API call: `https://api.blockchair.com/{:chain}/raw/transaction/{txhash}` (where `{:chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `bitcoin-sv`, `litecoin`, `dogecoin`, `dash`, `ethereum`, `groestlcoin`)

The response contains two keys which are:
* `raw_transaction` - raw transaction represented as hex string
* `decoded_raw_transaction` (not available for Ethereum) - raw transaction encoded in JSON by our nodes. Please note that the structure of this JSON array may change as we upgrade our nodes, and this won't be reflected in our change logs.

### <a name="link_nodes"></a> Node list: {:chain}/nodes

Returns a list of full network nodes (except for Ethereum)

### <a name="link_state"></a> State changes

It's possible to query state changes caused by a block for all chains we support (except for Ripple).

The endpoint is `https://api.blockchair.com/{:chain}/state/changes/block/{:block_id)`.

The response contains an array where the keys are addresses which were affected by the block, and the values are balance changes.

Example: `https://api.blockchair.com/bitcoin/state/changes/block/1` returns `12c6DSiU4Rq3P4ZxziKxzrL5LmMBrzjrJX => 5000000000` which means that the only state change caused by this block was rewarding the miner with 50 bitcoins.

This is useful if you need to track balance changes for a lot of addresses - you can now simply track state changes and find the needed addresses there instead of constantly retrieving information about the balances.

Note: values are returned as strings for Ethereum.

It's also possible to query potential state changes caused by mempool transactions (this is not supported for Ethereum yet).

The endpoint for mempool state changes is `https://api.blockchair.com/{:chain}/state/changes/mempool`.

Here's an example logic for an application watching for Bitcoin transactions incoming/outgoing to/from 1 million addresses:

```
latest_known_block_height = 0
addresses = [1Abc, 1Efg, 1Hij, ...]
while (true)
    api_response = api_request('https://api.blockchair.com/bitcoin/state/changes/mempool')
    if any of api_response.data keys are in the addresses array
        print 'Found new transaction in the mempool'
    if latest_known_block_height < api_response.context.state // A new block has been mined (context.state always yields the latest block number)
        latest_known_block_height = api_response.context.state
        api_response_block = api_request('https://api.blockchair.com/bitcoin/state/changes/block/{:api_response.context.state}')
        if any of api_response_block.data keys are in the addresses array
            print 'Found new transaction in the latest block'
    sleep(10) // The mempool data is cached for 10 seconds on our servers by default
```

### <a name="link_support"></a> Support

* E-mail: [info@blockchair.com](mailto:info@blockchair.com)
* Telegram chat: [@Blockchair](https://telegram.me/Blockchair)
* Twitter: [@Blockchair](https://twitter.com/Blockchair)
