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
resource refer to an aspect of a system, Each resource has ***attributes*** that described  the desired state for that system resource.
This form the fundamental unit for modeling system configurations.
```
<RESOURCE TYPE> { '<TITLE>': <ATTRIBUTE> => <VALUE>, }
```

### RESOURCES TYPE

* file -> `file { '<TITLE>': <ATTRIBUTE> => <VALUE>, }`
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


       
