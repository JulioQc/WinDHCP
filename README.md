# WinDHCP
Windows DHCP Debug Content Pack for Graylog

##Includes

* Input (WinDHCPLogs-gelf - GELF/UDP/5441)
* Extractors (WinDHCP_Debug_Log)
* Dashboard (WinDHCP Summary)

##Requirements

* Windows DHCP server configured for "Enable DHCP audit logging" (https://technet.microsoft.com/en-ca/library/cc780941(v=ws.10).aspx)
* A GELF capable log exporter/collector such as nxlog or Graylog Collector monitoring the log file path

###Example of a working NXlog.conf file input/output configuration (using Collector Sidecar):

    <Input 578f9dbc0ae2f10b1139b6a9>
        Module im_file
        File "C:\Windows\Sysnative\dhcp\DhcpSrvLog-*.log"
        PollInterval 1
        SavePos	True
        ReadFromLast True
        Recursive False
        RenameCheck True
        Exec $FileName = file_name(); # Send file name with each message
    </Input>

    <Output 578f97f40ae2f10b1139b093>
        Module om_udp
        Host 192.168.20.210
        Port 5441
        OutputType  GELF
        Exec $short_message = $raw_event; # Avoids truncation of the short_message field.
        Exec $gl2_source_collector = 'ae1187a3-48ae-42bc-a820-7033d7438dbd';
        Exec $Hostname = hostname_fqdn();
    </Output>
