# go-log

[![GoDoc](https://pkg.go.dev/badge/github.com/tnmproject/zap-log.svg)](https://pkg.go.dev/github.com/tnmproject/zap-log)

This logging library used by go-log

zap-log wraps [zap](https://github.com/uber-go/zap) to provide a logging facade. go-log manages logging
instances and allows for their levels to be controlled individually.

## Install

```sh
go get github.com/tnmproject/zap-log
```

## Usage

Once the package is imported under the name `logging`, an instance of `EventLogger` can be created like so:

```go
var log = logging.Logger("subsystem name")
```

It can then be used to emit log messages in plain printf-style messages at seven standard levels:

Levels may be set for all loggers:

```go
lvl, err := logging.LevelFromString("error")
if err != nil {
	panic(err)
}
logging.SetAllLoggers(lvl)
```

or individually:

```go
err := logging.SetLogLevel("net:pubsub", "info")
if err != nil {
	panic(err)
}
```

or by regular expression:

```go
err := logging.SetLogLevelRegex("net:.*", "info")
if err != nil {
	panic(err)
}
```

### Environment Variables

This package can be configured through various environment variables.

#### `ZAP_LOG_LEVEL`

Specifies the log-level, both globally and on a per-subsystem basis.

For example, the following will set the global minimum log level to `error`, but reduce the minimum
log level for `subsystem1` to `info` and reduce the minimum log level for `subsystem2` to debug.

```bash
export ZAP_LOG_LEVEL="error,subsystem1=info,subsystem2=debug"
```

#### `ZAP_FILE`

Specifies that logs should be written to the specified file. If this option is _not_ specified, logs are written to standard error.

```bash
export ZAP_FILE="/path/to/my/file.log"
```

#### `ZAP_OUTPUT`

Specifies where logging output should be written. Can take one or more of the following values, combined with `+`:

- `stdout` -- write logs to standard out.
- `stderr` -- write logs to standard error.
- `file` -- write logs to the file specified by `ZAP_FILE`

For example, if you want to log to both a file and standard error:

```bash
export ZAP_FILE="/path/to/my/file.log"
export ZAP_OUTPUT="stderr+file"
```

Setting _only_ `ZAP_FILE` will prevent logs from being written to standard error.

#### `ZAP_LOG_FMT`

Specifies the log message format. It supports the following values:

- `color` -- human readable, colorized (ANSI) output
- `nocolor` -- human readable, plain-text output.
- `json` -- structured JSON.

For example, to log structured JSON (for easier parsing):

```bash
export ZAP_LOG_FMT="json"
```

The logging format defaults to `color` when the output is a terminal, and `nocolor` otherwise.

#### `ZAP_LOG_LABELS`

Specifies a set of labels that should be added to all log messages as comma-separated key-value
pairs. For example, the following add `{"app": "example_app", "dc": "sjc-1"}` to every log entry.

```bash
export ZAP_LOG_LABELS="app=example_app,dc=sjc-1"
```

## Contribute

Feel free to join in. All welcome. Open an [issue](https://github.com/tnmproject/zero-log/issues)!

## License

MIT
