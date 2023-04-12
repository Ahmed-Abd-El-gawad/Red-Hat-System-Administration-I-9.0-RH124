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

### Examples and Notes

* Redirects standard output to the ```output.log``` file and then redirects standard error messages to the same place as standard output (```output.log```).
```bash
> output.log 2>&1
```
* redirects standard error messages to the default place for standard output (the terminal window, so **no change**) and then redirects only standard output to ```output.log```.
```bash
2>&1 > output.log
```
* ```&> output.log``` instead of ```> output.log 2>&1```
* ```&>> output.log``` instead of ```>> output.log 2>&1``` (in Bash 4 / RHEL 6 and later)
* Save a time stamp in the ```/tmp/saved-timestamp``` file for later reference.
```console
[user@host ~]$ date > /tmp/saved-timestamp
```
* Copy the last 100 lines from the ```/var/log/dmesg``` file to the ```/tmp/last-100-boot-messages``` file.
```console
[user@host ~]$ tail -n 100 /var/log/dmesg > /tmp/last-100-boot-messages
```
* Concatenate all four step files files into one in the tmp directory.
```console
[user@host ~]$ cat step1.sh step2.log step3 step4 > /tmp/all-four-steps-in-one
```
* List the home directory's hidden and regular file names and save the output to the ```my-file-names``` file.
```console
[user@host ~]$ ls -a > my-file-names
```
* Append a line to the existing ```/tmp/many-lines-of-information``` file.
```console
[user@host ~]$ echo "new line of information" >> /tmp/many-lines-of-information
```
* The next few commands generate error messages because some system directories are inaccessible to normal users. Observe the error messages redirection.
* Redirect errors from the find command to the ```/tmp/errors``` file while viewing normal command output on the terminal.
```console
[user@host ~]$ find /etc -name passwd 2> /tmp/errors
```
* Save process output to the ```/tmp/output``` file and error messages to the ```/tmp/errors``` file.
```console
[user@host ~]$ find /etc -name passwd > /tmp/output 2> /tmp/errors
```
* Save process output to the ```/tmp/output``` file and discard error messages.
```console
[user@host ~]$ find /etc -name passwd > /tmp/output 2> /dev/null
```
* Store output and generated errors together to the ```/tmp/all-message-output``` file.
```console
[user@host ~]$ find /etc -name passwd &> /tmp/all-message-output
```
* Append output and generated errors to the ```/tmp/all-message-output``` file.
```console
[user@host ~]$ find /etc -name passwd >> /tmp/all-message-output 2>&1
```

### Construct Pipelines

* Redirect the output of the ```ls``` command to the ```less``` command to display it on the terminal one screen at a time.
```console
[user@host ~]$ ls -l /usr/bin | less
```
* Redirect the output of the ```ls``` command to the ```wc -l``` command, which counts the number of received lines from ```ls``` and prints that value to the terminal.
```console
[user@host ~]$ ls | wc -l
```
* Redirect the output of the ```ls -t``` command to the ```head``` command to display the first 10 lines, with the final result redirected to the ```/tmp/first-ten-changed-files``` file.
```console
[user@host ~]$ ls -t | head -n 10 > /tmp/first-ten-changed-files
```
*  In a pipeline, ```tee``` copies its standard input to its standard output and also redirects its standard output to the files that are given as arguments to the command. If you imagine data as water that flows through a pipeline, then you can visualize ```tee``` as a "T" joint in the pipe that directs output in two directions.
```console
[user@host ~]$ ls -l | tee /tmp/saved-output | less
[user@host ~]$ ls -t | head -n 10 | tee /tmp/ten-last-changed-files
[user@host ~]$ ls -l | tee -a /tmp/append-files
```

<a name="5.3"></a>
## 5.3 Edit Text Files from the Shell Prompt



<a name="5.5"></a>
## 5.5 Change the Shell Environment


