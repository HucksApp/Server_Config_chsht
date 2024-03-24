# Server_Config_chsht


## Server Configuration Management

refers to the process of systematically handling changes to a system in a way that it maintains integrity over time
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


## Ansible














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
