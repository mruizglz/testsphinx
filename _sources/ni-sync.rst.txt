==========================================
ni-sync Setup for PXIe 6683h with IEEE1588
==========================================

Verification of the IP configuration of the PXIe6683h
=====================================================

.. code-block:: bash

	$ ifconfig
	


.. note:: 
	Identify the interface of the PXIe6683h
	

Configuration of the IEEE1588 profile
=====================================

.. code-block:: bash

	$ cd /usr/lib/x86_64-linux-gnu/ni-timesync
	$ sudo ./configgen --listTRInstances --tsmJsonFile /var/lib/ni-timesync/tsm.json
    Enabled TR instances: [ "IEEE 802.1AS-2011_1", "GPS_1" ]
    Disabled TR instances: [ "IRIG-B_1", "IEEE 1588-2008_1", "PPS_1" ]
    $ sudo  ./configgen --tsmJsonFile /var/lib/ni-timesync/tsm.json --disableTRInstances "GPS_1"
    $ sudo ./configgen --tsmJsonFile /var/lib/ni-timesync/tsm.json --disableTRInstances "IEEE 802.1AS-2011_1"
    $ sudo ./configgen --tsmJsonFile /var/lib/ni-timesync/tsm.json --enableTRInstances "IEEE 1588-2008_1"
    
    
Verify the content of /var/lib/ni-timesync/tsm.json

.. code-block:: json

	{
	  "timeReferences": {
	     "IEEE 1588-2008_1": {
	        "correctionNanos": "0",
	        "defaults": "/usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/ieee1588-2008.json",
	        "protocolConfig": {
	           "priority2": 128,
	           "priority1": 128,
	           "announceReceiptTimeout": 6,
	           "bmcaMode": "masterslave",
	           "enableBridging": true,
	           "ptpTimescale": true,
	           "logSyncInterval": 0,
	           "logAnnounceInterval": 1,
	           "localClockId": "00:80:2F:FF:FE:36:FD:26",
	           "applyUtcOffset": false,
	           "hwTimestamping": 1,
	           "statusFile": "/var/run/ptp_enp13s14.status",
	           "interface": "enp13s14"
	        },
	        "priority": "unknown"
	     }
	  },
	  "timeKeeper": {
	     "library": "/usr/lib/x86_64-linux-gnu/ni-timesync/timekeepers/libphctimekeeper.so.23.8.0",
	     "pluginConfig": {
	        "seedWithOsTime": true,
	        "phc2sysEnable": false,
	        "interface": "enp13s14"
	     }
	  },
	  "servo": {
	     "library": "/usr/lib/x86_64-linux-gnu/ni-timesync/servos/libdefaultservo.so.23.8.0"
	  },
	  "disabled": {
	     "IEEE 802.1AS-2011_1": {
	        "defaults": "/usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/ieee8021as-2011.json",
	        "correctionNanos": "0",
	        "protocolConfig": {
	           "logSyncInterval": -3,
	           "priority1": 246,
	           "logAnnounceInterval": 0,
	           "localClockId": "00:80:2F:FF:FE:36:FD:26",
	           "priority2": 247,
	           "interface": "enp13s14",
	           "neighborPropDelayThreshold": "800"
	        },
	        "priority": "unknown"
	     },
	     "IRIG-B_1": {
	        "correctionNanos": "0",
	        "defaults": "/usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/irig.json",
	        "protocolConfig": {
	           "irigTerminal": "/dev/pxi6683h.0.pfi0",
	           "irigMode": "AM",
	           "rocoRoutingTerminal": "/pxi6683h.0/PFI0"
	        },
	        "priority": "unknown"
	     },
	     "GPS_1": {
	        "correctionNanos": "0",
	        "defaults": "/usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/gps.json",
	        "protocolConfig": {
	           "recalculatePosition": true,
	           "gpsSerialPort": "/dev/pxi6683h.0.gps0",
	           "gpsDeviceInterface": "/dev/pxi6683h.0",
	           "mobileMode": false
	        },
	        "priority": "unusable"
	     },
	     "PPS_1": {
	        "correctionNanos": "0",
	        "defaults": "/usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/pps.json",
	        "protocolConfig": {
	           "initialTimeFractionalNanoseconds": 0,
	           "initialTimeNanoseconds": 0,
	           "ppsTerminal": "/dev/pxi6683h.0.pfi0",
	           "initialTimeSeconds": 0,
	           "useManualTime": false
	        },
	        "priority": "unknown"
	     }
	  }


Reboot the system

Verify if the ptpd daemon is running

.. code-block:: bash

   ps -ax | grep ptp
   5794 ?        Sl     0:00 /usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/ptpd_wrapper instID 1792f10 
   rpcServerInst 0 logAnnounceInterval 1 logSyncInterval 0 ptpTimescale 1 localClockId 00:80:2F:FF:FE:36:FD:26 
   announceReceiptTimeout 6 priority1 128 priority2 128 statusFile /var/run/ptp_enp7s14.status applyUtcOffset 0 
   hwTimestamping 1 interface enp7s14 enableBridging 1 bmcaMode masterslave 
   configFile /usr/lib/x86_64-linux-gnu/ni-timesync/timereferences/ptp.conf
   
Use the following `link <https://www.ni.com/en/support/documentation/supplemental/19/using-ni-timesync-to-configure-ieee-1588-and-802-1as-time-refere.html>`_ to see the meaning of the different parameters



	