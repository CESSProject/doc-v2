# @cessnetwork/util

Utility functions for CESS SDK

## Installation

```bash
npm install @cessnetwork/util
```

## Overview

The `@cessnetwork/util` package provides a comprehensive set of utility functions for working with the CESS (Cumulus Encrypted Storage System) network. This package includes utilities for:

- Account management and address validation
- Cryptographic operations and signing
- File hashing and integrity verification
- Network operations
- Data formatting and conversion
- Error handling

## Core Modules

### Account Utilities
Functions for managing CESS accounts, validating addresses, and converting between different formats.

```typescript
import { 
  parsePublicKey, 
  validateAddress, 
  convertAddressFormat,
  encodePublicKeyAsTestNetAddr,
  encodePublicKeyAsMainNetAddr,
  convertAddrToMainNet,
  convertAddrToTestNet,
  evmAddrToCessAddr
} from '@cessnetwork/util';

// Parse a public key from an address
const publicKey = parsePublicKey('cXabc123...');
console.log(publicKey);

// Validate an address
const isValid = validateAddress('cXabc123...');
console.log(isValid);

// Validate an address with specific SS58 format
const isValidTestnet = validateAddress('cXabc123...', 11330);
console.log(isValidTestnet);

// Convert address format
const converted = convertAddressFormat('cXabc123...', 11331);
console.log(converted);

// Convert testnet address to mainnet
const mainnetAddr = convertAddrToMainNet('cXabc123...');
console.log(mainnetAddr);

// Convert mainnet address to testnet
const testnetAddr = convertAddrToTestNet('ceabc123...');
console.log(testnetAddr);

// Convert Ethereum address to CESS address
const cessAddr = evmAddrToCessAddr('0x742d35Cc6634C0532925a3b844Bc454e4438f44e', 11331);
console.log(cessAddr);
```

### Signing Utilities
Functions for signing messages and verifying signatures using mnemonics and cryptographic keys.

```typescript
import { 
  sr25519SignWithMnemonic, 
  verifySignature,
  safeSignUnixTime, 
  parseEvmAccFromEvmSign,
  getWrappedMsg
} from '@cessnetwork/util';

// Sign a message with mnemonic using SR25519
const mnemonic = 'your twelve word mnemonic phrase here';
const signature = sr25519SignWithMnemonic(mnemonic, 'Hello, CESS!');
console.log(signature);

// Verify a signature
const isValid = verifySignature('Hello, CESS!', signature, 'ceabc123...');
console.log(isValid);

// Sign current unix time for gateway requests (as used in gateway operations)
const unixTime = Date.now().toString();
const wrappedMsg = getWrappedMsg(unixTime);
const gatewaySignature = safeSignUnixTime(unixTime, mnemonic);
console.log(gatewaySignature);

// Parse Ethereum account from Ethereum signature
const ethAddress = parseEvmAccFromEvmSign('Hello, CESS!', '0x...signature...');
console.log(ethAddress);
```

### Hash Utilities
Functions for calculating file and data hashes.

```typescript
import { calcSHA256, calcPathSHA256 } from '@cessnetwork/util';

// Calculate SHA256 hash of a buffer
const buffer = Buffer.from('Hello, CESS!');
const hash = calcSHA256(buffer);
console.log(hash);

// Calculate SHA256 hash of a file
const fileHash = await calcPathSHA256('/path/to/file');
console.log(fileHash);
```

## API Reference

### Account Functions
- `parsePublicKey(address: string): Uint8Array` - Parse public key from address
- `encodePublicKeyAsTestNetAddr(publicKey: Uint8Array): string` - Encode public key as testnet address (ss58Format: 11330)
- `encodePublicKeyAsMainNetAddr(publicKey: Uint8Array): string` - Encode public key as mainnet address (ss58Format: 11331)
- `validateAddress(address: string, ss58Format?: number): boolean` - Validate address with optional ss58Format check
- `convertAddressFormat(address: string, targetSs58Format: number): string` - Convert address to specified ss58Format
- `convertAddrToMainNet(address: string): string` - Convert address from ss58Format 11330 to 11331 (cX... -> ce...)
- `convertAddrToTestNet(address: string): string` - Convert address from ss58Format 11331 to 11330 (ce... -> cX...)
- `evmAddrToCessAddr(evmAddr: string, targetSs58Format: number): string` - Convert Ethereum address to CESS address with specified ss58Format

