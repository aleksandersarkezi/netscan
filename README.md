# netscan

[![CI](https://github.com/aleksandersarkezi/netscan/actions/workflows/ci.yml/badge.svg)](https://github.com/aleksandersarkezi/netscan/actions/workflows/ci.yml)

A small async network scanner for the command line, written in Python 3.11+.
It does TCP connect scans, so it needs no root privileges.

## Ethical use

Only scan networks you own or have permission to test. Unauthorized scanning may be
illegal.

## Features

- Async TCP connect scan, no root required
- Basic host discovery over TCP (no ICMP)
- Banner grabbing for HTTP, SSH, FTP, SMTP and more
- Simple OS guessing from banners and port patterns (heuristics, not reliable)
- Colored terminal output with [rich](https://github.com/Textualize/rich)
- Export to JSON, CSV or XML
- Targets as IPs, CIDR ranges, IP ranges or hostnames
- Rate limiting and concurrency control

## Installation

Requires Python 3.11+.

```bash
git clone https://github.com/aleksandersarkezi/netscan.git
cd netscan
pip install -e .
```

For development:

```bash
pip install -e ".[dev]"
```

## Usage

Basic:

```bash
netscan 192.168.1.1
netscan 192.168.1.0/24
netscan scanme.nmap.org
```

Ports:

```bash
netscan 10.0.0.1 -p 22,80,443
netscan 10.0.0.1 -p 1-1024
netscan 10.0.0.1 -p top100
```

Ranges:

```bash
netscan 192.168.1.1-50
netscan 10.0.0.1-10.0.0.100
```

Export:

```bash
netscan 192.168.1.0/24 --export json -o results.json
netscan 10.0.0.1 --export csv
netscan target.example.com --export xml
```

### Common flags

| Flag | What it does |
| --- | --- |
| `-p`, `--ports` | Ports to scan: `80,443`, `1-1024`, `top100` or `top1000` |
| `-t`, `--timeout` | Connection timeout per port, in seconds |
| `-c`, `--concurrency` | Maximum parallel connections |
| `--rate` | Limit connections per second |
| `--no-discovery` | Skip the host check and treat every target as up |
| `--no-banners` | Skip service detection |
| `--no-os` | Skip OS guessing |
| `-q`, `--quiet` | Quiet output |
| `-v`, `--verbose` | Verbose output |

Run `netscan --help` for every option.

## Project structure

```text
src/netscan/
├── cli.py          command-line interface
├── scanner.py      async TCP connect scanning
├── discovery.py    host discovery
├── ports.py        port lists and parsing
├── services.py     banner grabbing
├── fingerprint.py  OS guessing
├── exporters.py    JSON, CSV and XML output
└── utils.py        target parsing and helpers
```

## Tests

```bash
pytest
pytest --cov=netscan
```

CI runs ruff and the test suite on Python 3.11, 3.12 and 3.13 for every push.

## Notes

This is a small project for learning and experimentation. For serious work, use
[nmap](https://nmap.org).

## License

MIT, see [LICENSE](LICENSE).
