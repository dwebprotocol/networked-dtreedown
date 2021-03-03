# networked-dtreedown
LevelDB-compatible module for loading p2p databases using dWebTree and dSwarm

## Usage

```shell
npm i --save networked-dtreedown levelup
```

```
const levelup = require('levelup')
const { once } = require('events')

const NetworkedDWebTreedown = require('networked-dtreedown')()

const down = new NetworkedDWebTreedown('hyper://someurlhere', {
  keyEncoding: 'utf-8'
})

const db = levelup(db)

await db.open()

// Wait for an initial peer before tryinng to load data
await once(down.base, 'peer-open')

const value = await db.get('Example')

console.log('Got value from remote:', value)
```

## API

### `const NetworkedDWebTreedown = makeNetworkedHyperbeedown({DDatabase, close, ...opts})`

Creates an instance of NetworkedHyperbeeDown.

Provide a custom `DDatabase` constructor and optional `close` for cleaning up if you want full control.

Else you can pass in `opts` which will initialize [dWeb SDK](https://github.com/dwebprotocol/dweb-sdk#const-ddatabase-ddrive-resolvename-keypair-derivesecret-registerextension-close--await-sdkopts)

This will return a constructor for `NetworkedDWebTreedown`.

### `const dwebtreedown = new NetworkedDWebTreedown(url)`

Once you have a `NetworkedDWebTreedown` reference, you can initialize storage with urls.

The `dwebtreedown` instance returned can be passed to `levelup` or anything else that uses `abstract-leveldown`.

The `url` should be a `hyper://` URL which will be passed to the `DDatabase` constructor.

You can pass in an actual public key in the URL if you want to load from the network (which will make it readonly) like `hyper://aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa`.

You can also pass a custom `name` for the URL which will derive a key for you like `hyper://example`

### `const url = await dwebtreedown.getURL()`

If you used a custom `name` for your hyperbee, you can resolve it to the actual publicly accessible URL through this.

Make sure your db has been opened before calling this.

The `url` is a string.

### `const base = dwebtreedown.base`

You can get a reference to the `ddatabase` with this property
