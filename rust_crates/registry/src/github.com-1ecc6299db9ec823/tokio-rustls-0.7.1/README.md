# tokio-rustls
[![travis-ci](https://travis-ci.org/quininer/tokio-rustls.svg?branch=master)](https://travis-ci.org/quininer/tokio-rustls)
[![appveyor](https://ci.appveyor.com/api/projects/status/4ukw15enii50suqi?svg=true)](https://ci.appveyor.com/project/quininer/tokio-rustls)
[![crates](https://img.shields.io/crates/v/tokio-rustls.svg)](https://crates.io/crates/tokio-rustls)
[![license](https://img.shields.io/badge/License-MIT-blue.svg)](https://github.com/quininer/tokio-rustls/blob/master/LICENSE-MIT)
[![license](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://github.com/quininer/tokio-rustls/blob/master/LICENSE-APACHE)
[![docs.rs](https://docs.rs/tokio-rustls/badge.svg)](https://docs.rs/tokio-rustls/)

Asynchronous TLS/SSL streams for [Tokio](https://tokio.rs/) using
[Rustls](https://github.com/ctz/rustls).

### Basic Structure of a Client

```rust
use webpki::DNSNameRef;
use tokio_rustls::{ ClientConfigExt, rustls::ClientConfig };

// ...

let mut config = ClientConfig::new();
config.root_store.add_server_trust_anchors(&webpki_roots::TLS_SERVER_ROOTS);
let config = Arc::new(config);
let domain = DNSNameRef::try_from_ascii_str("www.rust-lang.org").unwrap();

TcpStream::connect(&addr)
	.and_then(|socket| config.connect_async(domain, socket))

// ...
```

### Client Example Program

See [examples/client](examples/client/src/main.rs). You can run it with:

```sh
cd examples/client
cargo run -- hsts.badssl.com
```

Currently on Windows the example client reads from stdin and writes to stdout using
blocking I/O. Until this is fixed, do something this on Windows:

```sh
cd examples/client
echo | cargo run -- hsts.badssl.com
```

### Server Example Program

See [examples/server](examples/server/src/main.rs). You can run it with:

```sh
cd examples/server
cargo run -- 127.0.0.1 --cert mycert.der --key mykey.der
```

### License & Origin

tokio-rustls is primarily distributed under the terms of both the [MIT license](LICENSE-MIT) and
the [Apache License (Version 2.0)](LICENSE-APACHE), with portions covered by various BSD-like
licenses.

This started as a fork of [tokio-tls](https://github.com/tokio-rs/tokio-tls).
