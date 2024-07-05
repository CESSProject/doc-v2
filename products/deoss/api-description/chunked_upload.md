## Chunked Upload
Compared with uploading the entire file directly, resumable upload has some more parameter requirements, but has the same return result. At the same time, the uploaded file can also be encrypted.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/chunks</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Bucket           | bucket name    |
| Territory        | territory name |
| Cipher(optional) | cipher         |
| FileName         | file name or alias  |
| BlockNumber      | The number of chunks the file is to be divided into  |
| BlockIndex       | index of chunk to be uploaded, [0,BlockNumber)  |
| TotalSize        | the byte size of the file, the sum of the sizes of all chunks  |

_Identity signature required: yes_

**Request Body:**

The file is provided in the form.
| key  | value        |
| ---- | ------------ |
| file | file[binary] |

**Request example:**
```shell
curl -X PUT URL/chunks -F 'file=@test-chunk0;type=application/octet-stream' -H "Bucket: bucket_name" -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x... -H FileName: test.log -H BlockNumber: 5 -H BlockIndex: 0 -H TotalSize: 1000"
```