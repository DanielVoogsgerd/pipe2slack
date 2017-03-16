pipe2slack
==========

Simple script that outputs all data inserted into a named pipe to slack via an incoming webhook (https://api.slack.com/incoming-webhooks).

## Dependencies
This script uses a couple python libraries
- requests
- configparser
- argparse

## Installation
A couple things need to be done to get everything up and running

- Install dependencies
- Download the source (I choose to place it in `/opt/pipe2slack` and symlink it to `/usr/local/bin/pipe2slack`)
- Create the following tmpfiles.d(5) fragment:
  `p       /run/syslog-slack 0770    root    root    -       -`
- Named pipes can also be created adhoc with mknod(1):  `mknod NAME p` 
  or mkfifo(1): `mkfifo NAME` 
- Setup the daemon using the supplied pipe2slack.service
- Create a environment file in /etc/default/pipe2slack

To set this up with syslog just pipe the output of syslog to the pipe like this:
`*.* |/run/syslog-slack`
* DO NOT DO THIS EXACTLY, THIS WILL LOG _EVERYTHING_ TO SLACK*, WHICH IS A TERRIBLE IDEA*
Think about what you want to send to slack.

### Environment File
```
PIPE=/run/syslog-slack
HOOK=<slack webhook url>
CHANNEL=General
NICKNAME=Syslog-bot
```

### Config file format
```
[config]
pipe = path to pipe

[slack]
hook = WEBHOOK URL 
channel = Channel name 
nickname = Nick used to post
```

For example:
```
[config]
pipe = ./pipe

[slack]
hook = https://hooks.slack.com/services/...
channel = general
nickname = log
```

This will send the messages written to the named pipe './pipe' to the incoming webhook at https://hooks.slack.com/services/... to the #general channel with the messages labeled as from the user 'log'.

### Command line parameters

The following command line parameters are supported:

```
pipe2slack.py [-h] [--config CONFIG] [--nickname NICKNAME]
                   [--hook HOOK] [--channel CHANNEL] [--pipe PIPE]
                   [--silent]

optional arguments:
  -h, --help           show this help message and exit
  --config CONFIG      config file to load
  --nickname NICKNAME  nick to use on the IRC server
  --hook HOOK          url for the slack webhook
  --channel CHANNEL    channel to join
  --pipe PIPE          pipe to read from
  --silent             do not print messages to stdout
```
