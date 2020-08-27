## [Blockchair.com](https://blockchair.com/) API

<img src="https://blockchair.com/images/logo_full.png" alt="Logo" width="250"/>

### API v.2 documentation
* English: [https://blockchair.com/api/docs](https://blockchair.com/api/docs) (up to v.2.0.64)

### Please apply for an API key first

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

### Changelog

* v.2.0.65 - August 27th, 2020
    * Fixed wrong `nonce` values for Ethereum transactions. `nonce` field now yields correct integers (thanks to Linmin Li for noticing this bug).
* v.2.0.64 - July 19th, 2020
    * Added `?transaction_details=true` option to `https://api.blockchair.com/{:btc_chain}/dashboards/addresses/{:address}₀,...,{:address}ᵩ` and `https://api.blockchair.com/{:btc_chain}/dashboards/xpub/{:extended_key}` endpoints. The additional cost for using this option is `1`. See its description in the v.2.0.37 changelog or in the documentation. Usage example: `https://api.blockchair.com/bitcoin/dashboards/xpub/xpub6CUGRUonZSQ4TWtTMmzXdrXDtypWKiKrhko4egpiMZbpiaQL2jkwSB1icqYh2cfDfVxdx4df189oLKnC5fSwqPfgyP3hooxujYzAu3fDVmz?transaction_details=true`
    * New `https://api.blockchair.com/multi/dashboards/addresses/{:address}₀,...,{:address}ᵩ` endpoint to check addresses from multiple blockchains at once. Supported blockchains: all Bitcoin-like blockchains and Ethereum. The maximum number of addresses is 100. See the documentation: https://blockchair.com/api/docs#link_391
    * Previously announced request cost formulas now come into full effect.
* v.2.0.63 - July 8th, 2020
    * Added `is_rbf` field to `https://api.blockchair.com/bitcoin/dashboards/transaction/{:hash}` and `https://api.blockchair.com/bitcoin/dashboards/transactions/{:hash}₀,...,{:hash}ᵩ` endpoints. It yields `true` if the transaction can be replaced with a transaction with a higher fee (replace-by-fee), and `false` otherwise. For blockchain transactions it shows whether the transaction could've been replaced before it has been included into the block. Available for Bitcoin Testnet as well.
    * Fixed a bug with Tezos when API returned error `500` for blocks with no transactions
* v.2.0.62 - July 2nd, 2020
    * We're happy to announce that Groestlcoin support has been extended up to at least January 1st, 2021.
    * `https://api.blockchair.com/{:eos_chain}/raw/account/{:address}` endpoint now has `?actions=true` option showing the last 10 actions for account
    * All EOS endpoints now have `context.price_usd` showing the USD price of EOS.
    * Fixed a bug where it wasn't possible to submit a transaction that is larger than 32 kB in size for broadcast (thanks to Koval for noticing that). Just a reminder, the transaction size still shouldn't be more than ~100 kB as it makes it non-standard and it won't be relayed by nodes.
    * The `https://api.blockchair.com/{:btc_chain}/addresses/balances` endpoint now works more stable.
* v.2.0.61 - Jun 24th, 2020
    * We're launching Privacy-o-meter - a tool to highlight transactions privacy issues. See the most detailed documentation: https://blockchair.com/api/docs#link_M6
    * `https://api.blockchair.com/{:btc_chain}/dashboards/transaction/{:hash}₀` and `https://api.blockchair.com/{:btc_chain}/dashboards/transactions/{:hash}₀,...,{:hash}ᵩ` now yield `coinbase_data_hex` for coinbase transactions if `?privacy-o-meter=true` option is set.    
* v.2.0.60 - Jun 19th, 2020
    * New way to fetch balances for multiple addresses: `https://api.blockchair.com/{:btc_chain}/addresses/balances` (`POST` or `GET`) with `addresses` key in a `POST` request (or `?addresses={:list}` in a `GET` request) as a comma-separated list of addresses (up to 25.000 at once). This endpoint returns confirmed balances only. If address hasn't been recorded on the blockchain or has a balance of zero, it won't be displayed in the results. This endpoint is extremely fast (under 1 second for 25.000 addresses) and cheap (it costs only 26 request points to fetch 25.000 addresses). See documentation here: https://blockchair.com/api/docs#link_390 (supported chains: Bitcoin, Bitcoin Cash, Bitcoin SV, Litecoin, Dash, Zcash, Dogecoin, Groestlcoin)
    * On Oct 26th, 2019 with v.2.0.38 we announced the "request cost" concept by which some requests cost more than others (i.e. requesting the data on 10 transactions costs more than requesting it for just 1 transaction, but since the request is done in bulk, it is cheaper for the API user in the end). Up until now we didn't enforce that policy counting each request as 1.
        * Starting July 19th, 2020 at 00:00 UTC we'll start enforcing this rule
        * As this is a somewhat compatibility-breaking change, we activate `context.api.next_major_update` and set it to `2020-07-19 00:00:00` (see [General Provisions](https://blockchair.com/api/docs#link_M06) for our policy on compatibility-breaking changes)
        * `context` array for all requests now has the `request_cost` field yielding the request cost
        * Premium API clients can track thier usage using [the control panel](https://api.blockchair.com/premium). If you have any questions, you can always reach us at <`info@blockchair.com`>
* v.2.0.59 - Jun 18th, 2020
    * Enhancements for Tezos:
        * New `blocks` infinitable: `https://api.blockchair.com/tezos/raw/blocks`
        * `context` array for every API call to the Tezos blockchain now has `price_usd` element returning the current USD price of Tezos
* v.2.0.58 - Jun 13th, 2020
    * Added EOS support. Please note we're not running a full history EOS node at the moment, so our API shows the most recent blocks and transactions only. New endpoints:
        * `https://api.blockchair.com/eos/stats`
        * `https://api.blockchair.com/eos/raw/block/{:id}`
        * `https://api.blockchair.com/eos/raw/transaction/{:hash}`
        * `https://api.blockchair.com/eos/raw/account/{:address}`
* v.2.0.57 - Jun 5th, 2020
    * We now show multisig types for P2SH and P2WSH addresses. The type has the following format: `multisig_{:m}_of_{:n}`. If the script is not P2SH or P2WSH multisig, the type is `null`. Affected endpoints:
        * `https://api.blockchair.com/{:btc_chain}/dashboards/address/{:address}` now has the `scripthash_type` field (example: `https://api.blockchair.com/bitcoin/dashboards/address/37cmSuMp7CuLDhjYkNJiTSewmbPuv8RBt1`). If address hasn't been seen spending, it's not possible to derive the multisig type, and `scripthash_type` will be `null`.
        * `https://api.blockchair.com/{:btc_chain}/dashboards/transaction/{:hash}₀` and `https://api.blockchair.com/{:btc_chain}/dashboards/transactions/{:hash}₀,...,{:hash}ᵩ` now have `scripthash_type` field for inputs and outputs (example: `https://api.blockchair.com/bitcoin/dashboards/transaction/4d41241148a7cb8f4e2820d4393415ccd3d0793053a3855b44c33e5053c231ff`). Please note that if output is unspent, `scripthash_type` will always be `null`, even if the associated address multisig type can be derived from some other spent output.
    * Fixed bug in the `https://api.blockchair.com/{:btc_chain}/mempool/outputs` infinitable where `spending_witness` returned values in an incorrect format for SegWit-enabled chains (this closes Issue #157)
* v.2.0.56 - Jun 4th, 2020
    * Improved the `https://api.blockchair.com/{:btc_chain}/dashboards/xpub/{:extended_key}` endpoint response time when requesting the same xpub for the second and subsequent times -- we now cache the minimum number of needed derivation cycles (e.g. now if an xpub contains 59 addresses on the first request API goes through 3 cycles -- 20 addresses each -- and then remembers that there are at least 60 addresses should be checked -- and on subsequent requests these 60 addresses will be checked using 1 database request instead of 3.
    * Fixed API returning error `500` for very large xpubs
    * Improved Cardano explorer stability
    * Improved `https://api.blockchair.com/{:chain}/stats` and `https://api.blockchair.com/stats` endpoints response time
* v.2.0.55 - May 28th, 2020
    * Added `circulation` and `circulation_approximate` fields to the `https://api.blockchair.com/ethereum/erc-20/{:token}/stats` endpoint output. These values yield total circulating supply of the token (`null` if the contract doesn't have the `totalSuply` function).
    * Fixed a precision issue in the `https://api.blockchair.com/ethereum/erc-20/{:contract}/dashboards/address/{:address}` endpoint, now `balance` returns precise value.
    * Added `?nonce=true` option to the `https://api.blockchair.com/ethereum/dashboards/address/{:address}` endpoint. If enabled, `nonce` will yield account's current transaction nonce.
* v.2.0.54 - May 27th, 2020
    * The limit on xpub discovery is raised to 20.000 (10.000 main addresses and 10.000 change addresses) for Premium API customers. Please note that according to the request cost formula for `xpub` endpoints, the cost of fetching an xpub consisting of 10.000 addresses is `1 + 2 * (10000 / 20) - 0.1 = 1000.9`
    * Fixed a bug with complex aggregation queries. Now instead throwing a `500` error, the following example works: `https://api.blockchair.com/bitcoin/transactions?q=has_witness(true),has_witness(false),time(2020-05),input_count(1),output_count(1),is_coinbase(false)&a=date,f(avg(fee)/avg(fee))&aq=0:0;1:1` (this particular example returns data on how many times more SegWit transactions with a single input and a single outputs pay in fees on average).
    * `https://api.blockchair.com/{:chain}/push/transaction` endpoint now returns a detailed error description in the `context.error` field in case you're trying to submit a malformed transaction. This is not yet available for Ethereum.
    * In addition to `POST`, `https://api.blockchair.com/{:chain}/push/transaction` endpoint now works using `GET` as well. Example usage: `https://api.blockchair.com/{:chain}/push/transaction?data={:raw_transaction}`. Available for all cryptos we support.
* v.2.0.53 - May 26th, 2020
    * Improved stability of our Omni Layer explorer
    * The `https://api.blockchair.com/bitcoin/dashboads/transaction/{:hash}?omni=true` endpoint now returns info for unconfirmed Omni Layer transfers. In case there are any, the `valid` field will yield `null`. Please note that it's not possible to know if an Omni Layer transfer is valid before it has at least 1 confirmation.
* v.2.0.52 - May 22th, 2020
    * Fixed a bug where it wasn't possible to filter Ethereum mempool tables by sender and recipient (thanks @emilianobonassi for noticing)
    * `https://api.blockchair.com/{:btc_chain}/dashboards/transaction/{:hash}/priority` endpoint now yields `confirmed` in the `position` field for confirmed transactions. Previously it wasn't possible to differ confirmed transactions from transactions that have completely fallen out of the mempool.
* v.2.0.51 - May 14th, 2020
    * Added Tezos support. New endpoints:
        * `https://api.blockchair.com/tezos/stats`
        * `https://api.blockchair.com/tezos/raw/block/{:id|hash}`
        * `https://api.blockchair.com/tezos/raw/operation/{:hash}`
        * `https://api.blockchair.com/tezos/raw/account/{:account|contract}`
* v.2.0.50 - May 13th, 2020
    * Removed support for Telegram Open Network (TON) as the project has been shut down (see https://telegra.ph/What-Was-TON-And-Why-It-Is-Over-05-12). All the relevant endpoints will now return a `404` error.
* v.2.0.49 - April 26th, 2020
    * It's now possible to discard unconfirmed transactions from the address dashboard by applying `?state=latest` option. If this option is applied, `balance` will show only confirmed balance, and `transactions` and `utxo` arrays won't include unconfirmed data. Affected endpoints (`{:btc_chain}` can be one of these: `bitcoin`, `bitcoin-cash`, `litecoin`, `bitcoin-sv`, `dogecoin`, `dash`, `groestlcoin`, `zcash`, `bitcoin/testnet`): 
        * `https://api.blockchair.com/{:btc_chain}/dashboards/address/{:address}₀?state=latest`
        * `https://api.blockchair.com/{:btc_chain}/dashboards/addresses/{:address}₀,...,{:address}ᵩ?state=latest`
        * `https://api.blockchair.com/{:btc_chain}/dashboards/xpub/{:extended_key}?state=latest`
* v.2.0.48 - April 22nd, 2020 (Lenin turns 150 today!)
    * Added Mixin support. The list of new endpoints (see the documentation for details):
        * `https://api.blockchair.com/mixin/stats`
        * `https://api.blockchair.com/mixin/raw/snapshots`
        * `https://api.blockchair.com/mixin/raw/mintings`
        * `https://api.blockchair.com/mixin/raw/nodes`
        * `https://api.blockchair.com/mixin/raw/graph`
        * `https://api.blockchair.com/mixin/raw/snapshot/{:hash}`
        * `https://api.blockchair.com/mixin/raw/snapshot/{:id}`
        * `https://api.blockchair.com/mixin/raw/transaction/{:hash}`
        * `https://api.blockchair.com/mixin/raw/round/{:hash}`
        * `https://api.blockchair.com/mixin/raw/round/({:id},{:node_hash})`
        * `https://api.blockchair.com/mixin/raw/node/{:node_hash}`
        * `https://api.blockchair.com/mixin/push/transaction`
* v.2.0.47 - March 12th, 2020
    * Added Zcash support. Features: dashboard endpoints, raw endpoints, infinitables (allowing to filter and sort blockchain data), node explorer, and many more.
* v.2.0.46 - March 3rd, 2020
    * There's a new interface for our Premium API users available at `https://api.blockchair.com/premium`. It's now possible to buy and extend a subscription using this interface as well as to track some basic stats. Subscription extending is not available for existing customers with custom plans, please contact us at <info@blockchair.com> if you'd like to have an automated interface.
* v.2.0.45 - February 6th, 2020
    * New endpoint to track halvening events: `https://api.blockchair.com/tools/halvening`. Documentation: https://blockchair.com/api/docs#link_511
* v.2.0.44 - January 28th, 2020
    * New endpoint to track new reference node software releases: `https://api.blockchair.com/tools/releases`. This endpoint returns the list of latest software (core clients) releases for blockchains we support. This may be useful for those who want to track blockchain development, new features, and hard forks (especially this is useful for multi-currency blockchain software — wallets or exchanges — developers). Never miss a BSV hard fork anymore! Documentation: https://blockchair.com/api/docs#link_510
* v.2.0.43 - January 23rd, 2020
    * Added alpha support for Cardano (ADA). Here's the list of new endpoints (please refer to our documentation for more info: https://blockchair.com/api/docs):
        * `https://api.blockchair.com/cardano/stats`
        * `https://api.blockchair.com/cardano/raw/block/{:id|hash}`
        * `https://api.blockchair.com/cardano/raw/transaction/{:hash}`
        * `https://api.blockchair.com/cardano/raw/address/{:address}`
* v.2.0.42 - January 20th, 2020
    * New endpoint to query the range of available blocks in blockchains we support: `https://api.blockchair.com/range`. Note that at this moment we don't store full historical data for Ripple and Stellar, so this endpoint is useful when you need to know which blocks can be queried.
* v.2.0.41 - January 18th, 2020
    * Added alpha support for Monero. Here's the list of new endpoints (please refer to our documentation for more info: https://blockchair.com/api/docs):
        * `https://api.blockchair.com/monero/stats`
        * `https://api.blockchair.com/monero/raw/block/{:id|hash}`
        * `https://api.blockchair.com/monero/raw/transaction/{:hash}`
        * `https://api.blockchair.com/monero/raw/outputs?{:query}`
* v.2.0.40 - December 3rd, 2019
    * Added alpha support for Telegram Open Network (TON). Here's the list of new endpoints (please refer to our documentation for more info: https://blockchair.com/api/docs):
        * `https://api.blockchair.com/ton/testnet/stats`
        * `https://api.blockchair.com/ton/testnet/raw/ledger/{:tuple}`
        * `https://api.blockchair.com/ton/testnet/raw/transaction/{:tuple}`
        * `https://api.blockchair.com/ton/testnet/raw/account/{:address}`
* v.2.0.39 - Nov 1st, 2019
    * Added alpha support for Stellar (XLM). Here's the list of new endpoints (please refer to our documentation for more info: https://blockchair.com/api/docs):
        * `https://api.blockchair.com/stellar/stats`
        * `https://api.blockchair.com/stellar/raw/ledger/{:id}`
        * `https://api.blockchair.com/stellar/raw/transaction/{:hash}`
        * `https://api.blockchair.com/stellar/raw/account/{:address}`
* v.2.0.38 - Oct 26th, 2019
    * We've published new documentation for our API which is lots more clear and describes all the functions we have, it's now available here: https://blockchair.com/api/docs
    * Changes to the Omni Layer and Wormhole support (as they're in Alpha test mode these are compatibility-breaking changes; we'll be bringing Omni to Stable the next year):
        * As Wormhole as a protocol isn't used anymore, we'll shut it down on our platform on January 1st, 2020
        * Retrieving infomation about Omni Layer transfers within a Bitcoin transaction now requires a `?omni=true` parameter
        * Retrieving infomation about Wormhole transfers within a Bitcoin Cash transaction now requires a `?wormhole=true` parameter
        * Retrieving infomation about Omni Layer token balances of a Bitcoin address now requires a `?omni=true` parameter
        * Retrieving infomation about Wormhole token balances of a Bitcoin Cash transaction now requires a `?wormhole=true` parameter
        * Data is now yielded in `layer_2.omni` and `layer_2.wormhole` arrays instead of `_omni` and `_wormhole`
    * Changes to Ripple endpoints (they're in Alpha test mode as well):
        * `https://api.blockchair.com/{:xrp_chain}/dashboards/ledger/{:hash|id}₀` is now `https://api.blockchair.com/{:xrp_chain}/raw/ledger/{:hash|id}₀`
        * `https://api.blockchair.com/{:xrp_chain}/dashboards/transaction/{:hash}₀` is now `https://api.blockchair.com/{:xrp_chain}/raw/transaction/{:hash}₀`
        * `https://api.blockchair.com/{:xrp_chain}/dashboards/account/{:address}₀` is now `https://api.blockchair.com/{:xrp_chain}/raw/account/{:address}₀`
    * Added `block_id` property to the output of the `?transaction_details=true` param introduced in v.2.0.37 - it's now easier to count the number of confirmations. Also a small bug has been fixed with the timestamps (thanks to Maxim Chistov for noticing)
    * We're introducing the "request cost" concept (full description is available in our new documentation; basically, the idea is that some of API requests will cost more than others). We'll not be forcing this rule at the moment, the launch date will be announced later. That won't affect our existing API customers until the end of their subscription periods.
* v.2.0.37 - Oct 9th, 2019
    * `{:chain}/dashboards/address/{:address}` endpoint now has an optional parameter `?transaction_details=true` which allows you to retrieve transaction details in the `transactions` array instead of just transaction hashes. Each `transactions` array element contains the following values:
        * `hash` - transaction hash
        * `time` - transaction timestamp (UTC)
        * `balance_change` - how the transaction affected the balance of `{:address}`
        
    Appending a request with `?transaction_details=true` makes it count as 2 separate requests in the matter of API limits (including Premium API).
    
    Usage example: `https://api.blockchair.com/bitcoin/dashboards/address/12cbQLTFMXRnSzktFkuoG3eHoMeFtpTu3S?transaction_details=true`
    
    This option is supported for Bitcoin, Bitcoin Cash, Litecoin, Dash, Bitcoin SV, Dogecoin, and Groestlcoin only.
* v.2.0.36 - Sep 17th, 2019
    * We begin a public beta test of ERC-20 support on our platform. Over 100.000 tokens are available to explore!
        * New ERC-20 endpoints:
            * ERC-20 tokens infinitable: `https://api.blockchair.com/ethereum/erc-20/tokens`
            * ERC-20 transactions infinitable: `https://api.blockchair.com/ethereum/erc-20/transactions` (example: `https://api.blockchair.com/ethereum/erc-20/transactions?q=token_address({:token_address})` - filter transactions by a specific token). These two infinitables act as other infinitables in our API, i.e. it's possible to sort and filter by many params, to paginate, etc.
            * ERC-20 token stats dashboard: `https://api.blockchair.com/ethereum/erc-20/{:token_address}/stats` - returns some basic stats about a token
            * ERC-20 token holder dashboard: `https://api.blockchair.com/ethereum/erc-20/{:token_address}/dashboards/address/{:address}` - returns info about a token holder (the balance, list of latest transactions, etc.)
        * New options and changes to the existing Ethereum dashboards:
            * Ethereum transaction dashboard: `https://api.blockchair.com/ethereum/dashboards/transaction/{:transaction_hash}?erc_20=true` - returns info about a transaction including ERC-20 transfers
            * Ethereum address dashboard: `https://api.blockchair.com/ethereum/dashboards/address/{:address}?erc_20=true` - returns info about an address including its ERC-20 token balances (tip: addresses and hashes should start with `0x`)
            * Ethereum stats dashboards (`https://api.blockchair.com/ethereum/stats`) now have `data.layer_2.erc_20` key yielding basic stats on ERC-20's: the total number of tokens, the total number of transactions, and the number of new tokens and transactions over the last 24 hours.
            * For all Ethereum endpoints there's a new key `context.state_layer_2` which yields the latest processed block number. It can be different from `context.state` as it takes some time to process second layer transactions.
            
      Please note these endpoints are in beta, so there may be some unannounced compatibility-breaking changes. More detailed documentation will come later this month. Appending `?erc_20=true` to requests makes it counted as 2 separate requests in the matter of API limits (including Premium API).
* v.2.0.35 - Sep 11th, 2019
    * `api.blockchair.com/{:chain}/nodes` endpoint now also returns the `heights` array showing distribution of the latest block numbers among nodes. See a visualisation on our web interface as an example: https://blockchair.com/bitcoin/nodes
    * There's a new endpoint `api.blockchair.com/nodes` showing some aggregated node stats across 7 networks (Bitcoin, Bitcoin Cash, Bitcoin SV, Litecoin, Dash, Dogecoin, and Groestlcoin).
    * Our database dumps now also feature daily balances snapshot, see Bitcoin for example: https://gz.blockchair.com/bitcoin/addresses/ (please note that the download speed is limited for non-premium users). The format is `address<TAB>balance`.
* v.2.0.34 - Aug 7th, 2019
    * It's now possible to retrieve raw block data directly from our nodes. The endpoint is `api.blockchair.com/{:chain}/raw/block/{:hash}|{:id}`. This endpoint supports all chains except for `ripple`.
        * For Bitcoin-like chains the `data` array returns two elements:
            * `raw_block` - contains hex representation of the block
            * `decoded_raw_block` - contains json representation of the block that is generated by our node
        * For Ethereum only `decoded_raw_block` is available 
      
      Please note that these endpoints are for development usage only. The result is returned directly from our nodes, so we can't guarantee backward compatibility in case we upgrade our nodes. For production, please use the `api.blockchair.com/{:chain}/dashboards/block/{:hash}|{:id}` endpoint - it pulls data from our databases.
    * Ethereum uncles now have an `id` property (previously they only had `parent_block_id`, `index`, and `hash` properties as identifiers). This affects `api.blockchair.com/ethereum/dashboards/uncle/{:hash}` and `api.blockchair.com/ethereum/uncles?{:params}` endpoints. Please note that `id` is not a unique identifier for an uncle (use `hash` or `parent_block_id:index` instead).
    * Fixed a bug where `api.blockchair.com/{:chain}/dashboards/addresses/{:address},{:address},...` endpoint returned `500` if `?limit=0` was applied
    * `context.state` doesn't return the latest block number anymore in case API returns an error (`400`, `402`, `404`, etc.)
* v.2.0.33 - Jul 23rd, 2019
    * According to the Bitcoin SV roadmap we've upgraded our nodes to support the Quasar upgrade
    * **BREAKING CHANGE**: Upon popular request (#161, #162, #193, #196, #211, and others), starting **July 29th 00:00:00+0000** we're switching to the legacy address format for Bitcoin SV. This will affect the output of the following endpoints:
        * `api.blockchair.com/bitcoin-sv/dashboards/transaction/{:hash}`
        * `api.blockchair.com/bitcoin-sv/dashboards/transactions/{:hash},{:hash},...`
        * `api.blockchair.com/bitcoin-sv/dashboards/address/{:address}` (it will be still possible to use CashAddr in the query string)
        * `api.blockchair.com/bitcoin-sv/dashboards/addresses/{:address},{:address},...` (the same)
        * `api.blockchair.com/bitcoin-sv/dashboards/xpub/{:[xyz]pub}`
        * `api.blockchair.com/bitcoin-sv/outputs?{:params}`
        * `api.blockchair.com/bitcoin-sv/mempool/outputs?{:params}`
        * `api.blockchair.com/bitcoin-sv/addresses?{:params}`
        * `api.blockchair.com/bitcoin-sv/state/changes/block/{:block_id)`
        * `api.blockchair.com/bitcoin-sv/state/changes/mempool`
* v.2.0.32 - Jul 15th, 2019
    * We're launching Bitcoin Testnet support for developers! All the functions available for Bitcoin are now available for Bitcoin Testnet as well, including:
        * Filtering and sorting blockchain data using infinitables (`api.blockchair.com/bitcoin/testnet/blocks`, `api.blockchair.com/bitcoin/testnet/transactions`, `api.blockchair.com/bitcoin/testnet/outputs`, `api.blockchair.com/bitcoin/testnet/mempool/transactions`, `api.blockchair.com/bitcoin/testnet/mempool/outputs`, and `api.blockchair.com/bitcoin/testnet/addresses`), including aggregation capabilities
        * Dashboards (`api.blockchair.com/bitcoin/testnet/dashboards/block/{:id|hash}`, `api.blockchair.com/bitcoin/testnet/dashboards/blocks/{:id|hash},{:id|hash},...`, `api.blockchair.com/bitcoin/testnet/dashboards/transaction/{:hash}`, `api.blockchair.com/bitcoin/testnet/dashboards/transactions/{:hash},{:hash},...`), `api.blockchair.com/bitcoin/testnet/dashboards/address/{:address}`, `api.blockchair.com/bitcoin/testnet/dashboards/addresses/{:address},{:address},...`, `api.blockchair.com/bitcoin/testnet/dashboards/xpub/{:[xyz]pub}`
        * Broadcasting transactions (`api.blockchair.com/bitcoin/testnet/push/transaction`)
        * State changes (`api.blockchair.com/bitcoin/testnet/state/changes/block/{:block_id)` and `api.blockchair.com/bitcoin/testnet/state/changes/mempool`)
        * Stats (`api.blockchair.com/bitcoin/testnet/stats`)
    
      See documentation for Bitcoin for more details. Please note that USD countervalue for testnet coins is always 0. In the future we also plan to launch support for Bitcoin Cash and Ethereum testnets.
    * `outputs.type` column for SegWit-coins can now yield `witness_unknown` type
    * `blocks.generation` column for Bitcoin-like coins can now yield negative values in case there are transaction fees, but the output for the coinbase transaction is less than the sum of the fees
    * The `utxo` array in the `{:chain}/dashboards/xpub/{[xyz]pub}` dashboard now shows which utxo belongs to which addresses (now it's identical to `{:address1}[,{:address2}…]`)
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
    * The [State changes](API_DOCUMENTATION_EN.md#link_state) feature now supports requesting potential state changes caused by mempool transactions. The endpoint is `https://api.blockchair.com/{:chain}/state/changes/mempool`. It's now possible to easily build an app watching for transactions incoming/outgoing to/from millions of addresses, see [an example](API_DOCUMENTATION_EN.md#link_state).
* v.2.0.28 - Jun 28th, 2019
    * Added support for Groestlcoin nodes. The endpoint is `https://api.blockchair.com/groestlcoin/nodes`. The `stats` endpoint (`https://api.blockchair.com/groestlcoin/stats`) now also shows the node count.
* v.2.0.27 - Jun 27th, 2019
    * Effective July 19th, there will be a new policy on using our Public API for both non-commercial and commercial projects. Please see [Applying for an API key first](API_DOCUMENTATION_EN.md#link_apikey) and apply for an API key before July 19th! Since this is a major compatibility-breaking change, `context.api.next_major_update` is set to `2019-07-19 18:07:19` (see [General Provisions](API_DOCUMENTATION_EN.md#link_generalprovisions))
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
