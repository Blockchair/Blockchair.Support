## [Blockchair.com](https://blockchair.com/) API v.2.0.9 - документация

![Blockchair logo](https://blockchair.com/images/logo_full.png "Blockchair logo")

### Содержание

+ [Changelog](#changelog)
  + [Тестируемые фичи](#changelog-тестируемых-фич)
+ [Общие положения](#общие-положения)
+ [Вызовы для работы с infinitables](#вызовы-для-работы-с-infinitables-таблицы-блокчейнов)
+ Bitcoin, Bitcoin Cash, Litecoin
  + [Блоки](#bitcoin-cashlitecoinmempoolblocks)
  + [Транзакции](#bitcoin-cashlitecoinmempooltransactions)
  + [Выходы](#bitcoin-cashlitecoinmempooloutputs)
+ Ethereum
  + [Блоки](#ethereummempoolblocks)
  + [Анклы](#ethereumuncles)
  + [Транзакции](#ethereummempooltransactions)
  + [Коллы](#ethereumcalls)
+ [Примечания](#примечания)
+ [Dashboard-вызовы](#dashboard-вызовы)
  + [Блоки](#bitcoin-cashlitecoinethereumdashboardsblocka-и-bitcoin-cashethereumdashboardsblocksab)
  + [Анклы (Ethereum)](#ethereumdashboardsunclea-и-ethereumdashboardsunclesab)
  + [Транзакции](#bitcoin-cashlitecoinethereumdashboardstransactiona-и-bitcoin-cashlitecoinethereumdashboardstransactionsab)
  + [Приоритет неподтверждённых транзакций (в мемпуле)](#bitcoin-cashlitecoinethereumdashboardstransactionhashpriority)
  + [Адрес Bitcoin, Bitcoin Cash, Litecoin](#bitcoin-cashlitecoindashboardsaddressa)
  + [Адрес Ethereum](#ethereumdashboardsaddressa)
  + [Статистика](#bitcoin-cashlitecoinethereumstats)
  + [Статистика по всем блокчейнам](#stats)
  + [Статистика сети](#bitcoin-cashlitecoinnodes) 
+ [Пример](#пример-работы-с-api)
+ [Рассылка транзакций](#рассылка-транзакций)
+ [Получение транзакций в сыром виде](#получение-транзакций-в-сыром-виде)
+ [Поддержка](#поддержка)

### Changelog

* v.2.0.9 - 12 декабря - Добавлена [поддержка Bitcoin SV](#поддержка-bitcoin-sv-с-12-декабря) в тестовом режиме; обновлены возможности [агрегации данных](#поддержка-агрегирования-данных-с-8-октября)
* v.2.0.8 - 26 ноября - Появилась возможность получать транзакции в сыром виде, см. [Получение транзакций в сыром виде](#получение-транзакций-в-сыром-виде)
* v.2.0.7 - 22 ноября - Появилась возможность рассылать транзакции через API, см. [Рассылка транзакций](#рассылка-транзакций)
* v.2.0.6 - 8 октября - В бета-режиме добавлена возможность агрегировать информацию из блокчейнов, см. `Поддержка агрегирования данных` ниже
* v.2.0.5 - 8 октября - Исправлен баг с подсчётом `balance` и `received` у bitcoin[-cash]|litecoin-адресов в колле `{chain}/dashboards/address/{address}`, когда имелись специфические неподтверждённые транзакции
* v.2.0.4 - 3 октября - Добавлены некоторые полезные поля к коллам `{chain}/stats`
* v.2.0.3 - 18 сентября - Добавлен ключ `context.api.tested_features` со списком тестируемых фич, поддерживаемых нашим API (у тестируемых фич нет гарантий поддержки в будущем, гарантий, что в какой-то момент не потеряется совместимость при обновлении). Добавлена поддержка Omni Layer и Wormhole в режиме тестирования (см. ниже)
* v.2.0.2 - 9 сентября - Добавлено поле `address.contract_created` для колла `ethereum/dashboards/address/{A}`
* v.2.0.1 - 1 сентября - Добавлена поддержка Litecoin

### Changelog тестируемых фич

##### Поддержка Bitcoin SV (с 12 декабря)

* v.b1 - 12 декабря - Ура! Теперь мы предоставляем данные по Bitcoin SV (BSV). Все API-вызовы совместимы с таковыми для Bitcoin Cash, например, если вы хотите получить последние nulldata-выходы (OP_RETURN), то просто замените `bitcoin-cash` на `bitcoin-sv`: https://api.blockchair.com/bitcoin-sv/outputs?q=type(nulldata)#

Пожалуйста, имейте в виду, что поддержка Bitcoin SV осуществляется в тестовом режиме и не предназначена для использования в рабочей среде, пока Bitcoin SV не продемонстрирует более конструктивную дорожную карту (например, мы не сможем предоставить некоторый функционал, если блоки внезапно увеличатся до 1 экзабайта...)

##### Поддержка агрегирования данных (с 8 октября)

* v.b1 - 8 октября - Внедрение возможности получать агрегированную информацию. Теперь вы можете использовать Blockchair не только для фильтрации и сортировки информации из блокчейнов, но и для агрегации данных.

Пожалуйста, не используйте это в рабочей среде, могут быть фундаментальные изменения!

См. примеры:
* https://api.blockchair.com/bitcoin/blocks?a=year,count()# - выдаёт количество блоков в Bitcoin по годам
* https://api.blockchair.com/bitcoin/transactions?a=month,median(fee_usd)# - медианные комиссии за транзакции в Bitcoin по месяцам
* https://api.blockchair.com/ethereum/blocks?a=miner,sum(generation)&s=sum(generation)(desc)# - список майнеров (кроме майнеров анклов) Ethereum (отсортированный по количеству созданных монет)
* https://api.blockchair.com/bitcoin-cash/blocks?a=sum(fee_total_usd)&q=id(478559..)# - сколько майнеры собрали на комиссиях в Bitcoin Cash с момента форка

Чтобы использовать агрегирование, укажите поля по которым вы хотите группировать (ни одного, одно, или несколько), и поля, которые вы хотите посчитать с помощью функций агрегирования (как минимум одно) в секции `?a=`. Вы также можете отсортировать результаты по одной из колонок (`asc` или `desc`), указанных в секции `?a=` используя секцию `?s=`, а также применить дополнительные фильтры (см. докуметацию для секции `?q=`)

Возможные поля:
* Bitcoin, Bitcoin Cash, Litecoin:
    * [Blocks](#bitcoin-cashlitecoinmempoolblocks)
        * Группировка по: date (или week, month, year), version, guessed_miner
        * Подсчёт: size, stripped_size (кроме BCH), weight (кроме BCH), transaction_count, witness_count, input_count, output_count, input_total, input_total_usd, output_total, output_total_usd, fee_total, fee_total_usd, fee_per_kb, fee_per_kb_usd, fee_per_kwu (кроме BCH), fee_per_kwu_usd (кроме BCH), cdd_total, generation, generation_usd, reward, reward_usd — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()
    * [Transactions](#bitcoin-cashlitecoinmempooltransactions)
        * Группировка по: block_id, date (или week, month, year), version, is_coinbase, has_witness (кроме BCH), input_count, output_count
        * Подсчёт: size, weight (кроме BCH), input_count, output_count, input_total, input_total_usd, output_total, output_total_usd, fee, fee_usd, fee_per_kb, fee_per_kb_usd, fee_per_kwu (кроме BCH), fee_per_kwu_usd (кроме BCH), cdd_total — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()
    * [Outputs](#bitcoin-cashlitecoinmempooloutputs)
        * Группировка по: block_id, date (или week, month, year), type, is_from_coinbase, is_spendable, is_spent, spending_block_id, spending_date (no support for spending_week, spending_month, spending_year yet)
        * Подсчёт: value, value_usd, spending_value_usd, lifespan, cdd — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()
* Ethereum:
    * [Blocks](#ethereummempoolblocks)
        * Группировка по: date (или week, month, year), miner
        * Подсчёт: size, difficulty, gas_used, gas_limit, uncle_count, transaction_count, synthetic_transaction_count, call_count, synthetic_call_count, value_total, value_total_usd, internal_value_total, internal_value_total_usd, generation, generation_usd, uncle_generation, uncle_generation_usd, fee_total, fee_total_usd, reward, reward_usd — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()
    * [Uncles](#ethereumuncles)
        * Группировка по: parent_block_id, date (или week, month, year), miner
        * Подсчёт: size, difficulty, gas_used, gas_limit, generation, generation_usd — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()
    * [Transactions](#ethereummempooltransactions)
        * Группировка по: block_id, date (или week, month, year), failed, type
        * Подсчёт: call_count, value, value_usd, internal_value, internal_value_usd, fee, fee_usd, gas_used, gas_limit, gas_price — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()
    * [Calls](#ethereumcalls)
        * Группировка по: block_id, date (или week, month, year), failed, fail_reason, type, transferred
        * Подсчёт: child_call_count, value, value_usd — возможные функции: avg(field), median(field), min(field), max(field), sum(field), count()

##### Поддержка Omni Layer и Wormhole (с 18 сентября)

* v.a1 - 18 сентября - В режиме альфа-версии добавлена поддержка Omni Layer в Bitcoin (`bitcoin/omni/properties`, `bitcoin/omni/dashboards/property/{id}` коллы, плюс ключ `_omni` в `bitcoin/dashboards/transaction` и ключ `_omni` в `bitcoin/dashboards/address`), а также поддержка Wormhole в Bitcoin Cash (`bitcoin-cash/wormhole/properties`, `bitcoin-cash/wormhole/dashboards/property/{id}` коллы, плюс ключ `_wormhole` в `bitcoin-cash/dashboards/transaction` и ключ `_wormhole` в `bitcoin-cash/dashboards/address`). Пожалуйста, не используйте тестируемые фичи в продакшене, будут изменения.

### Общие положения

* Запрос к серверу идёт через протокол https GET-запросами к домену `api.blockchair.com`

* Ответ от сервера возвращает JSON-массив всегда состоящий из двух подмассивов:
    * `data` - содержит непосредственно отдаваемые данные
    * `context` - содержит мета-данные, например, возвращаемый код, время выполнения запроса и т.д.

* `data` может содержать либо ассоциативный массив (например, для `bitcoin/stats`), либо для infinitable-запросов ненумерованный (например, `bitcoin/blocks`), либо для dashboard-запросов ассоциативный, для которого ключами являются части запроса (например, для `bitcoin/dashboards/transactions/A,B` ключами будут `A` и `B`), а значениями - массивы информации.
* `context` в зависимости от коллов может содержать в себе следующие полезные значения:
    * `context.code` - код ответа сервера, может вернуть:
        * `200` если запрос успешен
        * `400` если в запросе есть пользовательская ошибка
        * `402` если превышен какой-либо лимит на количество или объём запросов (см. ограничение на количество запросов, пожалуйста, свяжитесь с нами по адресу <info@blockchair.com> если вам нужно увеличение лимитов)
        * `500` или `503` в случае ошибки сервера (имеет смысл подождать и повторить тот же запрос или открыть тикет на https://github.com/Blockchair/Blockchair.Support/issues/new или написать на <info@blockchair.com>)
    * В случае ошибок `40x` и `50x` есть поле `context.error` с описанием ошибки
    * `context.state` содержит в себе номер последнего блока в случае запросов к блокчейнам (например, для всех запросов, начинающихся с `bitcoin` там будет последний блок блокчейна Биткоина на момент запроса). Полезно для того, чтобы, например, посчитать количество подтверждений сети, или правильно итерировать результаты с использованием `offset`
    * `context.results` - для dashboard-коллов содержит в себе количество найденных результатов
    * `context.limit` - применённый лимит количества результатов
    * `context.offset` - применённый сдвиг нумерации результатов
    * `context.rows` - для infinitable-коллов содержит в себе количество возвращаемых строк
    * `context.pre_rows` - для некоторых infinitable-коллов содержит в себе количество строк, которые должны были бы вернуться до склеивания одинаковых (примечание: подобная архитектура использутся только для таблиц `bitcoin[-cash].outputs`)
    * `context.total_rows` - сколько всего строк возвращает запрос
    * `context.api` - массив информации о состоянии API:
        * `context.api.version` - версия API
        * `context.api.last_major_update` - время последнего обновления, которое каким-либо образом утрачивает обратную совместимость
        * `context.api.next_major_update` - время следующего запланированного обновления, которое может изменить выдаваемые результаты, или `null`, если обновлений не запланировано
		* `context.api.tested_features` - the list (comma-separated) of features with version numbers our API supports, but with no guarantee for backward compatibility if updated (in this case there will be no changes to `context.api.next_major_update` as well)
		* `context.api.documentation` - an URL to the latest version of documentation

Примечание: разумно следить за `context.api.version` и, если `context.api.next_major_update` не `null`, уведомить себя и изучить changelog. В случае, если в changelog нет изменений, которые нарушат совместимость вашего приложения, следить за тем, чтобы значение `context.api.next_major_update` не стало больше текущего. В случае, если изменения есть, настроить логику приложения так, чтобы после указанного времени применялась новая логика. Дополнительное примечание: в случае, если обратная совместимость нарушается только для какого-то одного API-колла, то возможна ситуация, когда `context.api.next_major_update` будет не `null` только для этого колла.

* Ограничение на количество запросов: на текущий момент мы допускаем не более 30 запросов в минуту к нашему API от одного пользователя. В случае превышения этого лимита будет выдаваться ошибка `402`. В случае злоупотребления, может быть заблокирован IP-адрес. Если вашему приложению необходимо большее количество запросов, пожалуйста, свяжитесь с нами по адресу <info@blockchair.com>. Если вам необходимо разово выгрузить большой объём информации, свяжитесь с нами по адресу <export@blockchair.com> и, в случае, если цель выгрузки академическая или исследовательская - вы получите данные бесплатно в удобном формате.

* Отказ от ответственности: мы не гарантируем ни достоверность, ни целостность отдаваемой информации. Информация, выдаваемая нашим API не должна использоваться при принятии критически важных решений. Мы не гарантируем аптайм для нашего бесплатного API.

#### Вызовы для работы с infinitables (таблицы блокчейнов)

Возвращают информацию из таблиц в соответствии с фильтрами (`q`), сортировкой (`s`), лимитом (`limit`), и оффсетом (`offset`).

Запрос составляется следующим образом: `https://api.blockchair.com/{blockchain}/[/mempool]{table}[?q={query}][&s={sorting}][&limit={limit}][&offset={offset}]`

**Возможные комбинации блокчейнов и таблиц:**
* Bitcoin:
    * `bitcoin/blocks` - содержит все блоки Bitcoin, включая последний
    * `bitcoin/transactions` - содержит все транзакции Bitcoin, исключая транзакции, находящиеся в мемпуле, а также в последнем блоке
    * `bitcoin/outputs` - содержит все выходы Bitcoin, исключая выходы, содержащиеся в транзакциях в мемпуле, а также в транзакциях в последнем блоке
    * `bitcoin/mempool/blocks` - содержит в себе только последний блок Bitcoin
    * `bitcoin/mempool/transactions` - содержит в себе транзакции Bitcoin, находящиеся в мемпуле, а также транзакции последнего блока
    * `bitcoin/mempool/outputs` - содержит выходы Bitcoin, содержащиеся в транзакциях в мемпуле, а также в транзакциях в последнем блоке
* Litecoin, Bitcoin Cash - аналогично Bitcoin
* Ethereum:
    * `ethereum/blocks` - содержит все блоки Ethereum, кроме последних 6
    * `ethereum/uncles` - содержит все анклы Ethereum, кроме тех, которые принадлежат последним 6 блокам
    * `ethereum/transactions` - содержит все транзакции Ethereum, кроме входящих в последние 6 блоков
    * `ethereum/calls` - содержит все коллы транзакций Ethereum, кроме входящих в последние 6 блоков
    * `ethereum/mempool/blocks` - содержит последние 6 блоков Ethereum, некоторые колонки содержат null
    * `ethereum/mempool/transactions` - содержит все транзакции Ethereum из последних 6 блоков, а также находящиеся в мемпуле

Примечание: для быстроты работы наша архитектура содержит отдельные таблицы (`mempool*`) для неподтверждённых транзакций, а также для блоков, которые с определённой вероятностью могут форкнуться от основной цепи. Для Bitcoin, Bitcoin Cash, и Litecoin в `mempool*` содержится помимо мемпула последний блок, а для Ethereum - последние 6 блоков. Исключение: для Bitcoin, Bitcoin Cash, и Litecoin в таблице `blocks` также содержится информация о последнем блоке (это поведение может измениться в будущем). Для Ethereum мы не "проигрываем" транзакции целиком для последних 6 блоков, поэтому таблицы `mempool/calls` нет.

**Фильтром** можно пользоваться следующим образом: `?q=field(value)[,field(value)...]`, где `field` - это колонка, по которой необходим фильтр, а `value` - значение, специальное значение или диапазон значений. Возможные колонки приведены в табличках ниже. Возможные выражения для значений:
* `value` - например, `bitcoin/blocks?q=id(0)` найдёт информацию о блоке 0
* `left..` - нестрогое неравенство - например, `bitcoin/blocks?q=id(1..)` найдёт информацию о блоке 1 и выше
* `left...` - строгое неравенство - например, `bitcoin/blocks?q=id(1...)` найдёт информацию о блоке 2 и выше
* `..right` - нестрогое неравенство - например, `bitcoin/blocks?q=id(..1)` найдёт информацию о блоках 0 и 1
* `...right` - строгое неравенство - например, `bitcoin/blocks?q=id(...1)` найдёт информацию только о блоке 0
* `left..right` - нестрогое неравенство - например, `bitcoin/blocks?q=id(1..3)` найдёт информацию о блоках 1, 2 и 3
* `left...right` - строгое неравенство - например, `bitcoin/blocks?q=id(1...3)` найдёт информацию только о блоке 2
* `~like` - вхождение в строку (оператор `LIKE`), например, `bitcoin/blocks?q=coinbase_data_bin(~hello)` найдёт все блоки, в `coinbase_data_bin` которых содержится `hello`
* `^like` - вхождение в начало строки (оператор `STARTS WITH`), например, `bitcoin/blocks?q=coinbase_data_hex(^00)` найдёт все блоки, для которых `coinbase_data_hex` начинается с `00`

Для `time*`-полей значение можно указывать в следующих форматах:
* `YYYY-MM-DD HH:ii:ss`
* `YYYY-MM-DD`
* `YYYY-MM`

Неравенства также поддерживаются для таких значений, но и слева, и справа значения должны быть в одном формате, например: `bitcoin/blocks?q=time(2009-01-03..2009-01-31)`.

Если нужно перечислить несколько фильтров, их нужно перечислить через запятую в секции `?q=`, например, `bitcoin/blocks?q=id(500000..),coinbase_data_bin(~hello)`

В качестве тестируемой функции поддерживаются операторы `NOT` и `OR` (поведение может измениться в будущем).

Оператор `NOT` указывается через запятую перед выражением, которое нужно инвертировать, например, `bitcoin/blocks?q=not,id(1..)` вернёт блок `0`.

Оператор `OR` указывается между выражениями и имеет приоритет (два выражения вокруг `OR` как бы оборачиваются в скобки), например, `bitcoin/blocks?q=id(1),or,id(2)` вернет информацию о блоках 1 и 2.

Максимальное гарантированное поддерживаемое количество фильтров в одном запросе: 5.

**Сортировкой** можно пользоваться следующим образом: `?s=field(direction)`, где `direction` может быть либо `asc` для сортировки по возрастанию, либо `desc` для сортировки по убыванию, например `bitcoin/blocks?s=id(asc)`

Если нужно применить несколько сортировок, их можно перечислить через запятую аналогично фильтрам. Максимальное гарантированное количество сортировок - 2.

**Лимит** используется так: `?limit=N`, где N - натуральное число от 1 до 100. По умолчанию - 10. Для ряда случаев `LIMIT` игнорируется и возвращаются все результаты - в таком случае `context.limit` примет значение `NULL`.

**Оффсет** используется в качестве пагинатора, например `?offset=10` вернёт следующие 10 результатов. `context.offset` принимает значение установленного `OFFSET`. Максимальное значение - 10000. Если нужна последняя страница, то проще и быстрее изменить направление сотировки на противоположное. Важно: при итерировании результатов крайне вероятна ситуация, что пул результатов увеличится, т.к. были найдены новые блоки. Для избежания увеличения пула надо добавлять дополнительное условие, ограничивающее сверху id блока значением, полученным в `context.state` в первом запросе.

#### (bitcoin[-cash]|litecoin)/[mempool/]blocks

Возвращает информацию о блоках

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| id | int | Высота (height) блока | + | + |
| hash | string `[0-9a-f]{64}` | Хеш блока | + | + |
| date | string `YYYY-MM-DD` | Дата нахождения блока (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время нахождения блока (UTC) | + | + |
| median_time | string `YYYY-MM-DD HH:ii:ss` | Медианное время блока (UTC) | | + |
| size | int | Размер блока в байтах (может быть более 1 Мб для Биткоина, до 32 Мб для Кеша) | + | + |
| stripped_size(\*) | int | Размер блока в байтах без учёта witness-информации (до 1 Мб для Биткоина) | + | + |
| weight(\*) | int | Вес блока в weight units (до 4 MWU) | + | + |
| version | int | Поле версионности | + | + |
| version_hex | string `[0-9a-f]*` | Поле версионности в hex | | |
| version_bits | string `[01]{30}` | Поле версионности двоичным исчислением | | |
| merkle_root | `[0-9a-f]{64}` | Хеш merkle root | | |
| nonce | int | Значение nonce | + | + |
| bits | int | Поле bits | + | + |
| difficulty | float | Сложность | + | + |
| chainwork | string `[0-9a-f]{64}` | Поле chainwork | | |
| coinbase_data_hex | string `[0-9a-f]*` | Hex информации, содержащейся в coinbase-входе coinbase-транзакции блока | + | |
| transaction_count | int | Количество транзакций в блоке | + | + |
| witness_count(\*) | int | Количетство транзакций в блоке, которые содержат witness-информацию | + | + |
| input_count | int | Количество входов во всех транзакциях блока | + | + |
| output_count | int | Количество выходов во всех транзакциях блока | + | + |
| input_total | int | Сумма входов в сатоши | + | + |
| input_total_usd | float | Сумма входов в USD (здесь и далее для USD - на момент `date`) | + | + |
| output_total | int | Сумма выходов в сатоши | + | + |
| output_total_usd | float | Сумма выходов в USD | + | + |
| fee_total | int | Суммарная комиссия в сатоши | + | + |
| fee_total_usd | float | Суммарная комиссия в USD | + | + |
| fee_per_kb | float | Комиссия за килобайт (1000 байт) данных в сатоши | + | + |
| fee_per_kb_usd | float | Комиссия за килобайт данных в USD | + | + |
| fee_per_kwu(\*) | float | Комиссия за 1000 weight units данных в сатоши | + | + |
| fee_per_kwu_usd(\*) | float | Комиссия за 1000 weight units данных в USD | + | + |
| cdd_total | float | Количество уничтоженных монетодней всеми транзакциями блока | + | + |
| generation | int | Награда майнера за блок в сатоши | + | + |
| generation_usd | float | Награда майнера за блок в USD | + | + |
| reward | int | Суммарная награда майнера (награда + суммарная комиссия) в сатоши | + | + |
| reward_usd | float | Суммарная награда майнера (награда + суммарная комиссия) в USD | + | + |
| guessed_miner | string `.*` | Предположительное наименование майнера, нашедшего блок (эвристика по `coinbase_data_bin` и по адресам, на которые идёт награда) | + | + |

Дополнительные синтетические колонки (можно по ним искать и/или сортировать, но они не выводятся)

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| coinbase_data_bin | string `.*` | Текстовое представление coinbase data. Позволяет применять оператор `LIKE`: `?q=coinbase_data_bin(~hello)` | + | |

Примечания:
- для столбцов `id` и `hash` повышенная эффективность при выгрузке одной записи
- нет возможности поиска по столбцу `date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)`
- поиск по столбцу `coinbase_data_hex` ведётся оператором `^`, также можно искать `~` по `coinbase_data_bin` (однако, поля `coinbase_data_bin` всё равно не будет в выдаче)
- (\*) - только для Bitcoin
- сортировка по умолчанию - id DESC

#### (bitcoin[-cash]|litecoin)/[mempool/]transactions

Возвращает информацию о транзакциях

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| block_id | int | Высота (id) блока, содержащего транзакцию | + | + |
| id | int | Внутренний id транзакции (не имеет отношения к блокчейну, используется для внутренних целей) | + | + |
| hash | string `[0-9a-f]{64}` | Хеш транзакции | + | |
| date | string `YYYY-MM-DD` | Дата блока, в котором содержится транзакция (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время блока, в котором содержится транзакция (UTC) | + | + |
| size | int | Размер транзации в байтах | + | + |
| weight(\*) | int | Вес транзакции в weight units | + | + |
| version | int | Поле версионности | + | + |
| lock_time | int | Lock time - может быть или высотой блока, или unix timestamp | + | + |
| is_coinbase | boolean | Является ли coinbase-транзакцией (генерирование новых монет, у такой транзакции `input_count` равен `1` и означает синтетический coinbase-вход)? | + | |
| has_witness(\*) | boolean | Есть ли в транзакции witness-часть (использование SegWit)? | + | |
| input_count | int | Количество входов | + | + |
| output_count | int | Количество выходов | + | + |
| input_total | int | Сумма входов в сатоши | + | + |
| input_total_usd | float | Сумма входов в USD | + | + |
| output_total | int | Сумма выходов в сатоши | + | + |
| output_total_usd | float | Сумма выходов в USD | + | + |
| fee | int | Комиссия в сатоши | + | + |
| fee_usd | float | Комиссия в USD | + | + |
| fee_per_kb | float | Комиссия за килобайт (1000 байт) данных в сатоши | + | + |
| fee_per_kb_usd | float | Комиссия за килобайт данных в USD | + | + |
| fee_per_kwu(\*) | float | Комиссия за 1000 weight units данных в сатоши | + | + |
| fee_per_kwu_usd(\*) | float | Комиссия за 1000 weight units данных в USD | + | + |
| cdd_total | float | Количество уничтоженных монетодней | + | + |

Примечания:
- для столбцов `id` и `hash` повышенная эффективность при выгрузке одной записи
- нет возможности поиска по столбцу `date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)`
- (\*) - только для Bitcoin
- сортировка по умолчанию - id DESC

#### (bitcoin[-cash]|litecoin)/[mempool/]outputs

Возвращает информацию о выходах (которые становятся входами, когда тратятся, и тогда о них появляется `spending*`-информация)

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| block_id | int | Высота (id) блока, содержащего транзакцию, содержащую выход | + | + |
| transaction_id | int | Внутренний id транзакции, содержащей выход | + | + |
| index | int | Порядковый индекс выхода в транзакции (с 0) | + | + |
| transaction_hash | string `[0-9a-f]{64}` | Хеш транзакции, содержащей выход | | |
| date | string `YYYY-MM-DD` | Дата блока, в котором содержится выход (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время блока, в котором содержится выход (UTC) | + | + |
| value | int | Монетарное значение выхода в сатоши | + | + |
| value_usd | float | Монетарное значение выхода в USD на момент `date` | + | + |
| recipient | string `[0-9a-zA-Z\-]*` | Биткоин-адрес или синтетический адрес получатель выхода | + | + |
| type | string (enum) | Тип выхода - одно из следующих значений: `pubkey/pubkeyhash/scripthash/multisig/nulldata/nonstandard/witness_v0_scripthash/witness_v0_keyhash` | + | + |
| script_hex | string `[0-9a-f]*` | Hex скрипта выхода | + | |
| is_from_coinbase | boolean | Является ли выходом coinbase-транзакции? | + | |
| is_spendable | null or boolean | Можно ли теоретически потратить этот выход? Для `pubkey` и `multisig` выходов проводится проверка возможности существования соответствующего закрытого ключа, и выводится `true`/`false` в зависимости от результата проверки. Для `nulldata`-выходов всегда `false`. Для остальных типов тривиально проверить нельзя, в этом случае вовзращается `null` | + | |
| is_spent | boolean | Потрачен ли выход? (Для полей далее - если потрачен, то есть значения, если нет, то везде `null` | + | |
| spending_block_id | null or int | Высота (id) блока, в котором содержится транзакция, тратящая выход (там она является входом). `null` если выход пока не потрачен. | + | + |
| spending_transaction_id | null or int | Внутренний id транзакции, в которой тратится выход | + | + |
| spending_index | null or int | Порядковый индекс входа в тратящей транзакции (с 0) | + | + |
| spending_transaction_hash | null or string `[0-9a-f]{64}` | Хеш тратящей транзакции | | |
| spending_date | null or string `YYYY-MM-DD` | Дата блока, в котором тратится выход | | |
| spending_time | null or string `YYYY-MM-DD HH:ii:ss` | Время блока, в котором тратится выход | + | + |
| spending_value_usd | null or float | Монетарное значение выхода в USD на момент `spending_date` | + | + |
| spending_sequence | null or int | Техническое значение sequence входа | + | + |
| spending_signature_hex | null or string `[0-9a-f]*` | Hex скрита входа (подписи) | | |
| spending_witness(\*) | null or string (JSONB) | Witness-информация в виде экранированного JSONB | | |
| lifespan | null or int | Количество секунд с момента создания выхода (`time`) до его траты (`spending_time`), `null` если выход не потрачен | + | + |
| cdd | null or float | Количество уничтоженных монетодней при трате выхода | + | + |

Дополнительные синтетические колонки (можно по ним искать и/или сортировать, но они не выводятся)

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| script_bin | string `.*` | Текстовое представление script_hex. Позволяет применять оператор `LIKE`: `?q=script_bin(~hello)` | + | |

Примечания:
- для столбцов `transaction_id` и `spending_transaction_id` повышенная эффективность при выгрузке записей если указана одна конкретная транзакция, а не диапазон
- нет возможности поиска по столбцам `date` и `spending_date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)` и `?q=spending_time(YYYY-MM-DD)` соответственно
- поиск по столбцу `script_hex` ведётся оператором `^`, также можно искать `~` по `script_bin` (однако, поля `script_bin` всё равно не будет в выдаче)
- (\*) - только для Bitcoin
- сортировка по умолчанию - transaction_id DESC

#### ethereum/[mempool/]blocks

Возвращает информацию о блоках

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| id | int | Высота (height) блока | + | + |
| hash | string `0x[0-9a-f]{64}` | Хеш блока (с 0x) | + | |
| date | string `YYYY-MM-DD` | Дата нахождения блока (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время нахождения блока (UTC) | + | + |
| size | int |  Размер блока в байтах | + | + |
| miner | string `0x[0-9a-f]{40}` | Адрес майнера, получившего награду (с 0x) | + | |
| extra_data_hex | string `[0-9a-f]*` | Произвольные данные, включённые майнером в блок | + | |
| difficulty | int | Сложность | + | + |
| gas_used | int | Количество газа, использованного транзакциями блока | + | + |
| gas_limit | int | Лимит газа на блок, установленный майнером | + | + |
| logs_bloom | string `[0-9a-f]*` | Logs Bloom | | |
| mix_hash | string `[0-9a-f]{64}` | Хеш Mix Hash | | |
| nonce | string `[0-9a-f]*` | Значение nonce | | |
| receipts_root | string `[0-9a-f]{64}` | Хеш Receipts Root | | |
| sha3_uncles | string `[0-9a-f]{64}` | Хеш SHA3 Uncles  | | |
| state_root | string `[0-9a-f]{64}` | Хеш State Root | | |
| total_difficulty | numeric string | Суммарная сложность | | |
| transactions_root | string `[0-9a-f]{64}` | Хеш Transactions Root | | |
| uncle_count | int | Количество анклов блока | + | + |
| transaction_count | int | Количество транзакций в блоке | + | + |
| synthetic_transaction_count | int | Количество синтетических транзакций (они не существуют как отдельные транзакции в блокчейне, но меняют state, например - это транзации genesis-блока, награды майнерам, транзакции DAO-форка | + | + |
| call_count(\*) | int | Общее количество коллов, которые совершают транзакции в блоке | + | + |
| synthetic_call_count | int | Количество синтетических коллов (аналогично транзакциям) | + | + |
| value_total | numeric string | Монетарная сумма всех транзакций блока в wei, здесь и далее `numeric string` - передаётся как строка, т.к. wei-значения не влезают в uint64 | + | + |
| value_total_usd | float | Монетарная сумма всех транзакций блока в USD | + | + |
| internal_value_total(\*) | numeric string | Монетарная сумма всех внутренных коллов в блоке (см. примечание ниже) в wei | + | + |
| internal_value_total_usd(\*) | float | Монетарная сумма всех внутренных коллов в блоке в USD | + | + |
| generation | numeric string | Награда майнера за найденный блок в wei (3 или 5 эфиров + процент от анклов) | + | + |
| generation_usd | float | Награда майнера за найденный блок в USD | + | + |
| uncle_generation(\*) | numeric string | Суммарная награда uncle-майнеров в wei | + | + |
| uncle_generation_usd(\*) | float | Суммарная награда uncle-майнеров в USD | + | + |
| fee_total(\*) | numeric string | Суммарная комиссия в wei | + | + |
| fee_total_usd(\*) | float | Суммарная комиссия в USD | + | + |
| reward(\*) | numeric string | Суммарная награда майнера в wei (награда за нахождение + комиссии) | + | + |
| reward_usd(\*) | float | Суммарная награда майнера в USD | + | + |

Дополнительные синтетические колонки (можно по ним искать и/или сортировать, но они не выводятся)

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| extra_data_bin | string `.*` | Текстовое представление extra data. Позволяет применять оператор `LIKE`: `?q=extra_data_bin(~hello)` | + | |

Примечания:

- (\*) - всегда равно `null` для `mempool/blocks`
- для столбцов `id` и `hash` повышенная эффективность при выгрузке одной записи
- нет возможности поиска по столбцу `date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)`
- поиск по полям, в которых содержатся значения в wei (`value_total`, `internal_value_total`, `generation`, `uncle_generation`, `fee_total`, `reward`) может быть с погрешностями
- поиск по столбцу `extra_data_hex` ведётся оператором `^`, также можно искать `~` по `extra_data_bin` (однако, поля `extra_data_bin` всё равно не будет в выдаче)
- разница между `value_total` и `internal_value_total` такова: к примеру, транзакция сама по себе передаёт 0 эфиров, но эта транзакция - вызов контракта, который кому-то передаёт, допустим, 10 эфиров. Тогда `value` будет 0 эфиров, а `internal_value` - 10 эфиров
- сортировка по умолчанию - id DESC

#### ethereum/uncles

Возвращает информацию об анклах

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| parent_block_id | int | Высота (height) родительского блока | + | + |
| index | int | Порядковый номер анкла в блоке | + | + |
| hash | string `0x[0-9a-f]{64}` | Хеш анкла (с 0x) | + | |
| date | string `YYYY-MM-DD` | Дата нахождения (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время нахождения (UTC) | + | + |
| size | int |  Размер анкла в байтах | + | + |
| miner | string `0x[0-9a-f]{40}` | Адрес майнера, получившего награду (с 0x) | + | |
| extra_data_hex | string `[0-9a-f]*` | Произвольные данные, включённые майнером анкла | + | |
| difficulty | int | Сложность | + | + |
| gas_used | int | Количество газа, использованного транзакциями | + | + |
| gas_limit | int | Лимит газа на блок, установленный майнером | + | + |
| logs_bloom | string `[0-9a-f]*` | Logs Bloom | | |
| mix_hash | string `[0-9a-f]{64}` | Хеш Mix Hash | | |
| nonce | string `[0-9a-f]*` | Значение nonce | | |
| receipts_root | string `[0-9a-f]{64}` | Хеш Receipts Root | | |
| sha3_uncles | string `[0-9a-f]{64}` | Хеш SHA3 Uncles  | | |
| state_root | string `[0-9a-f]{64}` | Хеш State Root | | |
| total_difficulty | numeric string | Суммарная сложность | | |
| transactions_root | string `[0-9a-f]{64}` | Хеш Transactions Root | | |
| generation | numeric string | Награда майнера нашедшего анкл в wei | + | + |
| generation_usd | float | Награда майнера в USD | + | + |

Дополнительные синтетические колонки (можно по ним искать и/или сортировать, но они не выводятся)

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| extra_data_bin | string `.*` | Текстовое представление extra data. Позволяет применять оператор `LIKE`: `?q=extra_data_bin(~hello)` | + | |

Примечания:

- для столбцов `parent_block_id` и `hash` повышенная эффективность при выгрузке одной или нескольких записей
- нет возможности поиска по столбцу `date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)`
- поиск по полям, в которых содержатся значения в wei (`generation`) может быть с погрешностями
- поиск по столбцу `extra_data_hex` ведётся оператором `^`, также можно искать `~` по `extra_data_bin` (однако, поля `extra_data_bin` всё равно не будет в выдаче)
- сортировка по умолчанию - parent_block_id DESC

#### ethereum/[mempool/]transactions

Возвращает информацию о транзакциях

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| block_id | int | Высота (id) блока, содержащего транзакцию | + | + |
| id | int | Внутренний id транзакции (не имеет отношения к блокчейну, используется для внутренних целей) | + | + |
| index(\*)(\*\*) | int | Порядковый номер транзакции в блоке | + | + |
| hash(\*\*) | string `0x[0-9a-f]{64}` | Хеш транзакции (с 0x) | + | |
| date | string `YYYY-MM-DD` | Дата блока, в котором содержится транзакция (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время блока, в котором содержится транзакция (UTC) | + | + |
| size | int | Размер транзакции в байтах | + | + |
| failed(\*) | bool | Не зафейлилась ли транзакция | + | |
| type(\*) | string (enum) | Тип транзакции - одно из следующих значений: `call/create/call_tree/create_tree/synthetic_coinbase`. Описание в примечании. | + | |
| sender(\*\*) | string `0x[0-9a-f]{40}` | Адрес отправителя транзакции (с 0x) | + | |
| recipient | string `0x[0-9a-f]{40}` | Адрес получателя транзакции (с 0x) | + | |
| call_count(\*) | int | Количество коллов, которые совершает транзакция | + | + |
| value | numeric string | Cумма транзакции в wei, здесь и далее `numeric string` - передаётся как строка, т.к. wei-значения не влезают в uint64 | + | + |
| value_usd | float | Сумма транзакции в USD | + | + |
| internal_value(\*) | numeric string | Сумма всех внутренных коллов в блоке в wei | + | + |
| internal_value_usd(\*) | float | Сумма всех внутренных коллов в блоке в USD | + | + |
| fee(\*)(\*\*) | numeric string | Комиссия в wei | + | + |
| fee_usd(\*)(\*\*) | float | Комиссия в USD | + | + |
| gas_used(\*)(\*\*) | int | Количество газа, использованного транзакцией | + | + |
| gas_limit(\*\*) | int | Лимит газа на транзакцию, установленный отправителем транзакции | + | + |
| gas_price(\*\*) | int | Цена за газ, установленная отправителем транзакции | + | + |
| input_hex(\*\*) | string `[0-9a-f]*` | Входные данные транзакции | + | |
| nonce(\*\*) | string `[0-9a-f]*` | Значение nonce | | |
| v(\*\*) | string `[0-9a-f]*` | Значение v | | |
| r(\*\*) | string `[0-9a-f]*` | Значение r | | |
| s(\*\*) | string `[0-9a-f]*` | Значение s | | |

Дополнительные синтетические колонки (можно по ним искать и/или сортировать, но они не выводятся)

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| input_bin | string `.*` | Текстовое представление input data. Позволяет применять оператор `LIKE`: `?q=input_bin(~hello)` | + | |

Примечания:

- (\*) - всегда равно `null` для `mempool/transactions`
- (\*\*) - всегда равно `null` если `type` = `synthetic_coinbase`
- для столбцов `id` и `hash` повышенная эффективность при выгрузке одной записи
- нет возможности поиска по столбцу `date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)`
- поиск по полям, в которых содержатся значения в wei (`value`, `internal_value`) может быть с погрешностями
- поиск по столбцу `input_hex` ведётся оператором `^`, также можно искать `~` по `input_bin` (однако, поля `input_bin` всё равно не будет в выдаче)
- разница между `value` и `internal_value` такова: например, транзакция сама по себе передаёт 0 эфиров, но эта транзакция - вызов контракта, который кому-то передаёт, допустим, 10 эфиров. Тогда `value` будет 0 эфиров, а `internal_value` - 10 эфиров
- сортировка по умолчанию - id DESC
- возможные типы (`type`) транзакций:
    * call - транзакция передаёт value, при этом коллов больше нет (простая передача эфиров не в пользу контракта или вызов контракта, который ничего не делает)
    * create - создание нового контракта
    * call_tree - транзакция вызывает контракт, который производит какие-либо ещё коллы
    * create_tree - создание нового контракта, который начинает производить какие-либо коллы или создавать контракты сам
    * synthetic_coinbase - синтетическая транзакция начисления награды майнеру (блока или анкла)

#### ethereum/calls

Возвращает информацию о коллах

| Колонка |  Тип  |  Описание  | Q? | S? |
|---------|-------|------------|----|----|
| block_id | int | Высота (id) блока, содержащий колл | + | + |
| transaction_id | int | Внутренний id транзакции, содержащий колл | + | + |
| transaction_hash(\*\*) | string `0x[0-9a-f]{64}` | Хеш транзакции (с 0x), содержащий колл | | |
| index | string | Индекс колла внутри транзакции (древовидный, например, "0.8.1" | + | + |
| depth | int | Грубина колла внутри дерева коллов (начиная с 0) | + | + |
| date | string `YYYY-MM-DD` | Дата блока, в котором содержится колл (UTC) | | |
| time | string `YYYY-MM-DD HH:ii:ss` | Время блока, в котором содержится колл (UTC) | + | + |
| failed | bool | Не зафейлился ли колл | + | |
| fail_reason | string `.*` или null  | Если зафейлился, то строка с причиной, если нет, то `null` | + | |
| type | string (enum) | Тип колла - одно из следующих значений: `call/delegatecall/staticcall/callcode/selfdesctruct/create/synthetic_coinbase` | + | |
| sender(\*\*) | string `0x[0-9a-f]{40}` | Адрес вызывающего колл (с 0x) | + | |
| recipient | string `0x[0-9a-f]{40}` | Адрес вызываемого коллом (с 0x) | + | |
| child_call_count | int | Количество дочерних коллов | + | + |
| value | numeric string | Cумма колла в wei, здесь и далее `numeric string` - передаётся как строка, т.к. wei-значения не влезают в uint64 | + | + |
| value_usd | float | Сумма колла в USD | + | + |
| transferred | bool | Переданы ли эфиры этим коллом (`false` если `failed`, или если тип транзакции, который не изменяет state, например, `staticcall` | + | |
| input_hex(\*\*) | string `[0-9a-f]*` | Входные данные колла | | |
| output_hex(\*\*) | string `[0-9a-f]*` | Выходные данные колла | | |

Примечания:

- (\*\*) - всегда равно `null` если `type` = `synthetic_coinbase`
- для столбца `transaction_id` повышенная эффективность при выгрузке одной записи
- нет возможности поиска по столбцу `date` напрямую, есть возможность поиска `?q=time(YYYY-MM-DD)`
- поиск по полям, в которых содержатся значения в wei (`value`, `internal_value`) может быть с погрешностями
- сортировка по умолчанию - transaction_id DESC
- сортировка по `index` - по алфавиту (т.е. "0.2" идет после "0.11"), в некоторых случаях используется переключение на натуральную сортировка (например, когда есть фильтр по `transaction_id`)

### Примечания

- для неподтверждённых транзакций (и выходов в случае bitcoin[-cash]) характерны следующие признаки:
    - их `block_id` равен `-1`
    - `date` и `time` указывают на время получения транзакции нашей нодой
- при использовании `offset` разумно добавлять в фильтры максимальный номер блока (`?q=block_id(..N)`), т.к. очень вероятно, что за время итерирования добавятся новые записи в таблицу. Для удобства из первого результа любого запроса можно взять значение `context.state`, содержащее номер последнего блока на момент выполнения запроса и использовать далее именно его.

### Dashboard-вызовы

API поддерживает ряд коллов, которые выдают какую-либо агрегированную информацию, или просто информацию в более удобном виде по определённым сущностям.

#### (bitcoin[-cash]|litecoin|ethereum)/dashboards/block/{A} и (bitcoin[-cash]|ethereum)/dashboards/blocks/{A[,B,...]}

На входе принимает высоту или хеш блока (блоков). `data` вовзращает массив, ключами которого являются высоты блоков или хеши блоков, а значениями массив из элементов:
* `block` - информация о блоке в infinitable-формате `(bitcoin[-cash]|ethereum)/blocks`
* `transactions` - массив всех хешей транзакций, включённых в блок
* только для Ethereum - `synthetic_transactions` - массив внутренних id синтетических транзакций блока (у них нет хеша) (`null` вместо массива пока блок не получит 6 подтверждений)
* только для Ethereum - `uncles` - массив хешей анклов блока (`null` вместо массива пока блок не получит 6 подтверждений, в случае если нет анклов и больше 6 подтверждений - пустой массив (`[]`))

`context.results` содержит количество найденных блоков.

#### ethereum/dashboards/uncle/{A} и ethereum/dashboards/uncles/{A[,B,...]}

На входе принимает хеш анкла (анклов). `data` вовзращает массив, ключами которого являются хеши анклов, а значениями массив из элементов:
* `uncle` - информация о блоке в infinitable-формате `ethereum/uncles`

`context.results` содержит количество найденных анклов.

#### (bitcoin[-cash]|litecoin|ethereum)/dashboards/transaction/{A} и (bitcoin[-cash]|litecoin|ethereum)/dashboards/transactions/{A[,B,...]}

На входе принимает внутренний blockchair-id или хеш транзакции (транзакций). `data` вовзращает массив, ключами которого являются идентификаторы или хеши транзакций, а значениями массив из элементов:
* `transaction` - информация о транзакции в infinitable-формате `bitcoin[-cash]/transactions`
* (только bitcoin[-cash]) `inputs` - массив всех входов транзакции, отсортированный по `spending_index` в infinitable-формате `bitcoin[-cash]/outputs`
* (только bitcoin[-cash]) `outputs` - массив всех выходов транзакции, отсортированный по `index` в infinitable-формате `bitcoin[-cash]/outputs`
* (только ethereum) `calls` - массив всех коллов, совершённых в ходе выполнения транзакции, всегда `null` для транзакций из мемпула и последних 6 блоков

`context.results` содержит количество найденных транзакций.

#### (bitcoin[-cash]|litecoin|ethereum)/dashboards/transaction/{hash}/priority

Для транзакций в мемпуле показывает приоритет (`position`) (для Bitcoin - по `fee_per_kwu`, для Bitcoin Cash - по `fee_per_kb`, для Ethereum - по `gas_price`) перед остальным транзакциями (всего `out_of` транзакций в мемпуле). В остальном имеет такую же структуру, как и колл `(bitcoin[-cash]|ethereum)/dashboards/transaction/{A}`

#### (bitcoin[-cash]|litecoin)/dashboards/address/{A}

На входе принимает адрес. `data` вовзращает массив из одного элемента (если адрес найден), ключом которого является сам адрес, а значениями массив из элементов:
* `address`
    * `address.type` - тип адреса (тип выхода - совпадает с `bitcoin[-cash].outputs.type`)
    * `address.script_hex` - скрипт адреса
    * `address.balance` - баланс адреса в сатоши (здесь и далее - учитывая unconfirmed-выходы) - int
    * `address.balance_usd` - баланс адреса в USD - float
    * `address.received` - сколько всего получил адрес в сатоши
    * `address.received_usd` - сколько всего получил адрес в USD
    * `address.spent` - сколько всего потратил адрес в сатоши
    * `address.spent_usd` - сколько всего потратил адрес в USD
    * `address.output_count` - количество выходов в пользу этого адреса
    * `address.unspent_output_count` - количество непотраченных выходов для этого адреса (т.е. количество входов с этим адресом можно посчитать как `output_count`-`unspent_output_count`)
    * `address.first_seen_receiving` - timestamp (UTC) когда первый раз этот адрес получал коины
    * `address.last_seen_receiving` - timestamp (UTC) когда последний раз этот адрес получал коины
    * `address.first_seen_spending` - timestamp (UTC) когда первый раз этот адрес отправлял коины
    * `address.last_seen_spending` - timestamp (UTC) когда последний раз этот адрес отправлял коины
    * `address.transaction_count` - количество уникальных транзакций, в которых участвовал адрес
* `transactions` - массив последних 100 хешей транзакций, в которых участвовал адрес

`context.results` содержит количество найденных адресов (0 или 1, пока не реализован колл `addresses`).

Для итерирования `transactions` поддерживается `?offset=N`.

#### ethereum/dashboards/address/{A}

На входе принимает адрес. `data` вовзращает массив из одного элемента (если адрес найден), ключом которого является сам адрес, а значениями массив из элементов:
* `address`
    * `address.type` - тип адреса (`account` - для обычного адреса, `contract` - для контракта)
    * `address.contract_code_hex` - если контракт, то hex кода контракта при создании, для обычного адреса - null
    * `address.contract_created` - если контракт, если контракт был действительно создан - true, если нет (т.е. `create`-колл зафейленный) - false, для обычного адреса - null
    * `address.contract_destroyed` - если контракт, если контакт был уничтожен (SELFDESCTRUCT), то true, если действующий - false, для обычного адреса - null
    * `address.balance` - точный баланс адреса в wei (здесь и далее для значений в wei - numeric string)
    * `address.balance_usd` - баланс адреса в USD - float
    * `address.received_approximate` - сколько всего получил адрес в wei (приблизительно) (\*)
    * `address.received_usd` - сколько всего получил адрес в USD (приблизительно) (\*)
    * `address.spent_approximate` - сколько всего потратил адрес в wei (приблизительно) (\*)
    * `address.spent_usd` - сколько всего потратил адрес в USD (приблизительно) (\*)
    * `address.fees_approximate` - сколько всего адрес потратил на комиссиях в wei (приблизительно) (\*)
    * `address.fees_usd` - сколько всего адрес потратил на комиссиях в USD (приблизительно) (\*)
    * `address.receiving_call_count` - количество коллов в пользу этого адреса, в которых произошла передача value (\*\*)
    * `address.spending_call_count` - количество коллов, которые сделал этот адрес, в которых произошла передача value (\*\*)
    * `address.call_count` - суммарное количество всех коллов, в которых участвовал этот адрес (может быть больше `receiving_call_count` + `spending_call_count`, т.к. учитывает и зафейленные коллы)
    * `address.transaction_count` - количество транзакций, в которых участвовал адрес
    * `address.first_seen_receiving` - timestamp (UTC) когда первый раз этот адрес получал успешный входящий колл
    * `address.last_seen_receiving` - timestamp (UTC) когда последний раз этот адрес получал успешный входящий колл
    * `address.first_seen_spending` - timestamp (UTC) когда первый раз этот адрес отправлял успешный колл
    * `address.last_seen_spending` - timestamp (UTC) когда последний раз этот адрес отправлял успешный колл
* `calls` - массив последних 100 коллов с участием адреса, каждый элемент - массив, содержащий следующие колонки из `ethereum/calls`: `block_id`, `transaction_hash`, `index`, `time`, `sender`, `recipient`, `value` , `value_usd` , `transferred`

`context.results` содержит количество найденных адресов (0 или 1, пока не реализован колл `addresses`).

Для итерирования `calls` поддерживается `?offset=N`.

Примечания:
- (\*) - в этих колонках значение в wei может быть округлено. Для миллионов коллов погрешность может составлять более 1 эфира
- (\*\*) - учитываются коллы, для которых ethereum/calls.transferred = true (см. докуменацию `ethereum/calls`), т.е. здесь не учитываются коллы, которые не меняют state (staticcall и т.д.), а также коллы, которые зафейлились

#### (bitcoin[-cash]|litecoin|ethereum)/stats

Возвращает статистику по блокчейну массивом:
* `blocks` - общее количество блоков
* (только ethereum) `uncles` - общее количество анклов
* `transactions` - общее количество транзакций
* (только ethereum) `calls` - общее количество внутренних коллов
* `blocks_24h` - блоков за последние 24 часа
* `circulation` для bitcoin[-cash]|litecoin, `circulation_approximate` для ethereum - количество монет в обращении (в сатоши или в wei соответственно, для ethereum - приблизительное значение)
* `transactions_24h` - транзакций за последние 24 часа
* `difficulty` - текущая сложность
* `volume_24h` для bitcoin[-cash]|litecoin, `volume_24h_approximate` для ethereum - монетарный объём транзакций за последние 24 часа (для ethereum - приблизительное значение)
* `mempool_transactions` - количество транзакций в мемпуле
* (только ethereum) `mempool_median_gas_price` - медианная цена газа в мемпуле
* (только bitcoin[-cash]|litecoin) `mempool_size` - размер мемпула в байтах
* `mempool_tps` - количество транзакций в секунду, поступающих в мемпул
* (только ethereum) `mempool_total_value_approximate` - монетарный объём мемпула
* (только bitcoin[-cash]|litecoin) `mempool_total_fee_usd` - суммарная комиссия в USD в мемпуле
* `best_block_height` - высота последнего блока
* `best_block_hash` - хеш последнего блока
* `best_block_time` - время последнего блока
* (только ethereum) `uncles_24h` - количество анклов за последние 24 часа
* (только bitcoin[-cash]|litecoin) `nodes` - количество полных нод в сети
* `hashrate_24h` - хешрейт (хешей в секунду) майнеров в среднем за последние 24 часа
* `market_price_usd` - среднерыночная цена 1 монеты в USD (провайдер информации о рынках: CoinGecko)
* `market_price_btc` - среднерыночная цена 1 монеты в биткоинах (для Биткоина всегда 1)
* `market_price_usd_change_24h_percentage` - изменение рыночной цены в процентах за 24 часа
* `market_cap_usd` - капитализация рынка (монет в обращении * цена за монету в USD)
* `market_dominance_percentage` - индекс доминации (сколько % от всего рынка криптовалют занимает рыночная капитализация монеты)
... и некоторые другие ключи с очевидными названиями

#### stats

Возвращает инфорамацию сразу по четырём коллам:
* `bitcoin/stats`
* `bitcoin-cash/stats`
* `ethereum/stats`
* `litecoin/stats`

#### (bitcoin[-cash]|litecoin)/nodes
Возвращает информацию о доступных нодах.
* `nodes` - ноды
  * `version` - User Agent клиента
  * `country` - страна (определяется по GeoIP)
  * `height` - последний блок в цепочке ноды
  * `flags` - флаги [сервисов](https://en.bitcoin.it/wiki/Protocol_documentation#version)
* `count` - количество
* `countries` - количество нод по странам
* `versions` - количество нод по User Agent

### Пример работы с API

Допустим, нам требуется получать все последние транзакции из блокчейна Эфириума на сумму более 1 млн. долларов. Для этого необходимо составить следующий запрос:
* `https://api.blockchair.com/ethereum/transactions?q=internal_value_usd(10000000..)&s=id(desc)`

В этом запросе мы обращаемся к блокчейну (`ethereum`), таблице (`transactions`), устанасливаем условие суммы (`q=internal_value_usd(10000000..)`), и сортировку по убыванию (`&s=id(desc)`).

Допустим, скрипт, который обращался к API с этим запросом по какой-то причине не работал некоторое время. Или в блокчейне появилось очень много транзакций на сумму более 1 млн. долларов, и со стандартным лимитом в 10 результатов, скрипт пропустил какие-то транзакции. Тогда сначала мы делаем тот же запрос:
* `https://api.blockchair.com/ethereum/transactions?q=internal_value_usd(10000000..)&s=id(desc)`

Из его результата запоминаем `context.state`, кладём его в некую переменную `_S_`, и далее для получения следующих результатов применяем `offset`:
* `https://api.blockchair.com/ethereum/transactions?q=internal_value_usd(10000000..),block_id(.._S_)&s=id(desc)&offset=10`

Увеличиваем значение offset пока не получим выборку с транзакцией, о которой мы уже знали.

### Рассылка транзакций

Для рассылки транзакции по сети, нужно выполнить POST-запрос к `https://api.blockchair.com/{chain}/push/transaction` (где `{chain}` может быть: `bitcoin`, `bitcoin-cash`, `ethereum`, или `litecoin`) с `data`, содержащим транзакцию в сыром шестнадцатеричном виде (в Ethereum начинается с `0x`). Пример:

```
curl -v --data "data=01000000010000000000000000000000000000000000000000000000000000000000000000ffffffff0704ffff001d0104ffffffff0100f2052a0100000043410496b538e853519c726a2c91e61ec11600ae1390813a627c66fb8be7947be63c52da7589379515d4e0a604f8141781e62294721166bf621e73a82cbf2342c858eeac00000000" https://api.blockchair.com/bitcoin/push/transaction
```

Если транзакция была успешно разослана по сети, API вернёт JSON-ответ (код 200), содержищий массив `data` с ключом `transaction_hash`, содержащим хеш самой транзакции. В случае ошибки (неправильный формат транзакции, трата уже потраченных выходов, и т.д.) API вернёт код 400.

Пример успешного ответа:

```
{"data":{"transaction_hash": "0e3e2357e806b6cdb1f70b54c3a3a17b6714ee1f0e68bebb44a74b1efd512098…"},"context":{"code":200,…
```

### Получение транзакций в сыром виде

Можно получить транзакцию в сыром виде напрямую от наших нод. Для этого требуется выполнить следующий API-запрос: `https://api.blockchair.com/{chain}/raw/transaction/{txhash}` (где `{chain}` может быть: `bitcoin`, `bitcoin-cash`, `ethereum`, или `litecoin`)

Ответ содержит два ключа:
* `raw_transaction` — транзакция в сыром виде в шестнадцатеричной с.и.;
* `decoded_raw_transaction` (недоступно для Ethereum) — транзакция в сыром виде в JSON. Пожалуйста, имейте в виду, что структура JSON-массива может измениться с обновлением наших нод, и это не будет отмечено в журнале изменений.

### Поддержка

* E-mail: [info@blockchair.com](mailto:info@blockchair.com)
* Telegram-чат: [@Blockchair](https://telegram.me/Blockchair)
* Twitter: [@Blockchair](https://twitter.com/Blockchair)
