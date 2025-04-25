# BioLedger Smart Contract

BioLedger is a Clarity smart contract for managing biological specimen records and access control in a secure, decentralized environment.

## Overview

BioLedger provides a secure registry for biological specimens with controlled access mechanisms. It allows administrators to manage specimen information, authorize access for scientists, and maintain a secure record of specimens with their molecular signatures, valuations, and clearance levels.

## Features

- **Specimen Registry Management**: Store and track biological specimens with detailed metadata
- **Access Control**: Manage scientist permissions with time-limited authorization
- **Clearance Levels**: Implement hierarchical access control for sensitive specimens
- **Administrator Management**: Allow transfer of administrative privileges

## Contract Structure

### Traits

- `bioledger-functionality`: Defines the core functionality for specimen information retrieval and access authorization

### Constants

- Error codes for various error conditions:
  - `ERR-PERMISSION-DENIED` (u100): User lacks required permissions
  - `ERR-INCORRECT-VALUATION` (u101): Incorrect specimen valuation
  - `ERR-SPECIMEN-MISSING` (u102): Requested specimen doesn't exist
  - `ERR-INADEQUATE-FUNDS` (u103): Insufficient funds for operation
  - `ERR-INVALID-SUCCESSOR` (u104): Invalid administrator successor
  - `ERR-INVALID-SCIENTIST` (u105): Invalid scientist principal
  - `ERR-INVALID-SPECIMEN-ID` (u106): Invalid specimen identifier
  - `ERR-INVALID-CLEARANCE-LEVEL` (u107): Invalid clearance level

### Data Maps

- `specimen-registry`: Stores specimen records with the following data:
  - `custodian`: Principal responsible for the specimen
  - `valuation`: Specimen's value
  - `origin-contract`: Source contract principal
  - `genome-id`: Associated genome identifier
  - `enabled`: Active status flag
  - `clearance-level`: Required security clearance
  - `molecular-signature`: Unique molecular hash (32-byte buffer)

- `specimen-permissions`: Tracks access permissions with:
  - `grant-timestamp`: When access was granted
  - `expiration-date`: When access expires
  - `clearance-level`: Authorized clearance level

### Public Functions

- `designate-administrator`: Transfer administrative control to another principal
- `retrieve-specimen-information`: Get specimen metadata
- `confirm-specimen-authorization`: Verify if a scientist has valid access to a specimen
- `authorize-specimen-access`: Grant specimen access to a scientist with specified clearance

### Read-Only Functions

- `validate-specimen-id`: Check if a specimen ID exists in the registry

## Usage Examples

### Retrieving Specimen Information

```clarity
(contract-call? .bioledger retrieve-specimen-information u123)
```

### Authorizing Access

```clarity
;; Grant scientist access to specimen #123 with clearance level 3
(contract-call? .bioledger authorize-specimen-access u123 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM u3)
```

### Confirming Authorization

```clarity
;; Check if the specified scientist has authorization for specimen #123
(contract-call? .bioledger confirm-specimen-authorization u123 'ST1PQHQKV0RJXZFY1DGX8MNSNYVE3VGZJSRTPGZGM)
```

## Security Considerations

- Access control is time-limited with automatic expiration
- Administrator privileges are protected by permission checks
- Input validation is performed on specimen IDs and clearance levels
- Molecular signatures use 32-byte buffers for cryptographic security

## Deployment Notes

When deploying this contract:

1. The deploying address becomes the initial administrator
2. Administrative privileges can be transferred using `designate-administrator`
