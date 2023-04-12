# Chapter 5. Create, View, and Edit Text Files

| content |
| --- |
| [5.1 Redirect Output to a File or Program](#5.1) |
| [5.3 Edit Text Files from the Shell Prompt](#5.3) |
| [5.5 Change the Shell Environment](#5.5) |


<a name="5.1"></a>
## 5.1 Redirect Output to a File or Program

### Channels (File Descriptors)
| Number | Channel name | Description	| Default connection | Usage |
| --- | --- | --- | --- | --- |
| 0 |	stdin	| Standard | input | Keyboard	| read only |
| 1 |	stdout | Standard | output | Terminal	| write only |
| 2 |	stderr | Standard | error	| Terminal | write only |
| 3+ | filename	| Other | files	| none | read, write, or both |

### Output Redirection Operators
| Usage |	Explanation | Visual aid |
| --- | --- | --- |
| ```> file``` | Redirect ```stdout``` to overwrite a file. | ![combine-overwrite](https://user-images.githubusercontent.com/87609369/231502630-4aad8949-c5e4-453f-a526-ec2724bce6b2.svg) |


<a name="5.3"></a>
## 5.3 Edit Text Files from the Shell Prompt



<a name="5.5"></a>
## 5.5 Change the Shell Environment


