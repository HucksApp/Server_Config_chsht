# Server_Config_chsht


## Server Configuration Management

refers to the process of systematically handling changes to a system in a way that it maintains integrity over time.

most popular configuration management tools available  
* Ansible
* Puppet
* Chef

Property              |	Ansible       |	Puppet                      |	Chef
----------------------|---------------|-----------------------------|---------
Script Language	      | YAML	        | Custom DSL based on Ruby    |	Ruby
Infrastructure	      | Controller machine applies configuration on nodes via SSH | 	Puppet Master synchronizes configuration on Puppet Nodes |	Chef Workstations push configuration to Chef Server, from which the Chef Nodes will be updated
Requires specialized software for nodes |	No  |	Yes	 | Yes
Provides centralized point of control	| No. Any computer can be a controller |	Yes, via Puppet Master	 | Yes, via Chef Server
Script Terminology	| Playbook / Roles	| Manifests / Modules	 | Recipes / Cookbooks
Task Execution Order	| Sequential	| Non-Sequential	| Sequential

------------------------------------------------------------------------------
## Ansible



* Control Node: the machine where Ansible is installed, responsible for running the provisioning on the servers you are managing.
* Inventory: an INI file that contains information about the servers you are managing.
* Playbook: a YAML file containing a series of procedures that should be automated.
* Task: a block that defines a single procedure to be executed, e.g.: install a package.
* Module: a module typically abstracts a system task, like dealing with packages or creating and changing files. Ansible has a multitude of built-in modules, but you can also create custom ones.
* Role: a set of related playbooks, templates and other files, organized in a pre-defined way to facilitate reuse and share.
* Play: a provisioning executed from start to finish is called a play.
* Facts: global variables containing information about the system, like network interfaces or operating system.
* Handlers: used to trigger service status changes, like restarting or reloading a service.


### Task
 Defines a single automated step that should be executed by Ansible.
 Typically involves the usage of a module or the execution of a raw command
### structure
```
- name: <This is a task>
  <module>: <option>=<value> <option>=<value> ..
```
OR
```
- name: <This is a task>
    <module>:
      <option>: <value>
      <option>: <value>
      ..... : ....
```
 * name: task name
 * module: built-in Ansible module  e.g `apt`
 * option and value: modules variable to set.

### Playbook 

YAML files containing a series of ***Task*** directives 

```
---
- name: <play 1>
  <default_config>: <default_config_value>
  <default_config>: <default_config_value>
 .............    : ...............

  tasks:
  - name: <Task 1>
    <module>:
      <option>: <value>
      <option>: <value>
      ....... : ......

 - name: <Task 2>
    <module>:
      <option>: <value>
      <option>: <value>
      ....... : ......

 - name: <Task 3>
   <module>: <option>=<value> <option>=<value> ..

 - ....: ....
    .....:
      .....: .....
      .....: .....
      .....: .....


- name: <play 2>
  <default_config>: <value>
  <default_config>:
     <option>: <value>
     <option>: <value>
     ....... : ......
 .............: ...............

  tasks:
  - name: <Task 1>
    <module>:
      <option>: <value>
      <option>: <value>
      ....... : ......

 - name: <Task 2>
    <module>:
      <option>: <value>
      <option>: <value>
      ....... : ......

 - name: <Task 3>
   <module>: <option>=<value> <option>=<value> ..

 - ....: ....
    .....:
      .....: .....
      .....: .....
      .....: .....
```

## Variables


* Define  
```
  vars:
      <var_name>: <value>
```
* Reference
```
      <option>: "{{var_name}}"
```
* Types

  * Boolean variables -> `true/false` or `t/f`or `1/0` or `yes/no` or `y/n` or `True/False`
  * List variables ->
       ### DEFINE
 
       ```
           <list_name>: [<value 1>, <value 2>, ....]
       ```
       OR 
       
       ```
         <list_name>:
            - <value 1>
            - <value 2>
            - ......
       ```
        
       ### Reference
       
       
       ```
             <option>: {{list_name[<item index>]}}
       ```
  * Dictionary

     ### DEFINE
      
    ```
      <dict_name>:
         <key1>: <value 1>
         <key2>: <value 2>
         ..... : ......
    ```
    
    
    ### Reference
    
    
    ```
          <option>: {{dict_name.<key>}}
    ```
    OR
    ```
          <option>: {{dict_name['<key>']}}
    ```

  * Registered Variable -> You can create variables from the output of an Ansible task with the task keyword `register`
    ```
     register:  <value>
    ```

