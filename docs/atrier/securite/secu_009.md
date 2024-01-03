---
title: Chipsec, un outil pour les tests de conformité des firmwares
description: 
published: true
date: 2023-04-21T10:40:24.229Z
tags: bios, matériel, uefi, firmware, x86
editor: markdown
dateCreated: 2023-04-21T10:40:19.773Z
---

# Chipsec, un outil pour les tests de conformité des firmwares

> La vérification qu'une plateforme PC est correctement configurée pour limiter l'exposition de celle-ci est fastidieuse et complexe si elle est effectuée manuellement. L'outil Chipsec permet de faciliter grandement ces opérations, tant par son support des différents modes d'accès que par celui des différentes plateformes. Cet article présente son fonctionnement interne et son utilisation à des fins de tests de conformité.
{.is-info}

## 1. Contexte
Depuis quelques années, l’ANSSI est impliquée dans les procédures d’acquisition des matériels bureautiques pour l’administration française, afin de compléter les exigences fonctionnelles (taille, performances, etc.) par des exigences de sécurité. Ces exigences, publiées [1] sur le site de l’ANSSI, permettent de renforcer le niveau de sécurité des plateformes (postes clients et portables) et sont regroupées en plusieurs familles :

- maîtrise de la plateforme : pour s’assurer que les postes acquis sont sous le contrôle de l’administration ;
- caractéristiques matérielles : pour s’assurer que des dispositifs matériels bénéfiques à la sécurité (I/OMMU, TPM, etc.) soient présents ;
- caractéristiques du firmware : fonctionnalités de sécurité offertes par le firmware UEFI (ou BIOS), possibilités de configurations et protection du firmware lui-même ;
- maintien en condition de sécurité : pour s’assurer que le niveau de sécurité reste satisfaisant dans le temps.


La troisième famille comporte un certain nombre d’exigences sur la protection logicielle de la plateforme, en particulier eu égard aux vulnérabilités identifiées ces dernières années au niveau des BIOS. Dans la plupart des cas, ces vulnérabilités étaient liées à une mauvaise configuration de la plateforme au démarrage, dans d’autres cas, il s’agissait de vulnérabilités intrinsèques à l’architecture Intel, corrigées par l’ajout de fonctionnalités à activer.

Dans les deux cas, la prise en compte de ces vulnérabilités se fait via la configuration de la plateforme au démarrage, ce qui est habituellement du ressort du développeur du BIOS et du fournisseur de la machine. La vérification de la bonne configuration d’une plateforme peut se faire manuellement à l’aide de la documentation (processeurs et chipsets Intel en particulier) et d’outils tels que pcilib (lspci, setpci). Ce mode manuel est présenté dans l’article Relevé de configuration matérielle sur plateforme x86 dans ce même numéro de MISC, sa lecture est recommandée avant de lire celui-ci.

La taille et la complexité de la documentation, les dépendances nombreuses entre registres de configuration et les différents modes d’accès aux registres rendent cette vérification très fastidieuse, aussi nous avons décidé de nous reposer sur l’outil Chipsec pour valider la configuration des plateformes acquises par l’administration.

## 2. Pourquoi utiliser Chipsec ?
Chipsec [2] est un framework Python open source (publié en mars 2014) dédié à l’analyse du niveau de sécurité des plateformes Intel x86 en vérifiant les mécanismes de sécurité bas niveau, les paramètres de configuration de composants matériels ou encore des micrologiciels (BIOS/UEFI). Chipsec peut être lancé depuis un système d’exploitation (Windows, Linux, macOS) ou un shell UEFI.

Chipsec est composé de deux scripts principaux : chipsec_main.py et chipsec_util.py.

Le premier a été conçu pour détecter automatiquement les mauvaises sécurisations à travers une série de modules (ou plugins). Chacun de ces derniers a une fonction précise et cherche à détecter une configuration permettant l’exploitation d’une vulnérabilité spécifique. Chacune des vulnérabilités testées a généralement fait l’objet d’une publication et elles ont une finalité commune : exécuter du code malveillant sur le système d’exploitation depuis un accès plus bas niveau (par exemple, en modifiant le code du BIOS, en exécutant du code malveillant en mode SMM, ou en exploitant des options CPU mal documentées).

L’exécution de chipsec_main renvoie un rapport de résultats qui doit être analysé attentivement afin de valider que tous les résultats sont conformes (une seule non-conformité peut mener à la compromission de la plateforme).

En cas d’échec, l’analyste peut se reposer sur la sortie des différents modules pour comprendre la raison de la non-conformité.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_02.png">

*Figure 1 : Paramètre testé et conforme.*

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_03.png">

*Figure 2 : Paramètre non conforme détecté par Chipsec.*


Ce mode est celui principalement utilisé pour la validation des configurations.

Le second script, chipsec_util, est davantage utilisé pour réaliser des opérations manuelles en communiquant de manière ciblée avec un matériel.

Dans le cadre des évaluations ANSSI, pour comprendre le résultat d’un plugin ou pour extraire une donnée non prise en charge par l’un des plugins ou encore pour extraire le firmware, cet outil s’est avéré très précieux, le nombre de protocoles et de moyens d’accès aux registres de configuration pris en charge étant assez important. Il existe ainsi des modules PCI, MMIO, SPI, UEFI, TPM, IOMMU, etc.

