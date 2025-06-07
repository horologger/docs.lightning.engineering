# Release Notes
- [Bug Fixes](#bug-fixes)
- [New Features](#new-features)
    - [Functional Enhancements](#functional-enhancements)
    - [RPC Additions](#rpc-additions)
    - [lncli Additions](#lncli-additions)
- [Improvements](#improvements)
    - [Functional Updates](#functional-updates)
    - [RPC Updates](#rpc-updates)
    - [lncli Updates](#lncli-updates)
    - [Breaking Changes](#breaking-changes)
    - [Performance Improvements](#performance-improvements)
    - [Deprecations](#deprecations)
- [Technical and Architectural Updates](#technical-and-architectural-updates)
    - [BOLT Spec Updates](#bolt-spec-updates)
    - [Testing](#testing)
    - [Database](#database)
    - [Code Health](#code-health)
    - [Tooling and Documentation](#tooling-and-documentation)

# Bug Fixes

# New Features

## Functional Enhancements

## RPC Additions
* When querying [`ForwardingEvents`](https://github.com/lightningnetwork/lnd/pull/9813)
logs, the response now include the incoming and outgoing htlc indices of the payment 
circuit. The indices are only available for forwarding events saved after v0.20.


* The `lncli addinvoice --blind` command now has the option to include a
  [chained channels](https://github.com/lightningnetwork/lnd/pull/9127)
  incoming list `--blinded_path_incoming_channel_list` which gives users the 
  control of specifying the channels they prefer to receive the payment on. With
  the option to specify multiple channels this control can be extended to
  multiple hops leading to the node.

## lncli Additions

* [`lncli sendpayment` and `lncli queryroutes` now support the
  `--route_hints` flag](https://github.com/lightningnetwork/lnd/pull/9721) to
  support routing through private channels.

# Improvements
## Functional Updates

* Graph Store SQL implementation and migration project:
  * Introduce an [abstract graph 
    store](https://github.com/lightningnetwork/lnd/pull/9791) interface. 
  * Start [validating](https://github.com/lightningnetwork/lnd/pull/9787) that 
    byte blobs at the end of gossip messages are valid TLV streams.
  * Various [preparations](https://github.com/lightningnetwork/lnd/pull/9692) 
    of the graph code before the SQL implementation is added.
  * Add graph schemas, queries and CRUD:
    * [1](https://github.com/lightningnetwork/lnd/pull/9866)

## RPC Updates

## lncli Updates

## Code Health

## Breaking Changes
## Performance Improvements

## Deprecations

### ⚠️ **Warning:** The following RPCs will be removed in release version **0.21**:

| Deprecated RPC Method | REST Equivalent | HTTP Method | Path | Replaced By |
|----------------------|----------------|-------------|------------------------------|------------------|
| [`lnrpc.SendToRoute`](https://lightning.engineering/api-docs/api/lnd/lightning/send-to-route/index.html) <br> [`routerrpc.SendToRoute`](https://lightning.engineering/api-docs/api/lnd/router/send-to-route/) | ❌ (No direct REST equivalent) | — | — | [`routerrpc.SendToRouteV2`](https://lightning.engineering/api-docs/api/lnd/router/send-to-route-v2/) |
| [`lnrpc.SendPayment`](https://lightning.engineering/api-docs/api/lnd/lightning/send-payment/) <br> [`routerrpc.SendPayment`](https://lightning.engineering/api-docs/api/lnd/router/send-payment/) | ✅ | `POST` | `/v1/channels/transaction-stream` | [`routerrpc.SendPaymentV2`](https://lightning.engineering/api-docs/api/lnd/router/send-payment-v2/index.html) |
| [`lnrpc.SendToRouteSync`](https://lightning.engineering/api-docs/api/lnd/lightning/send-to-route-sync/index.html) | ✅ | `POST` | `/v1/channels/transactions/route` | [`routerrpc.SendToRouteV2`](https://lightning.engineering/api-docs/api/lnd/router/send-to-route-v2/) |
| [`lnrpc.SendPaymentSync`](https://lightning.engineering/api-docs/api/lnd/lightning/send-payment-sync/index.html) | ✅ | `POST` | `/v1/channels/transactions` | [`routerrpc.SendPaymentV2`](https://lightning.engineering/api-docs/api/lnd/router/send-payment-v2/index.html) |
| [`router.TrackPayment`](https://lightning.engineering/api-docs/api/lnd/router/track-payment/index.html) | ❌ (No direct REST equivalent) | — | — | [`routerrpc.TrackPaymentV2`](https://lightning.engineering/api-docs/api/lnd/router/track-payment-v2/) |

🚨 **Users are strongly encouraged** to transition to the new **V2 methods** before release **0.21** to ensure compatibility:

| New RPC Method | REST Equivalent | HTTP Method | Path |
|---------------|----------------|-------------|------------------------|
| [`routerrpc.SendToRouteV2`](https://lightning.engineering/api-docs/api/lnd/router/send-to-route-v2/) | ✅ | `POST` | `/v2/router/route/send` |
| [`routerrpc.SendPaymentV2`](https://lightning.engineering/api-docs/api/lnd/router/send-payment-v2/index.html) | ✅ | `POST` | `/v2/router/send` |
| [`routerrpc.TrackPaymentV2`](https://lightning.engineering/api-docs/api/lnd/router/track-payment-v2/) | ✅ | `GET` | `/v2/router/track/{payment_hash}` |

# Technical and Architectural Updates
## BOLT Spec Updates

## Testing

## Database

## Code Health

## Tooling and Documentation

# Contributors (Alphabetical Order)

* Abdulkbk
Pins
