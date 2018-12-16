#Netwerkbeheer

## DHCP

Dynamic Host Configuration Protocol. Zorgt voor de **verdeling van IP-adressen**. Elke DHCP-server heeft een pool van vrije IP-adressen om uit te delen, en verdeelt deze met een bepaalde lease-time, waarna deze opnieuw worden vrijgegeven.

**Voordelen** dynamische ip-adressen:

* Geen ip-configuratie nodig voor elk toestel, kan centraal beheerd worden.
* Efficiënter omgaan met IP-adressen. Beperkt hierdoor kosten.

Proces om IP-adres te verkrijgen loopt via **DORA**

* **Discover**: client verstuurt broadcast naar alle computers binnen Ethernet-segment.
* **Offer**: server biedt vrij IP-adres aan.
* **Request**: client doet werkelijke aanvraag voor dit IP-adres (bij één DHCP-server, andere DHCP-servers zien via broadcast dat hun aanbod niet nodig is)
* **Acknowledge**: server bevestigt de aanvraag


## DNS

**Domain Name System/Server** zorgt voor **vertaling tussen IP-adres en naam**. 

De DNS-Server houdt een tabel bij met Resource Records:

* **A**: Address records - hostname to ip-address
* **AAAA**: IPv6 Address records - hostname to IPv6-address
* **CNAME**: Alias - hostname to other hostname (redirects)
* **MX**: Mail Exchange record - specifiec mail server 

### Proces DNS request 

* surf naar www.google.com -> zoekt ip adres
* Twee lokale opzoekingen, daarna neemt uw dns server het over. (lokaal is dat dns server van bv. telenet)
    * check etc/hosts
    * daarna dns cache
    * daarna vraagt hij aan dns server in domein. Indien DNS server in domein adres niet heeft
        * root DNS Server
            * .com
                * google.com
                    * drive.google.com


## Active Directory

Kan niet zonder DNS! Voor AD moest je op elke PC een gebruiker aanmaken om te kunnen authenticeren. Authenticatie gaat via Kerberos op de Domain Controller (waar AD staat).

Kerberos stuurt tickets voor toegang -> zie youtube video.

Kleine bedrijven: kan via Azure AD, voor grote bedrijven niet voldoende (geen LDAP ondersteuning bvb.).

Kan onder meerdere domeinen (subdomeinen dan eigenlijk). Maar geeft problemen voor authenticatie in cloud, dus meer en meer één groot domein.


