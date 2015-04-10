# icinga_brightmail
Icinga (and Nagios) plugin for checking Symantec BrightMail performance. The plugin also
provides performance data.

# Requirements
- Perl 5.x with Getopt::Long module (included by default in most installations)
- snmpwalk binary, included with the net-snmp package
- Developed and tested on Debian 7.x GNU/Linux, but will most likely run on any
  Linux/Unix like OS
- Icinga or Nagios

# Installation
Put the check_brightmail perl script on your Icinga/Nagios plugin directory. Check that the 
default path for snmpwalk (/usr/bin/smmpwalk) matches your environment. Included is an 
example configuration for Icinga/Nagios which you can add to your setup

# Configuration
The script itself only takes a few arguments. Only the first two are required, unless you
define them statically within the script itself:
- address: the IP address or hostname of your Symantec Brightmail server
- community: the SNMP community defined in your BrightMail setup
- snmp_cmd: alternate location of your snmpwalk binary
- debug: enable/disable debug output. Default is off.

The default SNMP version used is 2c, but that can be changed via the "$snmp_version" variable
within the script.

# Icinga/Nagios alert thresholds
Load:
Warning/critical thresholds values are set in the script under "Warning and critical thresholds".
Future version will include an option to set these via the command line.
The average load thresholds are defined as follows:
5 min average = $load_warning / $load_critical
10 min average = $load_warning-0.25 / $load_critical-0.5
15 min average = $load_warning-1 / $load_critical-2

So if the load warning threshold is set to 3 the 5 minute average will have to be
above 3 to get a warning, the 10 minute average above 2.75 and the 15 minute 
average above 2.

SMTP connections:
You can also set the thresholds for current SMTP connections with the "$connections_warning" and 
"$connections_critical" variable. This value is calculated by adding the values for each
BrightMail MTA instance, which in this case is 3.

Queued messages:
The threshold values for queued messages are set with the "$queued_warning" and "$queued_critical" variables.
As with SMTP connections these values are calculated by adding together the values for each MTA
instance.

Memory usage:
Warning and critical thresholds are set with the "$mem_warning" and "$mem_critical" variables.
Usage limits are defined in percentage, so f.ex set the warning level to 90% and critical to 95%.
The values are calculated using the "Virtual memory" and "Virtual memory used" values from the
SNMP output.

# SNMP OIDs
If you for some reason need to change the SNMP OIDs used they are defined in the "%oid" hash array.
There is no need for installing the MIB file from Symantec as the numeric OIDs are used.

# Included files
- check_brightmail: the perl script/plugin for Icinga/Nagios
- brightmail.cfg: example configuration for Icinga/Nagios
