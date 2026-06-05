# Stimma Tools Protocol (STP)

STP is a JSON-RPC 2.0 protocol for exposing image and video generation tools to
applications, designed to make those tools legible to both software agents and
human-facing UIs.

**The specification lives in [`TOOLS_PROTOCOL.md`](TOOLS_PROTOCOL.md).** That document is
authoritative; this README is only an orientation. Where the two disagree, the spec wins.

## What problem it solves

Generation tools — text-to-image models, upscalers, video pipelines — each ship their own
API, parameters, and asset conventions. An application that wants to offer several of them
ends up writing a bespoke integration per tool. STP defines one wire protocol so that any
conformant tool can be discovered, inspected, and executed through a single interface, with
parameters described well enough that a UI can render them and an agent can reason about them.

## The model

STP has two roles:

- **Provider** — a process that exposes generation tools and executes them. It may be a
  local subprocess or a remote service.
- **Host** — the application that connects to providers, routes execution requests, and
  surfaces the aggregated tools to its users.

[Stimma](https://stimma.ai) is the reference host, but the protocol is host-agnostic — any
application can implement the host side.

A typical session: the provider announces itself (`provider.register`), the host asks for its
tools (`tools.list`), then runs one (`tools.execute`) and receives streamed progress
(`tools.progress`) and a final result (`tools.result`). Binary payloads — input and output
images, video, weights — travel out of band and are referenced in messages by opaque asset
IDs. Two transports are defined: **WebSocket** (the host connects to a provider's server, with
assets over HTTP) and **stdio** (the host spawns the provider, with assets over a shared
directory). See the spec for the full lifecycle, schemas, queueing, error codes, and uploads.

## Implementations

| Role | Project | Package |
|------|---------|---------|
| Provider SDK (Python) | [stimma-tools-protocol-python](https://github.com/stimma-ai/stimma-tools-protocol-python) | [`stimma-tools-protocol`](https://pypi.org/project/stimma-tools-protocol/) on PyPI |
| Provider SDK (TypeScript) | [stimma-tools-protocol-ts](https://github.com/stimma-ai/stimma-tools-protocol-ts) | [`stimma-tools-protocol`](https://www.npmjs.com/package/stimma-tools-protocol) on npm |
| Host CLI | [stimma-tools-protocol-cli](https://github.com/stimma-ai/stimma-tools-protocol-cli) | `stp` |

The SDKs implement the provider (server) side — use them to build a tool. The CLI implements
the host (client) side — use it to connect to a provider, list its tools, and run them from
the terminal.

## Contributing

Bug reports and feature requests for the protocol are welcome — please
[open an issue](https://github.com/stimma-ai/stimma-tools-protocol/issues). We are not
accepting pull requests at this time.

## License

Apache-2.0. See [`LICENSE`](LICENSE).