### Signing Functions
- `safeSignUnixTime(unixTime: string | number, mnemonic: string): Uint8Array` - Sign a current unix time with a mnemonic using SR25519
- `getWrappedMsg(unixTime: string | number): Uint8Array` - Wrap a unix time message with <Bytes> tags
- `sr25519SignWithMnemonic(mnemonic: string, msg: string): Uint8Array` - Sign a message with a mnemonic using SR25519
- `verifySignature(msg: string | Uint8Array, sign: Uint8Array, account: string): boolean` - Verify a signature with a public key
- `parseEvmAccFromEvmSign(message: string, sign: string): string` - Parse Ethereum account from Ethereum signature

### Hash Functions
- `calcSHA256(data: Buffer): string` - Calculate SHA256 hash of buffer data
- `calcPathSHA256(filePath: string): Promise<string>` - Calculate SHA256 hash of a file
- `calcFileSHA256(stream: NodeJS.ReadableStream): Promise<string>` - Calculate SHA256 hash of a stream
- `calcMD5(data: string): string` - Calculate MD5 hash of string data
- `calcPathSHA256Bytes(filePath: string): Promise<Buffer>` - Calculate SHA256 hash as Buffer
- `calcFileSHA256Bytes(stream: NodeJS.ReadableStream): Promise<Buffer>` - Calculate SHA256 hash of stream as Buffer

### File Functions
- `getFileSize(filePath: string): number` - Get the size of a file
- `readChunk(filePath: string, start: number, end: number): Promise<Buffer>` - Read a chunk of a file
- `ensureDir(dirPath: string): void` - Ensure directory exists

### Network Functions
- `checkConnectivity(host: string, port: number): Promise<boolean>` - Check network connectivity
- `pingHost(host: string): Promise<PingResponse>` - Ping a host

### String Functions
- `toBase64(data: string): string` - Convert string to base64
- `fromBase64(data: string): string` - Convert base64 to string
- `toHex(data: string): string` - Convert string to hex
- `fromHex(data: string): string` - Convert hex to string

### Time Functions
- `formatTimestamp(timestamp: number): string` - Format timestamp to readable string
- `getCurrentTimestamp(): number` - Get current timestamp

### Error Handling
- `handleError(error: any): CESSUtilError` - Standard error handling
- `isCESSUtilError(error: any): boolean` - Check if error is a CESSUtilError

### Logger
- `createLogger(name: string): Logger` - Create a named logger
- `setLogLevel(level: string): void` - Set global log level

## Gateway Utilities

The utility functions play a critical role in gateway operations, particularly for authentication and communication with the CESS storage network.

### Gateway Authentication Functions
When interacting with CESS gateways, you need specific utilities to sign messages and authenticate:

```typescript
import { getWrappedMsg, safeSignUnixTime } from '@cessnetwork/util';

// Example: Gateway authentication preparation
async function prepareGatewayAuthentication() {
  const mnemonic = process.env.CESS_PRIVATE_KEY || 'your twelve word mnemonic phrase here';
  
  // Step 1: Create a timestamp message to sign
  const sign_message = Date.now().toString();
  
  // Step 2: Wrap the message with <Bytes> tags for proper signature format
  const wrappedMsg = getWrappedMsg(sign_message);
  
  // Step 3: Sign the timestamp with your mnemonic
  const signature = safeSignUnixTime(sign_message, mnemonic);
  
  console.log('Timestamp message:', sign_message);
  console.log('Wrapped message for signing:', wrappedMsg);
  console.log('Signature:', signature);
  
  // The signature and message are then used to obtain a token from the gateway
  // as demonstrated in the @cessnetwork/api gateway operations
}

prepareGatewayAuthentication();
```

## Documentation

For more detailed documentation, please visit [CESS official documentation](https://doc.cess.network/developer/cess-sdk/javascript-sdk).

## License

This project is licensed under the Apache-2.0 License.
