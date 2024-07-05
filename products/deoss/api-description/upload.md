## Upload a file
This interface is used to upload files to the cess system. You need to submit the file as form data and use provide the specific field. If the upload is successful, you will get the fid of the file. If you want to encrypt your file, you can specify the cipher field in the header and enter your password (the length cannot exceed 32 characters), and the system will automatically encrypt it.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/file</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Bucket           | bucket name    |
| Territory        | territory name |
| Cipher(optional) | cipher         |

_Identity signature required: yes_

**Request Body:**

The file is provided in the form.
| key  | value        |
| ---- | ------------ |
| file | file[binary] |

**Request example:**
```shell
curl -X PUT URL/file -F 'file=@test.log;type=application/octet-stream' -H "Bucket: bucket_name" -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```

## Upload an object
This interface is used to upload an object, you can write what you want to store directly in the body instead of specifying a file. If the upload is successful, you will get the fid of the object. if you want to encrypt the object, you can specify the "Cipher" field in the header of the request and enter a password (the length can not be more than 32 characters), the system will encrypt it automatically.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/object</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Bucket           | bucket name    |
| Territory        | territory name |
| Cipher(optional) | cipher         |

_Identity signature required: yes_

**Request Body:**

[content]

**Request example:**
```shell
curl -X PUT URL/object --data "[content]" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..." -H "Bucket: bucket_name" -H "Territory: territory_name"
```