## PIPING -> 
```
|  ->

    {{ <string or varible or command> |  <commands, methods> }}
```
----------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------------------

# PUPPET

## TERMS

* Puppet Master: the master server that controls configuration on the nodes
* Puppet Agent Node: a node controlled by a Puppet Master
* Manifest: a file that contains a set of instructions to be executed
* Resource: a portion of code that declares an element of the system and how its state should be changed. For instance, to install a package we need to define a package resource and ensure its state is set to “installed”
* Module: a collection of manifests and other related files organized in a pre-defined way to facilitate sharing and reusing parts of a provisioning
* Class: just like with regular programming languages, classes are used in Puppet to better organize the provisioning and make it easier to reuse portions of the code
* Facts: global variables containing information about the system, like network interfaces and operating system
* Services: used to trigger service status changes, like restarting or stopping a service

Comments `#` ->  Anything after is ignored
## Variables

```
# DECLARATION
$<variable name> = <value>
```
```
# REFERENCE
$<variable name>
```
### Variable Unpacking

```
[$<var1>, $<var2>] = {$<var1> => <value 1>, $<var2> => <value 2>}


[$<var1>, $<var2>, $<var3>] = [<value 1>, <value 2>, <value 3>] 
```

## Resources
resource refer to an aspect of a system, each resource has ***attributes*** that described  the desired state for that system resource.
This form the fundamental unit for modeling system configurations.
```
<RESOURCE TYPE> { '<TITLE>': <ATTRIBUTE> => <VALUE>, .....: .... }
```

### RESOURCES TYPE


* file -> file type can manage files, directories, and symlinks `file`
  
