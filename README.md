# smokeping-youtube-dl
Smokeping::probes::YoutubeDL

Integrates youtube-dl (https://github.com/rg3/youtube-dl/blob/master/README.md) as a probe into smokeping. The variable "binary" must
point to your copy of the youtube-dl program. If it is not installed on
your system yet, you should install the latest version from https://rg3.github.io/youtube-dl/download.html.

The Probe asks for the given resource one time, ignoring the pings config variable (because pings can't be lower than 3).

Timing the download is done via the /usr/bin/time command (part of the "time" package, not the shell builtin time!), output is written to /dev/null. 
By default the best available video quality is requested.

Note: Some services might ban your IP if you do too many queries too often (I tried once per hour)!
Note: If the download time exceeds 180s, you may need this patch (and delete the rrds): https://github.com/oetiker/SmokePing/pull/39

Parameters:
* binary => The location of your youtube-dl binary.
* url => The URL you want to download. See youtube-dl's list of supported sites (youtube-dl --list-extractor)
* options => Extra options you wish to pass to youtube-dl. See 'youtube-dl -h' for reference. Default options sent are '-o /dev/null'

Logging:
You can get logs of what goes on inside the plugin either by running smokeping with --debug, or by changing this line:
  #set to LOG_ERROR to disable debugging
  setlogmask(LOG_MASK(LOG_ERROR));
  
  to
  
  #set to LOG_ERROR to disable debugging
  setlogmask(LOG_MASK(LOG_DEBUG));
  
After doing this (and restarting smokeping), the plugin's logs will go to syslog, local0.debug. You will need something like 
  local0.*     /var/log/local.log
in your syslog configuration.


Example probe configuration (poll every hour):

+ YoutubeDL1h
binary = /usr/local/bin/youtube-dl
timeout = 300
step    = 3600
pings   = 3

++ STAR_WARS_Episode_7_TRAILER
menu = STAR_WARS_Episode_7_TRAILER
title = STAR_WARS_Episode_7_TRAILER 1:47, 16M, 720p
probe = YoutubeDL1h
url = https://www.youtube.com/watch?v=clLYRvtsoZ0
host = youtube.com
