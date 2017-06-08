# CardScan4Linux
This script can be used to locally search through stored files for any Credit/Debit card details. It is portable and requires no additional Python (built with 2.7 in mind) libraries to operate.

## Basic Usage 
`cardscan4linux.py [-h] [-o] [-D DEPTH] [-d MINDEPTH] [-l LINES] [-p PATH] -e EXTENSIONS [EXTENSIONS ...] [-x EXCLUDE_DIR [EXCLUDE_DIR ...]] [-max MAXSIZE] [-min MINSIZE] [-mount] [-v]`

## Scan Depth
The `-d` and `-D` command flags are used to specify the minimum scan depth, and also the maximum scan depth. This is useful for instances where too many symlinked directories result in `find` errors.

* Default Min: 0
* Default Max: 3

## Remote Scanning via Mounting
By mounting a remote file system to the local (i.e. where the script will be run) Linux system you can effectively scan the remote host by using the `-mount` command flag when running the tool. By default remote mounted systems are not scanned.

* Default: False (Off)

## Excluding Directories
It is possible to exclude certain directories from being scanned by using the `-x/--exclude` command flag when running the script. Multiple directories can be excluded, which includes the use of wildcards using the asterisk character `*`. An example is as follows: `-x /var */adam/* /tmp`. 

Note: It is not neccessary to include wildcards, however if you are using a child-directory as the exclusions then the wildcards will be necessary either side of the forward slashes.

* Default: NONE

## Min Size / Max Size
The `-min/--max-size` and `-max/--max-size` command flags are used when performing the file discovery. Specifically each are used to set the minimum and maximum file sizes, respectively, of the files that will be audited for payment card-data.

Sizes are denoted within 'bytes', c = bytes, k = Kilobytes, M = Megabytes, G = Gigabytes

* Default Min: 16c
* Default Max: 100k

## Extensions
To specify the targeted file-extensions use the `-e/--extensions` command flag, with one or more extension types separated by spaces. For example: `-e txt doc xlsx csv`. There is no limit to the amount of extensions that can be used for the search, however bear in mind that the more extensions that are specified then the longer the scan could possibly take to complete. Be weary whilst scaning file-storage servers.

* Default: NONE

## Max Number of Lines to Audit
By default the maximum number of lines that will be audited within each file will be '50'. Specifying more will perform a more thorough scan for card data, but more resources will be used and the total scan time will increase. Example: `-l 250`

* Default: 50

## Optional Arguments:
`-h, --help`          Show this help message and exit
<br>  `-o, --output `       Output data to a file instead of the Terminal.
<br>  `-D DEPTH, --max-depth DEPTH` Enter the max depth that the scanner will search from the given directory (Default is 3).
<br>  `-d MINDEPTH, --min-depth MINDEPTH` Enter the min depth that the scanner will search from the given directory (No Default).
<br>  `-l LINES, --lines LINES` Enter the number of lines from the file to cycle through (Default is 50)
<br>  `-p PATH, --path PATH`  Input the directory path that you want to recursively search through, e.g. /var (Default is /)
<br>  `-e EXTENSIONS [EXTENSIONS ...], --extensions EXTENSIONS [EXTENSIONS ...]` Input the file extensions that should be searched for, separated by spaces.
<br>  `-x EXCLUDE_DIR [EXCLUDE_DIR ...], --exclude EXCLUDE_DIR [EXCLUDE_DIR ...]` Input the directories to exclude, separated by spaces. Wildcards can be used, e.g. /var/*
<br>  `-max MAXSIZE, --max-size MAXSIZE` Enter the maximum file-size to search for (Default 100 Kilobytes). Units: "c" for bytes, "k" for Kilobytes, "M" for Megabytes
<br>  `-min MINSIZE, --min-size MINSIZE` Enter the minimum file-size to search for (Default 16 Bytes). Units: "c" for bytes, "k" for Kilobytes, "M" for Megabytes
<br>  `-mount, --scan-mount`  Enable to scan the mounted remote file systems (Default is off.)
<br>  `-v, --verbose`         Display verbose messages (Warning: output can be huge).

## Example Output
```
[root@sc ~]# ./cardscan4linux.py -e txt -d 8
===================================
[ Root Path ]______________/
[ Max Size ]_______________100k
[ Min Size ]_______________16c
[ Extensions ]_____________['txt']
[ Lines per file ]_________50
[ Depth of search ]________8
[ Scan Mounted Dirs ]______False
[ Exclusions ]_____________/var
===================================

[*] Starting file-system scan. This may take a while...
[*] File-system search complete. 24855 files to check for card-data.
File: /home/carddetails.txt
 AMEX:    371449635398431
 AMEX:    371427352388125
File: /root/test.txt
 AMEX:    378282246310005
 AMEX:    371449635398431
 AMEX:    378734493671000
 MASTERCARD:  5105105105105100
 VISA:    4111111111111111
 VISA:    4012888888881881
 MASTERCARD:  5522131028402823

[*] Card scanning complete. 24855 total files were scanned in 8 seconds.
```

# To do

* Create some sort of progress bar for the 'find' subprocess
* Party
