# [Blockchair.com](https://blockchair.com/) API

### API v.2 documentation
* English: [API_DOCUMENTATION_EN.md](API_DOCUMENTATION_EN.md).
* Russian: [API_DOCUMENTATION_RU.md](API_DOCUMENTATION_RU.md).

### Changelog

* v.2.0.6 - Oct 8th - Added data aggregation of blockchain data in beta mode, see `Data aggregation support` below
* v.2.0.5 - Oct 8th - Fixed bug where `balance` and `received` for bitcoin[-cash]|litecoin addresses in the `{chain}/dashboards/address/{address}` call were calculated wrong if there were specific unconfirmed transactions
* v.2.0.4 - Oct 3rd - Added some new useful fields to `{chain}/stats` calls
* v.2.0.3 - Sep 18th - Added `context.api.tested_features` with the list of features our API supports, but with no guarantee for backward compatibility if updated. Added Omni Layer and Wormhole support in testing mode (see "Tested features changelog")
* v.2.0.2 - Sep 9th, 2018 - Added `address.contract_created` to the `ethereum/dashboards/address/{A}` call
* v.2.0.1 - Sep 1st, 2018 - Added Litecoin support
* v.2.0.0 - Migrating from API v.1 to API v.2 (see the docs)

##### Data aggregation support (since Oct 8th - please note this is a tested feature!)

* v.b1 - Oct 8th - Bringing the ability to receive aggregated data. Now you can use Blockchair not only to filter and sort blockchain data, but also aggregate it.

See the examples:
* https://api.blockchair.com/bitcoin/blocks?a=year,count()# - get the total number of Bitcoin blocks by year
* https://api.blockchair.com/bitcoin/transactions?a=month,median(fee_usd)# - get the median Bitcoin transaction fee by month
* https://api.blockchair.com/ethereum/blocks?a=miner,sum(generation)&s=sum(generation)(desc)# - get the list of Ethereum miners (except uncle miners) and sort it by the total amount minted
* https://api.blockchair.com/bitcoin-cash/blocks?a=sum(fee_total_usd)&q=id(478559..)# - calculate how much the miners have collected in fees since the fork
    
To use aggregation, put the fields by which you'd like to group by (zero, one, or several), and fields which you'd like to calculate using some aggregate function (at least one) under the `?a=` section. You can also sort the results by one of the fields included in the `?a=` section (`asc` or `desc`) using the `?s=` section, and apply additional filters (see the documentation for the `?q=` section).

Possible fields:
* Bitcoin, Bitcoin Cash, Litecoin:
    * Blocks table
        * Group by: date (or week, month, year), version, guessed_miner
        * To calculate: size, stripped_size (except BCH), weight (except BCH), transaction_count, witness_count, input_count, output_count, input_total, input_total_usd, output_total, output_total_usd, fee_total, fee_total_usd, fee_per_kb, fee_per_kb_usd, fee_per_kwu (except BCH), fee_per_kwu_usd (except BCH), cdd_total, generation, generation_usd, reward, reward_usd -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Transactions table
        * Group by: block_id, date (or week, month, year), version, is_coinbase, has_witness (except BCH), input_count, output_count
        * To calculate: size, weight (except BCH), input_count, output_count, input_total, input_total_usd, output_total, output_total_usd, fee, fee_usd, fee_per_kb, fee_per_kb_usd, fee_per_kwu (except BCH), fee_per_kwu_usd (except BCH), cdd_total -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Outputs table
        * Group by: block_id, date (or week, month, year), type, is_from_coinbase, is_spendable, is_spent, spending_block_id, spending_date (no support for spending_week, spending_month, spending_year yet)
        * To calculate: value, value_usd, spending_value_usd, lifespan, cdd -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
* Ethereum:
    * Blocks table
        * Group by: date (or week, month, year), miner
        * To calculate: size, difficulty, gas_used, gas_limit, uncle_count, transaction_count, synthetic_transaction_count, call_count, synthetic_call_count, value_total, value_total_usd, internal_value_total, internal_value_total_usd, generation, generation_usd, uncle_generation, uncle_generation_usd, fee_total, fee_total_usd, reward, reward_usd -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Uncles table
        * Group by: parent_block_id, date (or week, month, year), miner
        * To calculate: size, difficulty, gas_used, gas_limit, generation, generation_usd -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Transactions table
        * Group by: block_id, date (or week, month, year), failed, type
        * To calculate: call_count, value, value_usd, internal_value, internal_value_usd, fee, fee_usd, gas_used, gas_limit, gas_price -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()
    * Calls table
        * Group by: block_id, date (or week, month, year), failed, fail_reason, type, transferred
        * To calculate: child_call_count, value, value_usd -- possible functions: avg(field), median(field), min(field), max(field), sum(field), count()  

### Support

* E-mail: [info@blockchair.com](mailto:info@blockchair.com)
* Telegram chat: [@Blockchair](https://telegram.me/Blockchair)
* Twitter: [@Blockchair](https://twitter.com/Blockchair)
