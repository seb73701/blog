## [ PowerShell ] - Passer des arguments a un script
Explication
Lors de la création de script Powershell, il est parfois nécessaire d'utiliser des arguments afin de fournir des éléments variables qui seront exploités par le script.

L'utilisation de passage d'argument utilise la structure d'une fonction avancée.

Dans l'exemple ci-dessous, on utilisera les arguments suivants lors de l'appel du script :

Commande :

./mon_script.ps1 -Arg1 value1 -Arg2 value2
Déclaration
Pour exploiter les arguments, il faut déclarer le bloc suivant :

Code :

[CmdletBinding()]
Param
(
...
)
Ensuite, il faut définir les variables qui contiendront les valeurs passer en argument :

Code :

[CmdletBinding()]
Param
(
    [int]$Arg1,
    [string]Arg2
    ...
)
Il est possible d'ajouter des contrôles sur l'attribut CmdletBinding.

Exemple
Dans l'exemple ci-dessous, on appel la commande suivante :

Commande :

./mon_script.ps1 -Cmax 1 -Cmin 0 -Wmax 5 -Wmin 2 -Debug
Code :

[CmdletBinding()]
Param
(
    [string]$ProcessName,
    [int]$Wmin,
    [int]$Wmax,
    [int]$Cmin,
    [int]$Cmax,
    [boolean]$DebugMode,
    [int]$returnStateOK = 0,
    [int]$returnStateWarning = 1,
    [int]$returnStateCritical = 2,
    [int]$returnStateUnknown = 3
)
Liens utiles
Windows PowerShell - Fonctionnalités avancées - Attribut CmdletBinding (Edition-ENI) https://www.editions-eni.fr/Open/mediabook.aspx?idR=d2f61642bfdcad99ea647417468ada1f
Guide pratique pour valider l’entrée des paramètres - https://docs.microsoft.com/fr-fr/powershell/developer/cmdlet/how-to-validate-parameter-input