## 3. Détection de plateformes vulnérables via Chipsec
Nous n’allons pas présenter ici la totalité des vulnérabilités testées par Chipsec, mais présenter quelques modules adossés à des vulnérabilités ayant eu un certain retentissement médiatique dans la communauté et un impact important sur la sécurité de la plateforme. Un document complet détaillant les tests menés par Chipsec est toutefois présent sur le GitHub de l’ANSSI [3] et indique également le protocole de test permettant de valider la conformité d’une plateforme soumise.

### 3.1 Détection des vulnérabilités propres à la Flash SPI via les modules bios_wp et bios_smi
Un registre très important est celui protégeant en écriture des régions de la flash SPI une fois le système démarré : BIOS_CNTL (Bios Control Register, maintenant appelé BC) du PCH. Contenant 3 bits d’une importance cruciale pour protéger le firmware des altérations, il est probablement le registre le plus exploité par les vulnérabilités découvertes jusqu’à maintenant.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_04.png">

*Figure 3 : Contrôle du registre BIOS_CNTL via bios_wp.*

Chipsec vérifie notamment ces valeurs à travers le module bios_wp en vérifiant les bits :

- BIOSWE : BIOS Write Enable, si ce bit est à 1, un accès à la flash SPI en écriture sera possible depuis le système d’exploitation (avec les privilèges du ring 0) ;
- BLE : BIOS Lock Enable
  - si ce bit est à 1, à chaque tentative de modification du bit BIOSWE, une interruption SMI générée par le PCH empêchera tout simplement l’opération ;
  - par contre, si ce bit vaut 0 alors, depuis l’espace noyau, il sera possible de modifier le bit BIOSWE pour déverrouiller la flash SPI et donc altérer son contenu.

Cependant, cette protection SMI a déjà pu être contournée à plusieurs reprises notamment à travers l’attaque SpeedRacer [4]. Celle-ci consiste à être plus rapide que le contrôle SMI en jouant avec plusieurs cœurs du processeur afin d’écrire dans la région du BIOS avant le déclenchement de l’interruption et l’entrée en SMM.

> System Management Interrupt, interruption qui déclenche automatiquement et de façon transparente une bascule vers le System Management Mode et donc l’exécution de code fourni par le BIOS, avec des privilèges élevés (supérieur au CPL 0).
{.is-info}


Une autre attaque nommée Sandman [5] consiste quant à elle à désactiver complètement les SMI sur le système pour empêcher le contrôle en mode SMM et permettre la modification de BIOSWE. Cette désactivation des sources SMI se fait en passant le bit GBL_SMI_EN (du registre SMI_EN) à 0. Ce registre est lui-même verrouillé par le bit SMI_LOCK (du registre GEN_PMCON_1) positionné à 0. Cette attaque a été utilisée en pratique pour contourner Secure Boot [6].

Chipsec vérifie ces deux bits via le module bios_smi.

Une protection supplémentaire repose sur le bit SMM_BWP (SMM BIOS Write Protect Disable) du registre BIOS_CNTL, qui permet de protéger le BIOS d’une telle attaque en protégeant sa modification même si le contrôle SMI n’est pas configuré ou échoue. Si ce bit est à 1, la région SPI propre au BIOS ne pourra être modifiée que si le processeur s’exécute en mode SMM et donc que le code provient du BIOS. Ce mécanisme est typiquement utilisé lors d’une mise à jour du BIOS pour réécrire le contenu de la mémoire flash avec le nouveau BIOS.

Alors que les attaques présentées jusque-là ne nécessitaient qu’un accès au noyau (par un pilote par exemple), il est nécessaire, avec cette protection en place, d’exécuter du code en espace SMM pour altérer le BIOS, compliquant ainsi les exploitations. L’accès à un tel espace nécessite d’une part une élévation de privilèges à partir de l’espace noyau et d’autre part une porte dérobée SMM [7].

### 3.2 Détection des vulnérabilités S3 Resume via le module uefi.s3bootscript
En 2015, des chercheurs sont parvenus à infecter un firmware UEFI depuis le mode kernel alors que la flash SPI était correctement protégée par les bits de protection proposés par le registre BIOS_CNTL [8][9]. Deux vulnérabilités découvertes par les chercheurs et propres au mode de démarrage ACPI S3 Resume (sortie de veille de la plateforme) sont exploitées dans leur scénario.

Durant la phase de boot normal et notamment la phase DXE (chargement des drivers EFI) l’état du CPU et du chipset (comprenant donc les registres du PCH tels que BIOS_CNTL), est sauvegardé en mémoire dans une structure appelée UEFI boot script table. Cette table est ensuite utilisée lors de la phase de sortie de mise en veille (S3 resume) afin de réinitialiser plus rapidement l’état de la plateforme, en évitant notamment de devoir recharger tous les drivers EFI.

La première vulnérabilité exploite le fait que la sauvegarde de la table est réalisée durant la phase de boot avant que les registres aient été complètement verrouillés par le firmware EFI. Ainsi, au réveil de la plateforme plusieurs registres (dont BIOS_CNTL) ont un état déverrouillé et des valeurs telles que BIOSWP et SMM_BWP modifiables depuis un accès noyau. L’altération du firmware EFI devient alors possible après avoir reparamétré les valeurs du registre. Des mesures de sécurité ont depuis été poussées par Intel en recommandant par exemple d’effectuer la sauvegarde de l’état du CPU et du chipset après la demande de mise en veille S3 suspend et donc lorsque les registres sont correctement verrouillés.

