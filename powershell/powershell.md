Powershell V 4.0
================

Cmdlets 
-------


Cosa è un CommandLets (CmdLets)?  
    `Esempio: Get-Process / Get-Service / Get-Host`


La sintassi dei commandlets sono: Verbo-Sostantivo  

Tab per autocompletare e hotkeys  

Tab e parametri  
Get-S -Par  

Loggare:  
```
Start-TransCript c:\temp\transcript1.txt  
Stop-Transcript  
```

La Powersehll IDE (ISE)
-----------------
Powershell_Ise
Integrated Developer Engine con un Debugger a bordo.

Providers
---------
```
Get-PSDrive
C:\  
D:\  
HKLM:\\    
HKCU:\\  

CD HKCU:
Dir
MD Test // CD Test // RD Test
```

Count di comandi per amministrare la PS (Vedi Immagine)

Trovare comandi:
`Get-Command` (es: Start-Service / Stop-Service)
```
Get-Command | Sort-Object Noun
Get-Command *-Service
```

Help:
```
Update-Help * -Force
Get-Help Get-Service
Get-Service -?
Get-Help Get-Service -Examples
get-help Get-Service -Full

get-help get-process
get-help  * -Category CmdLet | ft Category,Name,Synopsis -AutoSize
get-help  * -Category CmdLet | ft Category,Name,Synopsis -AutoSize | Out-File -FilePath 'C:\Temp\Cmdlets.txt'
```

Sintassi:
NAME
    Get-Service

SYNTAX
    Get-Service [[-Name] <string[]>] [-ComputerName <string[]>] [-DependentServices] [-RequiredServices] [-Include
    <string[]>] [-Exclude <string[]>]  [<CommonParameters>]
Get-Service -Name wuauserv, winrm, dnscache


NAME
    Get-EventLog

SYNTAX
    Get-EventLog [-LogName] <string> [[-InstanceId] <long[]>] [-ComputerName <string[]>] [-Newest <int>] [-After
    <datetime>] [-Before <datetime>] [-UserName <string[]>] [-Index <int[]>] [-EntryType <string[]> {Error |
    Information | FailureAudit | SuccessAudit | Warning}] [-Source <string[]>] [-Message <string>] [-AsBaseObject]
    [<CommonParameters>]


