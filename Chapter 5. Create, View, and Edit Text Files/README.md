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
| ```> file``` | Redirect ```stdout``` to overwrite a file. | ![redirection-overview](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%205.%20Create%2C%20View%2C%20and%20Edit%20Text%20Files/redirection-overview.png) |
| ```>> file``` | Redirect ```stdout``` to append to a file. | ![redirection-append](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%205.%20Create%2C%20View%2C%20and%20Edit%20Text%20Files/redirection-append.png) |
| ```2> file``` | Redirect ```stderr``` to overwrite a file. | ![redirection-error](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%205.%20Create%2C%20View%2C%20and%20Edit%20Text%20Files/redirection-error.png) |
| ```2> /dev/null``` | Discard ```stderr``` error messages by redirecting them to ```/dev/null```. | ![dev-null](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%205.%20Create%2C%20View%2C%20and%20Edit%20Text%20Files/dev-null.png) |
| ```> file 2>&1``` == ```&> file``` | Redirect ```stdout``` and ```stder```r to overwrite the same file. | ![combine-overwrite](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%205.%20Create%2C%20View%2C%20and%20Edit%20Text%20Files/combine-overwrite.png) |
| ```>> file 2>&1``` == ```&>> file``` | Redirect ```stdout``` and ```stderr``` to append to the same file. | ![combine-append](https://github.com/Ahmed-Abd-El-gawad/Red-Hat-System-Administration-I-9.0-RH124/blob/main/Chapter%205.%20Create%2C%20View%2C%20and%20Edit%20Text%20Files/combine-append.png) |

<a name="5.3"></a>
## 5.3 Edit Text Files from the Shell Prompt



<a name="5.5"></a>
## 5.5 Change the Shell Environment