Chipsec vérifie notamment si la plateforme testée est vulnérable via le module s3bootscript et le module tools.uefi.s3script_modify aide également à effectuer des tests concrets de modification des scripts.

Pour renforcer davantage la sécurité de la flash SPI, cinq nouveaux registres supplémentaires du contrôleur SPI, PRx (Protected Range), spécifiques à chaque région de la flash SPI, ont vu le jour pour définir les droits d’écriture (WP) et lecture (RP) sur chacune d’elles et ainsi empêcher toute altération de la cible prioritaire, le contenu de la flash SPI. Cette protection, dont le code fonctionne indépendamment du mode SMM, empêche l’écriture des régions de la flash via un code malveillant s’exécutant en mode SMM.

La vérification de ces registres est effectuée par le module Chipsec bios_wp (vu au paragraphe précédent).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_06.png">

*Figure 4 : Contrôle des registres « Protected Range » via bios_wp.*

## 4. Extraction des registres avec Chipsec
Comme vu dans l’article Relevé de configuration matérielle sur plateforme x86, la récupération d’informations dans les registres de configuration peut se faire de différentes façons suivant le type de bus et de périphérique (processeur, périphérique PCI, etc.). La localisation des informations (quel périphérique est concerné, quel registre sur ce périphérique, et où il se situe) est aussi complexe, la documentation (Intel en particulier) étant complexe et évoluant au fil du temps.

Chipsec fournit une abstraction logicielle permettant à l’analyste de se concentrer sur les éléments importants. Les deux scripts (chipsec_main fournissant le rapport complet, et chipsec_util permettant les interrogations ciblées) reposent sur un ensemble de fonctions fournissant à la fois les accès matériels et la vision cohérente de la plateforme à l’aide de fichiers de configuration fournis avec Chipsec.

### 4.1 Vérification du support de la plateforme
À chaque lancement de Chipsec, la fonction detect_platform de chipsec/chipset.py identifie les vendor ID et product ID du contrôleur de la RAM (00:0.0) et du chipset ou PCH (00:1F.0) via des requêtes PCI pour en déduire le modèle de la plateforme et s’assurer ainsi que Chipsec sera pris en charge.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_07.png">

*Figure 5 : Affichage de la plateforme détectée au lancement de Chipsec.*

L’extrait de code ci-dessous indique que le fichier de configuration à utiliser sera cfg/HSW.XML pour la plateforme Desktop 4th Generation Core Processor (Haswell CPU / Lynx Point PCH).

```yaml
# 4th Generation Core Processor Family (Haswell)
Chipset_Dictionary[0x0C00].append({
    'name' : 'Haswell',
    'id' : CHIPSET_ID_HSW ,
    'code' : CHIPSET_CODE_HSW,
    'longname' : 'Desktop 4th Generation Core Processor (Haswell CPU / Lynx Point PCH)'
})
```

*Déclaration du support d’une plateforme dans chipsec/chipset.py.*

 

La base de traduction des identifiants VID:DID en nom de plateforme est fournie avec Chipsec, mais la source est identique à celle de pcilib et provient du dépôt GitHub PCIIDS [10].

Si la plateforme n’est pas connue de Chipsec alors l’opération échoue et Chipsec doit alors être adapté via les fichiers de configuration disponibles.

### 4.2 Spécifications des plateformes
Une force de Chipsec est que la présence des fichiers de configuration propres à un modèle de chipset de carte mère (chipsec/cfg/common.xml et chipsec/cfg/TAG.xml) remplace utilement les spécifications fournies par exemple par Intel. Cette configuration, issue des spécifications Intel et maintenue par les développeurs Chipsec (eux-mêmes développeurs chez Intel) spécifie les adresses des registres, leur mode d’accès ainsi que leur sémantique. Grâce à cette base de données, il n’est plus nécessaire de calculer ou de vérifier manuellement les paramètres comme effectué précédemment via setpci et le gain de temps est donc considérable.

```xml
<device name="EHCI1" bus="0" dev="0x1D" fun="0" vid="0x8086" />
<device name="LPC"   bus="0" dev="0x1F" fun="0" vid="0x8086" />
<device name="SATA1" bus="0" dev="0x1F" fun="2" vid="0x8086" />
```
*Adresses PCI des composants du PCH.*

 
```xml
<register name="BC" type="pcicfg" device="LPC" offset="0xDC" size="1" desc="BIOS Control">
  <field name="BIOSWE" bit="0" size="1" desc="BIOS Write Enable"/>
  <field name="BLE"     bit="1" size="1" desc="BIOS Lock Enable"/>
  <field name="SRC"     bit="2" size="2" desc="SPI Read Configuration"/>
  <field name="TSS"     bit="4" size="1" desc="Top Swap Status"/>
  <field name="SMM_BWP" bit="5" size="1" desc="SMM BIOS Write Protection"/>
</register>
```
*Emplacement et taille des variables du registre BIOS_CNTL du PCH.*

