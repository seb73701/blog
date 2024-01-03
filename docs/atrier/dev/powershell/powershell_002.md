 ## Trouver la version de powershell

  [ PowerShell ] - Trouver la version de PowerShell
Pour connaitre la version PowerShell, faire les actions suivantes :

Ouvrir Windows PowerShell.
Taper la commande suivante :
Commande :

$PSVersionTable
Le résultat retourne la version installée de PowerShell
PowerShell sur Windows 10
Commande :

PS C:\> $PSVersionTable
Sortie :

Name                           Value
----                           -----
PSVersion                      5.1.18362.145
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.18362.145
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
PowerShell sur Windows Server 2016
Commande :

PS C:\> $PSVersionTable
Sortie :

Name Value
---- -----
PSVersion 5.1.14393.206
PSEdition Desktop
PSCompatibleVersions {1.0, 2.0, 3.0, 4.0...}
PowerShell sur Windows Server 2012 R2
Commande :

PS C:\> $PSVersionTable
Sortie :

Name Value
---- -----
PSVersion 4.0
WSManStackVersion 3.0
SerializationVersion 1.1.0.1
CLRVersion 4.0.30319.42000
BuildVersion 6.3.9600.17400
PSCompatibleVersions {1.0, 2.0, 3.0, 4.0}
PSRemotingProtocolVersion 2.2
PowerShell sur Windows Server 2008 R2
Commande :

PS C:\> $PSVersionTable
Sortie :

Name Value
---- -----
CLRVersion 2.0.50727.5472
BuildVersion 6.1.7601.17514
PSVersion 2.0
WSManStackVersion 2.0
PSCompatibleVersions {1.0, 2.0}
SerializationVersion 1.1.0.1
PSRemotingProtocolVersion 2.1