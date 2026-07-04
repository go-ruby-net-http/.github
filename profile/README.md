<p align="center"><img src="https://raw.githubusercontent.com/go-ruby-net-http/brand/main/social/go-ruby-net-http-net-http.png" alt="go-ruby-net-http" width="640"></p>

<h1 align="center">go-ruby-net-http</h1>
<p align="center"><strong>Ruby's <code>Net::HTTP</code> HTTP/1.1 message codec — in pure Go, no cgo, no I/O.</strong></p>

<p align="center">
  🌐 <a href="https://go-ruby-net-http.github.io">Website</a> ·
  📚 <a href="https://go-ruby-net-http.github.io/docs/">Documentation</a>
</p>

<p align="center">
  <a href="https://go-ruby-net-http.github.io/docs/"><img alt="Docs" src="https://img.shields.io/badge/docs-mkdocs--material-9B1C2E?style=flat-square"></a>
  <a href="https://github.com/go-ruby-net-http/net-http/blob/main/LICENSE"><img alt="License: BSD-3-Clause" src="https://img.shields.io/badge/license-BSD--3--Clause-blue?style=flat-square"></a>
  <img alt="Go 1.26.4+" src="https://img.shields.io/badge/go-1.26.4%2B-00ADD8?style=flat-square&logo=go&logoColor=white">
  <img alt="Coverage 100%" src="https://img.shields.io/badge/coverage-100%25-1a7f37?style=flat-square">
</p>

---

go-ruby-net-http is a pure-Go (no cgo) reimplementation of the deterministic, interpreter-independent core of Ruby's **`Net::HTTP`** — the HTTP/1.1 *message* codec from MRI 4.0.5. It builds request bytes **exactly as MRI writes them to the socket** (request line, default headers in MRI order, `Content-Length` / chunked framing, form and basic-auth encoding) and parses a raw response byte stream into MRI's `Net::HTTPResponse` subclass model (status line, folded multi-value headers, `Content-Length` *and* chunked decoding) — **without any Ruby runtime, and without performing any I/O itself**. The socket and TLS are a **host-side seam**: hand `Request.Bytes` to your socket's write, feed everything read back to `ParseResponse`. It is a **standalone, reusable** module — the HTTP-message backend for [go-embedded-ruby](https://github.com/go-embedded-ruby/ruby) (`rbgo`), the same pattern as go-ruby-yaml — differential-tested against MRI, 100% coverage, CI green across 6 arches and 3 OSes.

## Repositories

| Repo | What it is |
|------|------------|
| [**net-http**](https://github.com/go-ruby-net-http/net-http) | the library: the `Net::HTTP` message codec in pure Go |
| [**docs**](https://github.com/go-ruby-net-http/docs) | MkDocs Material documentation, versioned with [mike], served at [/docs/](https://go-ruby-net-http.github.io/docs/) |
| [**go-ruby-net-http.github.io**](https://github.com/go-ruby-net-http/go-ruby-net-http.github.io) | the Hugo landing page |
| [**brand**](https://github.com/go-ruby-net-http/brand) | logos and brand assets |

## Principles

- **Pure Go, zero cgo.** Cross-compiles and embeds anywhere; a static binary by
  default.
- **A codec, not a client.** It formats and parses HTTP/1.1 messages and performs
  **no I/O** — the socket, TLS and DNS are the host's job. `rbgo` supplies the
  byte transport and binds this module as its HTTP-message backend, the same
  pattern as go-ruby-yaml.
- **MRI byte-exact.** Request bytes and parsed responses match reference Ruby
  exactly, validated by a differential oracle against the `ruby` binary.
- **Standalone & reusable.** No dependency on the Ruby runtime; the dependency
  runs the other way.
- **100% test coverage** is the target, enforced as a CI gate.

## Status

**Complete — MRI byte-exact codec.** Faithful pure-Go port of `Net::HTTP`'s message codec, validated by a **differential oracle** against the system `ruby` — request bytes and parsed responses compared byte-for-byte — at 100% coverage, `gofmt` + `go vet` clean, CI green across the six 64-bit Go targets (amd64, arm64, riscv64, loong64, ppc64le, s390x) and three operating systems. It is designed to be bound into the [go-embedded-ruby](https://github.com/go-embedded-ruby/ruby) (`rbgo`) ecosystem as the HTTP-message backend, the same pattern as go-ruby-yaml.

BSD-3-Clause.

[mike]: https://github.com/jimporter/mike
