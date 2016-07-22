# WinDHCP
Windows DHCP Debug Content Pack for Graylog

##Includes

* Input (WinDHCPLogs-gelf - GELF/UDP/5441)
* Extractors (WinDHCP_Debug_Log)
* Dashboard (WinDHCP Summary)

##Requirements

* Windows DHCP server configured for "Enable DHCP audit logging" (https://technet.microsoft.com/en-ca/library/cc780941(v=ws.10).aspx)
* A GELF capable log exporter/collector such as nxlog or Graylog Collector monitoring the log file path

###Example NXlog.conf file input/output configuration:

    <Input 1234>
        Module im_file
        File "C:\Windows\Sysnative\dhcp\DhcpSrvLog-*.log"
        PollInterval 1
        SavePos	True
        ReadFromLast True
        Recursive False
        RenameCheck True
        Exec $FileName = file_name(); # Send file name with each message
    </Input>

    <Output 4321>
        Module om_udp
        Host 192.168.20.210
        Port 5441
        OutputType  GELF
        Exec $short_message = $raw_event; # Avoids truncation of the short_message field.
        Exec $Hostname = hostname_fqdn();
    </Output>
