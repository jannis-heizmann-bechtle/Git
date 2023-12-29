# WS1 – Start/Stop Service Script

Powershell Script um alle AirWatch Dienste (+IIS) zu starten und zu stoppen

```
################################
# Configuration                #
################################

#Service Name for Operation
$ServiceName ="AirWatch"

#Operation: start | stop
$Operation = "start"

#Include IIS: yes | no
$IncludeIIS = "yes"


################################
# Script                       #
################################

$Services = Get-Service

foreach ( $s in $Services ) {
  if($s.DisplayName.StartsWith($ServiceName)){
    #write ($s.DisplayName + "`t " + $s.Status)
    write $s

    if ($Operation.Equals("start")) {
      Start-Service -Name $s.Name
    }
    else {
      Stop-Service -Name $s.Name
    } 
  } 
}
if($IncludeIIS -eq "yes") {
  write (Get-Service -Name "W3svc")
  if($Operation.Equals("start")) {
    Start-Service -Name "W3svc"
  }
  else {
    Stop-Service -Name "W3svc"
  }
}
```

&#x20;