[[-Name] = parametro nome opzionale (default) [ XXX ]
Get-Service wuauserv

Aliase
Es. Get-ChildItem
dir funziona perche è un Alias
dir /s NON FUNZIONA / in cmd visualizza anche tutti i subfolders
dir -recurse

PS C:\Users\unterfad> Get-Alias dir
CommandType     Name
-----------     ----
Alias           dir -> Get-ChildItem

Get-Alias --> fornisce la tabella dei alias per i comandi powershell


pipeline
Concattenare comandi powershell. Ps fornische oggetti che si possono manovrare.
Es.
Get-Process | Sort CPU
Get-Process | Sort CPU | Select ProcessName, CPU
Get-Process | Sort CPU | Select ProcessName, CPU | Export-Csv C:\temp\processiCpu.csv
Get-Process | Sort CPU | Select ProcessName, CPU | Clip

Lavorare con dei file CSV
Get-Service > ouput.txt
Get-Service | Export-Csv ouput.csv
Import-Csv output.csv | get-member
Import-Csv .\ouput.csv | Get-Member

Lavorare con dei file XML
Get-Service | Export-CliXml xmlout.xml
Import-CliXml xmlout.xml

Formattare l'output
Get-Service | Select-Object Name,CanStop,MachineName (fifficultoso da leggere) meglio...
Get-Service | Format-Table -AutoSize Name,CanStop,MachineName  (ft / fl)
Get-Process | Out-GridView
Get-Process | Out-GridView -PassThru | Stop-Process  (solo dalla 4 via)

SnapIns / Modules: 
Get-PsSnapIn
Get-PsSnapIn -Registered
Add-PsSnapIn *SQL*
Get-Command *SQL*
Get-PsSnapIn -Registered | Select-String exchange
Add-PsSnapIn *exchange*
Get-Command *exchange*

Get-Module
Get-Module -ListAvailable
Import-Module ActiveDirectory    //Il comando carica il modulo automaticamente dalla versione 3

Data:
"05/03/14"  // "05/03/14" | get-member   --> String
[DateTime]"05/03/14" | get-member
Get-Date -Format "dd.MM.yyy"
(Get-Date).AddDays(-1)

Esempio:
get-EventLog -?
Get-EventLog -LogName System -After (Get-Date).AddDays(-1)


Classi e Oggetti
--------------
Get-Service | Get-Member
Ps torna oggetti (properti), di default non visualizza i properties ma i nomi.
Con Select-Object puoi selezionare il property
Get-Service | Select-Object Name, Status, CanStop, MachineName
Get-Service | Select-Object *
Get-Service -Name WwanSvc | Select-Object *

Notazioni Punto
(Get-Service wuauserv).Status
(Get-Service wuauserv).Stop()
(Get-Service wuauserv).Start()

Dir c:\Windows | Measure-Object
Count    : 105
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
Dir c:\Windows | Measure-Object -Property Length -Sum
(Dir c:\Windows | Measure-Object -Property Length -Sum).Sum
(Dir c:\Windows | Measure-Object -Property Length -Sum).Sum /1MB
Get-Service | Where-Object { $_.Status -eq "Stopped" }
"Ciao Powershell" | Get-Member
("Ciao Powershell").ToLower()
"Ciao Powershell".SubString(0,4)

Cercare Oggetti con Where-Object
Get-Service | Where-Object { $_.Status -eq "Running" }   $_ = Carattere Jolly e significa l'oggetto corrente

Esempio: Cercare tutti i dati nella cartella C:\Windows maggiori a 100Kb
dir C:\Windows | Where-Object { $_.Length -gt 100Kb }  / 100000 / Where / ?

Da PS3 abbreviato
Get-Service | Where Status -eq "Running"


For / Ciclo
"London","Parigi","Zurigo","Ticino" | foreach { $_.ToLower() }
"London","Parigi","Zurigo","Ticino" | foreach { "Il nome della città è: $_" }
"London","Parigi","Zurigo","Ticino" | foreach { Mkdir $_ }


Skriptare:
--------------
Powershell_ise
Write-Host / Read-Host

commenti #

$hereString = 
@"
Questo è un 
Here string 
su diverse linee
"@


Variabili
$nome = "Un Nome"
[string]$nome2 = "Un altro nome"
[int]$valore = 3
$nome = "Adrian"
"Ciao $nome"  -> Espansione variabile
"Pa$$w0rd" ($$ = ultimo comando) 
'Pa$$w0rd' Senza espansione


Validita varibili: Tutto Globali (global::) (script::) (local::)

booleans:
$true, $false, $null

Strings:
$mystring = "un valore"
$mystring = "Hello `t $ENV:Computername"

Operatori
-or -eq -and -ne -ge -lt -le (ecc. gaurdare nel libro)
+ - * / = += -= *= /=  ++ --
$a = 1
$a = $a + 1
$a++
$a+=5

(Domandare valori)
if else

$a=2
$b=2
if($a == $b) {
Write-Host "Ciao";
}

if( (Get-Date).Day -eq 1) {
	Write-Host "1. Giorno del Mese"
}
else
{
	"Altro Giorno"
}


switch
$a=5
switch($a)
{
	1 { "Il colre è rosso" }
	2 { "Il colre è blu" }
	3 { "Il colre è verde" }
	4 { "Il colre è giallo" }
	5 { "Il colre è bianco" }
	6 { "Il colre è nero" }
	default { "Il colore non è definito" }
}

switch((Get-Date).Day)
{
	1 { "Primo esimo giorno" }
	20 { "20 esimo giorno" }
	21 { "21 esimo giorno" }
	default { "Un altro giorno" }
}


for
$max=3
for($i=0;$i<$max;$i++)
{
	write-host $i
}

array
$listaDiNumeri = "Uno", "Due", "Tre", "Quatro"
$altryLista = 1,2,"Tre",3.14

foreach
foreach ($numero in $listaDiNumeri)
{
	if($numero -eq "Uno")
	{
		Write-Host "Ho scelto il numero" + $numero
	}
}






