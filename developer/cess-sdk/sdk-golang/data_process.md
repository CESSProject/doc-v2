This section introduces the process of uploading files to the CESS network, how to ensure multiple copies of files, and how to recover files that have lost some data.

The data processing is divided into 3 steps in total:
Step 1: Padding the data
Step 2: Sequential cut of the data
Step 3: Redundancy calculation is performed on the data
Step 4: Calculate the Merkel hash of the data

## Data Padding
Data padding is in order to unify the size of the data, but also for subsequent processing can be obtained in the form of uniform data specifications, at present, the CESS in accordance with the smallest integer multiples of 32MiB file padding, padding content is '0' (ASCII value is 48). Therefore, controlling the size of uploaded files to an integer multiple of 32MiB is the most effective use of space.

Example:
For a 1MiB file: it willbe padded to 32MiB
For 33MiB file: it will be padded to 64MiB
For a 500MiB file: it will be padded to 512MiB
And so on...

## Data Cutting
The data cut is a sequential cut of the data according to the 32MiB size, and since the data must be an integer multiple of 32MiB after padding, the number of chunks after the cut is also an integer. Due to the different sizes of the files, the final number of blocks after cutting is also not fixed. We call the cut block `segment`.

## Data Redundancy
Data redundancy is the redundancy calculation of the filled data, the redundant part of the data will be obtained, according to the amount of redundancy to determine the final size of the occupied space, CESS currently provides 2x the redundancy, plus 1x the original data, which is equivalent to 1 copy of the data occupies 3x space. The advantage of this multiple redundancy is that each redundant data is different, the redundant data and the body data are also different, so as to avoid exogenous attacks, this way allows the loss of any two data (body or redundancy can be) will be able to recover the original data.

CESS chooses 2x redundancy, 4 data slices and 8 redundancy slices, the slices are uniformly called `fragment`, see [config.go](https://github.com/CESSProject/cess-go-sdk/blob/main/config/config.go#L18-L19). 

CESS redundancy algorithm adopts the [Reed-Solomon](https://en.wikipedia.org/wiki/Reed%E2%80%93Solomon_error_correction), the specific implementation of the reference [klauspost/reedsolomon](github.com/klauspost/reedsolomon). See [rs.go](https://github.com/CESSProject/cess-go-sdk/blob/main/core/erasure/rs.go) for the implementation in sdk.

## Merkle Hash
A file after padded, cut, redundant processing, will get a batch of segments and corresponding fragments, the number of segments based on the size of the file, and the number of fragments for each segment is a fixed number of 12, all the segments as a Merkle tree, the calculation of the root hash of the Merkle tree as the file's unique identifier, which we call `fid`.

Please refer to [hashtree.go](https://github.com/CESSProject/cess-go-sdk/blob/main/core/hashtree/hashtree.go) for the implementation of calculation merkle hash.