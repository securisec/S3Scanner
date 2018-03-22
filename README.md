##Forked

Forked, and updated the code to use a custom parsing scheme so as to not use a file for urls. Also some additional changes.

Things changed:
- Use parsel and crt to grab `https` domains. This does not work for `http`, but safe to assume that aws s3 buckets will not be over http.
- added switch to read urls from file

## New usage example
```
python s3scanner.py example.com
```

# S3Scanner
[![License: CC BY-NC-SA 4.0]
A tool to find open S3 buckets and dump their contents :droplet:

![1 - s3finder.py](https://user-images.githubusercontent.com/3712226/37576404-34e02eee-2afa-11e8-8d18-bf2a63885c82.png)

## Using

<pre>
#  s3scanner - Find S3 buckets and dump!
#
#  Author: Dan Salmon - @bltjetpack, github.com/sa7mon

positional arguments:
  buckets                Name of text file containing buckets to check

optional arguments:
  -h, --help              show this help message and exit
  -o, --out-file OUTFILE  Name of file to save the successfully checked buckets in (Default: buckets.txt)
  -d, --dump              Dump all found open buckets locally
  -l, --list              List all found open buckets locally
</pre>

The tool takes in a list of bucket names to check. Found S3 buckets are output to file. The tool will also dump or list the contents of 'open' buckets locally.

### Interpreting Results

This tool will attempt to get all available information about a bucket, but it's up to you to interpret the results.

[Settings available](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/set-bucket-permissions.html) for buckets:
* Object Access (object in this case refers to files stored in the bucket)
  * List Objects
  * Write Objects
* ACL Access
  * Read Permissions
  * Write Permissions
  
Any or all of these permissions can be set for the 2 main user groups:
* Authenticated Users
* Public Users (those without AWS credentials set)
* (They can also be applied to specific users, but that's out of scope)
  
**What this means:** Just because a bucket returns "AccessDenied" for it's ACLs doesn't mean you can't read/write to it.
Conversely, you may be able to list ACLs but not read/write to the bucket


## Installation
  1. (Optional) `virtualenv venv && source ./venv/bin/activate`
  2. `pip install -r requirements.txt`
  3. `python ./s3scanner.py`

(Compatibility has been tested with Python 2.7 and 3.6)


## Examples
This tool accepts the following type of bucket formats to check:

- bucket name - `google-dev`
- domain name - `uber.com`, `sub.domain.com`
- full s3 url - `yahoo-staging.s3-us-west-2.amazonaws.com` (To easily combine with other tools like [bucket-stream](https://github.com/eth0izzle/bucket-stream))
- bucket:region - `flaws.cloud:us-west-2`

```bash
> cat names.txt
flaws.cloud
google-dev
testing.microsoft.com
yelp-production.s3-us-west-1.amazonaws.com
github-dev:us-east-1
```
	
1. Dump all open buckets, log both open and closed buckets to found.txt
	
	```bash
	> python ./s3scanner.py --include-closed --out-file found.txt --dump names.txt
	```
2. Just log open buckets to the default output file (buckets.txt)

	```bash
	> python ./s3scanner.py names.txt
	```
3. Save file listings of all open buckets to file
    ```bash
    > python ./s3scanner.py --list names.txt

    ```

## Contributing
Issues are welcome and Pull Requests are appreciated. All contributions should be compatible with both Python 2.7 and 3.6.

|    master    |    [![Build Status](https://travis-ci.org/sa7mon/S3Scanner.svg?branch=master)](https://travis-ci.org/sa7mon/S3Scanner)    |
|:------------:|:-------------------------------------------------------------------------------------------------------------------------:|
| enhancements | [![Build Status](https://travis-ci.org/sa7mon/S3Scanner.svg?branch=enhancements)](https://travis-ci.org/sa7mon/S3Scanner) |
|     bugs     |     [![Build Status](https://travis-ci.org/sa7mon/S3Scanner.svg?branch=bugs)](https://travis-ci.org/sa7mon/S3Scanner)     |

### Testing
* All test are currently in `test_scanner.py`
* Run tests with in 2.7 and 3.6 virtual environments.
* This project uses **pytest-xdist** to run tests. Use `pytest -n NUM` where num is number of parallel processes.
* Run individual tests like this: `pytest -q -s test_scanner.py::test_namehere`

## License
Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International [(CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)
