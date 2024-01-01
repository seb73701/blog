## Affichage de la liste des paramètres de bureau
[ PowerShell ] - Affichage de la liste des paramètres de bureau
Commande :

Get-CimInstance -ClassName Win32_Desktop -ComputerName .
Cette commande retourne des informations sur tous les postes de travail, qu’ils soient en cours d’utilisation ou non.

Les informations retournées par certaines classes WMI peuvent être très détaillées, et incluent souvent des métadonnées sur la classe WMI. Étant donné que la plupart de ces propriétés de métadonnées portent des noms commençant par Cim, vous pouvez filtrer les propriétés à l’aide de Select-Object. Spécifiez le paramètre -ExcludeProperty avec "Cim*" comme valeur.
Par exemple : Sortie :

Get-CimInstance -ClassName Win32_Desktop -ComputerName . | Select-Object -ExcludeProperty "CIM*"
Pour exclure les métadonnées, utilisez un opérateur de pipeline (|) pour envoyer les résultats de la commande Get-CimInstance' à Select-Object -ExcludeProperty "CIM*".