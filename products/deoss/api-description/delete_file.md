## Delete a file
This interface is used to delete a file.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>DELETE</b></span> &nbsp; <b>/file/<fid></b>

**Request Header:**

_Identity signature required: yes_

**Request example:**
```shell
curl -X DELETE URL/file/<fid> -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```