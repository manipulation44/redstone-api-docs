---
description: >-
  Redstone query is an extra interface exported by the Redstone API. It uses a
  human readable syntax to facilitate query creation.
---

# Redstone query

## Fluent interface for redstone

Redstone API supports a fluent interface to simplify the syntax.

### Importing

To use the fluent interface you should first import the redstone-api in a standard way

```javascript
// Using Node.js `require()`
const redstone = require('redstone-api');

// Using ES6 imports
import redstone from 'redstone-api';
```

### Usage

All redstone queries consist of 4 parts:

* Query initialisation \(`redstone.query()`\)
* What to fetch \(`symbol,` `symbols` or `allSymbols`\)  
* For which date/dates \(`latest`, `atDate`, `forLastHours`, `hoursAgo`, `fromDate`, `toDate`\)
* Query execution \(`exec`\)

### Examples

#### Get the latest price for a single token

```javascript
const price = await redstone.query()
  .symbol("AR")
  .latest()
  .exec();

console.log(price.value); // latest price value for AR token (in USD)
console.log(price.timestamp); // the exact timestamp of the price
```

#### Get the historical price for a single token

```javascript
const price = await redstone.query()
  .symbol("AR")
  .atDate("2021-04-19")
  .exec();
```

💡 Note: The argument passed to `atDate` must be convertable to date. You may pass date \(e.g. `new Date(2021-04-01)`\), timestamp \(e.g. `1617709771289`\), or just string \(e.g. `2021-04-01` or `2021-04-01T12:30:58`\)

#### Get the historical price for the last X hours

```javascript
// Returns an array of prices with ~10 minutes interval
const prices = await redstone.query()
  .symbol("AR")
  .forLastHours(12)
  .exec();
```

#### Get the historical price for X hours ago

```javascript
const price = await redstone.query()
  .symbol("AR")
  .hoursAgo(24)
  .exec();
```

#### Get the historical price for the last X days

```javascript
// Returns an array of prices with ~1h minutes interval
const prices = await redstone.query()
  .symbol("AR")
  .forLastDays(7)
  .exec();
```

#### Get the historical prices in a time range

```javascript
// Returns an array of prices
// Interval depends on the time range
// For ranges more than 24 hours interval is 1h
const prices = await redstone.query()
  .symbol("AR")
  .fromDate("2021-04-19")
  .toDate("2021-04-20")
  .exec();
```

#### Get the latest prices for several tokens

```javascript
const prices = await redstone.query()
  .symbols(["AR", "BTC", "ETH"])
  .latest()
  .exec();

console.log(prices); // Example output below
/*
{
  "BTC": {
    value: 58953.39,
    timestamp: 1617152802779,
    ...
  },
  "ETH": {
    value: 1856.75,
    timestamp: 1617152802779,
    ...
  },
  ...
}
*/
```

#### Get the historical prices for several tokens

```javascript
const prices: any = await redstone.query()
  .symbols(["AR", "ETH", "BTC"])
  .atDate("2021-04-19")
  .exec();
```

#### Get prices for all available tokens

```javascript
const prices = await redstone.query()
  .allSymbols()
  .latest()
  .exec();

console.log(prices); // Example output below
/*
{
  "BTC": {...},
  "ETH": {...},
  ...
}
*/

console.log(prices["AR"].value); // latest price value for AR
console.log(prices["EUR"].value); // latest price value for EUR
```

