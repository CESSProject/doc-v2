The API specification only describes the API request method and relative path. You need to run the gateway yourself or use a public gateway.

The gateway will connect the communication between the HTTP request and the blockchain, so some operations will deduct a certain amount of gas fees from the wallet specified by the gateway. 

In the request example section, the GatewayURL field needs to be replaced with the server address of the gateway.

We provide a public gateway, The information is as follows:

| name    | value               |
| ------- | ------------------- |
| Address | http://deoss-pub-gateway.cess.network/ |
| Account | cXhwBytXqrZLr1qM5NHJhCzEMckSTzNKw17ci2aHft6ETSQm9 |

**Table of Contents**
- [Prerequisites](prerequisites.md)
- [Identity Signature](identity_signature.md)
- [Upload File/Object](upload.md)
- [Chunked Upload](chunked_upload.md)
- [Download File](download.md)
- [Preview File](preview.md)
- [Delete File](delete_file.md)
- [Delete Bucket](delete_bucket.md)
- [View File Metadata](metadata.md)
- [View Bucket](view_bucket.md)
- [View Bucket List](view_bucket_list.md)
- [View Version](view_version.md)

:warning: You can use it for testing, but it is not recommended to use it for production environments.