GPS-backed NTP server
---------------------

A GPS-backed NTP server will improve the reliability of your router and give you a dead-on clock.  You will need to assemble a custom cable and enclosure for the GPS module.  All the parts can probably be purchased for less than $50.  I got the idea from the following forum post.

[Set the router clock with a GPS receiver](http://www.snbforums.com/threads/set-the-router-clock-with-a-gps-receiver-for-under-70.26981/page-4#post-213194)

The ntp.conf configuration file shows how to configure the generic NEMA driver for NTP and the ntp-start script shows how to integrate it with the firmware.
