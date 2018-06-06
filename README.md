# slackIPReport

crontab -e


SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=pi
HOME=/home/pi

@reboot /usr/bin/curl -s rrc.ca -o /home/pi/file.txt >/dev/null 2>&1

#@reboot bash /home/pi/reportip.sh
#@reboot echo $(date) >> /home/pi/reportip.log
1 * * * * echo $(date) >> /home/pi/time.txt
1 * * * * /usr/bin/curl -s rrc.ca -o /home/pi/file.txt


#!/bin/bash
# This script reports IP address of the wlan to slack channel #reportip
# Example of message:
# 192.168.168.65 Wed Jun  6 11:11:33 CDT 2018
# Nico cai
# ncai@rrc.ca
# Last update Jun 6, 2018
#

#sleep 10

ip="$(date) | $(ifconfig wlan0 | grep 'inet' | cut -d: -f2 | awk '{print $2}')"
msg={\"text\":\"$ip\"}

set -e
set -x
/usr/bin/curl -S -X POST -H 'Content-type: application/json' --data "$msg" https://hooks.slack.com/services/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


echo "exiting script $(date), ip reported: $ip" >> reportip.log