``` xml
<bar
    name="SPIBAR"
    bus="0" dev="0x1F" fun="0" reg="0xF0"
    width="4" mask="0xFFFFC000" size="0x200" enable_bit="0"
    desc="SPI Controller Register Range" offset="0x3800"
/>insérer l'image ici
```
*Adresse PCI et registre stockant l’adresse mémoire SPIBAR.*

 
```xml
<register name="HSFS" type="mmio" bar="SPIBAR" offset="0x04" size="2"
    desc="Hardware Sequencing Flash Status Register">
  <field name="FDONE"   bit="0" size="1" desc="Flash Cycle Done"/>
  <field name="FCERR"   bit="1" size="1" desc="Flash Cycle Error"/>
  <field name="AEL"     bit="2" size="1" desc="Access Error Log"/>
  <field name="BERASE" bit="3" size="2" desc="Block/Sector Erase Size"/>
  <field name="SCIP"    bit="5" size="1" desc="SPI cycle in progress"/>
  <field name="FDOPSS" bit="13" size="1"
    desc="Flash Descriptor Override Pin-Strap Status"/>
  <field name="FDV"     bit="14" size="1"
    desc="Flash Descriptor Valid"/>
  <field name="FLOCKDN" bit="15" size="1"
    desc="Flash Configuration Lock-Down"/>
</register>
```
*Emplacement mémoire du registre HSFS du contrôleur SPI et de ses variables.*

### 4.3 Des modules au pilote chipsec.ko
#### 4.3.1 Utilisation des fichiers de configuration
Depuis les modules Python (chipsec/modules/MODULE.py), les différents registres peuvent être extraits via des fonctions (déclarées dans chipsec/chipset.py) telles que read_register, get_control ou get_register_field.

```python
ble = self.cs.get_control('BiosLockEnable', with_print=True)
#bioswe = self.cs.get_register_field('BC', reg_value, 'BIOSWE')
bioswe = self.cs.get_control('BiosWriteEnable')
#smmbwp = chipsec.chipset.get_register_field(self.cs, 'BC', reg_value, 'SMM_BWP')
smmbwp = self.cs.get_control( 'SmmBiosWriteProtection' )
```
*Exemple du module bios_wp.py.*

```python
def check_fd_security_override_strap(self):
    self.logger.start_test( "SPI Flash Descriptor Security Override Pin-Strap" )
    
    if not self.cs.is_register_defined( 'HSFS' ):
        self.logger.error( "Couldn't find definition of required configuration
        registers (HSFS)" )
        return ModuleResult.ERROR
    
    hsfs_reg = self.cs.read_register( 'HSFS' )
    self.cs.print_register( 'HSFS', hsfs_reg )
    fdopss = self.cs.get_register_field( 'HSFS', hsfs_reg, 'FDOPSS' )
```
*Exemple du module spi_fdopss.py.*

 

Les controls sont des abstractions propres à Chipsec qui permettent d’exprimer des informations de haut niveau (par exemple, le verrouillage en écriture de la zone de la flash contenant le BIOS) qui peuvent reposer sur différents registres suivant les plateformes. Ces controls sont définis dans les fichiers de configuration XML de chaque plateforme et indiquent les registres et champs à consulter pour cette plateforme. Par exemple, le control SMILock est défini différemment selon la plateforme :

Dans le cas général :

```xml
<control name="SMILock" register="GEN_PMCON_1" field="SMI_LOCK" desc="SMI
Global Configuration Lock"/>
```

Sur les PCH Intel Series 300 :

```xml
<control name="SMILock" register="GEN_PMCON_2" field="SMI_LOCK" desc="SMI
Global Configuration Lock"/>
```

Les fonctions proposées pour la lecture des registres sont toutes disponibles dans chipsec/chipset.py.

```python
# Main functionality to read/write configuration registers
# based on their XML configuration
#
# is_register_defined
#   checks if register is defined in the XML config
# get_register_bus/get_device_bus
#   returns list of buses device/register was discovered on
# read_register/write_register
#   reads/writes configuration register (by name)
# get_register_field (set_register_field)
#   reads/writes the value of the field (by name) of configuration register (by register value)
# read_register_field (write_register_field)
#   reads/writes the value of the field (by name) of configuration register (by register name)
# register_has_field
#   checks if the register has specific field
# print_register
#   prints configuration register
# get_control/set_control
#   reads/writes some control field (by name)
```
*Extrait des fonctions proposées par chipsec/chipset.py.*

 

La suite des opérations sera différente selon le moyen d’accès utilisé pour accéder au registre (par exemple, PCI ou MMIO). Cette spécificité a été traduite dans le fichier de configuration XML par le champ type lors de la déclaration du registre (voir illustration plus haut).

```xml
<register
    name="BC"
    type="pcicfg"
    device="LPC"
    offset="0xDC"
    size="1"
    desc="BIOS Control"
>
```
*Registre BC accessible via PCI (pcicfg).*

 

Les différentes valeurs de ce champ sont notamment déclarées dans chipsec/chipset.py :

```python
class RegisterType:
    PCICFG    = 'pcicfg'
    MMCFG     = 'mmcfg'
    MMIO      = 'mmio'
    MSR       = 'msr'
    PORTIO    = 'io'
    IOBAR     = 'iobar'
    MSGBUS    = 'msgbus'
    MM_MSGBUS = 'mm_msgbus'
    MEMORY    = 'memory'
```
*Les moyens d’accès aux registres supportés.*

 

Ainsi, le développement de nouveaux plugins est très simplifié et c’est donc la configuration chargée qui définira la suite des opérations à lancer : requêtes PCI, MMIO etc. via des fonctions spécifiques telles que pci.read_* (de chipsec/hal/pci.py) pour des accès aux registres en PCI ou mmio.read_* (de chipsec/hal/mmio.py) pour les accès au registre en MMIO.

