# check_minecraft
A plugin for monitoring a Minecraft server running version 1.7 or 1.8.  


## Description

This plugin is designed to work with Nagios, Icinga, or any other
monitoring application which uses the [Nagios Plugin API][1].

The plugin makes a TCP connection to a [Minecraft][2] server and collects
the server description, online user count, and round-trip delay (RTD) from
the monitor.


## Requirements

- Python 3
- The [nagiosplugin][3] library.


## Usage

```
usage: check_minecraft [-h] [-V] -H HOST [-p PORT] [-v] [-w RANGE] [-c RANGE]

check_minecraft: A plugin for monitoring a Minecraft server running version 1.7 or 1.8.

optional arguments:
  -h, --help            show this help message and exit
  -V, --version         show program's version number and exit
  -H HOST, --host HOST  hostname or IP address of the server
  -p PORT, --port PORT  TCP port number of the server
  -v, --verbose
  -w RANGE, --warning RANGE
                        return warning if RTD is outside RANGE
  -c RANGE, --critical RANGE
                        return critical if RTD is outside RANGE

```


## Command-Line Example

```
$ ./check_minecraft -H minecraft.example.org
MCSERVER OK - server is available | online=True players=3;;;0;144 rtd=40ms;120;300;0
```


## Nagios Example

```
define command{
    command_name    check_minecraft
    command_line    /usr/lib/nagios/plugins/check_minecraft -H $ARG1$
}

define service{
    hostgroup_name          minecraft-servers
    service_description     Minecraft
    check_command           check_minecraft!$HOSTADDRESS$
    use                     generic-service
    notification_interval   5 ; set > 0 if you want to be renotified
}

```


## Icinga2 Example

```
object CheckCommand "check-minecraft" {
  import "plugin-check-command"

  command = [ PluginDir + "/check_minecraft" ]

  arguments = {
    "-H" = {
      required = true
      value = "$mc_host$"
    }
    "-P" = "$mc_port$"
  }

  vars.mc_host = "$address$"
  vars.mc_port = 25565
}

apply Service "minecraft" {
  import "generic-service"

  check_command = "check-minecraft"

  assign where "minecraft-servers" in host.groups
}

```

[1]: https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/pluginapi.html "Nagios Plugin API"
[2]: https://minecraft.net/game "Minecraft"
[3]: https://pypi.python.org/pypi/nagiosplugin/ "nagiosplugin"

