## Delete a bucket
This interface is used to delete an empty Bucket.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>DELETE</b></span> &nbsp; <b>/bucket/{bucket_name}</b>

**Request Header:**
_Identity signature required: yes_

**Request example:**
```shell
# curl -X DELETE URL/bucket/<bucket_name> -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```