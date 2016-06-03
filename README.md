# persistent-cache

A simple Node module to persistently store/cache arbitrary data.

## How to install

Just run

    npm install persistent-cache

add the `--save` option to add `persistent-cache` to the `dependencies` in your `package.json`

## Usage

### Create cache

    var cache = require('persistent-cache');

    var cats = cache();

`catCache` is now a cache with the default options, meaning it has the name `cache`, caches data forever and is located in the `cache`-directory of the current main module. For all available options, see the bottom of this page.

An empty cache of cats is kind of sad. Let's bring some life to it.

### Insert data

    //Asynchronous
    cats.put('Cindy', {color: 'red'}, someCallback);

    //Synchronous
    cats.putSync('babies', ['Ron', 'Emily']);

Now our `cats`-cache has an entry `Cindy` containing an object `{color: 'red'}` and an entry `babies` containing an array of some names (strings).

`cache.put(key, data, cb)` will store any arbitrary `data` in the cache under the provided `key` and call the provided callback when done (passing err as the first argument, following node convention). `cache.putSync(key, data)` is the synchronous counterpart (`throw`ing possible errors).

If there is already an entry for the provided key, `cache.put` will overwrite it.

Argh, my dog quit the program. I miss my cats :-( Lets retrieve them from the cache.

### Retrieve data

    cats.get('babies', function(err, babies) {
        //check err for errors

        console.log(babies); //['Ron', 'Emily']
    });

    console.log(cats.getSync('Cindy')); //{ color: 'red' }

There they are :D

`cache.get(key, cb)` will get the data saved under `key` from the cache and call the provided callback when done, passing the retrieved data as the second argument (again, passing error first following node convention). `cache.getSync` is the synchronous counterpart, returning the data.

I found a new owner for my cute cat babies. So I need to remove them from my cache.

### Delete data

    cats.delete('babies', function(err) {
        //Handle errors

        console.log('babies removed from cache');
    });

I safely removed my cat babies from the cache. Yey!

`cache.delete(key, cb)` will remove the provided `key` from the cache and call the provided callback when done. `cache.deleteSync` is the synchronous counterpart.

## Options

    var someCache = cache({
        base: 'some/folder',
        name: 'foo',
        duration: 1000 * 3600 * 24 //one day
    });

When creating a new cache, you may pass an options object, with the following available properties:

### `options.base`

The base directory where `persistent-cache` will save its caches.

Defaults to the main modules directory.

### `options.name`

The name of the cache. Determines the name of the created folder where the data is stored, which is just `base + name`.

Defaults to `cache`.

### `options.duration`

The amount of milliseconds a cache entry should be valid for. If not set, cache entries are not invalidated (stay until deleted).

Defaults to `undefined` (infinite)
