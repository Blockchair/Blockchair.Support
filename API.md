# [Blockchair.com](https://blockchair.com/) API<sup>β</sup>

### A couple of ground rules

1. Our API is not currently intended for production use by 3rd parties. By that we mean the following:
    1. We can't guarantee that some API call won't be suddenly changed;
    2. There are no uptime promises;
    3. We may ban any user who consumes too many resources at any time without preliminary warning.
    
    So, please, use our API only for sandbox purposes. If you require an API for commercial use, please, reach us at [info@blockchair.com](mailto:info@blockchair.com) so we could provide you with a suitable solution. 
    
2. Currently we allow only 15 requests per minute to our API. If your application abuses the rate limits, it will be automatically temporary blocked. If you need a higher request rate, please contact us.

3. If you use data obtained using our API and show it on a website, please mention Blockchair as your data source.

4. If you want to be notified about changes in our API, drop us a line or two about how you use Blockchair. You’ll get included in the subscription list.

5. If you have any suggestions regarding our API, open an issue here: https://github.com/Blockchair/Blockchair.Support/issues

### API calls

#### Bitcoin and Bitcoin Cash

* https://api.blockchair.com/bitcoin — shows the basic info about the Bitcoin blockchain
* https://api.blockchair.com/bitcoin/blocks?{query}
* https://api.blockchair.com/bitcoin/transactions?{query}
* https://api.blockchair.com/bitcoin/outputs?{query}
* https://api.blockchair.com/bitcoin/mempool/transactions?{query}
* https://api.blockchair.com/bitcoin/mempool/outputs?{query}
* https://api.blockchair.com/bitcoin/dashboards/address/{address}

(replace `bitcoin` with `bitcoin-cash` in URLs for the Bitcoin Cash blockchain)

`{query}` allows you to query the database using the following parameters:

* `q=(...)` to filter the table,
* `s=(...)` to sort the table,
* `next={last_received_id}` and `next_sort={last_value_in_the_sorted_column}` to paginate.

You can use our front-end to build queries. Here's an example:

* https://blockchair.com/bitcoin/blocks?q=time(2017-01-01..2017-12-31)&s=fee_total(desc) — sort Bitcoin blocks mined in 2017 by fee descending. This page calls https://api.blockchair.com/bitcoin/blocks?q=time(2017-01-01..2017-12-31)&s=fee_total(desc)

Basically, using a web inspector you can analyze what queries our front-end sends to the API server.

You can also include `export=csv` or `export=tsv` to get the data in csv or tsv format instead of paginated JSON (max. 1 million cells).

Note that `mempool/transactions` and `mempool/outputs` calls return mempool transactions AND transactions from the latest block (there's also `https://api.blockchair.com/bitcoin/mempool/blocks` call showing the latest block alone).

#### We'll be expanding the documentation in the future. As of now, if you have any questions, please contact us using
* E-mail: [info@blockchair.com](mailto:info@blockchair.com)
* Telegram: [@Blockchair](https://telegram.me/Blockchair)
* Twitter: [@Blockchair](https://twitter.com/Blockchair)
