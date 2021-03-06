# Plugin Check Commands for Monitoring Plugins <a id="monitoring-plugins"></a>

The Plugin Check Commands provides example configuration for plugin check commands
provided by the [Monitoring Plugins](https://www.monitoring-plugins.org) project.

By default the Plugin Check Commands are included in the [icinga2.conf](04-configuring-icinga-2.md#icinga2-conf) configuration
file:

```
include <plugins>
```

The plugin check commands assume that there's a global constant named `PluginDir`
which contains the path of the plugins from the Monitoring Plugins project.

**Note**: If there are command parameters missing for the provided CheckCommand
definitions please kindly send a patch upstream. This should include an update
for the ITL CheckCommand itself and this documentation section.

## apt <a id="apt"></a>

The plugin [apt](https://www.monitoring-plugins.org/doc/index.html) checks for software updates on systems that use
package management systems based on the apt-get(8) command found in Debian based systems.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name              | Description
------------------|------------
apt_extra_opts    | Read options from an ini file.
apt_upgrade       | [Default] Perform an upgrade. If an optional OPTS argument is provided, apt-get will be run with these command line options instead of the default.
apt_dist_upgrade  | Perform a dist-upgrade instead of normal upgrade. Like with -U OPTS can be provided to override the default options.
apt_include       | Include only packages matching REGEXP. Can be specified multiple times the values will be combined together.
apt_exclude       | Exclude packages matching REGEXP from the list of packages that would otherwise be included. Can be specified multiple times.
apt_critical      | If the full package information of any of the upgradable packages match this REGEXP, the plugin will return CRITICAL status. Can be specified multiple times.
apt_timeout       | Seconds before plugin times out (default: 10).
apt_only_critical | Only warn about critical upgrades.

## breeze <a id="breeze"></a>

The [check_breeze](https://www.monitoring-plugins.org/doc/man/check_breeze.html) plugin reports the signal
strength of a Breezecom wireless equipment.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name             | Description
-----------------|------------
breeze_hostname  | **Required.** Name or IP address of host to check. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
breeze_community | SNMPv1 community. Defaults to "public".
breeze_warning   | **Required.** Percentage strength below which a WARNING status will result. Defaults to 50.
breeze_critical  | **Required.** Percentage strength below which a WARNING status will result. Defaults to 20.

## by_ssh <a id="by-ssh"></a>

The [check_by_ssh](https://www.monitoring-plugins.org/doc/man/check_by_ssh.html) plugin uses SSH to execute
commands on a remote host.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name               | Description
-------------------|------------
by_ssh_address     | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
by_ssh_port        | The SSH port. Defaults to 22.
by_ssh_command     | **Required.** The command that should be executed. Can be an array if multiple arguments should be passed to `check_by_ssh`.
by_ssh_arguments   | A dictionary with arguments for the command. This works exactly like the 'arguments' dictionary for ordinary CheckCommands.
by_ssh_logname     | The SSH username.
by_ssh_identity    | The SSH identity.
by_ssh_quiet       | Whether to suppress SSH warnings. Defaults to false.
by_ssh_warn        | The warning threshold.
by_ssh_crit        | The critical threshold.
by_ssh_timeout     | The timeout in seconds.
by_ssh_options     | Call ssh with '-o OPTION' (multiple options may be specified as an array).
by_ssh_ipv4        | Use IPv4 connection. Defaults to false.
by_ssh_ipv6        | Use IPv6 connection. Defaults to false.
by_ssh_skip_stderr | Ignore all or (if specified) first n lines on STDERR.

## clamd <a id="clamd"></a>

The [check_clamd](https://www.monitoring-plugins.org/doc/man/check_clamd.html) plugin tests CLAMD
connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name              | Description
------------------|------------
clamd_address     | **Required.** The host's address or unix socket (must be an absolute path).
clamd_port        | Port number (default: none).
clamd_expect      | String to expect in server response. Multiple strings must be defined as array.
clamd_all         | All expect strings need to occur in server response. Defaults to false.
clamd_escape_send | Enable usage of \\n, \\r, \\t or \\\\ in send string.
clamd_send        | String to send to the server.
clamd_escape_quit | Enable usage of \\n, \\r, \\t or \\\\ in quit string.
clamd_quit        | String to send server to initiate a clean close of the connection.
clamd_refuse      | Accept TCP refusals with states ok, warn, crit. Defaults to crit.
clamd_mismatch    | Accept expected string mismatches with states ok, warn, crit. Defaults to warn.
clamd_jail        | Hide output from TCP socket.
clamd_maxbytes    | Close connection once more than this number of bytes are received.
clamd_delay       | Seconds to wait between sending string and polling for response.
clamd_certificate | Minimum number of days a certificate has to be valid. 1st value is number of days for warning, 2nd is critical (if not specified: 0) -- separated by comma.
clamd_ssl         | Use SSL for the connection. Defaults to false.
clamd_wtime       | Response time to result in warning status (seconds).
clamd_ctime       | Response time to result in critical status (seconds).
clamd_timeout     | Seconds before connection times out. Defaults to 10.
clamd_ipv4        | Use IPv4 connection. Defaults to false.
clamd_ipv6        | Use IPv6 connection. Defaults to false.

## dhcp <a id="dhcp"></a>

The [check_dhcp](https://www.monitoring-plugins.org/doc/man/check_dhcp.html) plugin
tests the availability of DHCP servers on a network.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name             | Description
-----------------|------------
dhcp_serverip    | The IP address of the DHCP server which we should get a response from.
dhcp_requestedip | The IP address which we should be offered by a DHCP server.
dhcp_timeout     | The timeout in seconds.
dhcp_interface   | The interface to use.
dhcp_mac         | The MAC address to use in the DHCP request.
dhcp_unicast     | Whether to use unicast requests. Defaults to false.

## dig <a id="dig"></a>

The [check_dig](https://www.monitoring-plugins.org/doc/man/check_dig.html) plugin
test the DNS service on the specified host using dig.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
dig_server           | The DNS server to query. Defaults to "127.0.0.1".
dig_port             | Port number (default: 53).
dig_lookup           | **Required.** The address that should be looked up.
dig_record_type      | Record type to lookup (default: A).
dig_expected_address | An address expected to be in the answer section. If not set, uses whatever was in -l.
dig_arguments        | Pass STRING as argument(s) to dig.
dig_retries          | Number of retries passed to dig, timeout is divided by this value (Default: 3).
dig_warning          | Response time to result in warning status (seconds).
dig_critical         | Response time to result in critical status (seconds).
dig_timeout          | Seconds before connection times out (default: 10).
dig_ipv4             | Force dig to only use IPv4 query transport. Defaults to false.
dig_ipv6             | Force dig to only use IPv6 query transport. Defaults to false.

## disk <a id="disk"></a>

The [check_disk](https://www.monitoring-plugins.org/doc/man/check_disk.html) plugin
checks the amount of used disk space on a mounted file system and generates an alert
if free space is less than one of the threshold values.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                     | Description
-------------------------|------------
disk_wfree               | The free space warning threshold. Defaults to "20%". If the percent sign is omitted, units from `disk_units` are used.
disk_cfree               | The free space critical threshold. Defaults to "10%". If the percent sign is omitted, units from `disk_units` are used.
disk_inode_wfree         | The free inode warning threshold.
disk_inode_cfree         | The free inode critical threshold.
disk_partition           | The partition. **Deprecated in 2.3.**
disk_partition_excluded  | The excluded partition. **Deprecated in 2.3.**
disk_partitions          | The partition(s). Multiple partitions must be defined as array.
disk_partitions_excluded | The excluded partition(s). Multiple partitions must be defined as array.
disk_clear               | Clear thresholds. May be true or false.
disk_exact_match         | For paths or partitions specified with -p, only check for exact paths. May be true or false.
disk_errors_only         | Display only devices/mountpoints with errors. May be true or false.
disk_ignore_reserved     | If set, account root-reserved blocks are not accounted for freespace in perfdata. May be true or false.
disk_group               | Group paths. Thresholds apply to (free-)space of all partitions together.
disk_kilobytes           | Same as --units kB. May be true or false.
disk_local               | Only check local filesystems. May be true or false.
disk_stat_remote_fs      | Only check local filesystems against thresholds. Yet call stat on remote filesystems to test if they are accessible (e.g. to detect Stale NFS Handles). May be true or false.
disk_mountpoint          | Display the mountpoint instead of the partition. May be true or false.
disk_megabytes           | Same as --units MB. May be true or false.
disk_all                 | Explicitly select all paths. This is equivalent to -R '.\*'. May be true or false.
disk_eregi_path          | Case insensitive regular expression for path/partition. Multiple regular expression strings must be defined as array.
disk_ereg_path           | Regular expression for path or partition. Multiple regular expression strings must be defined as array.
disk_ignore_eregi_path   | Regular expression to ignore selected path/partition (case insensitive). Multiple regular expression strings must be defined as array.
disk_ignore_ereg_path    | Regular expression to ignore selected path or partition. Multiple regular expression strings must be defined as array.
disk_timeout             | Seconds before connection times out (default: 10).
disk_units               | Choose bytes, kB, MB, GB, TB (default: MB).
disk_exclude_type        | Ignore all filesystems of indicated type. Multiple regular expression strings must be defined as array. Defaults to "none", "tmpfs", "sysfs", "proc", "configfs", "devtmpfs", "devfs", "mtmfs", "tracefs", "cgroup", "fuse.gvfsd-fuse", "fuse.gvfs-fuse-daemon", "fdescfs".

## disk_smb <a id="disk-smb"></a>

The [check_disk_smb](https://www.monitoring-plugins.org/doc/man/check_disk_smb.html) plugin
uses the `smbclient` binary to check SMB shares.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name               | Description
-------------------|------------
disk_smb_hostname  | **Required.** NetBIOS name of the server.
disk_smb_share     | **Required.** Share name being queried.
disk_smb_workgroup | Workgroup or Domain used (defaults to 'WORKGROUP' if omitted).
disk_smb_address   | IP address of the host (only necessary if host belongs to another network).
disk_smb_username  | Username for server log-in (defaults to 'guest' if omitted).
disk_smb_password  | Password for server log-in (defaults to an empty password if omitted).
disk_smb_wused     | The used space warning threshold. Defaults to "85%". If the percent sign is omitted, use optional disk units.
disk_smb_cused     | The used space critical threshold. Defaults to "95%". If the percent sign is omitted, use optional disk units.
disk_smb_port      | Connection port, e.g. `139` or `445`. Defaults to `smbclient` default if omitted.

## dns <a id="dns"></a>

The [check_dns](https://www.monitoring-plugins.org/doc/man/check_dns.html) plugin
uses the nslookup program to obtain the IP address for the given host/domain query.
An optional DNS server to use may be specified. If no DNS server is specified, the
default server(s) specified in `/etc/resolv.conf` will be used.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
dns_lookup           | The hostname or IP to query the DNS for. Defaults to "$host_name$".
dns_server           | The DNS server to query. Defaults to the server configured in the OS.
dns_query_type       | The DNS record query type where TYPE =(A, AAAA, SRV, TXT, MX, ANY). The default query type is 'A' (IPv4 host entry)
dns_expected_answers | The answer(s) to look for. A hostname must end with a dot. Multiple answers must be defined as array.
dns_authoritative    | Expect the server to send an authoritative answer.
dns_accept_cname     | Accept cname responses as a valid result to a query.
dns_wtime            | Return warning if elapsed time exceeds value.
dns_ctime            | Return critical if elapsed time exceeds value.
dns_timeout          | Seconds before connection times out. Defaults to 10.

## file_age <a id="file-age"></a>

The [check_file_age](https://www.monitoring-plugins.org/doc/man/check_file_age.html) plugin
checks a file's size and modification time to make sure it's not empty and that it's sufficiently recent.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                   | Description
-----------------------|------------
file_age_file          | **Required.** File to monitor.
file_age_warning_time  | File must be no more than this many seconds old as warning threshold. Defaults to "240s".
file_age_critical_time | File must be no more than this many seconds old as critical threshold. Defaults to "600s".
file_age_warning_size  | File must be at least this many bytes long as warning threshold. No default given.
file_age_critical_size | File must be at least this many bytes long as critical threshold. Defaults to "0B".
file_age_ignoremissing | Return OK if the file does not exist. Defaults to false.

## flexlm <a id="flexlm"></a>

The [check_flexlm](https://www.monitoring-plugins.org/doc/man/check_flexlm.html) plugin
checks available flexlm license managers. Requires the `lmstat` command.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name               | Description
-------------------|------------
flexlm_licensefile | **Required.** Name of license file (usually license.dat).
flexlm_timeout     | Plugin time out in seconds. Defaults to 15.

## fping4 <a id="fping4"></a>

The [check_fping](https://www.monitoring-plugins.org/doc/man/check_fping.html) plugin
uses the `fping` command to ping the specified host for a fast check. Note that it is
necessary to set the `suid` flag on `fping`.

This CheckCommand expects an IPv4 address.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                   | Description
-----------------------|------------
fping_address          | The host's IPv4 address. Defaults to "$address$".
fping_wrta             | The RTA warning threshold in milliseconds. Defaults to 100.
fping_wpl              | The packet loss warning threshold in %. Defaults to 5.
fping_crta             | The RTA critical threshold in milliseconds. Defaults to 200.
fping_cpl              | The packet loss critical threshold in %. Defaults to 15.
fping_number           | The number of packets to send. Defaults to 5.
fping_interval         | The interval between packets in milli-seconds. Defaults to 500.
fping_bytes            | The size of ICMP packet.
fping_target_timeout   | The target timeout in milli-seconds.
fping_source_ip        | The name or ip address of the source ip.
fping_source_interface | The source interface name.

## fping6 <a id="fping6"></a>

The [check_fping](https://www.monitoring-plugins.org/doc/man/check_fping.html) plugin
will use the `fping` command to ping the specified host for a fast check. Note that it is
necessary to set the `suid` flag on `fping`.

This CheckCommand expects an IPv6 address.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                   | Description
-----------------------|------------
fping_address          | The host's IPv6 address. Defaults to "$address6$".
fping_wrta             | The RTA warning threshold in milliseconds. Defaults to 100.
fping_wpl              | The packet loss warning threshold in %. Defaults to 5.
fping_crta             | The RTA critical threshold in milliseconds. Defaults to 200.
fping_cpl              | The packet loss critical threshold in %. Defaults to 15.
fping_number           | The number of packets to send. Defaults to 5.
fping_interval         | The interval between packets in milli-seconds. Defaults to 500.
fping_bytes            | The size of ICMP packet.
fping_target_timeout   | The target timeout in milli-seconds.
fping_source_ip        | The name or ip address of the source ip.
fping_source_interface | The source interface name.

## ftp <a id="ftp"></a>

The [check_ftp](https://www.monitoring-plugins.org/doc/man/check_ftp.html) plugin
tests FTP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name            | Description
----------------|------------
ftp_address     | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ftp_port        | The FTP port number.
ftp_expect      | String to expect in server response. Multiple strings must be defined as array.
ftp_all         | All expect strings need to occur in server response. Defaults to false.
ftp_escape_send | Enable usage of \\n, \\r, \\t or \\\\ in send string.
ftp_send        | String to send to the server.
ftp_escape_quit | Enable usage of \\n, \\r, \\t or \\\\ in quit string.
ftp_quit        | String to send server to initiate a clean close of the connection.
ftp_refuse      | Accept TCP refusals with states ok, warn, crit. Defaults to crit.
ftp_mismatch    | Accept expected string mismatches with states ok, warn, crit. Defaults to warn.
ftp_jail        | Hide output from TCP socket.
ftp_maxbytes    | Close connection once more than this number of bytes are received.
ftp_delay       | Seconds to wait between sending string and polling for response.
ftp_certificate | Minimum number of days a certificate has to be valid. 1st value is number of days for warning, 2nd is critical (if not specified: 0) -- separated by comma.
ftp_ssl         | Use SSL for the connection. Defaults to false.
ftp_wtime       | Response time to result in warning status (seconds).
ftp_ctime       | Response time to result in critical status (seconds).
ftp_timeout     | Seconds before connection times out. Defaults to 10.
ftp_ipv4        | Use IPv4 connection. Defaults to false.
ftp_ipv6        | Use IPv6 connection. Defaults to false.

## game <a id="game"></a>

The [check_game](https://www.monitoring-plugins.org/doc/man/check_game.html) plugin
tests game server connections with the specified host.
This plugin uses the 'qstat' command, the popular game server status query tool.
If you don't have the package installed, you will need to [download](http://www.activesw.com/people/steve/qstat.html)
or install the package `quakestat` before you can use this plugin.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name           | Description
---------------|------------
game_game      | **Required.** Name of the game.
game_ipaddress | **Required.** Ipaddress of the game server to query.
game_timeout   | Seconds before connection times out. Defaults to 10.
game_port      | Port to connect to.
game_gamefield | Field number in raw qstat output that contains game name.
game_mapfield  | Field number in raw qstat output that contains map name.
game_pingfield | Field number in raw qstat output that contains ping time.
game_gametime  | Field number in raw qstat output that contains game time.
game_hostname  | Name of the host running the game.

## hostalive <a id="hostalive"></a>

Check command object for the [check_ping](https://www.monitoring-plugins.org/doc/man/check_ping.html)
plugin with host check default values. This variant uses the host's `address` attribute
if available and falls back to using the `address6` attribute if the `address` attribute is not set.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ping_address | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ping_wrta    | The RTA warning threshold in milliseconds. Defaults to 3000.
ping_wpl     | The packet loss warning threshold in %. Defaults to 80.
ping_crta    | The RTA critical threshold in milliseconds. Defaults to 5000.
ping_cpl     | The packet loss critical threshold in %. Defaults to 100.
ping_packets | The number of packets to send. Defaults to 5.
ping_timeout | The plugin timeout in seconds. Defaults to 0 (no timeout).

## hostalive4 <a id="hostalive4"></a>

Check command object for the [check_ping](https://www.monitoring-plugins.org/doc/man/check_ping.html)
plugin with host check default values. This variant uses the host's `address` attribute.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ping_address | The host's IPv4 address. Defaults to "$address$".
ping_wrta    | The RTA warning threshold in milliseconds. Defaults to 3000.
ping_wpl     | The packet loss warning threshold in %. Defaults to 80.
ping_crta    | The RTA critical threshold in milliseconds. Defaults to 5000.
ping_cpl     | The packet loss critical threshold in %. Defaults to 100.
ping_packets | The number of packets to send. Defaults to 5.
ping_timeout | The plugin timeout in seconds. Defaults to 0 (no timeout).

## hostalive6 <a id="hostalive6"></a>

Check command object for the [check_ping](https://www.monitoring-plugins.org/doc/man/check_ping.html)
plugin with host check default values. This variant uses the host's `address6` attribute.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ping_address | The host's IPv6 address. Defaults to "$address6$".
ping_wrta    | The RTA warning threshold in milliseconds. Defaults to 3000.
ping_wpl     | The packet loss warning threshold in %. Defaults to 80.
ping_crta    | The RTA critical threshold in milliseconds. Defaults to 5000.
ping_cpl     | The packet loss critical threshold in %. Defaults to 100.
ping_packets | The number of packets to send. Defaults to 5.
ping_timeout | The plugin timeout in seconds. Defaults to 0 (no timeout).

## hpjd <a id="hpjd"></a>

The [check_hpjd](https://www.monitoring-plugins.org/doc/man/check_hpjd.html) plugin
tests the state of an HP printer with a JetDirect card. Net-snmp must be installed
on the computer running the plugin.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name           | Description
---------------|------------
hpjd_address   | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
hpjd_port      | The host's SNMP port. Defaults to 161.
hpjd_community | The SNMP community. Defaults  to "public".

## http <a id="http"></a>

The [check_http](https://www.monitoring-plugins.org/doc/man/check_http.html) plugin
tests the HTTP service on the specified host. It can test normal (http) and secure
(https) servers, follow redirects, search for strings and regular expressions,
check connection times, and report on certificate expiration times.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                             | Description
---------------------------------|------------
http_address                     | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
http_vhost                       | The virtual host that should be sent in the "Host" header.
http_uri                         | The request URI for GET or POST. Defaults to `/`.
http_port                        | The TCP port. Defaults to 80 when not using SSL, 443 otherwise.
http_ssl                         | Whether to use SSL. Defaults to false.
http_ssl_force_tlsv1             | Whether to force TLSv1.
http_ssl_force_tlsv1_1           | Whether to force TLSv1.1.
http_ssl_force_tlsv1_2           | Whether to force TLSv1.2.
http_ssl_force_sslv2             | Whether to force SSLv2.
http_ssl_force_sslv3             | Whether to force SSLv3.
http_ssl_force_tlsv1_or_higher   | Whether to force TLSv1 or higher.
http_ssl_force_tlsv1_1_or_higher | Whether to force TLSv1.1 or higher.
http_ssl_force_tlsv1_2_or_higher | Whether to force TLSv1.2 or higher.
http_ssl_force_sslv2_or_higher   | Whether to force SSLv2 or higher.
http_ssl_force_sslv3_or_higher   | Whether to force SSLv3 or higher.
http_sni                         | Whether to use SNI. Defaults to false.
http_auth_pair                   | Add 'username:password' authorization pair.
http_proxy_auth_pair             | Add 'username:password' authorization pair for proxy.
http_ignore_body                 | Don't download the body, just the headers.
http_linespan                    | Allow regex to span newline.
http_expect_body_regex           | A regular expression which the body must match against. Incompatible with http_ignore_body.
http_expect_body_eregi           | A case-insensitive expression which the body must match against. Incompatible with http_ignore_body.
http_invertregex                 | Changes behavior of http_expect_body_regex and http_expect_body_eregi to return CRITICAL if found, OK if not.
http_warn_time                   | The warning threshold.
http_critical_time               | The critical threshold.
http_expect                      | Comma-delimited list of strings, at least one of them is expected in the first (status) line of the server response. Default: HTTP/1.
http_certificate                 | Minimum number of days a certificate has to be valid. Port defaults to 443. When this option is used the URL is not checked. The first parameter defines the warning threshold (in days), the second parameter the critical threshold (in days). (Example `http_certificate = "30,20"`).
http_clientcert                  | Name of file contains the client certificate (PEM format).
http_privatekey                  | Name of file contains the private key (PEM format).
http_headerstring                | String to expect in the response headers.
http_string                      | String to expect in the content.
http_post                        | URL encoded http POST data.
http_method                      | Set http method (for example: HEAD, OPTIONS, TRACE, PUT, DELETE).
http_maxage                      | Warn if document is more than seconds old.
http_contenttype                 | Specify Content-Type header when POSTing.
http_useragent                   | String to be sent in http header as User Agent.
http_header                      | Any other tags to be sent in http header.
http_extendedperfdata            | Print additional perfdata. Defaults to false.
http_onredirect                  | How to handle redirect pages. Possible values: "ok" (default), "warning", "critical", "follow", "sticky" (like follow but stick to address), "stickyport" (like sticky but also to port)
http_pagesize                    | Minimum page size required:Maximum page size required.
http_timeout                     | Seconds before connection times out.
http_ipv4                        | Use IPv4 connection. Defaults to false.
http_ipv6                        | Use IPv6 connection. Defaults to false.
http_link                        | Wrap output in HTML link. Defaults to false.
http_verbose                     | Show details for command-line debugging. Defaults to false.

## icmp <a id="icmp"></a>

The [check_icmp](https://www.monitoring-plugins.org/doc/man/check_icmp.html) plugin
check_icmp allows for checking multiple hosts at once compared to `check_ping`.
The main difference is that check_ping executes the system's ping(1) command and
parses its output while `check_icmp` talks ICMP itself. `check_icmp` must be installed with
`setuid` root.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
icmp_address         | The host's address. This can either be a single address or an array of addresses. Defaults to "$address$".
icmp_wrta            | The RTA warning threshold in milliseconds. Defaults to 100.
icmp_wpl             | The packet loss warning threshold in %. Defaults to 5.
icmp_crta            | The RTA critical threshold in milliseconds. Defaults to 200.
icmp_cpl             | The packet loss critical threshold in %. Defaults to 15.
icmp_source          | The source IP address to send packets from.
icmp_packets         | The number of packets to send. Defaults to 5.
icmp_packet_interval | The maximum packet interval. Defaults to 80 (milliseconds).
icmp_target_interval | The maximum target interval.
icmp_hosts_alive     | The number of hosts which have to be alive for the check to succeed.
icmp_data_bytes      | Payload size for each ICMP request. Defaults to 8.
icmp_timeout         | The plugin timeout in seconds. Defaults to 10 (seconds).
icmp_ttl             | The TTL on outgoing packets.

## imap <a id="imap"></a>

The [check_imap](https://www.monitoring-plugins.org/doc/man/check_imap.html) plugin
tests IMAP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
imap_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
imap_port            | The port that should be checked. Defaults to 143.
imap_escape          | Can use \\n, \\r, \\t or \\ in send or quit string. Must come before send or quit option. Default: nothing added to send, \\r\\n added to end of quit.
imap_send            | String to send to the server.
imap_expect          | String to expect in server response. Multiple strings must be defined as array.
imap_all             | All expect strings need to occur in server response. Default is any.
imap_quit            | String to send server to initiate a clean close of the connection.
imap_refuse          | Accept TCP refusals with states ok, warn, crit (default: crit).
imap_mismatch        | Accept expected string mismatches with states ok, warn, crit (default: warn).
imap_jail            | Hide output from TCP socket.
imap_maxbytes        | Close connection once more than this number of bytes are received.
imap_delay           | Seconds to wait between sending string and polling for response.
imap_certificate_age | Minimum number of days a certificate has to be valid.
imap_ssl             | Use SSL for the connection.
imap_warning         | Response time to result in warning status (seconds).
imap_critical        | Response time to result in critical status (seconds).
imap_timeout         | Seconds before connection times out (default: 10).
imap_ipv4            | Use IPv4 connection. Defaults to false.
imap_ipv6            | Use IPv6 connection. Defaults to false.

## ldap <a id="ldap"></a>

The [check_ldap](https://www.monitoring-plugins.org/doc/man/check_ldap.html) plugin
can be used to check LDAP servers.

The plugin can also be used for monitoring ldaps connections instead of the deprecated `check_ldaps`.
This can be ensured by enabling `ldap_starttls` or `ldap_ssl`.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                  | Description
----------------------|------------
ldap_address          | Host name, IP Address, or unix socket (must be an absolute path). Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ldap_port             | Port number. Defaults to 389.
ldap_attr             | LDAP attribute to search for (default: "(objectclass=*)")
ldap_base             | **Required.** LDAP base (eg. ou=myunit,o=myorg,c=at).
ldap_bind             | LDAP bind DN (if required).
ldap_pass             | LDAP password (if required).
ldap_starttls         | Use STARTSSL mechanism introduced in protocol version 3.
ldap_ssl              | Use LDAPS (LDAP v2 SSL method). This also sets the default port to 636.
ldap_v2               | Use LDAP protocol version 2 (enabled by default).
ldap_v3               | Use LDAP protocol version 3 (disabled by default)
ldap_warning          | Response time to result in warning status (seconds).
ldap_critical         | Response time to result in critical status (seconds).
ldap_warning_entries  | Number of found entries to result in warning status.
ldap_critical_entries | Number of found entries to result in critical status.
ldap_timeout          | Seconds before connection times out (default: 10).
ldap_verbose          | Show details for command-line debugging (disabled by default)

## load <a id="load"></a>

The [check_load](https://www.monitoring-plugins.org/doc/man/check_load.html) plugin
tests the current system load average.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
load_wload1  | The 1-minute warning threshold. Defaults to 5.
load_wload5  | The 5-minute warning threshold. Defaults to 4.
load_wload15 | The 15-minute warning threshold. Defaults to 3.
load_cload1  | The 1-minute critical threshold. Defaults to 10.
load_cload5  | The 5-minute critical threshold. Defaults to 6.
load_cload15 | The 15-minute critical threshold. Defaults to 4.
load_percpu  | Divide the load averages by the number of CPUs (when possible). Defaults to false.

## mailq <a id="mailq"></a>

The [check_mailq](https://www.monitoring-plugins.org/doc/man/check_mailq.html) plugin
checks the number of messages in the mail queue (supports multiple sendmail queues, qmail).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                  | Description
----------------------|------------
mailq_warning         | **Required.** Min. number of messages in queue to generate warning.
mailq_critical        | **Required.** Min. number of messages in queue to generate critical alert ( w < c ).
mailq_domain_warning  | Min. number of messages for same domain in queue to generate warning
mailq_domain_critical | Min. number of messages for same domain in queue to generate critical alert ( W < C ).
mailq_timeout         | Plugin timeout in seconds (default = 15).
mailq_servertype      | [ sendmail \| qmail \| postfix \| exim \| nullmailer ] (default = autodetect).
mailq_sudo            | Use sudo to execute the mailq command.

## mysql <a id="mysql"></a>

The [check_mysql](https://www.monitoring-plugins.org/doc/man/check_mysql.html) plugin
tests connections to a MySQL server.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name              | Description
------------------|------------
mysql_hostname    | Host name, IP Address, or unix socket (must be an absolute path).
mysql_port        | Port number (default: 3306).
mysql_socket      | Use the specified socket (has no effect if `mysql_hostname` is used).
mysql_ignore_auth | Ignore authentication failure and check for mysql connectivity only.
mysql_database    | Check database with indicated name.
mysql_file        | Read from the specified client options file.
mysql_group       | Use a client options group.
mysql_username    | Connect using the indicated username.
mysql_password    | Use the indicated password to authenticate the connection.
mysql_check_slave | Check if the slave thread is running properly.
mysql_warning     | Exit with WARNING status if slave server is more than INTEGER seconds behind master.
mysql_critical    | Exit with CRITICAL status if slave server is more then INTEGER seconds behind master.
mysql_ssl         | Use ssl encryption.
mysql_cacert      | Path to CA signing the cert.
mysql_cert        | Path to SSL certificate.
mysql_key         | Path to private SSL key.
mysql_cadir       | Path to CA directory.
mysql_ciphers     | List of valid SSL ciphers.

## mysql_query <a id="mysql-query"></a>

The [check_mysql_query](https://www.monitoring-plugins.org/doc/man/check_mysql_query.html) plugin
checks a query result against threshold levels.
The result from the query should be numeric. For extra security, create a user with minimal access.

**Note**: You must specify `mysql_query_password` with an empty string to force an empty password,
overriding any my.cnf settings.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
mysql_query_hostname | Host name, IP Address, or unix socket (must be an absolute path).
mysql_query_port     | Port number (default: 3306).
mysql_query_database | Check database with indicated name.
mysql_query_file     | Read from the specified client options file.
mysql_query_group    | Use a client options group.
mysql_query_username | Connect using the indicated username.
mysql_query_password | Use the indicated password to authenticate the connection.
mysql_query_execute  | **Required.** SQL Query to run on the MySQL Server.
mysql_query_warning  | Exit with WARNING status if query is outside of the range (format: start:end).
mysql_query_critical | Exit with CRITICAL status if query is outside of the range.

## negate <a id="negate"></a>

The [negate](https://www.monitoring-plugins.org/doc/man/negate.html) plugin
negates the status of a plugin (returns OK for CRITICAL and vice-versa).
Additional switches can be used to control which state becomes what.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                  | Description
----------------------|------------
negate_timeout        | Seconds before plugin times out (default: 11).
negate_timeout_result | Custom result on Negate timeouts, default to UNKNOWN.
negate_ok             | OK, WARNING, CRITICAL or UNKNOWN.
negate_warning        | Numeric values are accepted.
negate_critical       | If nothing is specified,
negate_unknown        | permutes OK and CRITICAL.
negate_substitute     | Substitute output text as well. Will only substitute text in CAPITALS.
negate_command        | **Required.** Command to be negated.
negate_arguments      | Arguments for the negated command.

## nrpe <a id="nrpe"></a>

The `check_nrpe` plugin can be used to query an [NRPE](https://docs.icinga.com/latest/en/nrpe.html)
server or [NSClient++](https://www.nsclient.org). **Note**: This plugin
is considered insecure/deprecated.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
nrpe_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
nrpe_port            | The NRPE port. Defaults to 5666.
nrpe_command         | The command that should be executed.
nrpe_no_ssl          | Whether to disable SSL or not. Defaults to `false`.
nrpe_timeout_unknown | Whether to set timeouts to unknown instead of critical state. Defaults to `false`.
nrpe_timeout         | The timeout in seconds.
nrpe_arguments       | Arguments that should be passed to the command. Multiple arguments must be defined as array.
nrpe_ipv4            | Use IPv4 connection. Defaults to false.
nrpe_ipv6            | Use IPv6 connection. Defaults to false.
nrpe_version_2       | Use this if you want to connect using NRPE v2 protocol. Defaults to false.

## nscp <a id="nscp"></a>

The [check_nt](https://www.monitoring-plugins.org/doc/man/check_nt.html) plugin
collects data from the [NSClient++](https://www.nsclient.org) service.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name          | Description
--------------|------------
nscp_address  | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
nscp_port     | The NSClient++ port. Defaults to 12489.
nscp_password | The NSClient++ password.
nscp_variable | **Required.** The variable that should be checked.
nscp_params   | Parameters for the query. Multiple parameters must be defined as array.
nscp_warn     | The warning threshold.
nscp_crit     | The critical threshold.
nscp_timeout  | The query timeout in seconds.
nscp_showall  | Use with SERVICESTATE to see working services or PROCSTATE for running processes. Defaults to false.

## ntp_peer <a id="ntp-peer"></a>

The [check_ntp_peer](https://www.monitoring-plugins.org/doc/man/check_ntp_peer.html) plugin
checks the health of an NTP server. It supports checking the offset with the sync peer, the
jitter and stratum. This plugin will not check the clock offset between the local host and NTP
 server; please use `ntp_time` for that purpose.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ntp_address  | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ntp_port     | The port to use. Default to 123.
ntp_quiet    | Returns UNKNOWN instead of CRITICAL or WARNING if server isn't synchronized.
ntp_warning  | Offset to result in warning status (seconds).
ntp_critical | Offset to result in critical status (seconds).
ntp_wstratum | Warning threshold for stratum of server's synchronization peer.
ntp_cstratum | Critical threshold for stratum of server's synchronization peer.
ntp_wjitter  | Warning threshold for jitter.
ntp_cjitter  | Critical threshold for jitter.
ntp_wsource  | Warning threshold for number of usable time sources.
ntp_csource  | Critical threshold for number of usable time sources.
ntp_timeout  | Seconds before connection times out (default: 10).
ntp_ipv4     | Use IPv4 connection. Defaults to false.
ntp_ipv6     | Use IPv6 connection. Defaults to false.

## ntp_time <a id="ntp-time"></a>

The [check_ntp_time](https://www.monitoring-plugins.org/doc/man/check_ntp_time.html) plugin
checks the clock offset between the local host and a remote NTP server.

**Note**: If you want to monitor an NTP server, please use `ntp_peer`.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name           | Description
---------------|------------
ntp_address    | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ntp_port       | Port number (default: 123).
ntp_quiet      | Returns UNKNOWN instead of CRITICAL if offset cannot be found.
ntp_warning    | Offset to result in warning status (seconds).
ntp_critical   | Offset to result in critical status (seconds).
ntp_timeoffset | Expected offset of the ntp server relative to local server (seconds).
ntp_timeout    | Seconds before connection times out (default: 10).
ntp_ipv4       | Use IPv4 connection. Defaults to false.
ntp_ipv6       | Use IPv6 connection. Defaults to false.

## passive <a id="passive"></a>

Specialised check command object for passive checks executing the `check_dummy` plugin with appropriate default values.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name        | Description
------------|------------
dummy_state | The state. Can be one of 0 (ok), 1 (warning), 2 (critical) and 3 (unknown). Defaults to 3.
dummy_text  | Plugin output. Defaults to "No Passive Check Result Received.".

## pgsql <a id="pgsql"></a>

The [check_pgsql](https://www.monitoring-plugins.org/doc/man/check_pgsql.html) plugin
tests a PostgreSQL DBMS to determine whether it is active and accepting queries.
If a query is specified using the `pgsql_query` attribute, it will be executed after
connecting to the server. The result from the query has to be numeric in order
to compare it against the query thresholds if set.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
pgsql_hostname       | Host name, IP Address, or unix socket (must be an absolute path).
pgsql_port           | Port number (default: 5432).
pgsql_database       | Database to check (default: template1).
pgsql_username       | Login name of user.
pgsql_password       | Password (BIG SECURITY ISSUE).
pgsql_options        | Connection parameters (keyword = value), see below.
pgsql_warning        | Response time to result in warning status (seconds).
pgsql_critical       | Response time to result in critical status (seconds).
pgsql_timeout        | Seconds before connection times out (default: 10).
pgsql_query          | SQL query to run. Only first column in first row will be read.
pgsql_query_warning  | SQL query value to result in warning status (double).
pgsql_query_critical | SQL query value to result in critical status (double).

## ping <a id="ping"></a>

The [check_ping](https://www.monitoring-plugins.org/doc/man/check_ping.html) plugin
uses the ping command to probe the specified host for packet loss (percentage) and
round trip average (milliseconds).

This command uses the host's `address` attribute if available and falls back to using
the `address6` attribute if the `address` attribute is not set.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ping_address | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ping_wrta    | The RTA warning threshold in milliseconds. Defaults to 100.
ping_wpl     | The packet loss warning threshold in %. Defaults to 5.
ping_crta    | The RTA critical threshold in milliseconds. Defaults to 200.
ping_cpl     | The packet loss critical threshold in %. Defaults to 15.
ping_packets | The number of packets to send. Defaults to 5.
ping_timeout | The plugin timeout in seconds. Defaults to 0 (no timeout).

## ping4 <a id="ping4"></a>

The [check_ping](https://www.monitoring-plugins.org/doc/man/check_ping.html) plugin
uses the ping command to probe the specified host for packet loss (percentage) and
round trip average (milliseconds).

This command uses the host's `address` attribute if not explicitly specified using
the `ping_address` attribute.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ping_address | The host's IPv4 address. Defaults to "$address$".
ping_wrta    | The RTA warning threshold in milliseconds. Defaults to 100.
ping_wpl     | The packet loss warning threshold in %. Defaults to 5.
ping_crta    | The RTA critical threshold in milliseconds. Defaults to 200.
ping_cpl     | The packet loss critical threshold in %. Defaults to 15.
ping_packets | The number of packets to send. Defaults to 5.
ping_timeout | The plugin timeout in seconds. Defaults to 0 (no timeout).

## ping6 <a id="ping6"></a>

The [check_ping](https://www.monitoring-plugins.org/doc/man/check_ping.html) plugin
uses the ping command to probe the specified host for packet loss (percentage) and
round trip average (milliseconds).

This command uses the host's `address6` attribute if not explicitly specified using
the `ping_address` attribute.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ping_address | The host's IPv6 address. Defaults to "$address6$".
ping_wrta    | The RTA warning threshold in milliseconds. Defaults to 100.
ping_wpl     | The packet loss warning threshold in %. Defaults to 5.
ping_crta    | The RTA critical threshold in milliseconds. Defaults to 200.
ping_cpl     | The packet loss critical threshold in %. Defaults to 15.
ping_packets | The number of packets to send. Defaults to 5.
ping_timeout | The plugin timeout in seconds. Defaults to 0 (no timeout).

## pop <a id="pop"></a>

The [check_pop](https://www.monitoring-plugins.org/doc/man/check_pop.html) plugin
tests POP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                | Description
--------------------|------------
pop_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
pop_port            | The port that should be checked. Defaults to 110.
pop_escape          | Can use \\n, \\r, \\t or \\ in send or quit string. Must come before send or quit option. Default: nothing added to send, \\r\\n added to end of quit.
pop_send            | String to send to the server.
pop_expect          | String to expect in server response. Multiple strings must be defined as array.
pop_all             | All expect strings need to occur in server response. Default is any.
pop_quit            | String to send server to initiate a clean close of the connection.
pop_refuse          | Accept TCP refusals with states ok, warn, crit (default: crit).
pop_mismatch        | Accept expected string mismatches with states ok, warn, crit (default: warn).
pop_jail            | Hide output from TCP socket.
pop_maxbytes        | Close connection once more than this number of bytes are received.
pop_delay           | Seconds to wait between sending string and polling for response.
pop_certificate_age | Minimum number of days a certificate has to be valid.
pop_ssl             | Use SSL for the connection.
pop_warning         | Response time to result in warning status (seconds).
pop_critical        | Response time to result in critical status (seconds).
pop_timeout         | Seconds before connection times out (default: 10).
pop_ipv4            | Use IPv4 connection. Defaults to false.
pop_ipv6            | Use IPv6 connection. Defaults to false.

## procs <a id="processes"></a>

The [check_procs](https://www.monitoring-plugins.org/doc/man/check_procs.html) plugin
checks all processes and generates WARNING or CRITICAL states if the specified
metric is outside the required threshold ranges. The metric defaults to number
of processes. Search filters can be applied to limit the processes to check.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
procs_warning        | The process count warning threshold. Defaults to 250.
procs_critical       | The process count critical threshold. Defaults to 400.
procs_metric         | Check thresholds against metric.
procs_timeout        | Seconds before plugin times out.
procs_traditional    | Filter own process the traditional way by PID instead of /proc/pid/exe. Defaults to false.
procs_state          | Only scan for processes that have one or more of the status flags you specify.
procs_ppid           | Only scan for children of the parent process ID indicated.
procs_vsz            | Only scan for processes with VSZ higher than indicated.
procs_rss            | Only scan for processes with RSS higher than indicated.
procs_pcpu           | Only scan for processes with PCPU higher than indicated.
procs_user           | Only scan for processes with user name or ID indicated.
procs_argument       | Only scan for processes with args that contain STRING.
procs_argument_regex | Only scan for processes with args that contain the regex STRING.
procs_command        | Only scan for exact matches of COMMAND (without path).
procs_nokthreads     | Only scan for non kernel threads. Defaults to false.

## radius <a id="radius"></a>

The [check_radius](https://www.monitoring-plugins.org/doc/man/check_radius.html) plugin
checks a RADIUS server to see if it is accepting connections.  The server to test
must be specified in the invocation, as well as a user name and password. A configuration
file may also be present. The format of the configuration file is described in the
radiusclient library sources.  The password option presents a substantial security
issue because the password can possibly be determined by careful watching of the
command line in a process listing. This risk is exacerbated because the plugin will
typically be executed at regular predictable intervals. Please be sure that the
password used does not allow access to sensitive system resources.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name               | Description
-------------------|------------
radius_address     | The radius server's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
radius_config_file | **Required.** The radius configuration file.
radius_username    | **Required.** The radius username to test.
radius_password    | **Required.** The radius password to test.
radius_port        | The radius port number (default 1645).
radius_nas_id      | The NAS identifier.
radius_nas_address | The NAS IP address.
radius_expect      | The response string to expect from the server.
radius_retries     | The number of times to retry a failed connection.
radius_timeout     | The number of seconds before connection times out (default: 10).

## rpc <a id="rpc"></a>

The [check_rpc](https://www.monitoring-plugins.org/doc/man/check_rpc.html)
plugin tests if a service is registered and running using `rpcinfo -H host -C rpc_command`.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name        | Description
------------|------------
rpc_address | The rpc host address. Defaults to "$address$ if the host `address` attribute is set, "$address6$" otherwise.
rpc_command | **Required.** The programm name (or number).
rpc_port    | The port that should be checked.
rpc_version | The version you want to check for (one or more).
rpc_udp     | Use UDP test. Defaults to false.
rpc_tcp     | Use TCP test. Defaults to false.
rpc_verbose | Show verbose output. Defaults to false.

## simap <a id="simap"></a>

The [check_simap](https://www.monitoring-plugins.org/doc/man/check_simap.html) plugin
tests SIMAP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                  | Description
----------------------|------------
simap_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
simap_port            | The port that should be checked. Defaults to 993.
simap_escape          | Can use \\n, \\r, \\t or \\ in send or quit string. Must come before send or quit option. Default: nothing added to send, \\r\\n added to end of quit.
simap_send            | String to send to the server.
simap_expect          | String to expect in server response. Multiple strings must be defined as array.
simap_all             | All expect strings need to occur in server response. Default is any.
simap_quit            | String to send server to initiate a clean close of the connection.
simap_refuse          | Accept TCP refusals with states ok, warn, crit (default: crit).
simap_mismatch        | Accept expected string mismatches with states ok, warn, crit (default: warn).
simap_jail            | Hide output from TCP socket.
simap_maxbytes        | Close connection once more than this number of bytes are received.
simap_delay           | Seconds to wait between sending string and polling for response.
simap_certificate_age | Minimum number of days a certificate has to be valid.
simap_ssl             | Use SSL for the connection.
simap_warning         | Response time to result in warning status (seconds).
simap_critical        | Response time to result in critical status (seconds).
simap_timeout         | Seconds before connection times out (default: 10).
simap_ipv4            | Use IPv4 connection. Defaults to false.
simap_ipv6            | Use IPv6 connection. Defaults to false.

## smart <a id="smart"></a>

The [check_ide_smart](https://www.monitoring-plugins.org/doc/man/check_ide_smart.html) plugin
checks a local hard drive with the (Linux specific) SMART interface. Requires installation of `smartctl`.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
smart_device | **Required.** The name of a local hard drive to monitor.

## smtp <a id="smtp"></a>

The [check_smtp](https://www.monitoring-plugins.org/doc/man/check_smtp.html) plugin
will attempt to open an SMTP connection with the host.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
smtp_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
smtp_port            | The port that should be checked. Defaults to 25.
smtp_mail_from       | Test a MAIL FROM command with the given email address.
smtp_expect          | String to expect in first line of server response (default: '220').
smtp_command         | SMTP command (may be used repeatedly).
smtp_response        | Expected response to command (may be used repeatedly).
smtp_helo_fqdn       | FQDN used for HELO
smtp_certificate_age | Minimum number of days a certificate has to be valid.
smtp_starttls        | Use STARTTLS for the connection.
smtp_authtype        | SMTP AUTH type to check (default none, only LOGIN supported).
smtp_authuser        | SMTP AUTH username.
smtp_authpass        | SMTP AUTH password.
smtp_ignore_quit     | Ignore failure when sending QUIT command to server.
smtp_warning         | Response time to result in warning status (seconds).
smtp_critical        | Response time to result in critical status (seconds).
smtp_timeout         | Seconds before connection times out (default: 10).
smtp_ipv4            | Use IPv4 connection. Defaults to false.
smtp_ipv6            | Use IPv6 connection. Defaults to false.

## snmp <a id="snmp"></a>

The [check_snmp](https://www.monitoring-plugins.org/doc/man/check_snmp.html) plugin
checks the status of remote machines and obtains system information via SNMP.

**Note**: This plugin uses the `snmpget` command included with the NET-SNMP package.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                  | Description
----------------------|------------
snmp_address          | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
snmp_oid              | **Required.** The SNMP OID.
snmp_community        | The SNMP community. Defaults to "public".
snmp_port             | The SNMP port. Defaults to "161".
snmp_retries          | Number of retries to be used in the SNMP requests.
snmp_warn             | The warning threshold.
snmp_crit             | The critical threshold.
snmp_string           | Return OK state if the string matches exactly with the output value
snmp_ereg             | Return OK state if extended regular expression REGEX matches with the output value
snmp_eregi            | Return OK state if case-insensitive extended REGEX matches with the output value
snmp_label            | Prefix label for output value
snmp_invert_search    | Invert search result and return CRITICAL state if found
snmp_units            | Units label(s) for output value (e.g., 'sec.').
snmp_version          | Version to use. E.g. 1, 2, 2c or 3.
snmp_miblist          | MIB's to use, comma separated. Defaults to "ALL".
snmp_rate_multiplier  | Converts rate per second. For example, set to 60 to convert to per minute.
snmp_rate             | Boolean. Enable rate calculation.
snmp_getnext          | Boolean. Use SNMP GETNEXT. Defaults to false.
snmp_timeout          | The command timeout in seconds. Defaults to 10 seconds.
snmp_offset           | Add/subtract the specified OFFSET to numeric sensor data.
snmp_output_delimiter | Separates output on multiple OID requests.
snmp_perf_oids        | Label performance data with OIDs instead of --label's.

## snmp-uptime <a id="snmp-uptime"></a>

Check command object for the [check_snmp](https://www.monitoring-plugins.org/doc/man/check_snmp.html)
plugin, using the uptime OID by default.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name           | Description
---------------|------------
snmp_address   | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
snmp_oid       | The SNMP OID. Defaults to "1.3.6.1.2.1.1.3.0".
snmp_community | The SNMP community. Defaults to "public".

## snmpv3 <a id="snmpv3"></a>

Check command object for the [check_snmp](https://www.monitoring-plugins.org/doc/man/check_snmp.html)
plugin, using SNMPv3 authentication and encryption options.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                   | Description
-----------------------|------------
snmpv3_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
snmpv3_getnext         | Use SNMP GETNEXT instead of SNMP GET.
snmpv3_seclevel        | The security level. Defaults to authPriv.
snmpv3_auth_alg        | The authentication algorithm. Defaults to SHA.
snmpv3_user            | **Required.** The username to log in with.
snmpv3_auth_key        | **Required,** The authentication key. Required if `snmpv3_seclevel` is set to `authPriv` otherwise optional.
snmpv3_priv_key        | **Required.** The encryption key.
snmpv3_oid             | **Required.** The SNMP OID.
snmpv3_priv_alg        | The encryption algorithm. Defaults to AES.
snmpv3_warn            | The warning threshold.
snmpv3_crit            | The critical threshold.
snmpv3_string          | Return OK state (for that OID) if STRING is an exact match.
snmpv3_ereg            | Return OK state (for that OID) if extended regular expression REGEX matches.
snmpv3_eregi           | Return OK state (for that OID) if case-insensitive extended REGEX matches.
snmpv3_invert_search   | Invert search result and return CRITICAL if found
snmpv3_label           | Prefix label for output value.
snmpv3_units           | Units label(s) for output value (e.g., 'sec.').
snmpv3_rate_multiplier | Converts rate per second. For example, set to 60 to convert to per minute.
snmpv3_rate            | Boolean. Enable rate calculation.
snmpv3_timeout         | The command timeout in seconds. Defaults to 10 seconds.

## spop <a id="spop"></a>

The [check_spop](https://www.monitoring-plugins.org/doc/man/check_spop.html) plugin
tests SPOP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                 | Description
---------------------|------------
spop_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
spop_port            | The port that should be checked. Defaults to 995.
spop_escape          | Can use \\n, \\r, \\t or \\ in send or quit string. Must come before send or quit option. Default: nothing added to send, \\r\\n added to end of quit.
spop_send            | String to send to the server.
spop_expect          | String to expect in server response. Multiple strings must be defined as array.
spop_all             | All expect strings need to occur in server response. Default is any.
spop_quit            | String to send server to initiate a clean close of the connection.
spop_refuse          | Accept TCP refusals with states ok, warn, crit (default: crit).
spop_mismatch        | Accept expected string mismatches with states ok, warn, crit (default: warn).
spop_jail            | Hide output from TCP socket.
spop_maxbytes        | Close connection once more than this number of bytes are received.
spop_delay           | Seconds to wait between sending string and polling for response.
spop_certificate_age | Minimum number of days a certificate has to be valid.
spop_ssl             | Use SSL for the connection.
spop_warning         | Response time to result in warning status (seconds).
spop_critical        | Response time to result in critical status (seconds).
spop_timeout         | Seconds before connection times out (default: 10).
spop_ipv4            | Use IPv4 connection. Defaults to false.
spop_ipv6            | Use IPv6 connection. Defaults to false.

## ssh <a id="ssh"></a>

The [check_ssh](https://www.monitoring-plugins.org/doc/man/check_ssh.html) plugin
connects to an SSH server at a specified host and port.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name        | Description
------------|------------
ssh_address | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ssh_port    | The port that should be checked. Defaults to 22.
ssh_timeout | Seconds before connection times out. Defaults to 10.
ssh_ipv4    | Use IPv4 connection. Defaults to false.
ssh_ipv6    | Use IPv6 connection. Defaults to false.

## ssl <a id="ssl"></a>

Check command object for the [check_tcp](https://www.monitoring-plugins.org/doc/man/check_tcp.html) plugin,
using ssl-related options.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                         | Description
-----------------------------|------------
ssl_address                  | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ssl_port                     | The port that should be checked. Defaults to 443.
ssl_timeout                  | Timeout in seconds for the connect and handshake. The plugin default is 10 seconds.
ssl_cert_valid_days_warn     | Warning threshold for days before the certificate will expire. When used, the default for ssl_cert_valid_days_critical is 0.
ssl_cert_valid_days_critical | Critical threshold for days before the certificate will expire. When used, ssl_cert_valid_days_warn must also be set.
ssl_sni                      | The `server_name` that is send to select the SSL certificate to check. Important if SNI is used.

## ssmtp <a id="ssmtp"></a>

The [check_ssmtp](https://www.monitoring-plugins.org/doc/man/check_ssmtp.html) plugin
tests SSMTP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name                  | Description
----------------------|------------
ssmtp_address         | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ssmtp_port            | The port that should be checked. Defaults to 465.
ssmtp_escape          | Can use \\n, \\r, \\t or \\ in send or quit string. Must come before send or quit option. Default: nothing added to send, \\r\\n added to end of quit.
ssmtp_send            | String to send to the server.
ssmtp_expect          | String to expect in server response. Multiple strings must be defined as array.
ssmtp_all             | All expect strings need to occur in server response. Default is any.
ssmtp_quit            | String to send server to initiate a clean close of the connection.
ssmtp_refuse          | Accept TCP refusals with states ok, warn, crit (default: crit).
ssmtp_mismatch        | Accept expected string mismatches with states ok, warn, crit (default: warn).
ssmtp_jail            | Hide output from TCP socket.
ssmtp_maxbytes        | Close connection once more than this number of bytes are received.
ssmtp_delay           | Seconds to wait between sending string and polling for response.
ssmtp_certificate_age | Minimum number of days a certificate has to be valid.
ssmtp_ssl             | Use SSL for the connection.
ssmtp_warning         | Response time to result in warning status (seconds).
ssmtp_critical        | Response time to result in critical status (seconds).
ssmtp_timeout         | Seconds before connection times out (default: 10).
ssmtp_ipv4            | Use IPv4 connection. Defaults to false.
ssmtp_ipv6            | Use IPv6 connection. Defaults to false.

## swap <a id="swap"></a>

The [check_swap](https://www.monitoring-plugins.org/doc/man/check_swap.html) plugin
checks the swap space on a local machine.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name          | Description
--------------|------------
swap_wfree    | The free swap space warning threshold in % (enable `swap_integer` for number values). Defaults to `50%`.
swap_cfree    | The free swap space critical threshold in % (enable `swap_integer` for number values). Defaults to `25%`.
swap_integer  | Specifies whether the thresholds are passed as number or percent value. Defaults to false (percent values).
swap_allswaps | Conduct comparisons for all swap partitions, one by one. Defaults to false.
swap_noswap   | Resulting state when there is no swap regardless of thresholds. Possible values are "ok", "warning", "critical", "unknown". Defaults to "critical".

## tcp <a id="tcp"></a>

The [check_tcp](https://www.monitoring-plugins.org/doc/man/check_tcp.html) plugin
tests TCP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name            | Description
----------------|------------
tcp_address     | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
tcp_port        | **Required.** The port that should be checked.
tcp_expect      | String to expect in server response. Multiple strings must be defined as array.
tcp_all         | All expect strings need to occur in server response. Defaults to false.
tcp_escape_send | Enable usage of \\n, \\r, \\t or \\\\ in send string.
tcp_send        | String to send to the server.
tcp_escape_quit | Enable usage of \\n, \\r, \\t or \\\\ in quit string.
tcp_quit        | String to send server to initiate a clean close of the connection.
tcp_refuse      | Accept TCP refusals with states ok, warn, crit. Defaults to crit.
tcp_mismatch    | Accept expected string mismatches with states ok, warn, crit. Defaults to warn.
tcp_jail        | Hide output from TCP socket.
tcp_maxbytes    | Close connection once more than this number of bytes are received.
tcp_delay       | Seconds to wait between sending string and polling for response.
tcp_certificate | Minimum number of days a certificate has to be valid. 1st value is number of days for warning, 2nd is critical (if not specified: 0) -- separated by comma.
tcp_ssl         | Use SSL for the connection. Defaults to false.
tcp_wtime       | Response time to result in warning status (seconds).
tcp_ctime       | Response time to result in critical status (seconds).
tcp_timeout     | Seconds before connection times out. Defaults to 10.
tcp_ipv4        | Use IPv4 connection. Defaults to false.
tcp_ipv6        | Use IPv6 connection. Defaults to false.

## udp <a id="udp"></a>

The [check_udp](https://www.monitoring-plugins.org/doc/man/check_udp.html) plugin
tests UDP connections with the specified host (or unix socket).

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name        | Description
------------|------------
udp_address | The host's address. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
udp_port    | **Required.** The port that should be checked.
udp_send    | **Required.** The payload to send in the UDP datagram.
udp_expect  | **Required.** The payload to expect in the response datagram.
udp_quit    | The payload to send to 'close' the session.
udp_ipv4    | Use IPv4 connection. Defaults to false.
udp_ipv6    | Use IPv6 connection. Defaults to false.

## ups <a id="ups"></a>

The [check_ups](https://www.monitoring-plugins.org/doc/man/check_ups.html) plugin
tests the UPS service on the specified host. [Network UPS Tools](http://www.networkupstools.org)
 must be running for this plugin to work.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name         | Description
-------------|------------
ups_address  | **Required.** The address of the host running upsd. Defaults to "$address$" if the host's `address` attribute is set, "$address6$" otherwise.
ups_name     | **Required.** The UPS name. Defaults to `ups`.
ups_port     | The port to which to connect. Defaults to 3493.
ups_variable | The variable to monitor. Must be one of LINE, TEMP, BATTPCT or LOADPCT. If this is not set, the check only relies on the value of `ups.status`.
ups_warning  | The warning threshold for the selected variable.
ups_critical | The critical threshold for the selected variable.
ups_celsius  | Display the temperature in degrees Celsius instead of Fahrenheit. Defaults to `false`.
ups_timeout  | The number of seconds before the connection times out. Defaults to 10.

## users <a id="users"></a>

The [check_users](https://www.monitoring-plugins.org/doc/man/check_users.html) plugin
checks the number of users currently logged in on the local system and generates an
error if the number exceeds the thresholds specified.

Custom attributes passed as [command parameters](03-monitoring-basics.md#command-passing-parameters):

Name           | Description
---------------|------------
users_wgreater | The user count warning threshold. Defaults to 20.
users_cgreater | The user count critical threshold. Defaults to 50.
