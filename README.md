pipe2slack
==========

Simple script that outputs all data inserted into a named pipe to slack.

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
- Setup the daemon using the supplied pipe2slack.service
- Create a environment file in /etc/default/pipe2slack

To set this up with syslog just pipe the output of syslog to the pipe like this:
`*.* |/run/syslog-slack`
* DO NOT DO THIS EXACTLY, THIS WILL LOG _EVERYTHING_ TO SLACK*, WHICH IS A TERRIBLE IDEA*
Think about what you want to send to slack.

### Environment File
````
PIPE=/run/syslog-slack
HOOK=<slack webhook url>
CHANNEL=General
NICKNAME=Syslog-bot
````