```python
def read_register(self, reg_name, cpu_thread=0, bus_index=0):
    reg = self.get_register_def( reg_name, bus_index )
    rtype = reg['type']
    reg_value = 0
    if RegisterType.PCICFG == rtype:
        b = int(reg['bus'], 16)
        d = int(reg['dev'], 16)
        f = int(reg['fun'], 16)
        o = int(reg['offset'], 16)
        size = int(reg['size'], 16)
        if   1 == size: reg_value = self.pci.read_byte ( b, d, f, o )
        elif 2 == size: reg_value = self.pci.read_word ( b, d, f, o )
        elif 4 == size: reg_value = self.pci.read_dword( b, d, f, o )
        elif 8 == size: reg_value = (self.pci.read_dword( b, d, f, o+4 ) << 32) | self.pci.read_dword(b, d, f, o)
    elif RegisterType.MMCFG == rtype:
        reg_value = self.mmio.read_mmcfg_reg(int(reg['bus'],16), int(reg['dev'],16), int(reg['fun'],16), int(reg['offset'],16), int(reg['size'],16) )
    elif RegisterType.MMIO == rtype:
        reg_value = self.mmio.read_MMIO_BAR_reg(reg['bar'], int(reg['offset'],16), int(reg['size'],16) )
```
*Extrait de read_register de chipsec/chipset.py.*

 

#### 4.3.2 Pour des accès PCI
Le module principal réalisant les requêtes PCI est situé dans le fichier chipsec/hal/pci.py et comprend différentes fonctions prenant en argument une adresse PCI (B:D.F) et un offset représentant le début d’un registre (de 8, 16 ou 32 bits) : read_byte, read_word, read_dword. Chacune de ces fonctions fait ensuite appel à la fonction read_pci_reg(bus, device, function, address, size) (disponible dans chipsec/helper/linux/linuxhelper.py).

```python
    def read_pci_reg( self, bus, device, function, offset, size = 4 ):
        _PCI_DOM = 0 #Change PCI domain, if there is more than one.
        d = struct.pack("5"+self._pack, ((_PCI_DOM << 16) | bus), ((device << 16) | function), offset, size, 0)
        try:
            ret = self.ioctl(IOCTL_RDPCI, d)
        except IOError:
            if logger().DEBUG: logger().error("IOError\n")
            return None
        x = struct.unpack("5"+self._pack, ret)
        return x[4]
```
*Appel système ioctl pour un accès PCI.*

 

Cette fonction prépare les données à transmettre au module (B:D.F et size) dans un buffer, et initie ensuite un appel système ioctl. Sous Linux, l’appel système ioctl permet d’échanger des données avec le noyau au travers d’un pseudo-fichier. Le prototype de l’appel ioctl(2) est :

```python
int ioctl(int fd, unsigned long request, ...);
```

Le fd (file descriptor) pointe vers un pseudo-fichier créé par un driver noyau, tandis que request est un code spécifique identifiant la requête et que le troisième argument est un pointeur vers un buffer contenant les arguments. Les codes de requête font partie de l’interface entre le code python et le code noyau, par exemple 0x3 pour READ_PCI.

```python
IOCTL_BASE                     = 0x0
IOCTL_RDIO                     = 0x1
IOCTL_WRIO                     = 0x2
IOCTL_RDPCI                    = 0x3
IOCTL_WRPCI                    = 0x4
IOCTL_RDMSR                    = 0x5
IOCTL_WRMSR                    = 0x6
IOCTL_CPUID                    = 0x7
IOCTL_GET_CPU_DESCRIPTOR_TABLE = 0x8
IOCTL_HYPERCALL                = 0x9
IOCTL_SWSMI                    = 0xA
IOCTL_LOAD_UCODE_PATCH         = 0xB
IOCTL_ALLOC_PHYSMEM            = 0xC
IOCTL_GET_EFIVAR               = 0xD
IOCTL_SET_EFIVAR               = 0xE
IOCTL_RDCR                     = 0x10
IOCTL_WRCR                     = 0x11
IOCTL_RDMMIO                   = 0x12
IOCTL_WRMMIO                   = 0x13
IOCTL_VA2PA                    = 0x14
IOCTL_MSGBUS_SEND_MESSAGE      = 0x15
IOCTL_FREE_PHYSMEM             = 0x16
```
*Codes IOCTL disponibles (nr).*

 

Les fichiers chipsec/helper/linux/linuxhelper.py et chipsec/drivers/linux/include/chipsec.h définissent clairement les structures et les autres paramètres utilisés :

```python
def compute_ioctlbase(self,itype = 'C'):
    #define _IOWR(type,nr,size) _IOC(_IOC_READ|_IOC_WRITE,(type),(nr),(_IOC_TYPECHECK(size)))
```
*Définition de _IOWR.*

```python
#define IOCTL_RDPCI _IOWR(IOCTL_NUM, 0x3, int*)
```
*Définition de IOCTL_RDPCI.*
 

