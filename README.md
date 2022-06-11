# Send-ZabbixTrap
Send-ZabbixTrap it's PowerShell module for send data to zabbix server over TCP without zabbix_sender.exe

How to use:

1) Put Send-ZabbixTrap.psm1 in the place where you will store the modules, can be any directory
2) Import module Send-ZabbixTrap.psm1 in youre code
      Example: Import-Module "C:\PSModules\Send-ZabbixTrap.psm1"
4) Сall the function Send-ZabbixTrap with parameters
      You can use parameter aliases as in zabbix_sender.exe
      z = zabbix server
      p = port
      s = host
      k = key
      o = value
     
Example:
 Send-ZabbixTrap -z 172.16.5.2 -p 10051 -s Srv1 -k trap -o OK
 Send message "OK" on zabbix item "trap" in zabbix host "Srv1" zabbix server 172.16.5.2 on port 10051
 
Example:
 Send-ZabbixTrap -z 172.16.5.2 -p 10051 -s Srv1 -k trap -o OK -OnlyPreview
 Only generating Json string for preview
 
 Example:
 Send-ZabbixTrap -Server 172.16.5.2 -Port 10051 -JsonString $json
 Json format example:
 [pscustomobject][ordered]@{
    'request' = 'sender data';
    'data' = @(
        1..3 | % {
            [pscustomobject][ordered]@{
                'host' = 'HOST'
                'key' = 'KEY'
                'value' = 'VALUE'
            }
        }
    )
} | ConvertTo-Json

 Example:
 Send-ZabbixTrap -Server 172.16.5.2 -Port 10051 -InputObject (Import-Csv -Encoding utf8 -Delimiter ';' -Path $home\srv.csv) -Header ComputerName,Service,Status
 The PropertyNames of the input objects must be "host, key, value" or be specified in the header parameter
