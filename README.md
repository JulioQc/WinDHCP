# WinDHCP
Windows DHCP Debug Content Pack

Example NXlog.conf file input/output configuration:

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
	Exec $Hostname = hostname_fqdn();
</Output>
