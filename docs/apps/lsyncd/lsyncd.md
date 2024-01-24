# LSYNCD - LIVE SYNCING (MIRROR) DAEMON
----

## <i class="fa-solid fa-hashtag"></i> Information


> <i class="fa-solid fa-quote-left"></i> **Lsyncd** uses a filesystem event interface (inotify or fsevents) to watch for changes to local files and directories. **Lsyncd** collates these events for several seconds and then spawns one or more processes to synchronize the changes to a remote filesystem. The default synchronization method is rsync. Thus, **Lsyncd** is a light-weight live mirror solution. **Lsyncd** is comparatively easy to install and does not require new filesystems or block devices. Lysncd does not hamper local filesystem performance.
>
> As an alternative to rsync, **Lsyncd** can also push changes via rsync+ssh. Rsync+ssh allows for much more efficient synchronization when a file or direcotry is renamed or moved to a new location in the local tree. (In contrast, plain rsync performs a move by deleting the old file and then retransmitting the whole file.)
>
> Fine-grained customization can be achieved through the config file. Custom action configs can even be written from scratch in cascading layers ranging from shell scripts to code written in the Lua language. Thus, simple, powerful and flexible configurations are possible.
>
> **Lsyncd** 2.2.1 requires rsync >= 3.1 on all source and target machines. <i class="fa-solid fa-quote-left fa-rotate-180"></i>


- <i class="fa-solid fa-globe"></i> **Site Officiel** : https://lsyncd.github.io/lsyncd/
- <i class="fa-solid fa-book"></i> **Documentation** : https://lsyncd.github.io/lsyncd/manual/building/
- <i class="fa-solid fa-file-circle-question"></i> **Changelog** : https://github.com/lsyncd/lsyncd/releases
- <i class="fa-brands fa-github"></i> **Github** : https://github.com/lsyncd/lsyncd
- <i class="fas fa-comments"></i> **Forum** : https://github.com/lsyncd/lsyncd/issues
- <i class="far fa-question-circle"></i> **FAQ** : https://lsyncd.github.io/lsyncd/faq/

---

## <i class="fa-regular fa-newspaper"></i> Articles

<details open>

</details>

---

## <i class="fa-solid fa-glasses"></i> Articles externes

<details>

- [10+ lsyncd examples to sync directories real time in CentOS/RHEL 7](https://www.golinuxcloud.com/lsyncd-centos-sync-directories-config-example/)
- [Comment synchroniser des répertoires à l'aide de Lsyncd sur Ubuntu 20.04](https://fr.linux-console.net/?p=2786)
- [How to sync files with lsyncd](https://www.unixmen.com/how-to-sync-files-with-lsyncd/)
- [How to Synchronize Directories Using Lsyncd on Ubuntu 20.04](https://www.howtoforge.com/how-to-synchronize-directories-using-lsyncd-on-ubuntu/)
- [How to Synchronize Directories Using Lsyncd on Ubuntu 20.04](https://www.howtoforge.com/how-to-synchronize-directories-using-lsyncd-on-ubuntu/)
- [Implementing a two-way sync with Lsyncd](https://medium.com/@jpandithas/implementing-a-two-way-sync-with-lsyncd-a82026725e66)
- [INSTALLATION ET CONFIGURATION DE LSYNCD SUR DEBIAN](https://igln.fr/ops/installation-et-configuration-de-lsyncd-sur-debian/)
- [L’INFRASTRUCTURE HAUTE DISPO VUE PAR NEXYLAN](https://www.nexylan.com/linfrastructure-haute-dispo-vue-par-nexylan/)
- [lsyncd(1) - Linux man page](https://linux.die.net/man/1/lsyncd)
- [Mirroring Solution - lsyncd](https://docs.rockylinux.org/guides/backup/mirroring_lsyncd/)
- [Set up Lsyncd locally and over SSH to sync directories](https://docs.rackspace.com/docs/set-up-lsyncd-locally-and-over-ssh-to-sync-directories)
- [Utiliser Lsyncd pour synchroniser deux dossiers](https://blog.seboss666.info/2017/09/utiliser-lsyncd-pour-synchroniser-deux-dossiers/)

</details>