## Chunked Upload
This interface is used for range upload, supports breakpoint resuming, and does not support encrypted file.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/resume/<file_name></b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Territory        | territory name |
| Content-Range    | content range  |

_Identity signature required: yes_

**Request Body:**

The file is provided in the form.
| key  | value        |
| ---- | ------------ |
| file | file[binary] |

**Request example:**
```shell
curl -X PUT URL/resume/<file_name> -F 'file=@test.log' -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..." -H "Content-Range: bytes 0-201326591/445531360"
curl -X PUT URL/resume/<file_name> -F 'file=@test.log' -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..." -H "Content-Range: bytes 201326592-402653183/445531360"
curl -X PUT URL/resume/<file_name> -F 'file=@test.log' -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..." -H "Content-Range: bytes 402653184-445531359/445531360"
```