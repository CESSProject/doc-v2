## Preview a file
This interface is used to preview a file, it has two prerequisites: one is that the file is not encrypted, and the other is that the file format supports preview.

**HTTP Interface:**

<span style="background-color: red; padding: 10px;"><b>GET</b></span> &nbsp; <b>/file/open/<fid></b>

**Request Header:**
| key               | description        | example |
| ----------------- | ------------------ | ------- |
| Account(optional) | file owner account | cX...   |
| Format(optional)  | file format        | png     |

If you fill in the `Account`, the file will be previewed in the file type set by the account. If you fill in the `Format`, it will be previewed in the specified format, account will not work at this time. If you do not fill in either, it will be previewed in the format set by the first owner.

**Request example:**

Open in browser: URL/file/open/<fid>