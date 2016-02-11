# RethinkPlus

This is a simple wrapper (~70 lines of code) over the standard RethinkDB driver.  It mostly just adds connection pooling.

## Installation

    $ npm install --save rethink-plus


## Usage

First, import the library and instantiate the class.

```js
import RethinkPlus from 'rethink-plus';

let db = new RethinkPlus({
  // standard RethinkDB connection options, e.g.
  database: 'test'
});
```

This will create a connection pool for you.  The instance has all the same methods as `r` would in the normal driver docs.  The only difference is that the `run` method doesn't take a connection anymore: it takes one from the pool.

For example, to get a document with a specific ID:

```js
db
  .table('test')
  .get('07290fa5-1691-430c-a397-111575370881')
  .run(/*..optional options..*/)
  .then(/*...*/);
```

If you don't have any options to pass, you can just skip the call to `run` entirely, and it'll be invoked on `then`.  If you're using `async`/`await` like it's 2016, this leads to quite a natural-feeling usage:

```js
let table = db.table('test');
let doc = await table.get('07290fa5-1691-430c-a397-111575370881');
```

If, for some weird reason, you wanted to use the callback interface like it was 2014, you could do that too:

```js
db
  .table('test')
  .get('07290fa5-1691-430c-a397-111575370881')
  .run(/*..optional options..*/, function (err, result) {

  });
```

The library exports `r` in case you need it, for filters for example:

```js
import RethinkPlus, {r} from 'rethink-plus';
// ...
let people = db.table('people');
let adults = await people.filter(r.row('age').gt(16));
```
