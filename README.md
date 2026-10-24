# 🛡️ Sentinel

**Lightweight network intrusion detection system — a Snort alternative in pure Rust.**

Sentinel is a fast, minimal IDS that analyzes network traffic from pcap captures using signature-based rules. Built for performance with zero-copy packet parsing.

## Features

- **Protocol Parsing** — Ethernet, IPv4, IPv6, TCP, UDP, ICMP, ARP (zero-copy)
- **Detection Engine** — Rule matching with protocol, IP, port, TCP flags, and content checks
- **TOML Rules** — Human-readable rule format with severity levels and categories
- **Colored Alerts** — Console output with severity-colored alerts (JSON serialization available in the library API)
- **Connection tracking** — per-flow packet/byte counts and TCP state (`analyze -c`)
- **CLI** — `analyze` and `rules` subcommands (`monitor` is a placeholder, see below)

## Quick Start

```bash
cargo install --git https://github.com/quantumnic/sentinel
# or: git clone https://github.com/quantumnic/sentinel && cd sentinel && cargo build --release
#     (binary at ./target/release/sentinel)

# List the rules in ./rules.toml (run from the repo root, or pass a path)
sentinel rules list --path rules.toml

# Validate a rules file
sentinel rules validate rules.toml

# Analyze a pcap capture (e.g. recorded with tcpdump -w capture.pcap)
sentinel analyze capture.pcap --rules rules.toml --connections
```

> **Note:** live capture is not implemented yet. `sentinel monitor --interface <if>` currently only loads the rules file and exits; use `analyze` on a pcap file instead.

## Rule Format

```toml
[[rule]]
id = "SID-1001"
name = "TCP SYN scan detected"
severity = "high"        # low | medium | high | critical
category = "scan"        # scan | exploit | malware | policy | anomaly
protocol = "tcp"         # tcp | udp | icmp | any
dst_port = 80
tcp_flags = "S"
content = "pattern"
```

## Building

```bash
cargo build --release
cargo test
```

## License

MIT
