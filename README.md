# stdin-rotate

`stdin-rotate` reads lines from `stdin` and writes them into output file rotating and compressing them as specified by flags.

## Why not logrotate

`logrotate` cannot handle well high traffic.
The `logrotate` file backup suffix does not contain the time component, like `-yyyymmdd`.
If it tries to create a file `application.log-20170601` and the file already exists, `application.log` will not be rotated.
`application.log` will grow until the disk is full.
In other words, `daily` is the highest log rotation rate.

If the service logs multiple MBs per minute, there will be no free space on disk in a couple of hours.

## Usage

Here is a sample usage:
```sh
./application-bin | stdin-rotate -output my-application.log -max-files 10 -max-size $((5 * 1024 * 1024))
```

Call `stdin-rotate -h` to see all flags.
