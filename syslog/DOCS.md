# Syslog Home Assistant app

## How to use

This app allows you to send your HAOS logs to a remote syslog server.

## Configuration

App configuration:

```yaml
syslog_host: syslog.local
syslog_port: 514
syslog_protocol: udp
syslog_ssl: false
syslog_ssl_verfify: false
syslog_format: rfc3164
redact_patterns: []
```

| key | name | description |
| --- | ---- | ----------- |
| `syslog_host` | Syslog host | The hostname or IP address of the remote syslog server to send HAOS logs to. |
| `syslog_port` | Syslog port | The port of the remote syslog server to send HAOS logs to. |
| `syslog_protocol` | Transfer protocol | The protocol to be used to send HAOS logs. |
| `syslog_ssl` | SSL encryption | Whether or not to to use ssl encryption (only supported with tcp). |
| `syslog_ssl_verfify` | SSL verify | Whether or not to verify ssl certificate. |
| `syslog_format` | Syslog format | The syslog message format, either `rfc3164` (BSD syslog) or `rfc5424` (ISO 8601 timestamps, structured data header). |
| `redact_patterns` | Redact patterns | Regular expressions (Python syntax) whose matches are replaced by `***` before a message is sent. |

### Redacting secrets

Some apps write credentials into their logs, for example a secret URL path in every access log line. Once such a line is sent, it is stored on the remote syslog server and travels the network in clear text with UDP or TCP without TLS. `redact_patterns` masks these values on the Home Assistant host, before the message leaves it:

```yaml
redact_patterns:
  - "/private_[A-Za-z0-9_-]{20,}"
  - "(?<=token=)[^&\\s]+"
```

The first pattern replaces a secret path like `/private_AbC…` completely, the second one keeps `token=` and masks only the value. Patterns are applied to the message text in the given order. An invalid pattern stops the app with an error, so no unmasked messages are sent.

## Support

In case you've found a bug, please [open an issue on GitHub][issue].

[issue]: https://github.com/mib1185/ha-addon-syslog/issues

# You like my work?

<a href="https://www.buymeacoffee.com/mib1185" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png" alt="Buy Me A Coffee" height="41" width="174"></a>
