Postmortem Report

visiting a specific URL results in a 504 error 

Site outage/504 error incident report 

Summary

Anyone attempting to browse a website received a 504 error on February 4, 2023 at dawn because the server access went down.
Background regarding the LAMP stack on which the server is based. 

Timeline (all time in GMT + 1)

. 6:25 - Anyone attempting to access the website receives a 500 error 

. 6:30 - ensuring that Apache and MySQL are operational.

. 6:35 - An investigation into the website's loading issues discovered that the database and server were both functioning properly.

. 7:00 - When trying to curl the website after a brief restart of the Apache server, a status of 200 and OK was returned.

. 7:30 - checking error logs for potential sources of the error

. 8:00 - To see if the Apache server was abruptly shut down, look in /var/log. There was no sign of the PHP error log.

. 8:12 - All error logging had been off, as was discovered by looking at the php.ini settings. activating error logging

. 8:30 - restarting the apache server and reviewing the php error logs to see what is being logged.

. 8:35 - An incorrectly written file name was causing erroneous loading and premature Apache closure, according to a review of the php error logs.

. 8:40 - Changing the filename and restarting Apache.

. 8:50 - The website is currently loaded properly and the server is operating regularly.

Cause and effect and remedy

The problem was caused by the wp-settings.php file referencing the incorrect file name. The server responded with a 500 error code when the user attempted to curl it. Checking the error logs revealed that no error log file was being written for the PHP failures, and reading the normal Apache error log did not reveal much about the server's early shutdown. The engineer decided to investigate the php.ini file's error log option after realizing that the php log errors were not being directed anywhere, and discovered that all error logging was disabled. The Apache server was restarted to examine the error logging after it was switched on.

Correctional and Preventative Actions

. Error logging should be enabled on all servers and websites to make it simple to find faults when something goes wrong.

. Before deploying on a multi-server setup, all servers and sites should be tested locally. This will allow issues to be fixed before coming live and reduce the amount of time needed to fix a downed site.
