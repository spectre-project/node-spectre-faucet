# Spectre Faucet

[![Lint Check](https://github.com/spectre-project/node-spectre-faucet/actions/workflows/lint.yml/badge.svg)](https://github.com/spectre-project/node-spectre-faucet/actions/workflows/lint.yml)

Miniature Spectre faucet website based on [Spectre Wallet](https://github.com/spectre-project/node-spectre-wallet)
framework. The faucet can be run on single or multiple networks.
In case you run it on multiple networks it will use the same
seed phrase for all together.

## Install Rust (if necessary)

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
rustc --version
```

## Setup rusty-spectre

```
git clone https://github.com/spectre-project/rusty-spectre
cd rusty-spectre
cargo build --release --bin spectred
cd target/release
```

## Run Spectred

```
spectred --utxoindex
```

## Configuration

You need to have a hCaptcha subscription. You can create a free one
at: https://www.hcaptcha.com/

Once you have created it copy the `sitekey` and `secret` into
`config/spectre-faucet.conf` file.

Lastly you need to create a file `.seeds` with the 12-word seed
phrase of a wallet to disable address derivation. This is optional
but recommended if you run the faucet on a secure and privately
owned system.

It is highly recommended to run the faucet behind a reverse proxy
like NGINX and add the following header into your proxy
configuration:

```
proxy_set_header X-Real-IP $remote_addr;
```

It will ensure that single IP addresses cannot empty the faucet and
limit their amount in the 24 hour timeframe.

## Running

```
git clone https://github.com/spectre-project/node-spectre-faucet
cd node-spectre-faucet
npm install
node spectre-faucet.js --mainnet --host 0.0.0.0 --limit 10 --rpc 127.0.0.1:18110
```

### Global Options:

- `--version`: Displays the current version of the Spectre Faucet.
- `--help`: Displays the help information for the command.

- `--log <level>`: Sets the log level for the application. Available log levels are:

  - `error`
  - `warn`
  - `info`
  - `verbose`
  - `debug`

- `--verbose`: Logs additional wallet activity for verbose output.
- `--debug`: Logs detailed wallet activity for debugging purposes.

### Network Options:

- `--mainnet`: Uses the **mainnet** network for transactions.
- `--testnet`: Uses the **testnet** network for transactions.
- `--devnet`: Uses the **devnet** network for transactions.
- `--simnet`: Uses the **simnet** network for transactions.

### Host and Port Options:

- `--host <host>`: Sets the HTTP host for the faucet (default: `localhost`).
- `--port <port>`: Sets the HTTP port for the faucet (default: `3000`). You can specify a custom port if necessary.

### SPR/Day Limit Options:

- `--limit <limit>`: Sets the SPR/day limit per IP address. This limits how much SPR can be requested by a single IP within a 24-hour period.

- `--no-limit`: Disables the SPR/day limit entirely, allowing unlimited faucet requests.

### RPC Options:

- `--rpc <address>`: Specifies a custom RPC address in the format `host:port`. This option is useful when you want to use a different RPC server.

## Example Usage:

To run the faucet with specific options:

```bash
node spectre-faucet.js --mainnet --host 0.0.0.0 --limit 10 --rpc 127.0.0.1:18110
```

This will:

- Run the faucet on the **mainnet**.
- Bind the HTTP service to `0.0.0.0` (all available network interfaces).
- Set a limit of `10 SPR` per IP address per day.
- Use a custom RPC server at `127.0.0.1:18110`.
