The API specification only describes the API request method and relative path. You need to run the gateway yourself or use a public gateway.

The gateway will connect the communication between the HTTP request and the blockchain, so some operations will deduct a certain amount of gas fees from the wallet specified by the gateway. 

In the request example section, the GatewayURL field needs to be replaced with the server address of the gateway.

We provide some public gateways, their information is as follows:

| Account    | Address               |
| ---------- | --------------------- |
| cXf3X3ugTnivQA9iDRYmLNzxSqybgDtpStBjFcBZEoH33UVaz | https://deoss-sgp.cess.network |
| cXjy16zpi3kFU6ThDHeTifpwHop4YjaF3EvYipTeJSbTjmayP | https://deoss-sv.cess.network  |
| cXhkf7fFTToo8476oeRqxyWVnxF8ESsd8b7Yh258v6n26RTkL | https://deoss-fra.cess.network |

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