```
# file resource attributes

file { 'resource title':
  path                    => # (namevar) The path to the file to manage.  Must be fully...
  ensure                  => # Whether the file should exist, and if so what...
  backup                  => # Whether (and how) file content should be backed...
  checksum                => # The checksum type to use when determining...
  checksum_value          => # The checksum of the source contents. Only md5...
  content                 => # The desired contents of a file, as a string...
  ctime                   => # A read-only state to check the file ctime. On...
  force                   => # Perform the file operation even if it will...
  group                   => # Which group should own the file.  Argument can...
  ignore                  => # A parameter which omits action on files matching 
  links                   => # How to handle links during file actions.  During 
  mode                    => # The desired permissions mode for the file, in...
  mtime                   => # A read-only state to check the file mtime. On...
  owner                   => # The user to whom the file should belong....
  provider                => # The specific backend to use for this `file...
  purge                   => # Whether unmanaged files should be purged. This...
  recurse                 => # Whether to recursively manage the _contents_ of...
  recurselimit            => # How far Puppet should descend into...
  replace                 => # Whether to replace a file or symlink that...
  selinux_ignore_defaults => # If this is set then Puppet will not ask SELinux...
  selrange                => # What the SELinux range component of the context...
  selrole                 => # What the SELinux role component of the context...
  seltype                 => # What the SELinux type component of the context...
  seluser                 => # What the SELinux user component of the context...
  show_diff               => # Whether to display differences when the file...
  source                  => # A source file, which will be copied into place...
  source_permissions      => # Whether (and how) Puppet should copy owner...
  sourceselect            => # Whether to copy all valid sources, or just the...
  target                  => # The target for creating a link.  Currently...
  type                    => # A read-only state to check the file...
  validate_cmd            => # A command for validating the file's syntax...
  validate_replacement    => # The replacement string in a `validate_cmd` that...
  # ...plus any applicable metaparameters.
}
```
* filebucket -> For storing and retrieving file content by MD5 checksum. mainly for ***Content backups**

```
 filebucket { 'resource title':
  name   => # (namevar) The name of the...
  path   => # The path to the _local_ filebucket; defaults to...
  port   => # The port on which the remote server is...
  server => # The server providing the remote filebucket...
  # ...plus any applicable metaparameters.
}
```
  
* exec -> Executes external commands `exec`

```
exec { 'resource title':
  command     => # (namevar) The actual command to execute.  Must either be...
  creates     => # A file to look for before running the command...
  cwd         => # The directory from which to run the command.  If 
  environment => # An array of any additional environment variables 
  group       => # The group to run the command as.  This seems to...
  logoutput   => # Whether to log command output in addition to...
  onlyif      => # A test command that checks the state of the...
  path        => # The search path used for command execution...
  provider    => # The specific backend to use for this `exec...
  refresh     => # An alternate command to run when the `exec...
  refreshonly => # The command should only be run as a refresh...
  returns     => # The expected exit code(s).  An error will be...
  timeout     => # The maximum time the command should take.  If...
  tries       => # The number of times execution of the command...
  try_sleep   => # The time to sleep in seconds between...
  umask       => # Sets the umask to be used while executing this...
  unless      => # A test command that checks the state of the...
  user        => # The user to run the command as.  > **Note:*...
  # ...plus any applicable metaparameters.
}
```

* group -> Manage groups membership

```
group { 'resource title':
  name                 => # (namevar) The group name. While naming limitations vary by 
  ensure               => # Create or remove the group.  Valid values are...
  allowdupe            => # Whether to allow duplicate GIDs.  Valid values...
  attribute_membership => # AIX only. Configures the behavior of the...
  attributes           => # Specify group AIX attributes, as an array of...
  auth_membership      => # Configures the behavior of the `members...
  forcelocal           => # Forces the management of local accounts when...
  gid                  => # The group ID.  Must be specified numerically....
  ia_load_module       => # The name of the I&A module to use to manage this 
  members              => # The members of the group. For platforms or...
  provider             => # The specific backend to use for this `group...
  system               => # Whether the group is a system group with lower...
  # ...plus any applicable metaparameters.
}
```

* notify -> Sends an arbitrary message, specified as a string, to run-time log (logging) `notify`

```
notify { 'resource title':
  name     => # (namevar) An arbitrary tag for your own reference; the...
  message  => # The message to be sent to the log. Note that the 
  withpath => # Whether to show the full object path.  Valid...
  # ...plus any applicable metaparameters.
}
```
* package -> Manage packages `package`

```
package { 'resource title':
  name                 => # (namevar) The package name.  This is the name that the...
  command              => # (namevar) The targeted command to use when managing a...
  provider             => # (namevar) The specific backend to use for this `package...
  ensure               => # What state the package should be in. On...
  adminfile            => # A file containing package defaults for...
  allow_virtual        => # Specifies if virtual package names are allowed...
  allowcdrom           => # Tells apt to allow cdrom sources in the...
  category             => # A read-only parameter set by the...
  configfiles          => # Whether to keep or replace modified config files 
  description          => # A read-only parameter set by the...
  enable_only          => # Tells `dnf module` to only enable a specific...
  flavor               => # OpenBSD and DNF modules support 'flavors', which 
  install_only         => # It should be set for packages that should only...
  install_options      => # An array of additional options to pass when...
  instance             => # A read-only parameter set by the...
  mark                 => # Set to hold to tell Debian apt/Solaris pkg to...
  package_settings     => # Settings that can change the contents or...
  platform             => # A read-only parameter set by the...
  reinstall_on_refresh => # Whether this resource should respond to refresh...
  responsefile         => # A file containing any necessary answers to...
  root                 => # A read-only parameter set by the...
  source               => # Where to find the package file. This is mostly...
  status               => # A read-only parameter set by the...
  uninstall_options    => # An array of additional options to pass when...
  vendor               => # A read-only parameter set by the...
  # ...plus any applicable metaparameters.
}
```
* Other resource type -> manage all other non builtin resource type `resources`

```
resources { 'resource title':
  name               => # (namevar) The name of the type to be...
  purge              => # Whether to purge unmanaged resources.  When set...
  unless_system_user => # This keeps system users from being purged.  By...
  unless_uid         => # This keeps specific uids or ranges of uids from...
  # ...plus any applicable metaparameters.
}
```
* schedule -> Define schedules for Puppet `schedule`
```
schedule { 'maint':
  range  => '2 - 4',
  period => daily,
  repeat => 1,
}
```
* service -> Manage running services `service`

```
service { 'resource title':
  name          => # (namevar) The name of the service to run.  This name is...
  ensure        => # Whether a service should be running. Default...
  binary        => # The path to the daemon.  This is only used for...
  control       => # The control variable used to manage services...
  enable        => # Whether a service should be enabled to start at...
  flags         => # Specify a string of flags to pass to the startup 
  hasrestart    => # Specify that an init script has a `restart...
  hasstatus     => # Declare whether the service's init script has a...
  logonaccount  => # Specify an account for service logon    Requires 
  logonpassword => # Specify a password for service logon. Default...
  manifest      => # Specify a command to config a service, or a path 
  path          => # The search path for finding init scripts....
  pattern       => # The pattern to search for in the process table...
  provider      => # The specific backend to use for this `service...
  restart       => # Specify a *restart* command manually.  If left...
  start         => # Specify a *start* command manually.  Most...
  status        => # Specify a *status* command manually.  This...
  stop          => # Specify a *stop* command...
  timeout       => # Specify an optional minimum timeout (in seconds) 
  # ...plus any applicable metaparameters.
}
```
* stage -> resource type for creating new run stages `stage`

```
stage { 'resource title':
  name => # (namevar) The name of the stage. Use this as the value for 
  # ...plus any applicable metaparameters.
}
```

* tidy -> Remove unwanted files based on specific criteria `tidy`

```
tidy { 'resource title':
  path      => # (namevar) The path to the file or directory to manage....
  age       => # Tidy files whose age is equal to or greater than 
  backup    => # Whether tidied files should be backed up.  Any...
  matches   => # One or more (shell type) file glob patterns...
  max_files => # In case the resource is a directory and the...
  recurse   => # If target is a directory, recursively descend...
  rmdirs    => # Tidy directories in addition to files; that is...
  size      => # Tidy files whose size is equal to or greater...
  type      => # Set the mechanism for determining age.  Valid...
  # ...plus any applicable metaparameters.
}
```
* user -> Manage users `user`

```
user { 'resource title':
  name                 => # (namevar) The user name. While naming limitations vary by...
  ensure               => # The basic state that the object should be in....
  allowdupe            => # Whether to allow duplicate UIDs.  Valid values...
  attribute_membership => # Whether specified attribute value pairs should...
  attributes           => # Specify AIX attributes for the user in an array...
  auth_membership      => # Whether specified auths should be considered the 
  auths                => # The auths the user has.  Multiple auths should...
  comment              => # A description of the user.  Generally the user's 
  expiry               => # The expiry date for this user. Provide as either 
  forcelocal           => # Forces the management of local accounts when...
  gid                  => # The user's primary group.  Can be specified...
  groups               => # The groups to which the user belongs.  The...
  home                 => # The home directory of the user.  The directory...
  ia_load_module       => # The name of the I&A module to use to manage this 
  iterations           => # This is the number of iterations of a chained...
  key_membership       => # Whether specified key/value pairs should be...
  keys                 => # Specify user attributes in an array of key ...
  loginclass           => # The name of login class to which the user...
  managehome           => # Whether to manage the home directory when Puppet 
  membership           => # If `minimum` is specified, Puppet will ensure...
  password             => # The user's password, in whatever encrypted...
  password_max_age     => # The maximum number of days a password may be...
  password_min_age     => # The minimum number of days a password must be...
  password_warn_days   => # The number of days before a password is going to 
  profile_membership   => # Whether specified roles should be treated as the 
  profiles             => # The profiles the user has.  Multiple profiles...
  project              => # The name of the project associated with a user.  
  provider             => # The specific backend to use for this `user...
  purge_ssh_keys       => # Whether to purge authorized SSH keys for this...
  role_membership      => # Whether specified roles should be considered the 
  roles                => # The roles the user has.  Multiple roles should...
  salt                 => # This is the 32-byte salt used to generate the...
  shell                => # The user's login shell.  The shell must exist...
  system               => # Whether the user is a system user, according to...
  uid                  => # The user ID; must be specified numerically. If...
  # ...plus any applicable metaparameters.
}
```










---------------------------------------------------
------------------------------------------------------

## DOMAIN NAME SYSTEM (DNS)

translates human-adapted, text-based domain names to machine-adapted, numerical-based IP

### Dns translation process


```
                                                                                                    🗃 AUTHORATIVE NAME SERVERS
                                                                                                      This are all servers that serves at the host name
                                                                                                      and address
                                                                                                     (13) process: returns the address of the hostname given
                                                                                                                      ↗️
                                                                        (12) Call to the 1st Authoritative name server

                                                                                                             ↗️

                                       
                         (3) calls the Operating system           (5) calls the RESOLVER (ISP)       ↗️
