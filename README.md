# inetmonitor
Internet Connection Monitor

ever encountered a poor internet connection? I did. In March 2022 it looked as shown in the following picture
![internet outages](https://blog.lindenberg.one/documents/VodafoneKabelGate/MarchSmall.jpg)

I got the picture by running a service that monitored the internet connection and aggregated the resulting history, while also sending automatically complaint mails to my internet provider. Several folks raised their interest in the monitoring tool and that´s why I am going to publish it here. Obviously the machine you want to use should run all the time, or at least whenever internet connection is relevant. It is probably a bad idea to send any message directly to your internet service provider.

I am not familiar with creating packages for any distro nor do I run anything other than Ubuntu regularly. Thus I am assuming the following workflow:

* cd /opt
* git clone https://github.com/joachimlindenberg/inetmonitor.git
* install dependencies - either postfix or ssmtp are required to send an email. With Ubuntu that translates to apt-get install ssmtp
* cd /opt/inetmonitor
* bin/install - run that once, which creates some directories and copies default config files that you need to edit
* you configure and test postfix or ssmtp. I used the latter, which uses a config file at /etc/ssmtp/ssmtp.conf - details are in https://linux.die.net/man/5/ssmtp.conf. Be sure to also configure a reverse alias and test sending a mail.
* modify /etc/inetmonitor/inetmonitor.conf and /etc/inetmonitor/mailtemplate to match your configuration.
* bin/install - second time
* you wait until something happens.. no, you can check configuration with bin/simdown 

If that doesn´t work for you and you cannot find the issue yourself, please create an issue on this repository.

The monitor does not create the picture itself. I used Excel to create it. Someone may want to create it automatically though, contributions wellcome.

Known issues:

* if your router disconnects automatically in order to obtain a new IP-address or for updates, the monitor doesn´t know. If you don´t want to get erroneous mails, you can use systemctl stop/start inetmonitor inetmonitor.timer around your activities (also in /etc/crontab). Also I haven´t spent time on making this a single start/stop.
* Quality of service is up to you. You may use something like -Q 0x24 as an argument to ping (check out e.g. https://linux.die.net/man/8/ping), you may have to configure your router or other network equipment to somehow prioritize the pings over other traffic. If you overload your network and do not prioritize the monitor, then the monitor will report outages that are just overloads.
* I started experimenting with logrotate to compress the logs - will update when that is stable
* timezone handling may have to be improved





