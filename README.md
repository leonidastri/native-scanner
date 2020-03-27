This repository hosts the native scanner, a library that searches for
strings in binary libraries, to inform static analyses.

This library has the following modes:

* [binutils-based](https://www.gnu.org/software/binutils/) analysis: this assumes the existence of utilities
  such as `nm`, `objdump`, and `gdb`.

* Radare2-based analysis: this assumes the existence of a
  [Radare2](https://rada.re/) installation with
  [r2pipe](https://github.com/radareorg/radare2-r2pipe).

## Setup ##

### Standalone application ###

Install the application locally:

```
./gradlew installDist
```

The resulting binary is in `build/install/native-scanner/bin/native-scanner`.

### Library ###

Add the Bintray repository in your application build.gradle
and add the library as a dependency:

```
repositories {
  maven { url 'https://dl.bintray.com/gfour/plast-lab' }
  ...
}

dependencies {
  implementation 'org.clyze:native-scanner:0.5.0'
}
```

Note: this project also supports publishing to the local Maven
repository via `./gradlew publishToMavenLocal`.

### Binutils mode ###

This mode uses command-line programs available in your POSIX system,
such as `nm`, `objdump`, and `gdb`. This works for analyzing binaries
on a system with the same ABI (for example, x86 binaries on a x86
system).

To analyze ARM binaries on a x86 system, appropriate toolchains should
be used. Depending on the actual ARM target (32-bit or 64-bit), set
environment variables `ARMEABI_TOOLCHAIN` and `AARCH64_TOOLCHAIN` to
point to the correct toolchains (to generate such toolchains, consult
the [Android NDK documentation](https://developer.android.com/ndk/guides/standalone_toolchain)
or equivalent binary SDK distribution).

## Use ##

For the standalone application, pass `--help` to see the available
options.

For the library, instantiate a NativeScanner object and a BinaryAnalysis
object, and use method `NativeScanner.scanBinaryCode()` to scan a native
library. To consume the results, implement interface `NativeDatabaseConsumer`.
See the standalone entry point org.clyze.scanner.Main.main() for an actual
piece of code using this library.
