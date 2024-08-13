## View bucket list
This interface is used to view the bucket list created by the user.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>GET</b></span> &nbsp; <b>/bucket</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Account          | account        |


**Request example:**
```shell
curl -X GET URL/bucket -H "Account: cX..."
```