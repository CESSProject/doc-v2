User data uploaded to the CESS network undergo the following lifecycle. We will cover them in-depth here.

* [**Data Upload**](upload.md): Through CESS clients or gateways, users upload data to the rented storage space. CESS uses erasure codes to store the data on multiple storage nodes with 1.5 times redundancy.

* [**Data Download**](download.md): Data on CESS is available for download. The file meta information is saved on-chain. Users can use a client or gateway to retrieve the data locally.

* [**Data Deletion**](delete.md): CESS node deletes the data upon their owner requests and intelligently handles replacing active data back with idle data.

* [**Data Restore**](restore.md): CESS ensures the integrity of user data through a storage challenge mechanism. When a data block is found to be lost, CESS immediately repairs the data block.

* [**Data Audit**](audit.md): CESS has developed a series of audit algorithms and reward mechanisms to ensure the data stored in the storage nodes is honestly and objectively served.
