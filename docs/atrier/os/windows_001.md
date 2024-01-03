---
title: Windows - Clés de licence génériques pour l’installation de Windows
description: 
published: true
date: 2023-04-21T11:16:55.292Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:16:51.330Z
---

====== Windows - Clés de licence génériques pour l’installation de Windows ======

==== Présentation ====

Si comme moi vous aimez faire des tests, installer des machines virtuelles, alors il vous sera peut être nécessaire d’utiliser des clés de licence génériques. Ces clés vous seront utiles pour l’installation du système, cependant, elles ne vous permettront pas l’activation de votre version de Windows.

Pour la plupart des Produits Microsoft, il existe des clés génériques qui permettent de faire l’installation du logiciel voulu, ou faciliter une mise à jour.
Ces clés sont aussi appelées « Clés d’installation des clients KMS ». Ces clés ne servent qu’a l’installation et non à l’activation du produit, sauf pour un client-serveur (ordinateur hôte KMS).
Windows 10 – Windows 8.1 – Windows 8 – Windows Server 2012 R2 – Windows 7 – Windows Server 2016 – Windows Server 2012 – Windows Server 2008 R2 – Office 2013 – Office 2010 – Viso.

====  Clés d’installation des clients Windows 10 ====

```
Windows 10 Professional :               W269N-WFGWX-YVC9B-4J6C9-T83GX
Windows 10 Enterprise :                 NPPR9-FWDCX-D2C8J-H872K-2YT43
Windows 10 Famille :                    TX9XD-98N7V-6WMQ6-BX7FG-H8Q99
Windows 10 Education :                  NW6C2-QMPVW-D7KKK-3GKT6-VCFB2
Windows 10 Enterprise 2015 LTSB :       WNMTR-4C88C-JK8YV-HQ7T2-76DF9
```

==== Clés d’installation des clients Windows 7 ====

```
Windows 7 Professional :                FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4
Windows 7 Enterprise :                  33PXH-7Y6KF-2VJC9-XBBR8-HVTHH
```

==== Clés d’installation des clients Windows Server 2016 ====

```
Windows Server 2016 Datacenter :        CB7KF-BWN84-R7R2Y-793K2-8XDDG
Windows Server 2016 Standard :          WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY
Windows Server 2016 Essentials :        JCKRF-N37P4-C2D82-9YXRT-4M63B
```

==== Clés d’installation des clients Windows Server 2012 ====

```
Windows Server 2012 R2 Datacenter :     W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9
Windows Server 2012 R2 Essentials :     KNC87-3J2TX-XB4WP-VCPJV-M4FWM
Windows Server 2012 R2 Server Standard : D2N9P-3P6X9-2R39C-7RTCD-MDVJX
Windows Server 2012 :                   BN3D2-R7TKB-3YPBD-8DRP2-27GG4
Windows Server 2012 Server Standard :   XC9B7-NBPP2-83J2H-RHMBY-92BT4
Windows Server 2012 Datacenter :        48HP8-DN98B-MYWDG-T2DCC-8W83P
Windows Server 2012 MultiPoint Standard : HM7DN-YVMH3-46JC3-XYTG7-CYQJJ
Windows Server 2012 MultiPoint Premium : XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G
```

==== Clés d’installation des clients Windows Server 2008 ====

```
Windows Server 2008 R2 Standard :       YC6KT-GKW9T-YTKYR-T4X34-R7VHC
Windows Server 2008 R2 Enterprise :     489J6-VHDMP-X63PK-3K798-CPX3Y
Windows Server 2008 R2 Web :            6TPJF-RBVHG-WBW2R-86QPH-6RTM4
Windows Server 2008 R2 HPC edition :    TT8MH-CG224-D3D7Q-498W2-9QCTX
Windows Server 2008 R2 Datacenter :     74YFP-3QFB3-KQT8W-PMXWJ-7M648
```


===== Sources =====

  * [[http://technet.microsoft.com/en-us/library/jj612867.aspx]]

  * [[http://technet.microsoft.com/fr-fr/library/jj612867.aspx]]

  * [[http://technet.microsoft.com/fr-fr/library/dn385360.aspx]]


Clé Windows Server 2019 Standard : N69G4-B89J2-4G8F4-WWYCC-J464C
Clé Windows Server 2019 Datacenter : WMDGN-G9PQG-XVVXX-R3X43-63DFG
Clé Windows Server 2019 Essentials : WVDHN-86M7X-466P6-VHXV7-YY726

---

a solution se trouve du côté de DISM.

Mais avant tout, vous devez avoir la clé produit générique :

Windows Server 2016 Datacenter : CB7KF-BWN84-R7R2Y-793K2-8XDDG
Windows Server 2016 Standard : WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY
Windows Server 2016 Essentials : JCKRF-N37P4-C2D82-9YXRT-4M63B

Windows Server 2019 Datacenter : WMDGN-G9PQG-XVVXX-R3X43-63DFG
Windows Server 2019 Standard : N69G4-B89J2-4G8F4-WWYCC-J464C
Windows Server 2019 Essentials : WVDHN-86M7X-466P6-VHXV7-YY726

Windows Server 2022 Datacenter WX4NM-KYWYW-QJJR4-XV3QB-6VM33
Windows Server 2022 Standard VDYBN-27WPP-V4HQT-9VMD4-VMK7H

Attention. Ces clés ne sont pas des licences, ce sont des clés produits qui indiquent quel type de produit. Inutile de croire que ca vous exemptera l’achat de la licence.

Vous ouvrez une fenêtre Powershell avec une élévation de privilèges, soit « Administrateur ».

Et vous tapez :
DISM /online /Set-Edition:ServerStandard /ProductKey:XXXXX-YYYYY-XXXXX-YYYYY-XXXXX /AcceptEula

Note : Pour la mise à niveau d’une édition Standard, utiliser le Set-Edition:ServerStandard, pour un Datacenter, utiliser Set-Edition:ServerDatacenter

