# ccpaid

`ccpaid` runs Claude Code and submits its Anthropic token usage to Paid for a customer selected at session start.

## Usage

```sh
bun run ccpaid.ts -- [claude args...]
```

Build a standalone binary:

```sh
bun build ./ccpaid.ts --compile --outfile ccpaid
./ccpaid -- [claude args...]
```

## Install From GitHub Releases

Download the binary for your platform from the latest release:

```sh
curl -L -o ccpaid https://github.com/paid-ai/ccpaid/releases/latest/download/ccpaid-darwin-arm64
chmod +x ccpaid
./ccpaid -h
```

Available release assets:

- `ccpaid-darwin-arm64`
- `ccpaid-linux-x64`
- `ccpaid-linux-arm64`

Verify a downloaded binary with the release checksums:

```sh
curl -L -o checksums.txt https://github.com/paid-ai/ccpaid/releases/latest/download/checksums.txt
shasum -a 256 -c checksums.txt --ignore-missing
```

## Configuration

`ccpaid` needs a Paid API key. Set it once with:

```sh
ccpaid configure
```

This writes the key to:

```text
~/.ccpaid/config.json
```

The config file stores only:

```json
{
  "paidApiKey": "paid_xxx"
}
```

The file is written with restricted permissions. The key is still plaintext on disk, so use `PAID_API_KEY` instead if you prefer not to save it.

API key precedence:

```text
PAID_API_KEY > ~/.ccpaid/config.json > first-run prompt
```

Optional runtime settings:

- `--external-product-id <id>` or `PAID_EXTERNAL_PRODUCT_ID`
- `--flush-ms <ms>` or `CCPAID_FLUSH_MS`
- `--debug` or `CCPAID_DEBUG=1` to write compact telemetry diagnostics
- `--log-file <path>` or `CCPAID_LOG_FILE` to enable diagnostics at a specific path
- `CCPAID_QUERY_SOURCE_ALLOW` / `CCPAID_QUERY_SOURCE_DENY` as comma-separated `query_source` filters

No log file is created unless `--debug`, `CCPAID_DEBUG`, `--log-file`, or `CCPAID_LOG_FILE` is set.

Everything after `--` is passed to `claude`.

Interactive runs use terminal prompts for API-key setup, customer selection, and optional custom metadata. Non-interactive runs should set `PAID_API_KEY` and are expected to fail instead of prompting when customer selection is required.
