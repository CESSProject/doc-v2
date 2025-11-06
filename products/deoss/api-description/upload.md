## Upload a file
This interface is used to upload files to the cess system. You need to submit the file as form data and use provide the specific field. If the upload is successful, you will get the fid of the file. If you want to encrypt your file, you can specify the cipher field in the header and enter your password (the length cannot exceed 32 characters), and the system will automatically encrypt it.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/file</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
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
curl -X PUT URL/file -F 'file=@test.log' -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```

## Upload multiple files
This interface is similar to the ```/file``` interface, which can accept multiple form files at the same time.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/files</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Territory        | territory name |
| Cipher(optional) | cipher         |

_Identity signature required: yes_

**Request Body:**

The file is provided in the form.
| key  | value        |
| ---- | ------------ |
| file | file[binary] |
| ......              |

**Request example:**
```shell
curl -X PUT URL/files -F 'file=@test.log' -F 'file=@test1.log' -F 'file=@test2.log' -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```

## Upload an object
This interface is used to upload an object, you can write what you want to store directly in the body instead of specifying a file. If the upload is successful, you will get the fid of the object. if you want to encrypt the object, you can specify the "Cipher" field in the header of the request and enter a password (the length can not be more than 32 characters), the system will encrypt it automatically.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>PUT</b></span> &nbsp; <b>/object/<object_name></b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Territory        | territory name |
| Cipher(optional) | cipher         |

_Identity signature required: yes_

**Request Body:**

[content]

**Request example:**
```shell
curl -X PUT URL/object/<object_name> --data "[content]" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..." -H "Territory: territory_name"
```

## Specify miner storage
The gateway supports you to store files on the specified miners. When the number of miners you specify is greater than 12, the gateway will randomly store them in the 12 miners you specify. When the number of miners you specify is less than 12, the gateway will store them in the miners you specify first, and then randomly store them in other miners.

The method to specify the storage miner is to specify it by filling in the header of `Miner` in the upload interface. All upload interfaces support it.

The example is as follows:
```shell
curl -X PUT URL/file -F 'file=@test.log;type=application/octet-stream' -H "Miner: cX..." -H "Miner: cX..." -H "Miner: cX..." -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```

## Specify coordinate range storage
The gateway supports you to set a batch of longitude and latitude coordinate ranges for storage. The number of coordinates cannot be less than 3. The file will be randomly stored in the coordinate range you specify. Set a coordinate point by setting the Latitude and Longitude pair in the request header. Note: Please set the Latitude and Longitude coordinate points in order. All upload interfaces support it.

The example is as follows:
```shell
curl -X PUT URL/file -F 'file=@test.log;type=application/octet-stream' -H "Latitude: 0.0" -H "Longitude: 0.0" -H "Latitude: 3.0" -H "Longitude: 3.0" -H "Latitude: -3.0" -H "Longitude: -3.0" -H "Territory: territory_name" -H "Account: cX..." -H "Message: ..." -H "Signature: 0x..."
```