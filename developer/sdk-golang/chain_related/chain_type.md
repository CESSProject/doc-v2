This section describes the definition of cess chain data types.

## Configuration definition
```golang
const (
	// unit precision of CESS token
	TokenPrecision_CESS = "000000000000000000"

	// the minimum account balance required for transaction
	MinTransactionBalance = "1000000000000000000"

	// staking per TiB of space for storage miner, the unit is CESS
	StakingStakePerTiB = 4000

	// block packing interval, the unit is second
	BlockIntervalSec = 6

	// treasury account
	TreasuryAccount = "cXhT9Xh3DhrBMDmXcGeMPDmTzDm1J8vDxBtKvogV33pPshnWS"

	// number of data slices after redundancy
	DataShards   = 4

	// number of redundant slices after redundancy
	ParShards    = 8

	// minimum length of bucket name
	MinBucketNameLength = 3

	// maximum length of a bucket name
	MaxBucketNameLength = 63

	// maximum length of gateway domain names
	MaxDomainNameLength = 100

	// maximum number of segments in a file
	MaxSegmentNum = 1000

	// sdk default name
	CharacterName_Default = "cess-sdk-go"

	// offcial gateway address
	PublicGatewayAddr = "http://deoss-pub-gateway.cess.cloud/"

	// offcial gateway account
	PublicGatewayAccount = "cXhwBytXqrZLr1qM5NHJhCzEMckSTzNKw17ci2aHft6ETSQm9"
)
```

## Constant definition
```golang
const (
	FileHashLen            = 64
	RandomLen              = 20
	PeerIdPublicKeyLen     = 38
	PoISKeyLen             = 256
	TeeSignatureLen        = 256
	AccumulatorLen         = 256
	SpaceChallengeParamLen = 8
	BloomFilterLen         = 256
	WorkerPublicKeyLen     = 32
	MasterPublicKeyLen     = 32
	EcdhPublicKeyLen       = 32
	TeeSigLen              = 64
	RrscAppPublicLen       = 32
)
```

## Type definition
```golang
type FileHash [FileHashLen]types.U8
type Random [RandomLen]types.U8
type PeerId [PeerIdPublicKeyLen]types.U8
type PoISKey_G [PoISKeyLen]types.U8
type PoISKey_N [PoISKeyLen]types.U8
type TeeSignature [TeeSignatureLen]types.U8
type Accumulator [AccumulatorLen]types.U8
type SpaceChallengeParam [SpaceChallengeParamLen]types.U64
type BloomFilter [BloomFilterLen]types.U64
type WorkerPublicKey [WorkerPublicKeyLen]types.U8
type MasterPublicKey [MasterPublicKeyLen]types.U8
type EcdhPublicKey [EcdhPublicKeyLen]types.U8
type TeeSig [TeeSigLen]types.U8
type RrscAppPublic [RrscAppPublicLen]types.U8
type AppPublicType [4]types.U8
```

## Variable definition
```golang
var RrscAppPublicType = AppPublicType{'r', 'r', 's', 'c'}
var AudiAppPublicType = AppPublicType{'a', 'u', 'd', 'i'}
var GranAppPublicType = AppPublicType{'g', 'r', 'a', 'n'}
var ImonAppPublicType = AppPublicType{'i', 'm', 'o', 'n'}
```

## Struct definition
### ChallengeInfo
```golang
type ChallengeInfo struct {
	MinerSnapshot    MinerSnapShot
	ChallengeElement ChallengeElement
	ProveInfo        ProveInfo
}

type MinerSnapShot struct {
	IdleSpace          types.U128
	ServiceSpace       types.U128
	ServiceBloomFilter BloomFilter
	SpaceProofInfo     SpaceProofInfo
	TeeSig             TeeSig
}

type ChallengeElement struct {
	Start        types.U32
	IdleSlip     types.U32
	ServiceSlip  types.U32
	VerifySlip   types.U32
	SpaceParam   SpaceChallengeParam
	ServiceParam QElement
}

type ProveInfo struct {
	Assign       types.U8
	IdleProve    types.Option[IdleProveInfo]
	ServiceProve types.Option[ServiceProveInfo]
}

type QElement struct {
	Index []types.U32
	Value []Random
}

type IdleProveInfo struct {
	TeePubkey    WorkerPublicKey
	IdleProve    types.Bytes
	VerifyResult types.Option[bool]
}

type ServiceProveInfo struct {
	TeePubkey    WorkerPublicKey
	ServiceProve types.Bytes
	VerifyResult types.Option[bool]
}
```

### PoISKeyInfo
```golang
type PoISKeyInfo struct {
	G PoISKey_G
	N PoISKey_N
}
```

### SpaceProofInfo
```golang
type SpaceProofInfo struct {
	Miner       types.AccountID
	Front       types.U64
	Rear        types.U64
	PoisKey     PoISKeyInfo
	Accumulator Accumulator
}

### ConsensusRrscAppPublic
```golang
type ConsensusRrscAppPublic struct {
	Public  RrscAppPublic
	Unknown types.U64
}
```
### OssInfo
```golang
type OssInfo struct {
	Peerid PeerId
	Domain types.Bytes
}
```

### BucketInfo
```golang
type BucketInfo struct {
	FileList  []FileHash
	Authority []types.AccountID
}
```

### StorageOrder
```golang
type StorageOrder struct {
	FileSize     types.U128
	SegmentList  []SegmentList
	User         UserBrief
	CompleteList []CompleteInfo
}

