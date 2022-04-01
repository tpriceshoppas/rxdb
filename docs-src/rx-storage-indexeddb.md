# RxStorage IndexedDB

The IndexedDB `RxStorage` is based on plain IndexedDB and can be used in browsers, electron or hybrid apps.




## Pros & Cons

### Pros

- It is really fast because it uses all of the known performance optimizations for IndexedDB.
- It has a small build size.

### Cons

- It is part of the `rxdb-premium` plugin that must be purchased.
- Does not support CouchDB replication.
- Only runs on runtimes that support [IndexedDB v2](https://caniuse.com/indexeddb2), so it does not work on Internet Explorer. 


## Usage

```ts
import {
    createRxDatabase
} from 'rxdb';
import {
    getRxStorageIndexedDB
} from 'rxdb-premium/plugins/indexeddb';

const db = await createRxDatabase({
    name: 'exampledb',
    storage: getRxStorageIndexedDB({
        /**
         * For better performance, queries run with a batched cursor.
         * You can change the batchSize to optimize the query time
         * for specific queries.
         * You should only change this value when you are also doing performance measurements.
         * [default=50]
         */
        batchSize: 50
    })
});
```


## Overwrite/Polyfill the native IndexedDB

Node.js has no IndexedDB API. To still run the IndexedDB `RxStorage` in Node.js, for example to run unit tests, you have to polyfill it.
You can do that by using the [fake-indexeddb](https://github.com/dumbmatter/fakeIndexedDB) module and pass it to the `getRxStorageDexie()` function.

```ts
import { createRxDatabase } from 'rxdb';
import { getRxStorageIndexedDB } from 'rxdb-premium/plugins/indexeddb';

//> npm install fake-indexeddb --save
const fakeIndexedDB = require('fake-indexeddb');
const fakeIDBKeyRange = require('fake-indexeddb/lib/FDBKeyRange');

const db = await createRxDatabase({
    name: 'exampledb',
    storage: getRxStorageIndexedDB({
        indexedDB: fakeIndexedDB,
        IDBKeyRange: fakeIDBKeyRange
    })
});

```