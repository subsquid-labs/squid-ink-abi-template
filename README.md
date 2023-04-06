[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/subsquid-labs/squid-ink-abi-template)


# Ink! ABI squid template

An experimental template is used to generate a squid that indexes Ink! events of choice from a contract address.

## Quickstart

0. Install @subsquid/cli a.k.a. the sqd command globally

```bash
npm i -g @subsquid/cli
```

1. Retrieve the template and install the dependencies

```bash
sqd init my-abi-squid --template https://github.com/subsquid-labs/squid-ink-abi-template/
cd my-abi-squid
npm i
```

2. Run sqd generate with the appropriate flags

```bash
Usage: sqd generate [options]

Options:
  --address <contract>   Contract address.
  --name <name>          Contract name.
  --archive <alias|url>  Source Squid Archive for a network that supports 'contracts' pallet. Can be a URL or an alias defined by @subsquid/archive-registry.
  --abi <path>           Path to the contract JSON ABI.
  -e, --event <name...>  One or multiple contract events to be indexed. '*' indexes all events defined in the ABI. (default: [])
  --from <block>         (optional) Start indexing from the given block.
  --to <block>           (optional) End indexing on the given block.
  -h, --help             display help for command
```

3. Build and run the squid

```bash
sqd build
sqd up
sqd migration:generate
sqd process
```

The indexing will start.

In a separate window, start the GraphQL API server at `localhost:4350/graphql`:

```bash
sqd serve
```

4. Inspect schema.graphql, src/processor.ts and start hacking!

For more details on how to build and deploy a squid, see the docs.

## Example
### Generate
```bash
npx squid-gen-abi \
  --address 0x5207202c27b646ceeb294ce516d4334edafbd771f869215cb070ba51dd7e2c72 \
  --archive shibuya \
  --abi ./abi/erc20.json \
  --event '*'
```
### Explore
```gql
query MyQuery {
  events(orderBy: blockNumber_ASC) {
    blockNumber
    contract
    eventName
    ... on Erc20EventTransfer {
      id
      to
      value
      eventName
    }
    ... on Erc20EventApproval {
      id
      owner
      spender
      value
      eventName
    }
  }
}
```
<img width="1000" alt="Example" src="https://user-images.githubusercontent.com/27631177/226672019-7f1ae79f-27a5-445c-a708-e2470ed64908.png">
