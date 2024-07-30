## View bucket
This interface is used to view bucket information, including the number of stored files and file IDs.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>GET</b></span> &nbsp; <b>/bucket</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Account          | account        |
| Bucket           | bucket name    |


**Request example:**
```shell
curl -X GET URL/bucket -H "Account: cX..." -H "Bucket: bucket_name"
```