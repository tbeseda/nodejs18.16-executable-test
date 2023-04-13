# Node.js 18.16.0 Single Executable Applications (SEA)

Nifty. Hope we can get the file size down:

```
.rwxr-xr-x@  82M  hello
```

For what it's worth this is on par with a compiled Deno binary.

## macOS process

Make sure you've got Node.js 18.16.0 installed.

```sh
echo 'console.log(`Hello, ${process.argv[2]}!`);' > hello.js
```

```sh
cp $(command -v node) hello
```

```sh
codesign --remove-signature hello
```

```sh
npx postject hello NODE_JS_CODE hello.js \
  --sentinel-fuse NODE_JS_FUSE_fce680ab2cc467b6e072b8b5df1996b2 \
  --macho-segment-name NODE_JS
```

```sh
codesign --sign - hello
```

```sh
./hello world
```

♠️

## v19 (unreleased)

Make use of the `--experimental-sea-config` flag to generate the binary blob ahead of injection.

Example config in sea-config.json:

```json
{ "main": "hello.js", "output": "sea-prep.blob" }
```

## References:

- [v18.16 SEA doc](https://nodejs.org/dist/latest-v18.x/docs/api/single-executable-applications.html)
  - currently doesn't mention binary signing, but the rest works
- [18.16.0 Release Notes](https://nodejs.org/en/blog/release/v18.16.0)
- [SEA PR](https://github.com/nodejs/node/pull/45038)
- [SEA Initiative task](https://github.com/nodejs/node/issues/43432)
  - started June 14, 2022
- [mention of `codesign`](https://github.com/nodejs/postject/issues/76)
  - same issue I had where the process is immediately killed

#### Future version is configurable

- [unreleased v19 doc](https://github.com/nodejs/node/blob/527394783ece910bf8c543f5a01f100ae37e5c33/doc/api/single-executable-applications.md)
  - with `--experimental-sea-config` flag
- [related PR](https://github.com/nodejs/node/pull/47125)
