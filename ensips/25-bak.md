---
title: Node Classification Metadata
description: Optional metadata to describe the organizational role of an ENS node
contributors: 
    - 1a35e1.eth
    - jkm.eth
ensip:
  created: "2025-12-15"
  status: draft
---

# ENSIP-25: Node Classification Metadata

## Abstract

This ENSIP proposes a standard format of text record to describe the role of an ENS name/subname. Two types of standardized text records are introduced, to allow labeling the role of subname nodes in a larger organizational structure, and to append context-specific metadata on each node.

## Motivation

In practice, ENS subnames are often used to represent organizational structures. For example, unique subnames are often created to represent a smart contract, or a wallet that was created for a specific purpose. This ENSIP offers a way to add metadata to each node to express, in a machine-readable way, the organizational purpose of the subname and to allow additional context-specific metadata to be attached.

## Specification

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in RFC 2119 and RFC 8174.

### Metadata Records

Metadata values for each node are added as text records attached to that node in accordance with ENSIP-5. Each metadata attribute is added as a separate text record. All text record key names MUST use the `metadata` namespace and follow the naming conventions outlined in this document. If any key names using the `metadata` namespace are present but not defined in this document, they SHOULD be ignored for the purpose of this ENSIP.

### Node Type Declaration

Every node that is intended to be a part of the organizational structure MUST include a record with the key name of `metadata.type`. The value of this record MUST match the node type ID of one of the options defined below, which will indicate how that node should be interpreted.

Any node that does not include a valid `metadata.type` record or declares an unknown type SHOULD be ignored. Any nodes that are children of an ignored node SHOULD also be ignored, even if they contain a valid `metadata.type` record.

Node types ID's are hierarchical using dot notation, allowing for subtype definitions. Any node type that has a parent type MUST follow the rules of all parent types in the hierarchy.

`metadata.[type].[subtype]...`

### Node Address Resolution

The address a node resolves to may have special meaning depending on the node type. Unless otherwise specified, every node MAY resolve to an address that is an externally owned account (EOA), multisig, contract, or other type. It SHOULD be understood that any aspects of or actions taken by a resolved address, (including signatures, transactions, asset holdings, etc) are under the authority of the entity the node describes.

Some node types may intend for the node's address to resolve to a contract address or EOA. For these node types, the node SHOULD resolve to an address of the designated type, and if it doesn't, the node SHOULD be deemed invalid.

The node type definitions below each include one of the following designations:

| Address Requirement | Description    |
|-------------|-------------|
| NONE  | This node type has no specific address requirements |
| ACCOUNT  | This node's address is expected to be an account, capable of providing signatures and receiving funds |
| CONTRACT  | This node's address is expected to point to a smart contract |

Node type definitions may also include additional notes on how the node's address should be handled, if applicable.

### Additional Metadata Declaration

Global keys as outlined in ENSIP-5 are expected to already be recognized for every node, and should be referenced in addition to what is presented in this ENSIP. A node's declared type will determine what additional metadata text records are expected to also be added to that node. The node type definition includes rules around what additional text records may or must be included, as well as if the node's resolver may or must resolve to an address or other value.

If the node type is a subtype of another type, the additional metadata values listed in the type's definition SHALL be applied to the node in addition to the additional values defined for all parent types.

//TODO: attributes that have a variable/discriminator (e.g. statement[body])

## Node Type Definitions

Below you will find definitions for all possible values for the `metadata.type` record. Each definition will include the following information:

* Type name: The identifier for this type, to be used as the value for a `metadata.type` record.
* Description: An explanation of how this type is expected to be interpreted.
* Address requirement: `required`, `optional`, or `none` to indicate if the node is expected to resolve to an Ethereum address.
* Address description [optional]: If the address requirement is `required` or `optional`, how that address should be interpreted.
* Additional attributes: A list of additional metadata attributes that can also be set for this node, including key names, descriptions, and whether the attribute is required or optional.

### Node Type List

TBD




## Backwards Compatibility

This proposal is built upon existing ENSIPs and does not affect existing ENS functionality. It introduces no breaking changes.

## Security Considerations

None.

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).

[ERC-165]: https://eips.ethereum.org/EIPS/eip-165