#### 4.3.3 Pour des accès MMIO
En ce qui concerne les accès MMIO, le module principal est chipsec/hal/mmio.py et propose des fonctions prenant en paramètre une adresse mémoire et un offset représentant le début d’un registre (8, 16, 32 bits ou défini par l’utilisateur) : read_MMIO_reg_byte, read_MMIO_reg_word, read_MMIO_reg_dword, read_MMIO_reg. Une autre fonction, read_MMIO_BAR_reg, prend en paramètre un nom d’espace mémoire BAR et permet de réaliser la lecture d’un registre en se basant simplement sur le nom déclaré dans la configuration XML.

```xml
<bar name="SPIBAR"
    bus="0" dev="0x1F" fun="0" reg="0xF0"
    width="4" mask="0xFFFFC000"
    size="0x200" enable_bit="0"
    desc="SPI Controller Register Range"
    offset="0x3800"
/>
```
*SPIBAR déclaré dans le fichier de configuration XML.*

 

Chacune de ces fonctions fait ensuite appel à read_mmio_reg(self, phys_address, size) disponible dans chipsec/helper/linux/linuxhelper.py pour initier un appel système ioctl, de façon identique aux accès PCI (le code de requête pour RDMMIO vaut 0x12).

```python
def read_mmio_reg(self, phys_address, size):
    in_buf = struct.pack( "2"+self._pack, phys_address, size)
    out_buf = self.ioctl(IOCTL_RDMMIO, in_buf)
    reg = out_buf[:size]
    return defines.unpack1(reg, size)
```
*Appel système ioctl pour un accès MMIO.*

 

Un exemple de trace d’exécution de chipsec_main via strace montre l’utilisation d’un ioctl pour un accès PCI suivi d’un ioctl pour un accès MMIO.

Une option intéressante de chipsec_main est --hal qui permet d’afficher les informations d’accès MMIO, PCI et CPU ainsi que les valeurs extraites.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_09.png">

*Figure 6 : Mode --hal de chipsec_main.*

#### 4.3.4 Module noyau
Comme évoqué précédemment, la lecture ou l’écriture de registres matériels nécessitent des permissions élevées sur un système. L’accès à l’espace des ports ou à l’espace mémoire physique nécessite au moins les permissions du super-utilisateur (typiquement root sous un système Unix). Sur des systèmes récents et avec un niveau de sécurité à l’état de l’art, même ces permissions ne sont parfois pas suffisantes.

L’accès complet à la mémoire physique (exposé sous Linux au travers du pseudo-fichier en mode caractère /dev/mem) est ainsi restreint à des zones particulières : sur architecture x86, uniquement le 1er Mo de mémoire physique ainsi que les zones PCI MMIO sont accessibles depuis le noyau 2.6.27, au travers de l’option CONFIG_STRICT_DEVMEM, habituellement activée sur les distributions standards.

Sur des noyaux plus récents (5.4 upstream, mais depuis plus longtemps sur des distributions comme Debian, Red Hat ou Ubuntu), la présence de la fonctionnalité Lockdown en présence de SecureBoot, interdit complètement l’accès à /dev/mem ainsi qu’à l’espace de configuration des ports.

Il n’est donc pas aisé sur une distribution standard d’accéder aux informations nécessaires, aussi Chipsec vient avec un driver noyau pour les différents systèmes d’exploitation. C’est ce driver noyau (drivers/linux/chipsec.ko), chargé au démarrage de Chipsec (par chipsec/helper/linux/linuxhelper.py), qui effectue les accès matériels, au service du code Python. L’interface entre les deux passe par un nœud de périphérique, un pseudo-fichier en mode caractère /dev/chipsec, sur lequel le code en espace utilisateur effectue des ioctl, comme vu aux paragraphes précédents.

### 4.4 Lister les contrôleurs périphériques PCI
Comme indiqué plus haut, le code dans chipsec/hal/pci.py est en charge de l’énumération des périphériques PCI, que ce soit de manière automatisée via chipsec_main ou avec l’option pci enumerate de chipsec_util.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_10.png">

*Figure 7 : Énumération des périphériques PCI via chipsec.*

L’énumération se fait de façon extensive en examinant, pour tous les bus (de 0 à 255), device (de 0 à 31) et function (de 0 à 7) le contenu des 8 premiers octets du PCI Configuration Space, qui correspondent au Vendor ID et Product ID du périphérique concerné (ou 0xFFFF/0XFFFF en cas d’absence ou de masquage du périphérique).

### 4.5 Extraire les valeurs des registres d’un périphérique PCI
À travers ses plugins et ses configurations XML, chipsec_main réalise automatiquement l’extraction des registres spécifiques. Par exemple, le plugin bios_wp récupère le contenu du registre BIOS_CNTL dans le PCH et ses valeurs telles que BIOSWE, BLE, etc. en indiquant si celles-ci sont conformes d’un point de vue sécurité.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_11.png">

*Figure 8 : Résultat du module bios_wp (accès PCI).*

La même opération peut être effectuée manuellement, sans se baser sur les fichiers de configuration, à l’aide de chipsec_util en spécifiant l’adresse du périphérique.

On peut ainsi effectuer un dump complet du PCI Configuration Space (similaire à lspci -xxx) :

```
# chipsec_util pci dump 00 1F 00
```
ou encore demander explicitement le contenu du registre à l’aide de son adresse (de façon similaire à setpci) :

```
# chipsec_util pci 00 1F 00 BC
```

