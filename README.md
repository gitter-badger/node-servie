# ![Servie](https://cdn.rawgit.com/blakeembrey/node-servie/master/logo.svg)

[![NPM version][npm-image]][npm-url]
[![NPM downloads][downloads-image]][downloads-url]
[![Build status][travis-image]][travis-url]
[![Test coverage][coveralls-image]][coveralls-url]

> **Servie** provides standard, framework-agnostic HTTP interfaces for servers and clients.

## Installation

```
npm install servie --save
```

## Usage

* [`throwback`](https://github.com/blakeembrey/throwback) Compose middleware into a single function
* [`servie-lambda`](https://github.com/blakeembrey/node-servie-lambda) Transport layer for AWS Lambda

### Common

> Base HTTP class for common request and response logic.

```ts
import { Common } from 'servie'
```

#### Options

* `headers?` HTTP headers (`Headers | HeadersObject | string[]`)
* `trailers?` HTTP trailers (`Headers | HeadersObject | string[]`)
* `events?` An event emitter object (`EventEmitter`)
* `body?` Allowed HTTP bodies (`string | Buffer | Readable | object`)

#### Properties

* `events` The request/response event emitter
* `headers` The submitted headers as a `Headers` instance
* `trailers` The submitted trailers as a `Headers` instance
* `body` The submitted body payload
* `bodyUsed` A boolean indicating where the body was read
* `type` A shorthand property for reading and writing the `Content-Type` header
* `length` A shorthand property for reading and writing `Content-Length` as a number
* `started` Boolean indicating if a request/response has started
* `finished` Boolean indicating if a request/response has finished
* `bytesTransferred` The number of bytes sent in the HTTP request/response

#### Methods

* `buffer(maxBufferSize): Promise<Buffer>` Read the body into a `Buffer` object
* `text(maxBufferSize): Promise<string>` Read the body as a `string`
* `stream(): Readable` Read the body as a `Readable` stream
* `setTimeout(ms): void` Set a timeout on the request or response to be marked as finished
* `clearTimeout(ms): void` Clear a previous timeout

#### Events

* `headers` Emitted when the `headers` object becomes available
* `trailers` Emitted when the `trailers` object becomes available
* `error` Emitted when an out-of-band error occurs (e.g. abort or timeout) and MUST be handled by the transport
* `started` Emitted when the request/response has started
* `finished` Emitted when the request/respone has finished
* `progress` Emitted when the `bytesTransferred` properties is incremented

### Request

> HTTP class for encapsulating a `Request`, extends `Common`.

```ts
import { Request } from 'servie'
```

#### Options

```ts
const request = new Request({
  url: '/',
  method: 'GET'
})
```

> Extends `Common` options.

* `url` The HTTP request url (`string`)
* `method?` The HTTP request method (`string`, default: `GET`)
* `connection?` Connection information (`{ remoteAddress?: string, remotePort?: number, localAddress?: string, localPort?: number, encrypted?: boolean }`)

#### Properties

* `url` The HTTP request url (`string`)
* `method` The HTTP request method upper-cased (`string`)
* `Url` The HTTP request url as a read-only parsed object (`object`)
* `connection` Connection information (`{}`)

#### Methods

* `abort(): boolean` Emit an abort event
* `error(message, code, status?, original?): HttpError` Create a HTTP error instance

#### Events

* `abort` Emitted when the request is aborted and MUST be handled by transport

### Response

> HTTP class for encapsulating a `Response`, extends `Common`.

```ts
import { Response } from 'servie'
```

#### Options

```ts
const response = new Response(request, {})
```

> Extends `Common` options.

* `status?` The HTTP response status code (`number`)
* `statusText?` The HTTP response status message (`string`)

#### Properties

* `status?` The HTTP response status code (`number`)
* `statusText?` The HTTP response status message (`string`)

### Headers

> Used by `Common` for `Request` and `Response` objects.

#### Options

Take a single parameter with the headers in object, array or `Headers` format.

#### Properties

* `raw` The raw HTTP headers list (`string[]`)

#### Methods

* `object(obj?: HeadersObject | null): HeadersObject | void` A getter/setter method for reading the headers as a lower-cased object (like node.js)
* `set(name: string, value: string | string[]): this` Set a HTTP header by overriding case-insensitive headers of the same name
* `append(name: string, value: string | string[]): this` Append a HTTP header
* `get(name: string): string | undefined` Retrieve a case-insensitive HTTP header
* `getAll(name: string): string[]` Retrieve a list of matching case-insensitive HTTP headers
* `has(name: string): boolean` Check if a case-insensitive header is already set
* `delete(name: string): this` Delete a case-insensitive header

### HTTP Error

> Internally and externally triggered HTTP errors.

#### Properties

* `code` A unique error code (`string`)
* `status` A HTTP status code (`number`)
* `request` The `Request` instance that triggered the error (`Request`)
* `message` Standard error message (`string`)
* `cause` Specified when the HTTP error was triggered by an underlying error

## JavaScript

This module is designed for ES5 environments, but also requires `Promise` to be available.

## TypeScript

This project is written using [TypeScript](https://github.com/Microsoft/TypeScript) and publishes the definitions directly to NPM.

## License

MIT

[npm-image]: https://img.shields.io/npm/v/servie.svg?style=flat
[npm-url]: https://npmjs.org/package/servie
[downloads-image]: https://img.shields.io/npm/dm/servie.svg?style=flat
[downloads-url]: https://npmjs.org/package/servie
[travis-image]: https://img.shields.io/travis/blakeembrey/node-servie.svg?style=flat
[travis-url]: https://travis-ci.org/blakeembrey/node-servie
[coveralls-image]: https://img.shields.io/coveralls/blakeembrey/node-servie.svg?style=flat
[coveralls-url]: https://coveralls.io/r/blakeembrey/node-servie?branch=master
