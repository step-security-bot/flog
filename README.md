<img alt="flog" src="images/trunk.png" width="180" align="right">

# flog

[![OpenSSF Scorecard](https://img.shields.io/ossf-scorecard/github.com/marcransome/flog?style=flat&label=OpenSSF%20Scorecard)](https://securityscorecards.dev/viewer/?uri=github.com/marcransome/flog) [![CodeQL](https://github.com/marcransome/flog/actions/workflows/codeql-analysis.yml/badge.svg?branch=main)](https://github.com/marcransome/flog/actions/workflows/codeql-analysis.yml) [![Issues](https://img.shields.io/github/issues/marcransome/flog)](https://github.com/marcransome/flog/issues) [![License](https://img.shields.io/badge/license-MIT-blue)](https://opensource.org/licenses/mit-license.php) [![macOS](https://img.shields.io/badge/macOS-11+-blue)](https://www.apple.com/macos/)

`flog` is a command-line tool for sending log messages to Apple's unified logging system and is primaily intended for use in scripts.

<hr>

## Rationale

> _Why not use `logger(1)`?_

`logger` doesn't support the full set of log levels provided by Apple's unified logging system, nor does it support specifying _subsystem_ and _category_ strings. `flog` on the other hand uses Apple's unified logging system C language APIs and supports both.

## Getting started

### Installation

Install with [Homebrew](https://brew.sh):

```bash
brew install marcransome/tap/flog
```

### Logging messages

To log a message using the `default` log level:

```bash
flog '<message>'
```

Optionally specify a _subsystem_ and _category_ using the `-s, --subsystem` and `-c, --category` options:

```bash
flog -s uk.co.fidgetbox -c general 'informational message'
```

Override the default log level using the `-l, --level` option and one of the values `info`, `debug`, `error` or `fault`:

```bash
flog -l fault -s uk.co.fidgetbox -c general 'unrecoverable failure'
```

`flog` can also read the log message from the standard input stream using a pipe:

```bash
./some-script | flog -l info
```

Or the log message can be read from a file using shell redirection:

```bash
flog < /var/log/some-script.log
```

Use the `-a, --append` option to also append the log message to a file (creating the file if necessary):

```bash
flog -a /var/log/some-script.log -l fault -s uk.co.fidgetbox -c general 'unrecoverable failure'
```

> [!WARNING]
> Log message strings are _public_ by default and can be read using the `log(1)` command or [Console](https://support.apple.com/en-gb/guide/console/welcome/mac) app. To mark a message as private add the `-p|--private` option to the command. Doing so will redact the message string, which will be shown as `'<private>'` when accessed using the methods previously mentioned. [Device Management Profiles](https://developer.apple.com/documentation/devicemanagement) can be used to grant access to private log messages.

### Reading log messages

Refer to the `log(1)` man page provided on macOS-based systems for extensive documentation on how to access system wide log messages, or alternatively use the [Console](https://support.apple.com/en-gb/guide/console/welcome/mac) app.

Here's an example log stream in Console, filtered by subsystem name, showing messages generated by `flog`:

<img width="1004" alt="console" src="images/console.png">

And here's a similar log stream viewed with Apple's `log(1)` command:

<img width="995" alt="log" src="images/log.png">

## Building

### Requirements

* macOS `11.x` (Big Sur) or later
* A C17 compiler
* CMake version `>=3.22`
* `pkg-config` version `>=0.29.2`
* `libpopt` version `>=1.19`
* `libcmocka` version `>=1.1.7` (if building unit test targets)

### Building from source

To perform an out-of-source build from the project directory:

```bash
cmake -S . -B build
cmake --build build
```

### Building unit test targets

To perform an out-of-source build for unit test targets only:

```bash
cmake -S . -B build -DUNIT_TESTING=ON
cmake --build build
```

To execute all unit test targets:

```bash
cd build/test
ctest -V
```

## Acknowledgements

* Trunk icon made by [Freepik](https://www.flaticon.com/authors/freepik) from [www.flaticon.com](https://www.flaticon.com/)

## License

`flog` is provided under the terms of the [MIT License](https://opensource.org/licenses/mit-license.php).

## Contact

Email me at [marc.ransome@fidgetbox.co.uk](mailto:marc.ransome@fidgetbox.co.uk) or [create an issue](https://github.com/marcransome/flog/issues).