Functie Domein Controller(s):
Host databank van AD
Repliceren elkaar (niet zoals bij DNS waarbij één hoofd DNS (primary zone) en een backup 
DNS (secondary zone)

### Group Policies

* Settings voor meerdere gebruikers of computers.
* Twee verschillende zaken:
    * Policies: afschermen van zaken
    * Preferences: settings die de gebruiker kan het aanpassen
* Client driven: de client vraagt aan de server welke policies hij/zij moet volgen
* **Inheritence**: overerving van policies (kan afgezet worden). Wordt aangegeven door uitroepteken.
* **Enforce**: wordt steeds toegepast (ook zonder inheritence). Wordt aangegeven door slotje.
* Steeds groeperen per functie. Bvb. alle rdp policies groeperen. Scheiding van gebruiker/groepen.

### Domain Controller configureren

* geef ip adres: 10.10
* installeer rollen
* maak domeincontroller: cursusdom.be

5 rollen: (FSMO) (kunnen bij één domeincontroller)

* RID Master: verdeeld groepen van security identifiers aan de andere domein controllers: zijn deze op moet je opnieuw bij de RID master nieuwe keys ophalen.
* PDC emulator: distributie van verandering naar andere domein controllers
* Schema Master: houdt bij hoe u database er uit ziet.
* Domein Naming Master:
* Infrastructure Master:



### PC toevoegen aan domain

**!Moet altijd vanuit Client!**

To join a computer to a domain

* On the Start screen, typeControl Panel, and then press ENTER.
* Navigate to System and Security, and then click System.
* Under Computer name, domain, and workgroup settings, click Change settings.
* On the Computer Name tab, click Change.
* Under Member of, click Domain, type the name of the domain that this computer will join, and then click OK.
* Click OK, and then restart the computer.


Via powershell: 

1. Eerst moet je DNS Server configureren, anders geraak je niet aan het domein
    * netsh interface ipv4 add dnsserver "Ethernet" address=192.168.x.x index=1
1. Met volgende command kan je de lokale pc toevoegen aan het domein. Hierbij moet je je authenticeren met een account van het domein.
    * netdom join %THE_COMPUTER_NAME% /domain:OPSCODEDEMO.COM /userd:Administrator /passwordd:xxx



#### Mogelijke problemen:

- IP instellingen:
- fixed IP
- DNS Server
- Gateway instellingen
- Gekoppeld aan virtuele switch?
- Firewall? Pingen indien geen reply en bovenstaande is ok -> Firewall staat op
- Check of DNS werkt. (ping domein ipv ip-adres of NSLookup)


### AGDLP
Account
Global Group
Domain Local Group
Permission

| Account       | Global Group  | Domain Local Group    | Permission (use NTFS)     |
| ------------- |-------------  | -----                 |---                        |
| Jos.Smet      | GG_HR         | DL_HR_R(read)         |                           |
|               | centered      | DL_HR_W(write)        | HR (share map)            |
|               | are neat      |  DL_HR_FC(fullctr)    |                           |


**Bij folder sharen:**
Share altijd everyone full control en dan per gebruiker/group met NTFS
Inheritance altijd uit

Op examen niet rechtstreeks, maar altijd met **groep structuur** werken:

- OU: cursusdom
- OU: groepen/users/computers/servers
- groepen: Global Groups GG_HR, etc.


Voordeel van computer toe te voegen op domein: 
iedere gebruiker binnen domein kan aanmelden via die pc
pc kan beheerd worden vanaf netwerk


## Fileserver

* Kan rechtstreeks (via GUI) via system - 
* Kan eenvoudiger via de Domein Controller 
    * Server Manager 
    * All Servers 
    * Server toevoegen via zoekfunctie 
    * Functies en rollen installeren.
* start powershell
* Install-WindowsFeature –Name FS-Resource-Manager
* Folder C:DATA sharen

* Beter via Server manager: fileserver - shares - tasks - new
* New-SMBShare –Name "Shared" –Path "C:\Shared" ` –ContinuouslyAvailable ` –FullAccess domain\admingroup `
* Test via nieuwe netwerklocatie aan te maken: \\ServerCore\Data
* Kan ook gewoon via \\ServerCore -> laat alle shared mappen zien

##Powershell

###Syntax

####$PSItem or $_
Gets the item in a foreach or after the pipeline.

$_ (dollar underscore) 'THIS' token. Typically refers to the item inside a foreach loop. Task: Print all items in a collection. Solution. ... | foreach { Write-Host $_ }
```
get-process | where-object {$_.cpu -gt 1 -and $_.company -ne 'Microsoft Corporation'} | ft name, cpu, company
```
####Comparators
```
 -eq             Equal
 -ne             Not equal
 -ge             Greater than or equal
 -gt             Greater than
 -lt             Less than
 -le             Less than or equal
 -like           Wildcard comparison
 -notlike        Wildcard comparison
 -match          Regular expression comparison
 -notmatch       Regular expression comparison
 -replace        Replace operator
 -contains       Containment operator
 -notcontains    Containment operator
 -in             Like –contains, but with the operands reversed.(PowerShell 3.0)
 -notin          Like –notcontains, but with the operands reversed.(PowerShell 3.0)

get-childitem | c:\windows | where-object LastAccessTime -gt (Get-Date).AddMonths(-1)
```
###Useful commands

####Get-process

```
get-process --- toont alle processen
get-process -name notepad --- toont running notepad processen
get-process -name n* --- toont alle processen starting with N
```

####Stop-process
```
stop-process -name notepad --- stop all notepad processen
stop-process -name notepad -whatif --- check which processes would have been stopped 
```

####Get-Date
Shows current date.
```
(Get-Date).DayOfWeek --- dag van de week
(Get-Date).AddYears/AddDays/ --- add amount of time to dat
Get-Date -Day 1 -Month 7 -Year (Get-Date).Year
--- toont op welke dag 1 juli van het huidig jaar valt 
```



Oefing Core

Veel instellingen kunnen ook via sconfig

1. naam wijzigen
    ```
    get current computername: hostname
    wmic computersystem where caption='xp-pc' rename windows7-pc
    ```
1. ip aanpassen
    * get name of interface:
        ```netsh interface show interface```
    * change ip adress:
        ```netsh interface ip set address "Ethernet" static 10.0.0.100 255.255.0.0 10.0.0.1 1```
    * show change:
        ```netsh interface show interface```

1. windows update uitzetten
    * BETER via powershell:

        ```
        stop-service wuauserv
        set-service wuauserv -StartupType Disabled
        ```

1. firewall uitzetten

    ```netsh advfirewall set  allprofiles state off```