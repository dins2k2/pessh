# pessh

pessh (Parallel Execution with SSH) is a robust and efficient SSH client toolkit designed for seamless execution of commands across a multitude of servers. With support for parallel execution, this toolkit enables the simultaneous processing of commands on hundreds of servers, optimizing efficiency in server management. pessh simplifies the process by utilizing username and password (encrypted via encpass.sh) authentication, making it a versatile and user-friendly solution for executing commands across distributed server environments.

## Acknowledgments

pessh toolkit utilizes code from the following external repositories:

- [passh](https://github.com/clarkwang/passh): Incorporated passh library for SSH authenticaiton and commands execution. [License](https://github.com/clarkwang/passh?tab=GPL-3.0-1-ov-file)
- [encpass.sh](https://github.com/plyint/encpass.sh): Utilized encpass.sh script for encrypted password storage and retrieval. [License](https://github.com/plyint/encpass.sh?tab=MIT-1-ov-file)

## Dependencies

- POSIX compliant shell environment
- OpenSSL

## Installation

Download the pessh toolkit from [releases](https://github.com/dins2k2/pessh/releases) and extract in your preferred directory.

_Be sure to have execute permission for ./bin/passh, ./security/encpass.sh and ./pessh scripts and other required permissions for input and output directories._

## Password Encryption

Using encpass.sh library host user password can be encrypted. encapss.sh library is already kept in the ./security directory. Be sure to restrict the ./security/.encpass directory permissions where all the user defined buckets are stored.

**Add a password to an user defined bucket**
```
encpass.sh add -f <bucket_name> secret
```
Example:
```
[server01 pesshv11]$ ./security/encpass.sh add -f personal secret
```
## Usage
```
Usage: ./pessh [options]... [arguments]...

Usage: $0 [options]... [arguments]...

Options: ( * - mandatory option with argument )

  -u <user>      *remote server user name; alternative for -s '-l <user>'
                 ignore this when -U option is used
  -U             *for running as root or sudo user
                 when the -U option is used, the -u and -e options can be ignored as they are superseded by -U 
                 assuming passwordless ssh has been setup for root/sudo user, otherwise use -u and -e options
  -e <bucket>    *encrypted password bucket name, ignore this when -U option is used
                 use ./security/encpass.sh script to add a bucket and secret 
                 to protect the remote server user password exposing to others
                 usage: ./security/encpass.sh add -f <bucket_name> secret 
                 ( be sure the secret parameter should be named as 'secret' )
                 eg: ./security/encpass.sh add -f personal secret
  -f <file>      *connect to hosts listed in file, one per line. pessh removes the duplicate host names.
  -c <file>      *commands file path (contains the set of command(s) to be executed on the remote system)
                 incude the respective (remote server) shell environment (ksh, bash, etc.) in the commands file 
  -d <directory> output directory. each host output will be saved as the <outDir>/<host>.txt file
                 ( default output dir: ./pesshv11/output )
  -r             connect rate in new SSH connections/sec ( default: 25 )
  -s <options>   pass options to SSH, eg: -s '-p 2222'
  -a             aggregates each host output and saves in a single file
  -w             aggregates each host output and writes to the console
                       
  -h             display this help and exit
  -v             display pessh version information and exit
```
## Examples
1. Only the mandatory options with arguments
```
[server01 pesshv11]$ ./pessh -u user1 -e personal -f ./input/hosts.txt -c ./input/commands.sh
```
2. Running as root or sudo user
```
[server01 pesshv11]$ ./pessh -U -f ./input/hosts.txt -c ./input/commands.sh
```
3. Aggregates the each host output as a single file
```
[server01 pesshv11]$ ./pessh -u user1 -e personal -f ./input/hosts.txt -c ./input/commands.sh -d ./output -a
```
4. Aggregates the each host output a file and writes to the console
```
[server01 pesshv11]$ ./pessh -U -f ./input/hosts.txt -c ./input/commands.sh -d ./output -a -w
```
5. Add ssh options and connection rate
```
[server01 pesshv11]$ ./pessh -u user1 -e personal -f ./input/hosts.txt -c ./input/commands.sh -s '-p 2122' -r 20
```
6. Prints pess version
```
[server01 pesshv11]$ ./pessh -v
```
7. Prints pess help
```
[server01 pesshv11]$ ./pessh -h
```
_Sample input and output files available in the input and output directories for reference._

## Issues

Issues can be created [here](https://github.com/dins2k2/pessh/issues/new/choose)
