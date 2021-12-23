## [Blockchair.com](https://blockchair.com/) API

<img src="https://blockchair.com/images/logo_full.png" alt="Logo" width="250"/>

### API v.2 documentation
* English: [https://blockchair.com/api/docs](https://blockchair.com/api/docs) (up to v.2.0.80)

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

* v.2.0.95 - December 23rd, 2021
    * Added Solana support in beta mode. We're not running a full history Solana node at the moment, so only the most recent blocks and transactions are available. The available block range can be obtained using `https://api.blockchair.com/range`. New endpoints:
        * `https://api.blockchair.com/solana/stats`
        * `https://api.blockchair.com/solana/raw/slot/{:id}`
        * `https://api.blockchair.com/solana/raw/transaction/{:signature}`
        * `https://api.blockchair.com/solana/raw/address/{:address}`. Possible options: `?tokens=true` (costs `1` additional request point to use).
        * `https://api.blockchair.com/solana/raw/validators`. Available options: `?offset={:offset}`.
        * `https://api.blockchair.com/solana/raw/slots`. Available options: `?offset={:offset}`.
* v.2.0.94 - December 1st, 2021
    * Added Kusama support in beta mode. The endpoints are compatible with Polkadot (we'll refer to Polkadot and Kusama as "Polkadot-like blockchains"):
        * `https://api.blockchair.com/kusama/stats` (+ `https://api.blockchair.com/stats` now also features Kusama)
        * `https://api.blockchair.com/kusama/raw/block/{:id}`
        * `https://api.blockchair.com/kusama/raw/block/{:hash}`
        * `https://api.blockchair.com/kusama/raw/extrinsic/{:id}`
        * `https://api.blockchair.com/kusama/raw/extrinsic/{:hash}`
        * `https://api.blockchair.com/kusama/raw/address/{:address}` (with `?offset={:offset} option to iterate through the latest extrinsics and transfers)
        * `https://api.blockchair.com/kusama/raw/blocks` (an infinitable with `?offset={:offset} option)
        * `https://api.blockchair.com/kusama/raw/extrinsics` (an infinitable with `?offset={:offset} option)
        * `https://api.blockchair.com/kusama/raw/events` (an infinitable with `?offset={:offset} option)
* v.2.0.93 - November 17th, 2021
    * Added support for almost 200 fiat currencies and precious metals with exchange rates going back to 2009.
        * All API endpoints now support the `?rates={:code}` option. If enabled, it adds extra data for the chosen currency code on top of where USD data is already available. Example: `https://api.blockchair.com/bitcoin/stats?rates=kzt` adds `average_transaction_fee_kzt_24h`. For historical data (example: `https://api.blockchair.com/bitcoin/dashboards/block/500000?rates=kzt`) it honors historical exchange rates.
        * The supported fiat currencies are: `AED` (United Arab Emirates Dirham), `AFN` (Afghan Afghani), `ALL` (Albanian Lek), `AMD` (Armenian Dram), `ANG` (Netherlands Antillean Guilder), `AOA` (Angolan Kwanza), `ARS` (Argentine Peso), `AUD` (Australian Dollar), `AWG` (Aruban Florin), `AZN` (Azerbaijani Manat), `BAM` (Bosnia-Herzegovina Convertible Mark), `BBD` (Barbadian Dollar), `BDT` (Bangladeshi Taka), `BGN` (Bulgarian Lev), `BHD` (Bahraini Dinar), `BIF` (Burundian Franc), `BMD` (Bermudan Dollar), `BND` (Brunei Dollar), `BOB` (Bolivian Boliviano), `BRL` (Brazilian Real), `BSD` (Bahamian Dollar), `BTN` (Bhutanese Ngultrum), `BWP` (Botswanan Pula), `BYN` (Belarusian Ruble), `BZD` (Belize Dollar), `CAD` (Canadian Dollar), `CDF` (Congolese Franc), `CHF` (Swiss Franc), `CLF` (Chilean Unit of Account (UF)), `CLP` (Chilean Peso), `CNH` (Chinese Yuan (Offshore)), `CNY` (Chinese Yuan), `COP` (Colombian Peso), `CRC` (Costa Rican Colón), `CUC` (Cuban Convertible Peso), `CUP` (Cuban Peso), `CVE` (Cape Verdean Escudo), `CZK` (Czech Republic Koruna), `DJF` (Djiboutian Franc), `DKK` (Danish Krone), `DOP` (Dominican Peso), `DZD` (Algerian Dinar), `EGP` (Egyptian Pound), `ERN` (Eritrean Nakfa), `ETB` (Ethiopian Birr), `EUR` (Euro), `FJD` (Fijian Dollar), `FKP` (Falkland Islands Pound), `GBP` (British Pound Sterling), `GEL` (Georgian Lari), `GGP` (Guernsey Pound), `GHS` (Ghanaian Cedi), `GIP` (Gibraltar Pound), `GMD` (Gambian Dalasi), `GNF` (Guinean Franc), `GTQ` (Guatemalan Quetzal), `GYD` (Guyanaese Dollar), `HKD` (Hong Kong Dollar), `HNL` (Honduran Lempira), `HRK` (Croatian Kuna), `HTG` (Haitian Gourde), `HUF` (Hungarian Forint), `IDR` (Indonesian Rupiah), `ILS` (Israeli New Sheqel), `IMP` (Manx pound), `INR` (Indian Rupee), `IQD` (Iraqi Dinar), `IRR` (Iranian Rial), `ISK` (Icelandic Króna), `JEP` (Jersey Pound), `JMD` (Jamaican Dollar), `JOD` (Jordanian Dinar), `JPY` (Japanese Yen), `KES` (Kenyan Shilling), `KGS` (Kyrgystani Som), `KHR` (Cambodian Riel), `KMF` (Comorian Franc), `KPW` (North Korean Won), `KRW` (South Korean Won), `KWD` (Kuwaiti Dinar), `KYD` (Cayman Islands Dollar), `KZT` (Kazakhstani Tenge), `LAK` (Laotian Kip), `LBP` (Lebanese Pound), `LKR` (Sri Lankan Rupee), `LRD` (Liberian Dollar), `LSL` (Lesotho Loti), `LYD` (Libyan Dinar), `MAD` (Moroccan Dirham), `MDL` (Moldovan Leu), `MGA` (Malagasy Ariary), `MKD` (Macedonian Denar), `MMK` (Myanma Kyat), `MNT` (Mongolian Tugrik), `MOP` (Macanese Pataca), `MRO` (Mauritanian Ouguiya (pre-2018)), `MRU` (Mauritanian Ouguiya), `MUR` (Mauritian Rupee), `MVR` (Maldivian Rufiyaa), `MWK` (Malawian Kwacha), `MXN` (Mexican Peso), `MYR` (Malaysian Ringgit), `MZN` (Mozambican Metical), `NAD` (Namibian Dollar), `NGN` (Nigerian Naira), `NIO` (Nicaraguan Córdoba), `NOK` (Norwegian Krone), `NPR` (Nepalese Rupee), `NZD` (New Zealand Dollar), `OMR` (Omani Rial), `PAB` (Panamanian Balboa), `PEN` (Peruvian Nuevo Sol), `PGK` (Papua New Guinean Kina), `PHP` (Philippine Peso), `PKR` (Pakistani Rupee), `PLN` (Polish Zloty), `PYG` (Paraguayan Guarani), `QAR` (Qatari Rial), `RON` (Romanian Leu), `RSD` (Serbian Dinar), `RUB` (Russian Ruble), `RWF` (Rwandan Franc), `SAR` (Saudi Riyal), `SBD` (Solomon Islands Dollar), `SCR` (Seychellois Rupee), `SDG` (Sudanese Pound), `SEK` (Swedish Krona), `SGD` (Singapore Dollar), `SHP` (Saint Helena Pound), `SLL` (Sierra Leonean Leone), `SOS` (Somali Shilling), `SRD` (Surinamese Dollar), `SSP` (South Sudanese Pound), `STD` (São Tomé and Príncipe Dobra (pre-2018)), `STN` (São Tomé and Príncipe Dobra), `SVC` (Salvadoran Colón), `SYP` (Syrian Pound), `SZL` (Swazi Lilangeni), `THB` (Thai Baht), `TJS` (Tajikistani Somoni), `TMT` (Turkmenistani Manat), `TND` (Tunisian Dinar), `TOP` (Tongan Pa'anga), `TRY` (Turkish Lira), `TTD` (Trinidad and Tobago Dollar), `TWD` (New Taiwan Dollar), `TZS` (Tanzanian Shilling), `UAH` (Ukrainian Hryvnia), `UGX` (Ugandan Shilling), `UYU` (Uruguayan Peso), `UZS` (Uzbekistan Som), `VEF` (Venezuelan Bolívar Fuerte (Old)), `VES` (Venezuelan Bolívar Soberano), `VND` (Vietnamese Dong), `VUV` (Vanuatu Vatu), `WST` (Samoan Tala), `XCD` (East Caribbean Dollar), `XPF` (CFP Franc), `YER` (Yemeni Rial), `ZAR` (South African Rand), `ZMW` (Zambian Kwacha), `ZWL` (Zimbabwean Dollar)
        * The precious metals are: `XAG` (Silver Ounce), `XAU` (Gold Ounce), `XPD` (Palladium Ounce) `XPT` (Platinum Ounce)
        * The cryptocurrencies are: `BTC` (Bitcoin), `ETH` (Ethereum)
        * Note that `?rates=USD` is not supported and will result in an error. When the `?rates={:code}` option is used, it only adds new data, and doesn't remove any `*usd*` fields from the output.
* v.2.0.92 - November 11th, 2021
    * Added Taproot support for Bitcoin, Groestlcoin, and Bitcoin Testnet:
        * Taproot scripts now convert to P2TR addresses (some older outputs that came before according activation dates may be reindexed a bit later)
        * `outputs.type` column can now yield and accept `witness_v1_taproot` type. Example: `https://api.blockchair.com/bitcoin/testnet/outputs?q=type(witness_v1_taproot)`
* v.2.0.91 - November 10th, 2021
    * Added Polkadot support in beta mode. New endpoints:
        * `https://api.blockchair.com/polkadot/stats` (+ `https://api.blockchair.com/stats` now also features Polkadot)
        * `https://api.blockchair.com/polkadot/raw/block/{:id}`
        * `https://api.blockchair.com/polkadot/raw/block/{:hash}`
        * `https://api.blockchair.com/polkadot/raw/extrinsic/{:id}`
        * `https://api.blockchair.com/polkadot/raw/extrinsic/{:hash}`
        * `https://api.blockchair.com/polkadot/raw/address/{:address}` (with `?offset={:offset} option to iterate through the latest extrinsics and transfers)
        * `https://api.blockchair.com/polkadot/raw/blocks` (an infinitable with `?offset={:offset} option)
        * `https://api.blockchair.com/polkadot/raw/extrinsics` (an infinitable with `?offset={:offset} option)
        * `https://api.blockchair.com/polkadot/raw/events` (an infinitable with `?offset={:offset} option)
* v.2.0.90 - October 24th, 2021
    * ERC-20 and ERC-721 transfers are now ordered within transactions as they were executed
    * Added `?offset={:offset}` option to the Cardano address endpoint (`https://api.blockchair.com/{:ada_chain}/raw/address/{:address}₀`) that allows to paginate the list of latest transactions
* v.2.0.89 - October 18th, 2021
    * Added ERC-721 support for Ethereum. New endpoints and options:
        * `https://api.blockchair.com/ethereum/erc-721/tokens` infinitable (the output is the same as for ERC-20 except for there's no `decimals`).
        * `https://api.blockchair.com/ethereum/erc-721/transactions` infinitable (also no `token_decimals`, and `token_id` is used instead of `value` Examples:
            * Find most used contracts over the last month: `https://api.blockchair.com/ethereum/erc-721/transactions?q=time(~P1M)&a=token_address,token_name,count()&s=count()(desc)&limit=10` 
            * Owner change history by token id: `https://api.blockchair.com/ethereum/erc-721/transactions?q=token_address(0xbd3531da5cf5857e7cfaa92426877b022e612cf8),token_id(5177)`
            * Find tokens that changed hands many times: `https://api.blockchair.com/ethereum/erc-721/transactions?a=token_address,token_id,count()&s=count()(desc)&limit=10`
        * `https://api.blockchair.com/ethereum/stats` now yields statistical data on ERC-721s in the `layer_2.erc_721` array: `tokens` is the total number of NFTs, `transactions` - the total number of transfers, `tokens_24h` - new NFTs over the last 24 hours, `transactions_24h` - token transfers over the last 24 hours
        * Find ERC-721 transfers inside of a transaction: `https://api.blockchair.com/ethereum/dashboards/transaction/{:hash}?erc_721=true`. Example: `https://api.blockchair.com/ethereum/dashboards/transaction/0x6e7dbd3e3835f5c08ac8a0e26216df17e9aa9d1b6956fc9f0c56c19a085ad888?erc_721=true`
        * List ERC-721 tokens for an address: `https://api.blockchair.com/ethereum/dashboards/address/{:address}?erc_721=true`. Example: `https://api.blockchair.com/ethereum/dashboards/address/0x943a48498c9d622273e214c1757e436f76a113ce?erc_721=true&limit=0`. This option lists token ids
        * ERC-721 contract dashboard: `https://api.blockchair.com/ethereum/erc-721/{:address}/stats`. Example: `https://api.blockchair.com/ethereum/erc-721/0x1dfe7ca09e99d10835bf73044a23b73fc20623df/stats`
        * ERC-721 contract inventory (token list): `https://api.blockchair.com/ethereum/erc-721/{:address}/inventory`. Available params: `?limit={:limit}` and `?offset={:offset}`. Example: `https://api.blockchair.com/ethereum/erc-721/0x2E956Ed3D7337F4Ed4316A6e8F2EdF74BF84bb54/inventory?limit=100&offset=100`. The list returns `transaction_count`, `first_time` (creation time), `last_time` (last ownership change time), `first_owner`, and `last_owner` (current owner).
    * Please note that some of popular NFTs may either not follow the ERC-721 standard at all or follow the ERC-20 standard instead. In the first case, our API won't return any data for such NFTs. In the second case, such contracts will be treated as ERC-20s. ERC-721 functionality is launched in beta mode, it's possible there will be compatibility-breaking changes. This functionality is available for the Goerli Testnet as well.
* v.2.0.88 - August 23rd, 2021
    * Added basic SLP support for Bitcoin Cash. New endpoints and options:
        * `https://api.blockchair.com/bitcoin-cash/stats` now yields statistical data on SLP in the `layer_2.slp` array: `tokens` is the total number of SLP tokens, `transactions` - the total number of SLP transfers, `tokens_24h` - new SLP tokens over the last 24 hours, `transactions_24h` - token transfers over the last 24 hours
        * Find SLP transfers inside of a transaction: `https://api.blockchair.com/bitcoin-cash/dashboards/transaction/{:hash}?slp=true`. Example: `https://api.blockchair.com/bitcoin-cash/dashboards/transaction/ca851afc516f6d1488924f4a064abdd8b82b45bc8a5890bee8aba58a1314f498?slp=true` (valid `SEND` transaction), `https://api.blockchair.com/bitcoin-cash/dashboards/transaction/71b31ecaf916fe2da690a61c45978542a654185caba643c3eda2a87f38f88d31?slp=true` (invalid `MINT` transaction`)
* v.2.0.87 - August 20th, 2021
    * For all API requests, the `context` array now yields `market_price_usd` value. It contains the current USD price of the blockchain's main token in request. E.g. all `https://api.blockchair.com/zcash/...` requests show ZEC price in `context.market_price_usd`. For Ethereum, it's always the ETH price, even if some token data is requested. This change also deprecates `context.price_usd` for EOS and Tezos in favour of `context.market_price_usd`.
     * New `number({:n})` function for aggregations that returns a number. It may be useful when you are too lazy to divide some values on your side, for example, when you're calculating the average interval between blocks in seconds: `https://api.blockchair.com/bitcoin/blocks?a=date,f(number(86400)/count())`. Our API can now also serve as your calculator for any needs: `https://api.blockchair.com/bitcoin/blocks?a=f(number(2)*number(2))&limit=1` returns `4`.
     * Tweaked `suggested_transaction_fee_per_byte_sat` for Dogecoin (in `https://api.blockchair.com/dogecoin/stats`) to honour the minimum fee of 1 DOGE per transaction
     * The following Ethereum endpoints are now also available for Ethereum Goerli Testnet:
        * `https://api.blockchair.com/ethereum/testnet/raw/block/{:id|hash}`
        * `https://api.blockchair.com/ethereum/testnet/raw/transaction/{:hash}`
        * `https://api.blockchair.com/ethereum/testnet/push/transaction`
        * `https://api.blockchair.com/ethereum/testnet/addresses`
        * `https://api.blockchair.com/ethereum/testnet/state/changes/block/{:id}`
* v.2.0.86 - August 19th, 2021
    * Error code `435` is now returned if you're using an API key and going over the maximum requests in parallel limit. By default, the limit is `daily_request_limit / 10000` request points. Also, by default, it is not enforced: it only comes into effect if our security system notices that your requests significantly overload our servers on a constant basis. This limit **supersedes** the 5 requests per second limit for premium API plans (which was also not enforced automatically). Example: if you're on a 5000 requests per day plan and do some daily calculations like fetching xpub balances right after the midnight, please try doing this in 2-3 app instances in parallel instead of spawning 1000 instances trying to complete the process in 10 seconds.
* v.2.0.85 - August 18th, 2021
    * New hashrate dashboard for all Bitcoin-like and Ethereum-like chains: `https://api.blockchair.com/{:chain}/dashboards/hashrate` that outputs the estimated hashrate by day. Example: `https://api.blockchair.com/bitcoin/dashboards/hashrate`.
    * New difficulty dashboard for Bitcoin: `https://api.blockchair.com/bitcoin/dashboards/difficulty`. It outputs:
        * All completed difficulty "periods" (2016 blocks each). `status` is `completed`, `hashrate` is the estimated average hashrate within the period, `avg_block_time` is the average time between blocks in seconds
        * The current difficulty period. `status` is `ongoing`
        * The next difficulty period. `status` is `planned`, `start_time` is the estimated time when the period will start based on the current hashrate and the number of blocks left in the current period, `difficulty` is the estimated difficulty for the next period, `hashrate` is the same as for the current period, `avg_block_time` is always `600` (10 minutes) as it's the target time between blocks
* v.2.0.84 - August 9th, 2021
    * Enhancements to the Ethereum transaction dashboard (`https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}`) for unconfirmed transactions. Please note that these values are not final and may change as transactions get confirmed: tracing unconfirmed transactions doesn't take possible changes to the state into account.
        * If the `?events=true` option is used, API returns values for `gas_used`, `fee`, `fee_usd` instead of `null`s
        * If the `?trace_mempool=true` option is used, API returns values for `internal_value` and `internal_value_usd` instead of `null`s
        * API still always returns `null` for `failed`
    * There's a new option for the Ethereum address dashboard (`https://api.blockchair.com/{:eth_chain}/dashboards/address/{:address}`): `?transactions_instead_of_calls=true`. If applied, it returns the list of latest transaction hashes in the `transactions` array instead of returning the `calls` array. An important difference is that ERC-20 transfers that weren't previously included in the `calls` array will be among `transactions`. This option also affects `transaction_count` behaviour: if enabled, it returns the transaction count for an address which takes ERC-20 transfers into account. Using this option costs `2`. The option has no effect on contracts (i.e. for contracts the `calls` array will be returned even if the option is enabled).
    * The Ethereum stats dashboard (`https://api.blockchair.com/{:eth_chain}/stats`) now also outputs `addresses` yielding the total number of addresses (including both accounts and contracts) ever seen on chain.
* v.2.0.83 - August 6th, 2021
    * Added support for the Ethereum London hard fork. New table fields (for Ethereum and Ethereum Goerli Testnet):
        * Blocks:
            * `base_fee_per_gas` (queryable, sortable, aggregatable). Average base fee per day example: `https://api.blockchair.com/ethereum/blocks?a=date,avg(base_fee_per_gas)&q=id(12965000..)`
            * `burned_total` (queryable, sortable, aggregatable). Examples:
                * Total ETH burned: `https://api.blockchair.com/ethereum/blocks?a=date,sum(burned_total)&q=id(12965000..)`
                * Burned to minted ratio by day: `https://api.blockchair.com/ethereum/blocks?a=date,f(sum(burned_total)/sum(generation))&q=id(12965000..)`
        * Uncles:
            * `base_fee_per_gas` (queryable, sortable, aggregatable)
        * Transactions:
            * `effective_gas_price` (queryable, sortable, aggregatable)
            * `max_fee_per_gas` (`null` for legacy transactions; queryable, sortable, aggregatable)
            * `max_priority_fee_per_gas` (`null` for legacy transactions; queryable, sortable, aggregatable)
            * `base_fee_per_gas` (base fee of the block; queryable, sortable, aggregatable)
            * `burned` (calculated as `gas_used * base_fee_per_gas`; queryable, sortable, aggregatable)  
    * Added `burned` and `burned_24h` to the `https://api.blockchair.com/{:eth_chain}/stats` dashboard
    * Added `version` field (queryable, aggregatable) to Ethereum transactions. `0` represents legacy transactions, `1` is for EIP-2718 transactions, `2` is for London transactions. In Ethereum terminology this is "type" instead of "version", but as we're already using "type" for another column, we decided to go with "version". Example: `https://api.blockchair.com/ethereum/transactions?a=version,count()&q=block_id(12965000..)`. Please note that for all transactions before block #12965000 it always returns `0`, even for EIP-2718 transactions: this will be fixed in one of the next updates.
    * Added support for the Ethereum Berlin hard fork. Transactions now have an additional field called `type_2718` (queryable) that represents transaction type as outlined in [EIP-2718](https://eips.ethereum.org/EIPS/eip-2718) 
* v.2.0.82 - July 30th, 2021
    * Now it's possible to broadcast Cardano transactions using the `https://api.blockchair.com/cardano/push/transaction` endpoint
    * Despite the request cost formulas for infinitables have been set in the documentation since July 19th, 2020, they haven't been effective, and every request to inifintables always cost 1 point. Now the formulas are respected.
    * Fixed a bug with the `https://api.blockchair.com/{:btc_chain}/addresses` infinitable where some balances weren't returned correctly
    * Fixed a bug with the `https://api.blockchair.com/{:btc_chain}/addresses/balances?addresses={:list}` endpoint where some balances weren't calculated correctly
    * Fixed some bugs with eCash
* v.2.0.81 - July 12th, 2021
    * Infinitable enhancements:
        * Intervals are now available for querying time columns. Example usage: `https://api.blockchair.com/bitcoin/blocks?q=time(~P1D)&a=count()` counts the number of blocks over the last 24 hours, `https://api.blockchair.com/ethereum/mempool/transactions?q=time(~PT30S)` lists transactions that were included to the Ethereum mempool for the last 30 seconds. Previously users were required to set the interval manually (e.g. `?q=time(2021-07-11 07:58:58..2021-07-12 07:58:58)`). The format is an ISO 8601 Duration.
        * New aggregation functions to calculate running totals: `runningcount()`, `runningsum({:column})`, `runningavg({:column})`, `runningmedian({:column})`, `runningmin({:column})`, `runningmax({:column})`. Example usage: `https://api.blockchair.com/bitcoin/blocks?a=month,sum(size),runningsum(size)` calculates the blockchain size, `https://api.blockchair.com/bitcoin/blocks?a=date,avg(size),runningavg(size)` calculates the running average for block size, `https://api.blockchair.com/bitcoin/blocks?a=year,count(),runningcount()` calculates the number of blocks by the end of each year since 2009.
* v.2.0.80 - July 8th, 2021
    * Database dumps now feature Ethereum addresses, ERC-20 tokens, ERC-20 transfers, Zcash blocks, Zcash transactions, Zcash outputs, Zcash inputs, and Zcash addresses (transparent): https://gz.blockchair.com
    * There's a new `addresses` infinitable for Ethereum: `https://api.blockchair.com/ethereum/addresses`. The columns are: `address`, `balance`, `nonce`, `is_contract`. The default sort is by balance descending. Unlike with Bitocin-like `addresses` infinitables which are updated once every 5 minutes, this infinitable is only updated once a day. The documentation is available here: https://blockchair.com/api/docs#link_310. Some cool examples:
        * `https://api.blockchair.com/ethereum/addresses?a=is_contract,count()` - count accounts and contracts
        * `https://api.blockchair.com/ethereum/addresses?q=balance(1000000..)&a=count()` - count the number of addresses hodling more than 1 million ethers
        * Full dump is available here: https://gz.blockchair.com/ethereum/addresses/ (updated daily)
    * Stats endpoint for Bitcoin-like chains (`https://api.blockchair.com/{:btc_chain}/stats`) now includes `mempool_outputs`.
    * **BREAKING CHANGE**: Bitcoin ABC (which is still in beta status on our platform) is now renamed to eCash and starting from July 19th, 2021 00:00:00 UTC:
        * All API paths will be renamed from `bitcoin-abc` to `ecash` (right now both work)
        * All array keys that previously were named `bitcoin-abc` will be renamed to `ecash`
        * The prefix for the address format will be changed from `bitcoincash:` to `ecash:`
* v.2.0.79 - April 23rd, 2021
    * Added a new `?events=true` option to the `https://api.blockchair.com/ethereum/dashboards/transaction/{:hash}` endpoint (works with the `transactions` endpoint as well). This option costs `1` additional request point to use. When enabled, it adds an array of event logs to the output. Every log contains `topics`, `data`, `contract`, `log_index`, and `decoded_event`. Depending on how much our API knows about the event signature, there are 3 detalization levels for `decoded_event` (example transaction with all 3: `https://api.blockchair.com/ethereum/dashboards/transaction/0x7d52cf58fe78403e8816dae6e900baff92b35760b4ed81cecd2590eafcde3dad?events=true`):
        * Full data: `decoded_event` contains both the full event name with its argument names (`name_full`, example: `Approval(address owner, address spender, uint256 value)`), and the argument values in the `arguments` array;
        * Partial data: only `name_with_types` is known (example: `Withdrawal(address, uint256)`), `arguments` yields `null`;
        * No data: `decoded_event` yields `null`.
    * Added `eta_seconds` to the `https://api.blockchair.com/{:btc_chain}/dashboards/transaction/{:hash}/priority` and `https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}/priority` endpoints returning an approximate time for the transaction to confirm (in seconds). Please note it's an experimental function and may be unreliable.
    * Upgraded Dogecoin infrastructure for better transaction broadcasting
* v.2.0.78 - April 17th, 2021
    * The `?state=latest` option can now also be applied to the Ethereum address dashboard endpoint. If this option is enabled, `balance` will yield the confirmed balance, and the `calls` array won't include unconfirmed data. Example: `https://api.blockchair.com/{:eth_chain}/dashboards/address/{:address}?state=latest`.
    * Added a new `?contract_details=true` option to the `https://api.blockchair.com/ethereum/dashboards/address/{:address}₀` endpoint. If applied, it adds additional data on the address if it's a contract. At the moment, it works with ERC-20 contracts only yielding `token_name`, `token_symbol`, and `token_decimals`. It also yields some additional fields for all contracts: `creating_transaction_hash`, `creating_address`, and `creating_transaction_time`. The additional cost of using this option is `0.5`.
    * Added `hodling_addresses` to the `https://api.blockchair.com/{:btc_chain}/stats` endpoint (stats on Bitcoin-like blockchains) yielding the total number of addresses with positive balance.
    * Added `market_price_usd`, `market_price_btc`, `market_cap_usd` to the `https://api.blockchair.com/ethereum/erc-20/{:token_address}/stats` endpoint. `null`s are returned if there's no market data for the specified token.
* v.2.0.77 - March 15th, 2021
    * Added special statistical endpoints for USDT, USDC, and BUSD. Please note this feature is currently in test mode, there may be compatibility-breaking changes. These endpoints show the distribution of tokens amongst blockchain platforms they are issued on:
        * `https://api.blockchair.com/cross-chain/tether/stats` for Tether (USDT)
        * `https://api.blockchair.com/cross-chain/usd-coin/stats` for USD Coin (USDC)
        * `https://api.blockchair.com/cross-chain/binance-usd/stats` for Binance USD (BUSD)
        * `https://api.blockchair.com/stats` was also updated to show these stats
* v.2.0.76 - March 12th, 2021
    * Added a new `?assets_in_usd=true` option to the `https://api.blockchair.com/ethereum/dashboards/transaction/{:hash}` endpoint (works with the `transactions` endpoint as well). When applied, it adds `value_usd_now` to all `layer_2.erc_20` items yielding the current (not at the moment of the transaction!) USD value of tokens (`null` if the price is unknown). Example: `https://api.blockchair.com/ethereum/dashboards/transaction/0x77025c5c7ff5eeb4bb164a4be84dd49192e12086cc321199f73888830c3ecd9e?erc_20=true&assets_in_usd=true`
    * Added `{:hash}.layer_2.erc_20.{:index}.value_approximate` to the `https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}` endpoint when using `?erc_20=true` option
* v.2.0.75 - March 11th, 2021
    * Added `suggested_transaction_fee_gwei_options` to the `https://api.blockchair.com/{:eth_chain}/stats` endpoint yielding an array of suggested gas prices (`sloth` if you can take the risk and wait; `slow`, `normal`, and `fast` if you want to get the transaction confirmed within 2-10 minutes; `cheetah` if you'd like to try to get into the next block).
    * The `https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}₀/priority` endpoint now supports Ethereum Testnet
* v.2.0.74 - March 4th, 2021
    * `address.nonce` now yields `0` instead of `null` when using the `?nonce=true` option with the `https://api.blockchair.com/ethereum/dashboards/address/{:address}` dashboard for addresses that have made no transactions
    * The `https://api.blockchair.com/{:eth_chain}/dashboards/address/{:address}` endpoint now correctly returns token details when the `?erc_20={:list}` option is fed with non-lowered token addresses
    * Cardano API enhancements
    * Mixin API enhancements
* v.2.0.73 - February 26th, 2021
    * Added a new `?assets_in_usd=true` option to the `https://api.blockchair.com/ethereum/dashboards/address/{:address}` endpoint. When applied, it adds `asset_balance_usd` to the output yielding the total USD value of all account assets (currently it's most popular ERC-20 tokens only), as well as `balance_usd` to all `layer_2.erc_20` items.
    * Fixed wrong nonce values for some Ethereum transactions
    * The `https://api.blockchair.com/premium/stats?key={:api_key}` now yields request points instead of raw requests in `requests_today`
    * The `https://api.blockchair.com/{:eth_chain}/push/transaction` endpoint now yields a more detailed error description (`context.error`) in case the broadcast fails (e.g. `nonce too low`, `insufficient funds for gas * price + value` or `already known`)
* v.2.0.72 - December 18th, 2020
    * Added an ability to retrieve internal calls for unconfirmed Ethereum transactions. Usage: `https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}?trace_mempool=true`. It's also possible to retrieve the list of ERC-20 transfers for mempool transactions: `https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}?trace_mempool=true&erc_20=true`. This is an experimental feature. Please note that internal transfers may get invalidated when transaction gets confirmed.
    * The `https://api.blockchair.com/{:eth_chain}/dashboards/transaction/{:hash}` dashboard is now more efficient at handling recently (1-30 seconds ago) confirmed transactions
    * Updated list of Bitcoin mining pools
    * Fixed some issues with Bitcoin ABC
* v.2.0.71 - December 14th, 2020
    * Improved Dogecoin transaction broadcasting
    * Added `?output=type` option to the `https://api.blockchair.com/{:eth_chain}/dashboards/address/{:address}` dashboard. When this option is enabled, only address type (`account` or `contract`) is returned. This may be a very fast handy way instead of requesting full address data. Example: `https://api.blockchair.com/ethereum/dashboards/address/0x00000000219ab540356cbb839cbe05303d7705fa?output=type`.
    * Fixed a bug where the `https://api.blockchair.com/{:btc_chain}/raw/transaction/{:hash}` endpoint returned code `200` even if there was a back end error
    * Fixed GitHub issue #320 ("No data returned for address balance mass check for some bitcoin-cash addresses", https://github.com/Blockchair/Blockchair.Support/issues/320)
    * Added new nodes to the Release monitor: `Bitcoin Cash Node` for Bitcoin Cash, `Cardano Node` for Cardano
* v.2.0.70 - November 17th, 2020
    * We're introducing News aggregator API! Starting today not only Blockchair API provides you with blockchain data, but also with some crypto news to integrate into your app. We're aggregating data from more than 60 news outlets in 14 languages, populating over 35,000 headlines into our database a month. Documentation: https://blockchair.com/api/docs#link_M7. Want your media outlet to be included to the aggregator? Please contact us at [info@blockchair.com](mailto:info@blockchair.com)!
* v.2.0.69 - November 16th, 2020
    * ERC-20 tokens in the `https://api.blockchair.com/{:eth_chain}/dashboards/address/{:address}?erc_20={approximate|precise|{:list}}` endpoint are now sorted by their market capitalization descending (so the most popular ones are on top now)
    * It's now possible to aggregate the Ethereum transactions infinitable (`https://api.blockchair.com/ethereum/transactions`) and the Ethereum calls infinitable (`https://api.blockchair.com/ethereum/calls`) by `sender` and `recipient` (the same applies for the Ethereum testnet). Example: show top 100 stakers of the eth2 contract (by number of deposits): `https://api.blockchair.com/ethereum/calls?q=recipient(0x00000000219ab540356cbb839cbe05303d7705fa),failed(false)&limit=100&a=sender,count()&s=count()(desc)`
* v.2.0.68 - November 10th, 2020
    * Added an experimental `?effects=true` option to the `https://api.blockchair.com/ethereum/dashboards/transaction/{:hash}` dashboard. Example: `https://api.blockchair.com/ethereum/dashboards/transaction/0xd9a24f57c713207c39c58e8ef3cb44e115fcc8bd0f85eb4ea82c78bc065a723f?effects=true&erc_20=true`. `effects` array yields the list of all changes to ETH and ERC-20 token balances.
    * Added an experimental endpoint to retrieve allowance for ERC-20 contracts: `https://api.blockchair.com/ethereum/erc-20/{:token_address}/utils/allowance?owner={:owner_address}&spender={:spender_address}`. Example: `https://api.blockchair.com/ethereum/erc-20/0x9f8f72aa9304c8b593d555f12ef6589cc3a579a2/utils/allowance?owner=0x448bb00f370da5af5d33d3e7fca686379fc782ea&spender=0xe0e6b25b22173849668c85e06bc2ce1f69baff8c`
    * The `https://api.blockchair.com/{:eth_chain}/dashboards/address/{:address}` dashboard now has 3 options to retrieve ERC-20 balances:
        * `?erc_20=approximate` (or `?erc_20=true`, default) - yields all token balances from our database. These values may miss some non-standard transfers in tokens that don't follow the ERC-20 standard in full. Please double-check if this option returns correct values for the tokens you'd want to get information about. Using this option costs `1`.
        * `?erc_20=precise` - yields all token balances from our node. The process is the following: we gather information from our database about potential ERC-20 tokens the address may hold, and then for each token we call `getBalance` function using our node to get precise balances. Please note that if for some reason some contract doesn't follow the ERC-20 standard, our database may still miss records about the address holding this token, and there will be no request to the node about this token. So while balances yielded with this option are precise, some non-standard tokens may still be missed. Using this option costs `2`.
        * `?erc_20={:token_address}₀,...,{:token_address}ᵩ` (recommended) - yields balances for the enlisted ERC-20 tokens from our node. That's the recommended way if you have an exact list of tokens you'd like to check. Even if some token doesn't follow the ERC-20 standard, but still has `getBalance` function implemented, the correct balance will be returned. Using this option costs `0.75` + `0.01` for each contract checked (the cheapest option!)
    * Improved efficiency of the `https://api.blockchair.com/ethereum/erc-20/{:token_address}/stats` endpoint
    * Fixed a bug with cUSDT Ethereum contract
    * Fixed some missing ERC-20 transfers
* v.2.0.67 - November 6th, 2020
    * Added Bitcoin ABC (Bitcoin Cash ABC) support. Please read our statement on the upcoming Bitcoin Cash split: https://twitter.com/Blockchair/status/1324424632179576832. Also please note that it is expected that Bitcoin ABC's hashrate will be very low so 51% attacks are possible. We'll be running Bitcoin ABC in beta mode and we don't guarantee neither its stability, nor that we'll run it if the chain won't be used by businesses.
* v.2.0.66 - September 23rd, 2020
    * We've upgraded our Ethereum engine - blocks, transactions, and ERC-20 transfers are now processed more than 10 times faster.
    * Added Ethereum Goerli testnet: https://blockchair.com/ethereum/testnet (API endpoints are the same as for Ethereum, just use `ethereum/testnet` instead of `ethereum`; ERC-20's are also supported for the testnet).
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
