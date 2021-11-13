# Argon2 in browser modularized, mainly done to work with angular.

Fork from [Antelle repository](https://github.com/antelle/argon2-browser)

## Motivation

I am doing several angular projects requiring client side hashing.
Argon2 is very attractive due to it's hardware parameters.

For this reason, I wanted a common way for thoses projects to use
Argon2. I am terminating an Angular/Typescript library to be directly available
and working with Angular to expose Argon2 code presented by Antelle.

Angular, even with lazy loading of the module, should keep a very small footprint
to have the quickest startup. That's why for now I am not including the WASM file
into my projects. I am opting to download this file asynchronously each time a
specific front-end module requires it (generally a singleton service downloads the file once).

But who says download, says potentially security risks. Therefore, this repo will soon have
some cryptography added with OpenSSL to securely share the WASM file.

1. Some checksums to ensure a good transport of the file through the internet.
2. ECDSA signature verification to authenticate the WASM in the front-end.

I am not very familiar for now with how an angular app can use certificate store of
the local machine. So for now the idea is:

1. Generate a ECDSA keypair locally after the dist/argon2.[js|wasm] are generated.
2. Compute the Checksum of the argon2.wasm file.
3. Compute ECDSA signature on the argon2.wasm file.
4. Bundle checksum and signature to a file being placed at the same location of argon2.wasm for it's distribution.
5. Checking what's the best way to distribute the public key, here my knowledge is more limited.
       * Generating a certificate contaning the publickey, but requires a self-signed CA to build chain of trust for the certificate.
         How this certificate is then installed on client machines...?

       * Pasting the ECDSA public key directly inside angular code? (hardcoding that kind of things looks ugly..?)

The point is that, trying to make the wasm file easily available and securely checked before being instanciated.
(Any comment on that part is soooo welcome!)


## changes from Antelle repo

1. Argon2 subrepo is now using HTTPS instead of ssh to be cloned using command such as `git submodule update --init --recursive`.
2. The `build-wasm.sh` script was slightly reworked to remove perl scripts and activate the modularize option.

## Coming soon

OpenSSL signing of the WASM file + checksum compute bundled to be placed next to the argon2.wasm file on webserver.


## License

[MIT](https://opensource.org/licenses/MIT)
