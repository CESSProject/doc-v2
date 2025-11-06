This section focuses on the use of some of the tools.

The list of interfaces is as follows:
| Method Name | Description |
| ----------- | ----------- |
| [BytesToFileHash](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L23) | Convert byte arrays to FileHash |
| [BytesToWorkPublickey](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L34) | Convert byte arrays to WorkPublickey |
| [BytesToPoISKeyInfo](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L45) | Convert byte arrays to PoISKeyInfo |
| [BytesToBloomFilter](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L56) | Convert byte arrays to BloomFilter |
| [RrscAppPublicToByte](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L76) | Convert RRSC public key to bytes |
| [H160ToSS58](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L91) | Converting Ether addresses to polka addresses |
| [IsWorkerPublicKeyAllZero](https://github.com/CESSProject/cess-go-sdk/blob/main/chain/util.go#L67) | Determine if the WorkerPublicKey is 0 |
| [ParsingPublickey](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/account.go#L23) | Parse the public key of the polka account |
| [EncodePublicKeyAsSubstrateAccount](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/account.go#L44) | Encoding the public key into the original polka account |
| [EncodePublicKeyAsCessAccount](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/account.go#L44) | Encoding the public key as a CESS account |
| [VerityAddress](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/account.go#L81) | Verify that the account is legitimate |
| [GetDirFreeSpace](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/cpu.go#L16) | Get the free space of a directory |
| [GetSysMemAvailable](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/cpu.go#L21) | Get system memory available |
| [CalcPathSHA256](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/hash.go#L21) | Calculate the sha256 hash value of a file |
| [CalcFileSHA256](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/hash.go#L32) | Calculate the sha256 hash value of a file |
| [CalcSHA256](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/hash.go#L42) | Calculate the sha256 hash value of the data |
| [CalcMD5](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/hash.go#L56) | Calculate the MD5 value of the file |
| [CalcPathSHA256Bytes](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/hash.go#L66) | Calculate the sha256 hash value of a file |
| [CalcFileSHA256Bytes](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/hash.go#L77) | Calculate the sha256 hash value of a file |
| [FildIpv4](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L27) | Find the IPv4 address from buf |
| [IsIntranetIpv4](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L32) | Determine if it is an internal IPv4 address |
| [IsIPv4](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L47) | Determine if it is an ipv4 address |
| [IsIPv6](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L53) | Determine if it is an ipv6 address |
| [IsPortInUse](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L58) | Determine if a port is occupied |
| [CheckDomain](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L70) | Check if the domain name is legal |
| [PingNode](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L126) | Get the response time of an address using the ping protocol |
| [TraceRoute](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/network.go#L143) | Number of routes and response time to get the address |
| [SignedSR25519WithMnemonic](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/sign.go#L29) | SR25519 signing with account private key |
| [ VerifySR25519WithPublickey](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/sign.go#L48) | Verify SR25519 signature with account public key |
| [ VerifyPolkadotJsHexSign](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/sign.go#L75) | Verify polkadot signature with account public key |
| [ParseEthAccFromEthSign](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/sign.go#L106) | Resolving addresses from eth signatures |
| [IsInterfaceNIL](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/utils.go#L14) | Determine if the interface is nil |
| [CompareSlice](https://github.com/CESSProject/cess-go-sdk/blob/main/utils/utils.go#L29) | Compare whether two slices are the same |