### 4.6 Accès MMIO
Chipsec dispose d’un module permettant les accès à des registres projetés en mémoire (MMIO). Ce module peut être utilisé directement pour lire des adresses arbitraires en mémoire, mais il est particulièrement utile pour récupérer des informations de configuration de périphériques pour lesquels celles-ci sont projetées en mémoire à des adresses précises, avec parfois plusieurs niveaux d’indirection, comme par exemple le contrôleur SPI vu dans l’article sur le relevé de configuration.

Tous les modules chipsec commençant par spi_ ont ainsi besoin d’accéder au contrôleur SPI via une zone mémoire projetée à l’adresse SPIBAR (SPI Base Address).

Tous les éléments nécessaires au calcul de cette adresse mémoire (emplacement du registre RCRA indiquant l’adresse du RCRB, constante à ajouter) sont stockés dans les fichiers de configuration XML et facilitent l’automatisation. Une requête PCI vers le PCH et un calcul suffisent ainsi à l’identification de l’adresse mémoire SPIBAR (0xFED1F800).

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_12.png">

*Figure 9 : Calcul automatique de l’adresse SPIBAR.*

Il est possible là encore de trouver manuellement l’adresse de SPIBAR, en effectuant à la main les différentes requêtes via chipsec_util pci.

Une fois SPIBAR identifiée, le module MMIO (situé dans le fichier chipsec/hal/mmio.py) peut être utilisé pour extraire les différents registres du contrôleur SPI, tel que HSFS.

chipsec_main réalise automatiquement l’extraction des registres nécessaires. Par exemple, le plugin spi_fdopss (SPI Flash Descriptor Security Override) récupère automatiquement le contenu du registre HSFS (Hardware Sequencing Flash Status Register, offset 0x4 de SPIBAR) et ses bits tels que FDOPSS, FLOCKDN etc. en indiquant si leurs valeurs sont conformes d’un point de vue sécurité.

<img src="https://connect.ed-diamond.com/sites/default/files/magazines/misc/misc-109/chipsec_figure_13.png">

*Figure 10 : Résultat du module spi_fdopss (accès MMIO).*

Encore une fois, la même opération peut être effectuée manuellement, sans se baser sur les fichiers de configuration, à l’aide de chipsec_util. Chipsec connaît déjà un certain nombre d’espaces mémoire importants, il est donc possible de ne spécifier que son nom :

```
# chipsec_util mmio dump SPIBAR
```

Il est aussi possible de spécifier l’offset et la taille d’un registre (par exemple, 0x4 pour HSFS) :

```
# chipsec_util mmio read SPIBAR 4 4
```

Il est par contre ensuite nécessaire d’évaluer manuellement la valeur retournée par Chipsec et sa conformité.

La liste des espaces mémoire connus de Chipsec peut être obtenue avec la commande :

```
# chipsec_util mmio list
```

### 4.7 Autres accès
Par souci de place dans l’article, nous ne pouvons pas présenter tous les moyens d’accès utilisés par Chipsec, mais nous pouvons néanmoins préciser que l’outil gère les accès aux informations exposées par le CPU (cpuid et MSR) ainsi que les accès par le protocole SPI.

À l’instar de PCI et MMIO, Chipsec utilise un ioctl (code 0x7 notamment) pour demander au module noyau de relever les registres du CPU via les instructions cpuid. Aucun control n’est utilisé puisque le mode de fonctionnement de cpuid est fixe quelle que soit la plateforme et les valeurs d’entrée peuvent donc être définies directement dans les modules Python (tels que le font cpu_info.py et spectre_v2.py). Ces derniers exécutent ensuite la fonction cpuid() de chipsec/hal/cpu.py, qui elle-même fait appel à cpuid() du module chipsec/helper/linux/linuxhelper.py. Enfin, ce dernier fait appel au driver au travers de l’ioctl et récupère les résultats de la requête à travers les registres.

Notons également que l’accès aux paramètres de configuration du CPU via les MSR est également possible au travers du driver Chipsec (codes IOCTL 0x5 pour la lecture et 0x6 pour l’écriture).

L’extraction de la flash SPI (également abordée dans l’article sur les relevés de configuration), fonctionne différemment et n’utilise pas directement le driver. L’opération est possible via la commande chipsec_util.py spi dump qui fait appel au script chipsec/utilcmd/spi_cmd.py. Après avoir identifié la taille de la région BIOS de la flash SPI pour fixer une limite de dump (il ne récupère que cette région ainsi que celle qui lui précède, le descripteur de la Flash), les fonctions de lecture du module chipsec/hal/cpu.py sont sollicitées : read_spi_to_file(0, spi_size, out_file), read_spi(0, spi_size) et spi_reg_read(FDATAN). Comme discuté précédemment, les registres FDATAN, suite à la programmation de FDATA0 et FDADDR, permettent de lire bloc par bloc le contenu de la flash SPI. Ces registres du contrôleur SPI étant mappés en mémoire, un accès MMIO est nécessaire pour en extraire les valeurs, d’où l’utilisation finale de la fonction mmio.read_MMIO_reg(self.rcba_spi_base, reg, size), cette fois via le driver.

## 5. Adaptabilité de Chipsec
Un intérêt supplémentaire de Chipsec est la possibilité de l’adapter à des cas d’usages particuliers. La configuration des plateformes, au format XML, a déjà été mentionnée. Il est possible d’ajouter le support d’une plateforme à Chipsec en ajoutant un fichier correspondant. Un système d’héritage permet de facilement ajouter le support de systèmes dérivés sans préciser l’intégralité des registres, il suffit de (re)définir les registres nécessaires, comme par exemple ici pour la plateforme Whiskey Lake qui ne redéfinit qu’un registre MMIO :