type CompleteInfo struct {
	Index types.U8
	Miner types.AccountID
}
```

### SegmentList
```golang
type SegmentList struct {
	SegmentHash  FileHash
	FragmentHash []FileHash
}
```

### FileMetadata
```golang
type FileMetadata struct {
	SegmentList []SegmentInfo
	Owner       []UserBrief
	FileSize    types.U128
	Completion  types.U32
	State       types.U8
}

type FragmentInfo struct {
	Hash  FileHash
	Avail types.Bool
	Tag   types.Option[types.U32]
	Miner types.AccountID
}
```

### SegmentInfo
```golang
type SegmentInfo struct {
	Hash         FileHash
	FragmentList []FragmentInfo
}
```

### UserBrief
```golang
type UserBrief struct {
	User       types.AccountID
	FileName   types.Bytes
	BucketName types.Bytes
}
```

### RestoralOrderInfo
```golang
type RestoralOrderInfo struct {
	Count        types.U32
	Miner        types.AccountID
	OriginMiner  types.AccountID
	FragmentHash FileHash
	FileHash     FileHash
	GenBlock     types.U32
	Deadline     types.U32
}
```

### TagSigInfo
```golang
type TagSigInfo struct {
	Miner    types.AccountID
	Digest   []DigestInfo
	Filehash FileHash
}
```

### SchedulerCredit
```golang
type SchedulerCounterEntry struct {
	ProceedBlockSize uint64
	PunishmentCount  uint32
}
```

### ExpendersInfo
```golang
type ExpendersInfo struct {
	K types.U64
	N types.U64
	D types.U64
}
```

### MinerInfo
```golang
type MinerInfo struct {
	BeneficiaryAccount types.AccountID
	StakingAccount     types.AccountID
	PeerId             PeerId
	Collaterals        types.U128
	Debt               types.U128
	State              types.Bytes // positive, exit, frozen, lock
	DeclarationSpace   types.U128
	IdleSpace          types.U128
	ServiceSpace       types.U128
	LockSpace          types.U128
	SpaceProofInfo     types.Option[SpaceProofInfo]
	ServiceBloomFilter BloomFilter
	TeeSig             TeeSig
}
```

### MinerReward
```golang
type MinerReward struct {
	TotalReward  types.U128
	RewardIssued types.U128
	OrderList    []RewardOrder
}
```

### RestoralTargetInfo
```golang
type RestoralTargetInfo struct {
	Miner         types.AccountID
	ServiceSpace  types.U128
	RestoredSpace types.U128
	CoolingBlock  types.U32
}
```

type UserFileSliceInfo struct {
	Filehash FileHash
	Filesize types.U128
}

// Session
type KeyOwnerParam struct {
	PublicType AppPublicType
	Public     types.Bytes
}

type RewardOrder struct {
	ReceiveCount     types.U8
	MaxCount         types.U8
	Atonce           types.Bool
	OrderReward      types.U128
	EachAmount       types.U128
	LastReceiveBlock types.U32
}

// StorageHandler
type UserSpaceInfo struct {
	TotalSpace     types.U128
	UsedSpace      types.U128
	LockedSpace    types.U128
	RemainingSpace types.U128
	Start          types.U32
	Deadline       types.U32
	State          types.Bytes
}

// Staking
type StakingEraRewardPoints struct {
	Total      types.U32
	Individual []Individual
}

type Individual struct {
	Acc    types.AccountID
	Reward types.U32
}

type StakingNominations struct {
	Targets     []types.AccountID
	SubmittedIn types.U32
	Suppressed  types.Bool
}

type StakingLedger struct {
	Stash          types.AccountID
	Total          types.UCompact
	Active         types.UCompact
	Unlocking      []UnlockChunk
	ClaimedRewards []types.U32
}

type UnlockChunk struct {
	Value types.UCompact
	Era   types.BlockNumber
}

// System
type SysProperties struct {
	Ss58Format    types.Bytes
	TokenDecimals types.U8
	TokenSymbol   types.Text
	SS58Prefix    types.U32
}

type SysSyncState struct {
	StartingBlock types.U32
	CurrentBlock  types.U32
	HighestBlock  types.U32
}

// TeeWorker
type WorkerInfo struct {
	Pubkey              WorkerPublicKey
	EcdhPubkey          EcdhPublicKey
	Version             types.U32
	LastUpdated         types.U64
	StashAccount        types.Option[types.AccountID]
	AttestationProvider types.Option[types.U8]
	ConfidenceLevel     types.U8
	Features            []types.U32
	Role                types.U8 // 0:Full 1:Verifier 2:Marker
}

type IdleSignInfo struct {
	Miner              types.AccountID
	Rear               types.U64
	Front              types.U64
	Accumulator        Accumulator
	LastOperationBlock types.U32
	PoisKey            PoISKeyInfo
}


type DigestInfo struct {
	Fragment  FileHash
	TeePubkey WorkerPublicKey
}

type StakingExposure struct {
	Total  types.UCompact
	Own    types.UCompact
	Others []OtherStakingExposure
}

type OtherStakingExposure struct {
	Who   types.AccountID
	Value types.UCompact
}

type StakingValidatorPrefs struct {
	Commission types.U32
	Blocked    types.Bool
}

type CompleteSnapShotType struct {
	MinerCount types.U32
	TotalPower types.U128
}

type RoundRewardType struct {
	TotalReward types.U128
	OtherReward types.U128
}