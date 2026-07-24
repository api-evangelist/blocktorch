---
name: Manage a Blocktorch managed Hardhat fork
description: >-
  Create and tear down a managed Hardhat fork instance through Blocktorch's
  Hardhat Forking API for continuous dApp development and testing against a
  forked EVM chain.
api: openapi/blocktorch-hardhat-forking-openapi.yml
operations:
- createHardhatInstance
- deleteHardhatInstance
generated: '2026-07-18'
method: generated
---

# Manage a Blocktorch managed Hardhat fork

Use Blocktorch's Hardhat Forking API to spin up and remove managed Hardhat fork
instances programmatically.

## Prerequisites
- A Blocktorch project (create one at https://beta.blocktorch.xyz).
- An API key created on the project's **Settings** page.
- Base URL: `https://c6yaznpyf4.execute-api.us-east-1.amazonaws.com/prod/api`

## Authentication
Send the API key in the `x-api-key` header on **every** request. Never hard-code
or log the key.

## Steps

1. **Create a fork** — call `createHardhatInstance`
   (`POST /hardhat/{projectId}`). Send `Content-Type: application/json` and a
   body `{ "providerUrl": "<ethereum-provider-url>" }`. On `200` capture the
   returned `instanceId`.

2. **Use the fork** for your development/testing against the forked chain.

3. **Delete the fork** — when finished, call `deleteHardhatInstance`
   (`DELETE /hardhat/{projectId}/{chainId}`) to release the instance. A `200`
   confirms deletion. Always tear forks down to avoid leaving instances running.

## Error handling
Both operations return `400` with a JSON `{ "error": "..." }` envelope on a bad
request (missing/invalid `providerUrl`, unknown project or chain id). Read the
`error` string, correct the input, and retry. Requests are **not** idempotent —
there is no idempotency-key contract, so do not blindly retry a create that may
have partially succeeded; check state first.
