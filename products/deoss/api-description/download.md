## Download a file
This interface is used to download a file with a specified fid. If you encrypted the file when you uploaded it, you also need to tell the gateway your cipher to decrypt your file.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>GET</b></span> &nbsp; <b>/download/{fid}</b>

**Request Header:**
| key              | description    |
| ---------------- | -------------- |
| Cipher(optional) | cipher         |

**Request example:**
```shell
# curl -X GET -o <save_file> URL/download/<fid>
```