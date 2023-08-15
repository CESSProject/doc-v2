User files uploaded to the CESS network undergo the following lifecycle. We will cover them in-depth here.

- [**File Upload**](upload.md): Through CESS clients or gateways, file owners upload the file to the purchased storage space. CESS uses erasure codes to store customer files on multiple storage nodes with 1.5 times redundancy.

- [**File Download**](download.md): Files on CESS can be downloaded by any user. The file meta information is saved on-chain. Users can use a client or gateway to retrieve the file locally.

- [**File Deletion**](delete.md): CESS node deletes user files upon their owner requests and intelligently handles replacing idle data with active data.

- [**File Restore**](restore.md): CESS ensures the integrity of user data through a storage challenge mechanism. When a data block is found to be lost, CESS immediately repairs the data block.

- [**Storage Audit**](audit.md): CESS has developed a series of audit algorithms and reward mechanisms to ensure the data stored in the storage nodes is honestly and objectively served to users.