(1) input  --->  🖥 🌐 WEB BROWSER          -->       💻 ⚙️ ⚒ OPERATING SYSTEM      -->    ♻️ RESOLVER (ITERNET SERVICE PROVIDER)       
(example.com)   (2) Process: searches                 (4) Process:searches                (6) process: the resolver 
                its cache for host                    specific folders and                checks its own cache  for host to ip 
                 name to ip mapping                   files like  /etc/host,...           mapping ->  "example.com":<ip address>
                 IF NOT fOUND❓proceed                for host to ip mapping              IF NOT fOUND❓proceed        
                                                      IF NOT fOUND❓proceed               (9) Process: the resolver records the 
                                                                                           TLD server address returned from the
                                                                                           root server.
                                                                                          (11) process: the resolver records the
                                                                                      ↙️    address of the Authoritative name servers
                                                                                           (12) Process: return the ip address to
                                                                                             the OS
                                                                                                                  ⬇️
                                                    (9) Calls the TLD server  ↙️                                (7) the resolver calls the root server
                                                                  
                                                                       ↙️                                           ⬇️
                                                                                          🗄 📍 ROOT SERVER
                                                                                           Root servers sit at the top of the DNS hierarchy.
                                🗄 📍 TOP LEVEL DOMAIN SERVER (TLD)                        There are  13 root name servers in world scattered arround
                                this has the address of all Authoritative name servers     named [letter].root-servers.net where [letter] ranges from A to M.
                                (All servers that serves at the host name space)           ex. B.root-servers.net
                                (10) Process: returns all the name servers serving         They hold the Top Level Domains (TLD) servers addresses
                                 at the host name space  [ns1.example.com,                (.NET, .COM, .ORG, ...) i.e '.com' : <.com TDL server address>
                                 ns2.example.com, ns3.example.com]                         (8) Process: return the right TLD server 
                                                                                            address for the domain asked -> '.COM': <server address>
