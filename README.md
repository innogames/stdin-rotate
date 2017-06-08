# stdin-rotate

`stdin-rotate` is a small tool that redirects its `stdin` to an output file, rotating and compressing it as specified by the configuration flags.

## Why not logrotate

`logrotate` cannot handle high traffic very well. Its file backup suffix does not contain the time component, only the date (in other words, it's `-yyyymmdd`). In addition to that, backup file creation fails when the target file already exists. This 2 facts combined make `logrotate` to fail if a file needs to be rotated more than once a day. 

For example, when rotating the file `application.log` at 2017.06.01, the first time a backup file whose name is `application.log-20170601` gets created. The second time the backup file already exists, so the original file (`application.log`) cannot be rotated anymore, growing until the disk is full.

In other words, `daily` is the highest log rotation rate that works, which is clearly insufficient for the logging rate of many modern applications, that can fill up the disk space in a matter of hours.

## Usage

Here is a sample usage:
```sh
./application-bin | stdin-rotate -output my-application.log -max-files 10 -max-size $((5 * 1024 * 1024))
```

Call `stdin-rotate -h` to see all the flags.
