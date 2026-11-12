
Building
--------

quiche requires Rust 1.88 or later to build. The latest stable Rust release can
be installed using [rustup](https://rustup.rs/).

Once the Rust build environment is setup, the quiche source code can be fetched
using git:

```bash
 $ git clone https://github.com/cloudflare/quiche
```

and then built using cargo:

```bash
 $ cargo build --examples
```

cargo can also be used to run the testsuite:

```bash
 $ cargo test
```

Note that [BoringSSL], which is used to implement QUIC's cryptographic handshake
based on TLS, needs to be built and linked to quiche. This is done automatically
by the [boring-sys] crate when building with cargo, but requires the `cmake`
command to be available during the build process.

On Windows [NASM](https://www.nasm.us/) is also required. The [official BoringSSL
documentation](https://github.com/google/boringssl/blob/master/BUILDING.md) has
more details.

In alternative you can use your own custom build of BoringSSL by configuring
the BoringSSL directory with the ``BORING_BSSL_PATH`` environment variable:

```bash
 $ BORING_BSSL_PATH="/path/to/boringssl" cargo build --examples
```

[BoringSSL]: https://boringssl.googlesource.com/boringssl/
[boring-sys]: https://crates.io/crates/boring-sys

### Building for Android

Building quiche for Android (NDK version 19 or higher, 21 recommended), can be
done using [cargo-ndk] (v2.0 or later).

First the [Android NDK] needs to be installed, either using Android Studio or
directly, and the `ANDROID_NDK_HOME` environment variable needs to be set to the
NDK installation path, e.g.:

```bash
 $ export ANDROID_NDK_HOME=/usr/local/share/android-ndk
```

Then the Rust toolchain for the Android architectures needed can be installed as
follows:

```bash
 $ rustup target add aarch64-linux-android armv7-linux-androideabi i686-linux-android x86_64-linux-android
```

Note that the minimum API level is 21 for all target architectures.

[cargo-ndk] (v2.0 or later) also needs to be installed:

```bash
 $ cargo install cargo-ndk
```

Finally the quiche library can be built using the following procedure. Note that
the `-t <architecture>` and `-p <NDK version>` options are mandatory.

```bash
 $ cargo ndk -t arm64-v8a -p 21 -- build --features ffi
```

See [build_android_ndk19.sh] for more information.

[Android NDK]: https://developer.android.com/ndk
[cargo-ndk]: https://docs.rs/crate/cargo-ndk
[build_android_ndk19.sh]: https://github.com/cloudflare/quiche/blob/master/tools/android/build_android_ndk19.sh

### Building for iOS

To build quiche for iOS, you need the following:

- Install Xcode command-line tools. You can install them with Xcode or with the
  following command:

```bash
 $ xcode-select --install
```

- Install the Rust toolchain for iOS architectures:

```bash
 $ rustup target add aarch64-apple-ios x86_64-apple-ios
```

- Install `cargo-lipo`:

```bash
 $ cargo install cargo-lipo
```

To build libquiche, run the following command:

```bash
 $ cargo lipo --features ffi
```

or

```bash
 $ cargo lipo --features ffi --release
```

iOS build is tested in Xcode 10.1 and Xcode 11.2.

### Building Docker images

In order to build the Docker images, simply run the following command:

```bash
 $ make docker-build
```
