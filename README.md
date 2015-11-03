# check_minecraft
A plugin for monitoring a Minecraft server running version 1.7 or 1.8.  


## Description

This plugin is designed to work with Nagios, Icinga, or any other
monitoring application which uses the [Nagios Plugin API][1].

The plugin makes a TCP connection to a [Minecraft][2] server and collects
the server description, online user count, and round-trip delay (RTD) from
the monitor.


[1]: https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/pluginapi.html "Nagios Plugin API"
[2]: https://minecraft.net/game "Minecraft"

