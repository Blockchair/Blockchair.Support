Main logo
Search for transactions, addresses, blocks and embedded text data...


PSA:
Some of our services (including Ethereum and Bitcoin Cash explorers) are currently unavailable due to a global Internet shutdown in Kazakhstan. We're working on activating our reserve infrastructure. Situation updates will be available at https://status.blockchair.com/ and at https://twitter.com/Blockchair/status/1478698203792949249
API
API documentation
API documentation
Introduction
General stats endpoints
Retrieve overall information about blockchains and tokens
Dashboard endpoints
Retrieve information about various entities in a neat format from our databases
Raw data endpoints
Retrieve raw information about various entities directly from our full nodes
Infinitable endpoints
SQL-like queries: filter, sort, and aggregate blockchain data
Misc endpoints
Privacy-o-meter
News aggregator
Support
Show all
Blockchair API
Blockchair API
Blockchair API provides developers, researchers, and businesses with access to data contained in 20 blockchains
Explore pricing plans
Blockchair.com API v.2.0.80 Documentation
    ____  __           __        __          _     
   / __ )/ /___  _____/ /_______/ /_  ____ _(_)____
  / __  / / __ \/ ___/ //_/ ___/ __ \/ __ `/ / ___/
 / /_/ / / /_/ / /__/ ,< / /__/ / / / /_/ / / /    
/_____/_/\____/\___/_/|_|\___/_/ /_/\__,_/_/_/     
                                                   
Introduction
Blockchair API provides developers with access to data contained in 18 different blockchains. Unlike other APIs, Blockchair also supports numerous analytical queries like filtering, sorting, and aggregating blockchain data.

Here are some examples of what you can build using our API:

A wallet supporting multiple blockchains (request transaction, address, xpub data, and also broadcast transactions)
An analytical service showing some blockchain stats and visualizations
A service tracking the integrity of your or your customers' cold wallets
A solid academic research
Some fun stuff like finding the first Bitcoin block over 1 megabyte in size
For some tasks like extracting lots of blockchain data (e.g. all transactions over a 2 month period) it's better to use our Database dumps feature instead (see https://blockchair.com/dumps for documentation) — it's possible to download the entire database dumps in TSV format and insert the data onto your own database server (like Postgresql or whatever) to further analyze it.

Almost every API endpoint description is accompanied with an example visualization of the data on our website (https://blockchair.com), and it's also worth it to note that the website is working completely using our API (yes, even the data for charts is pulled from one of our endpoints, and it's fully customizable).

Blockchair cares about user privacy, we neither collect nor share with anyone your personal data rather than for statistical purposes. That includes using the API as well. Please refer to our Privacy policy: https://blockchair.com/privacy. Please also check out our Terms of service available here: https://blockchair.com/terms — by using our API, you are agreeing to these terms.

We have a public tracker for bugs, issues, and questions available on GitHub: https://github.com/Blockchair/Blockchair.Support/issues — please use it or contact us by any other means available.

Our API is free to try under some limitations, and we have a variety of premium plans. Please check out the information about the limits and plans.

Blockchair logo
About Blockchair
FAQ
Changelog
Careers
Terms of service
Privacy policy
TwitterTelegramGitHubLinkedIn
© 2021 Blockchair
No 3d party trackers
Works without javascript