```


## DNS Records


Type of Record                     |    Usage and Description
-----------------------------------|-------------------------------------------------
A record (Address records)         | A record maps a domain name to the IP address (Version 4) of the computer hosting the domain,its primary records used in DNS servers
⇒                                  | A_obj:\<address> -> where  ***A_obj*** is an element with following fields
⇒                                  |             ***Name:*** The host name for the record, This is generally referred to as “subdomain”.
⇒                                  |             ***TTL:*** The time-to-live in seconds. This is the amount of time the record is allowed to be cached by a resolver
⇒                                  |             ***Address:*** The IPv4 address the A record points
CNAME (Canonical Name) record      |  maps one domain name (an alias) to another (the canonical name). when running multiple services (like an FTP server and a web server, each running on different ports and from a single IP address). use CNAME records to point ftp.example.com and www.example.com to the DNS entry for example.com, which in turn has an A record which points to the IP address





```
NAME                    TYPE   VALUE
--------------------------------------------------
bar.example.com.        CNAME  foo.example.com.
foo.example.com.        A      192.0.2.23

```













# SSH
The home `~` directory holds all hidden eviroments. .ssh/  .bashrc ......
The .ssh folder contains ***authorized_keys*** thats holds authorised public keys for login, when ssh is set as default login



* To generate key use `ssh-keygen`
  ssh-keygen atempts to overide or create a default key pairs as ***id_rsa*** as private key and ***id_rsa.pub*** as public key
  if name option not stated
  Command           |     Option             |        Description
  ------------------|------------------------|-----------------------
  `ssh-keygen`      |  -                     |


       
