---
layout: page
title: MySQLmon
permalink: /projects/mysqlmon
---


Project provides MySQL cluster (NDB) Monitoring using Erlang. The application can be incoporated with any system
built using Erlang. The application facilitates monitoring NDB cluster through following information

1. Connection Count to SQL node
   * Number of concurrent connections to a specific MySQL node in the cluster. (Application needs not to be running on MySQL node)
    * Warning Threshold - A warning generated if number of connections goes beyond
    * Critical Threshold - Critical state/ message generated if number of connections goes beyond
    * Check Interval - Polling interval in mili seconds
    * DSN - DSN String for database connection (Connection to the MySQL node to be monitored)
<br><br>

2. MySQL error log Monitoring
   * Periodic check on the mysqld error log (mysqld.log) to Detect Errors
     - Alert - Send a warning or alert on changes
     - Check Interval - polling interval
     - File Path - Path for the log File
     - File Lines - No of last lines to monitor
<br><br>

3. NDB Memory Usage
   * Periodic check on memory usage of NDB Storage Engine
     - Index Warning - Threshold for index memory warning
     - Index Critical - Threshold for index memory critical state
     - Data Warning - Threshold for data memory warning
     - Data Critical - Threshold for data memory critical state
<br><br>

4. NDB Process Monitoring
   * Periodic check for cluster manager or ndb node pid file
     - File Path - File path for NDB files
     - Node Id
     - Node Type - Cluster manager or data nodes
     - Check interval - Polling interval
<br><br>

5. MySQL Process Monitoring
   * Periodic check for the mysqld pid file.
      * File Path - MySQL file path
      * Check Interval - Polling interval
<br><br>

6. Transaction count
    * Periodic check on TPS of MySQL ( SQL Node)
      - Warning Threshold
      - Critical Threshold
      - Check Interval
      - DSN

<br>
The warnings, messages generated can be accessed by subscribing to the monitoring services through mysqlutil.
Example SNMP subscriber can be found in the repository.

[SNMP Subscriber Source](https://github.com/mahanama94/mysqlmon/blob/master/src/mysqlmon_snmp_server.erl)


For more information, refer the docs in the source.

### Links

Github: [https://github.com/mahanama94/mysqlmon]( https://github.com/mahanama94/mysqlmon ){:target="_blank"}