```xml
<mmio>
    <bar name="GTTMMADR" bus="0" dev="2" fun="0" reg="0x10" width="8"
    mask="0x7FFF000000" desc="Graphics Translation Table Range"/>
</mmio>
```
L’architecture modulaire de Chipsec, à base de plugins, permet de rajouter aisément des tests de conformité en validant la valeur de registres spécifiques. Tous les plugins peuvent s’appuyer sur la configuration ainsi que sur la couche d’abstraction des accès matériels (la fameuse HAL, Hardware Abstraction Layer, évoquée plus haut, ainsi que les controls).

Un module minimal doit hériter de la classe BaseModule et implémenter au moins deux fonctions : is_supported, exécutée au démarrage de chipsec_main, et run, exécutée si la première renvoie un résultat positif. Cette deuxième méthode peut ensuite utiliser les controls et la HAL pour déterminer un succès ou un échec au test, et renvoyer cette valeur comme résultat de la fonction.

## Conclusion
La complexité des systèmes actuels, illustrée dans l’article Relevé de configuration matérielle sur plateforme x86, montre l’importance d’un outil tel que Chipsec. Il ne fait aucun doute qu’un outil aussi pratique et facile d’utilisation doit faire partie de la boîte à outils de tout auditeur ou analyste cherchant à évaluer la sécurité des plateformes Intel.

L’ANSSI l’utilise aujourd’hui activement pour évaluer automatiquement les matériels bureautiques de l’administration française et ainsi savoir s’ils répondent aux exigences de sécurité [1]. Dans ce cadre, elle a publié sur son dépôt GitHub une distribution [11] prête à l’emploi sur laquelle Chipsec et d’autres outils ont été installés. Une documentation expliquant chacune des sorties de Chipsec [3] et consolidant les différents registres importants à vérifier est également disponible [12][13][14].

Si la présence d’un outil automatique facilitant le travail d’un analyste est appréciable, il faut pour autant être conscient de la dépendance qu’elle entraîne. En effet, l’auditeur repose sur la présence et le maintien à jour des configurations faites par les développeurs, que ce soit pour la localisation et la sémantique des registres, ou sur la validation ou l’échec des tests.

Enfin, un manque de Chipsec, lié à ses origines, est l’absence de gestion des plateformes AMD. Elles sont certes moins nombreuses sur le marché, et sans doute moins impactées par les vulnérabilités récentes, mais il ne faut pas perdre de vue cet aspect lorsqu’une plateforme de ce type est examinée.

## Références
[1] ANSSI, « Exigences de sécurité matérielle pour plate-formes x86 » : https://www.ssi.gouv.fr/guide/exigences-de-securite-materielles/
[2] Intel, « Platform Security Assessment Framework » : https://github.com/chipsec/chipsec
[3] ANSSI, « Modules Chipsec » : https://github.com/ANSSI-FR/chipsec-check/blob/master/doc/output/modules_chipsec.pdf
[4] C. Kallenberg et R. Wojtczuk, « Speed Racer: Exploiting an Intel Flash Protection Race Condition » : https://bromiumlabs.files.wordpress.com/2015/01/speed_racer_whitepaper.pdf
[5] C. Kallenberg, X. Kovah, J. Butterworth, et S. Cornwell, « SENTER Sandman: Using Intel TXT to Attack BIOSes » : https://conference.hitb.org/hitbsecconf2014kul/materials/D1T1%20-%20SENTER%20Sandman%20-%20Using%20Intel%20TXT%20to%20Attack%20BIOSes.pdf
[6] C. Kallenberg, X. Kovah, J. Butterworth, et S. Cornwell, « All your boot are belong to us », présenté à CanSecWest : https://cansecwest.com/slides/2014/AllYourBoot_csw14-mitre-final.pdf
[7] D. Oleksiuk, « Exploiting AMI Aptio firmware on example of Intel NUC » : http://blog.cr4.sh/2016/10/exploiting-ami-aptio-firmware.html
[8] R. Wojtczuk et C. Kallenberg, « Attacks on UEFI Security », présenté à CanSecWest : https://cansecwest.com/slides/2015/AttacksOnUEFI_Rafal.pptx
[9] D. Oleksiuk, « Exploiting UEFI boot script table vulnerability » : http://blog.cr4.sh/2015/02/exploiting-uefi-boot-script-table.html
[10] « The PCI ID Repository » : https://pci-ids.ucw.cz/
[11] ANSSI, « Tools for testing requirements » : https://github.com/anssi-fr/chipsec-check
[12] ANSSI, « Mindmap SPI Protection » : https://github.com/ANSSI-FR/chipsec-check/blob/master/doc/output/SPI%20Protection.pdf
[13] ANSSI, « Mindmap SMRAM Protection » : https://github.com/ANSSI-FR/chipsec-check/blob/master/doc/output/SRAM%20Protection.pdf
[14] ANSSI, « Mindmap CPU Options» : https://github.com/ANSSI-FR/chipsec-check/blob/master/doc/output/CPU%20Options.pdf

source : https://connect.ed-diamond.com/MISC/misc-109/chipsec-un-outil-pour-les-tests-de-conformite-des-firmwares