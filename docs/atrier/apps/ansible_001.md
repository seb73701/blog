---
title: Securing a Server with Ansible
description: 
published: true
date: 2023-04-21T10:45:59.398Z
tags: 
editor: markdown
dateCreated: 2023-04-21T10:45:54.387Z
---

# Securing a Server with Ansible
A while back, Bryan Kennedy wrote a [post](http://plusbryan.com/my-first-5-minutes-on-a-server-or-essential-security-for-linux-servers) describing how he spends the first 5 minutes configuring and securing a new linux server. He runs through the list of commands and configuration settings that address things like:

- secure passwords
- automatic updates
- basic intrusion detection
- public key authentication
- firewall settings
- log monitoring

There were a couple of [blog posts](https://lattejed.com/first-five-and-a-half-minutes-on-a-server-with-ansible.html) in response that took this one step further and demonstrated how to accomplish the same things in a more automated fashion using [Ansible](http://www.ansible.com/home). Things move pretty fast and I found both posts were a little outdated. So this post continues the tradition and automates the process using an Ansible playbook. It takes care of the basic things described in these posts with a couple of additions and enhancements.

> Note: This is tailored to Ubuntu servers and has been tested on Precise (12.04) and Trusty (14.04).

I will echo the disclaimer that the settings in this playbook do not represent the absolute pinnacle of a highly secure linux server. It is, however, a decent starting point that balances accessibility, maintainability and simplicity. Your mileage may vary and I encourage you to tweak things to fit your own requirements.

To reiterate, here are the things we are going to perform in our ansible playbook:

1. Change the password for the root account
1. Create and configure a deploy user account
1. Configure ssh public key authentication for the deploy account
1. Add the deploy account to the [sudoers](https://help.ubuntu.com/community/Sudoers) list
1. Run apt-get update
1. Run apt-get upgrade
1. Package Installation
1. Configure automatic updates
1. Configure a firewall
1. Install and configure [Logwatch](http://linux.die.net/man/8/logwatch)
1. Lockdown ssh access

So, let’s go through it step by step.

> Note: If you are brand new to ansible, you might want to check out my [quick start post](https://ryaneschinger.com/blog/ansible-quick-start/) and the [ansible documentation](http://docs.ansible.com/).

## Create Playbook Boilerplate
Ansible uses [YAML](http://docs.ansible.com/YAMLSyntax.html) syntax to define playbooks. To get started, create a YAML file called ‘five_minutes.yml’ that looks like the following:

```yaml
---
- hosts: all
  vars:
    UBUNTU_COMMON_ROOT_PASSWORD: 'xxx'
    UBUNTU_COMMON_DEPLOY_PASSWORD: 'xxx'
    UBUNTU_COMMON_LOGWATCH_EMAIL: youremail@example.com
    ubuntu_common_deploy_user_name: deploy
    ubuntu_common_deploy_public_keys:
      - ~/.ssh/id_rsa.pub

    ubuntu_common_required_packages:
      - ufw
      - fail2ban
      - unattended-upgrades
      - logwatch

    ubuntu_common_optional_packages:
      - mosh
      - vim

    ubuntu_common_ssh_port: 22
    ubuntu_common_mosh_from_port: 60000
    ubuntu_common_mosh_to_port: 60010
  tasks:
```

The only variables you **must** configure are:

- UBUNTU_COMMON_ROOT_PASSWORD
- UBUNTU_COMMON_DEPLOY_PASSWORD
- UBUNTU_COMMON_LOGWATCH_EMAIL

For the most part, the rest of the variables can be left at the defaults shown above. You may need to adjust ubuntu_common_deploy_public_keys also depending on how your control machine (the system you are running ansible from) is configured.

For detailed instructions on how to generate a crypted password that can be used in the password variables, see [here](http://docs.ansible.com/faq.html#how-do-i-generate-crypted-passwords-for-the-user-module).

In order to configure the email address where you will receive logwatch emails, simply replace youremail@example.com with a valid email address in the UBUNTU_COMMON_LOGWATCH_EMAIL variable.

Next, we need to define our tasks. All tasks will be listed underneath the ‘tasks:’ directive in the yaml file.

## Change the password for the root account

```yaml
- name: Change root password
  user: name=root password="{{ UBUNTU_COMMON_ROOT_PASSWORD }}"
```

We simply use the ansible [user](http://docs.ansible.com/user_module.html) module to set the password for the root account. The password is set via a variable that you need to override for your environment.

## Create and configure a deploy user account

```yaml
- name: Add deploy user
  user: name={{ ubuntu_common_deploy_user_name }} password="{{ UBUNTU_COMMON_DEPLOY_PASSWORD }}" shell=/bin/bash
```

Again we are using the user module. In this case, we are ensuring the user exists and we are setting the password with the (required) variable. We also set the user account’s shell to bash. By default, we are sticking with the convention of creating an account called deploy. However, the task does allow you to customize that name by setting the ubuntu_common_deploy_user_name variable.

## Configure Public Key Authentication for the deploy Account

```yaml
- name: Add authorized keys for deploy user
  authorized_key: user={{ ubuntu_common_deploy_user_name }}
                  key="{{ lookup('file', item) }}"
  with_items: ubuntu_common_deploy_public_keys
```

Here we are adding public keys to the deploy user’s authorized_keys file on the server (using the [authorized_key module](http://docs.ansible.com/authorized_key_module.html). This is an interesting task as we are leveraging two powerful features in Ansible: [loops](http://docs.ansible.com/playbooks_loops.html) and [lookups](http://docs.ansible.com/playbooks_lookups.html). ubuntu_common_deploy_public_keys is a variable containing a list of file paths containing public keys. Note that these paths should refer to a location on the control machine. Each of these public keys will be added to the authorized_keys file for the deploy account. By default, the ubuntu_common_deploy_public_keys is set to:

```yaml
ubuntu_common_deploy_public_keys:
- ~/.ssh/id_rsa.pub
```

If you want to change the location of the public key or if you want to add multiple keys, you can set the variable like the following:
```yaml
ubuntu_common_deploy_public_keys:
- /path/to/key1.pub
- /path/to/key2.pub
- /path/to/key3.pub
```
Ansible uses ‘with_items’ to loop through each path in the list. The authorized_key module is run for each path and uses a file lookup to read the contents of that file and add it to the deploy user’s authorized_key file on the server you are provisioning. Pretty cool.

## Add the deploy account to the [sudoers](https://help.ubuntu.com/community/Sudoers) list

```yaml
- name: Add deploy user to sudoers
  lineinfile: dest=/etc/sudoers
              regexp="{{ ubuntu_common_deploy_user_name }} ALL"
              line="{{ ubuntu_common_deploy_user_name }} ALL=(ALL) ALL"
              state=present
```

[lineinfile](http://docs.ansible.com/lineinfile_module.html) is another powerful module that basically does what it says. It lets you add, remove, or update a specific line in a file. This is extremely useful when you want make small changes to a configuration file but don’t want to have to maintain your own full version or template. There are a lot of capabilities in this module but, in this case, we are just going to ensure that the deploy account is added to the sudoers list on the server. The dest option tells the module which file to update. Setting state to present ensures that the line will exist in the file (use absent if you want to remove a line from a file). line defines exactly what we want the line to look like after this module runs and supports variable substitution. Finally, regexp lets you define a regular expression to look for in each line of the file. If the regexp matches, that is the line that is going to be replaced. If there is no match, the line will be added to the file. It also supports variable substitution, of course.

## Run apt-get update

```yaml
- name: Update APT package cache
  apt: update_cache=yes cache_valid_time=3600
```

This a very simple task that uses the [apt module](http://docs.ansible.com/apt_module.html). The apt module is just an interface to run apt ([Advanced Packaging Tool](https://wiki.debian.org/Apt)) commands for package management tasks. In this case, we are simply using the module to run

```
apt-get update
```

which downloads the latest package indexes from the distribution’s repositories. The cache_valid_time can be a helpful setting. apt-get update has to communicate over the network to download the package indexes and can be a slow operation. If you need to run this playbook multiple times (which is fully supported due to Ansible’s [idempotency](http://docs.ansible.com/glossary.html#idempotency)), you won’t have to wait again while the package index is updated. That is, unless it has been more than one hour since you last ran the playbook.


## Run apt-get upgrade

```yaml
- name: Upgrade APT to the latest packages
  apt: upgrade=safe
```

Another simple apt module command to run

```
aptitude safe-upgrade
```
which upgrades all installed packages.


## Package Installation
### Required Packages
This playbook is dependent on several packages that provide various security capabilities.

- ufw

The Uncomplicated Firewall ([ufw](https://wiki.ubuntu.com/UncomplicatedFirewall)) is a frontend configuration tool for an [iptables](http://linux.die.net/man/8/iptables) firewall. We will discuss this more later in the article.

- unattended-upgrades

A package that makes it easier to configure automatic package upgrades. We will configure this later in the playbook.

- Fail2ban

[Fail2ban](http://www.fail2ban.org/wiki/index.php/Main_Page) monitors remote authentication attempts, identifies possible brute-force or dictionary attacks and bans hosts that are the source of suspicious activity. Fail2ban will run automatically after it is installed and is configured well-enough for our purposes by default. Feel free to read the documentation and perform additional configuration if it is required for your servers.

- Logwatch

[Logwatch](http://linux.die.net/man/8/logwatch) is a log analysis and reporting program. Later in the playbook, we will configure it to send a daily email summary of activity on your server.

The playbook contains a list of these packages in an internal variable:

```yaml
ubuntu_common_required_packages:
  - ufw
  - fail2ban
  - unattended-upgrades
  - logwatch
```

We use the apt module to install each package:

```yaml
- name: Install required packages
  apt: state=installed pkg={{ item }}
  with_items: ubuntu_common_required_packages
```

### Optional Packages
While you are at it, you may have some packages that you like to install on every single server you build. The playbook supports that by allowing you to configure a list of optional packages to install. It is almost identical to the previous task:

```yaml
- name: Install optional packages
  apt: state=installed pkg={{ item }}
  with_items: ubuntu_common_optional_packages
```

All you need to do is customize the ubuntu_common_optional_packages variable with the list of packages you want to install. By default, it looks like:

```yaml
ubuntu_common_optional_packages:
  - mosh
  - vim
```

## Configure automatic updates
We already installed the unattended-upgrades package. Now we need to configure it.

```yaml
- name: Adjust APT update intervals
  copy: src=apt_periodic dest=/etc/apt/apt.conf.d/10periodic
```

We are using the [copy module](http://docs.ansible.com/copy_module.html) to take a file (specified by src) and copy it to the server as-is at the path configured by dest. That’s it. Our apt_periodic file looks like the following:

```
APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "1";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";
```

It should live in the same directory as your playbook.

## Configure a firewall
Now we are going to use the [ufw module](http://docs.ansible.com/ufw_module.html) to create some firewall rules.

```yaml
- name: Setup ufw
  ufw: state=enabled policy=deny
```

This enables the firewall so it will be running across reboots. It also defaults the default incoming policy to deny so that all incoming traffic is denied by default.

Next, we go ahead and punch a hole in the firewall to allow SSH traffic through. By default, we are going to be using the standard SSH port (22/tcp) but you can customize this with the ubuntu_common_ssh_port variable.

```yaml
- name: Allow ssh traffic
  ufw: rule=allow port={{ ubuntu_common_ssh_port }} proto=tcp
```

The default list of optional packages includes [mosh](https://mosh.mit.edu/) (the mobile shell). As someone who moves around between locations and (sometimes slow) networks a lot, I like to have this installed on servers I manage so that I can resume SSH connections. The following task opens up the firewall to support mosh:

```yaml
- name: Allow mosh traffic
  ufw: rule=allow proto=udp port={{ ubuntu_common_mosh_from_port }}:{{ ubuntu_common_mosh_to_port }}
  when: "'mosh' in ubuntu_common_optional_packages"
```

The playbook defines a pretty tight port range by default (60000 - 60010) but you can open that up more if needed by customizing ubuntu_common_mosh_from_port and ubuntu_common_mosh_to_port. This task also uses the clever [conditional](http://docs.ansible.com/playbooks_conditionals.html) feature of Ansible. If you customize ubuntu_common_optional_packages and do not include mosh, the task won’t run and those ports will not be opened up.

## Install and configure [Logwatch](http://linux.die.net/man/8/logwatch)
Now it is time to setup logwatch to send a daily summary email. The first thing we have to do is configure [Postfix](http://www.postfix.org/) to relay mail:

```yaml
- name: Set up Postfix to relay mail
  debconf: name=postfix
           question='{{ item.question }}'
           value='{{ item.value }}'
           vtype='{{ item.vtype }}'
  with_items:
    - { question: 'postfix/mailname', value: '{{ ansible_fqdn }}', vtype: 'string' }
    - { question: 'postfix/main_mailer_type', value: 'Internet Site', vtype: 'string' }
```

This might be the most complicated task in the playbook. We use the [debconf module](http://docs.ansible.com/debconf_module.html) (an interface to the debconf-set-selections command) to set a couple of Postfix configuration settings. This time, we are using key-value pairs for our with-items loop. So, we end up running debconf-set-selections with the question, value and vtype defined in each item. Note that the ansible_fqdn variable is one that is populated internally by Ansible’s [Facts](http://docs.ansible.com/glossary.html#facts) discovery process.

Now we need to schedule a cron job to send the log summary.

```yaml
- name: Email log summary daily
  lineinfile: dest=/etc/cron.daily/00logwatch
              regexp="^/usr/sbin/logwatch"
              line="/usr/sbin/logwatch --output mail --mailto {{ UBUNTU_COMMON_LOGWATCH_EMAIL }} --detail high"
              state=present create=yes
```

Here we are using the lineinfile module again to update the /etc/cron.daily/00logwatch file with the logwatch command to run with a mailto value populated from the required UBUNTU_COMMON_LOGWATCH_EMAIL variable. Note the use of the create option. This tells the lineinfile module to create the file if it does not already exist.

## Lockdown ssh access
We are approaching the end now. We just need to perform a few additional security tasks. First, we will set the SSH port.

```yaml
- name: Change ssh port
  lineinfile: dest=/etc/ssh/sshd_config
              regexp="^Port\s"
              line="Port {{ ubuntu_common_ssh_port }}"
              state=present
  notify: Restart ssh
```

If you do not customize the ubuntu_common_ssh_port variable, this will not really change anything.

Next, we are going to turn of password based authentication for SSH. We want to require all SSH access to use public key authentication (which we already configured for the deploy account).

```yaml
- name: Disallow password authentication
  lineinfile: dest=/etc/ssh/sshd_config
              regexp="^PasswordAuthentication"
              line="PasswordAuthentication no"
              state=present
  notify: Restart ssh
```

Finally, we are going to disable SSH access for the root account:

```yaml
- name: Disallow root SSH access
  lineinfile: dest=/etc/ssh/sshd_config
              regexp="^PermitRootLogin"
              line="PermitRootLogin no"
              state=present
  notify: Restart ssh
```

Note the use of Restart ssh in the notify directive on the above tasks. Restart ssh is an Ansible [handler](http://docs.ansible.com/glossary.html#handlers) and is responsible for restarting the SSH service. Handlers are like regular Ansible tasks except that they are only run if a task that includes the notify directive for that handler changes something on the server. So, in this example, if you were running this playbook on server that already had the above sshd_config settings configured as above (or you were running the playbook for a second time), the Restart ssh handler would not run. One other special behavior of handlers: they run only once even if notified by several different tasks. In our playbook, Restart ssh is defined as follows:

```yaml
handlers:
  - name: Restart ssh
    service: name=ssh state=restarted
```

Once the playbook is done running, you will only be able to authenticate with the deploy account and a corresponding private key. You will have to use sudo for all administrative tasks going forward.

## Running the Playbook
Whew, that may seem like a lot. And, it is. But the power of this approach is that we can now perform all of this configuration against 1 or 10 or even 100 servers by running one (somewhat) simple Ansible command.

### Using an Inventory File
Ansible uses an [inventory file](http://docs.ansible.com/intro_inventory.html) to decide what servers to operate against. There are a variety of ways you can specify your inventory but for this example, we are just going to pass in the path to a basic inventory file into the Ansible command. First, create an inventory file to test with:

```
echo "<x.x.x.x>" > inv.ini
```

Replace ‘x.x.x.x’ with the IP address or (resolvalbe) host name of your server.

### Test Ansible Connection
Now, we are going to test connectivity to your server. The options you pass to this command can vary depending on where your server is hosted and how it is configured. The command below assumes you can already authenticate to your server using the root account with public key authentication. If this is not the case, you can use the ‘-u’ option to specify a different account name and ‘-k’ to ask Ansible to prompt you for an SSH password.

```
ansible all -i inv.ini -m ping -u root
```

In the command ‘all’ indicates that you want to run against all servers defined in inv.ini. The ‘-i’ option lets you specify the specific inventory file you want to use (which we just created above). The ‘-m ping’ option tells Ansible to run the [ping module](http://docs.ansible.com/ping_module.html) against all matching hosts. This is useful for testing SSH connectivity to the hosts. If everything works, you should get back a response that looks something like this:

```yaml
x.x.x.x | success >> {
    "changed": false,
    "ping": "pong"
}
```

If you are getting back an error, you mostly likely have to tweak some command line options to configure the SSH connection appropriately (user, port, password, key, etc.). You can pass a ‘-vvvv’ option to get verbose debugging output for your ssh connection.

### Running a Playbook
Now that you’ve established that you can connect to the server with Ansible, it is time to run the playbook.

```
ansible-playbook five_minutes.yml -i inv.ini -u root
```

Here we are running the ‘ansible-playbook’ command with the path to our playbook YAML file. If you are running the playbook with an account other than root, you may need to tell Ansible to use sudo. You can do this by passing the ‘-s’ option. You may also need ‘-K’ to ask Ansible to prompt you for the sudo password. For example:

```
ansible-playbook five_minutes.yml -i inv.ini -u myuser -s -K
```

Now you should see the Ansible output fly by as it does all of the hard work configuring and securing your server. Depending on your network connections, it should certainly take less than the promised 5 minutes.

You can find the full playbook [here](https://gist.github.com/ryane/e0ea8e4a75b140bf799f).

I hope this proves useful for establishing a baseline security configuration for any servers you are building. And I hope it illustrates the power of using a configuration management tool like Ansible to make this process easy to apply, repeatable and fast. Please let me know if you have any feedback!

source = https://ryaneschinger.com/tags/ansible/