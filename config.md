# Parameters #

| **Parameter Name** | **Required?** | **Parameter Scope** | **Default Value** | **Example** |
|:-------------------|:--------------|:--------------------|:------------------|:------------|
|[hostname](#hostname)|Yes            |Local Only           |-                  |hostname=mysql_server1, hostname=192.168.0.1, etc|
|[ip](#ip)        |No             |Local Only           |gethostbyname($hostname)|ip=192.168.1.3|
|[port](#port)    |No             |Local/App/Global     |3306               |port=3306    |
|[ssh_host](#ssh_host)|No             |Local Only           |same as hostname   |ssh_host=mysql_server1, ssh_host=192.168.0.1, etc|
|[ssh_ip](#ssh_ip)|No             |Local Only           |gethostbyname($ssh_host)|ssh_ip=192.168.1.3|
|[ssh_port](#ssh_port)|No             |Local/App/Global     |22                 |ssh_port=22 |
|[ssh_connection_timeout](#ssh_connection_timeout)|No             |Local/App/Global     |5                  |ssh_connection_timeout=20|
|[ssh_options](#ssh_options)|No             |Local/App/Global     |""(empty string)   |ssh_options="-i /root/.ssh/id_dsa2"|
|[candidate_master](#candidate_master)|No             |Local Only           |0                  |candidate_master=1|
|[no_master](#no_master)|No             |Local Only           |0                  |no_master=1 |
|[ignore_fail](#ignore_fail)|No             |Local Only           |0                  |ignore_fail=1|
|[skip_init_ssh_check](#skip_init_ssh_check)|No             |Local Only           |0                  |skip_init_ssh_check=1|
|[skip_reset_slave](#skip_reset_slave)|No             |Local/App/Global     |0                  |skip_reset_slave=1|
|[user](#user)    |No             |Local/App/Global     |root               |user=mysql_root|
|[password](#password)|No             |Local/App/Global     |""(empty string)   |password=rootpass|
|[repl_user](#repl_user)|No             |Local/App/Global     |Master_User value from SHOW SLAVE STATUS|repl_user=repl|
|[repl_password](#repl_password)|No             |Local/App/Global     |- (current replication password)|repl_user=replpass|
|[disable_log_bin](#disable_log_bin)|No             |Local/App/Global     |0                  |disable_log_bin=1|
|[master_pid_file](#master_pid_file)|No             |Local/App/Global     |""(empty string)   |master_pid_file=/var/lib/mysql/master1.pid|
|[ssh_user](#ssh_user)|No             |Local/App/Global     |current OS user    |ssh_user=root|
|[remote_workdir](#remote_workdir)|No             |Local/App/Global     |/var/tmp           |remote_workdir=/var/log/masterha/app1|
|[master_binlog_dir](#master_binlog_dir)|No             |Local/App/Global     |/var/lib/mysql     |master_binlog_dir=/data/mysql1,/data/mysql2|
|[log_level](#log_level)|No             |App/Global           |info               |log_level=debug|
|[manager_workdir](#manager_workdir)|No             |App                  |/var/tmp           |manager_workdir=/var/log/masterha|
|[client_bindir](#client_bindir)|No             |App                  |-                  |client_bindir=/usr/mysql/bin|
|[client_libdir](#client_libdir)|No             |App                  |-                  |client_libdir=/usr/lib/mysql|
|[manager_log](#manager_log)|No             |App                  |STDERR             |manager_log=/var/log/masterha/app1.log|
|[check_repl_delay](#check_repl_delay)|No             |App/Global           |1                  |check_repl_delay=0|
|[check_repl_filter](#check_repl_filter)|No             |App/Global           |1                  |check_repl_filter=0|
|[latest_priority](#latest_priority)|No             |App/Global           |1                  |latest_priority=0|
|[multi_tier_slave](#multi_tier_slave)|No             |App/Global           |0                  |multi_tier_slave=1|
|[ping_interval](#ping_interval)|No             |App/Global           |3                  |ping_interval=5|
|[ping_type](#ping_type)|No             |App/Global           |SELECT             |ping_type=CONNECT|
|[secondary_check_script](#secondary_check_script)|No             |App/Global           |null               |secondary_check_script= masterha_secondary_check -s remote_dc1 -s remote_dc2|
|[master_ip_failover_script](#master_ip_failover_script)|No             |App/Global           |null               |master_ip_failover_script=/usr/local/custom_script/master_ip_failover|
|[master_ip_online_change_script](#master_ip_online_change_script)|No             |App/Global           |null               |master_ip_online_change_script= /usr/local/custom_script/master_ip_online_change|
|[shutdown_script](#shutdown_script)|No             |App/Global           |null               |shutdown_script= /usr/local/custom_script/master_shutdown|
|[report_script](#report_script)|No             |App/Global           |null               |report_script= /usr/local/custom_script/report|
|[init_conf_load_script](#init_conf_load_script)|No             |App/Global           |null               |init_conf_load_script = /usr/local/custom_script/init_conf_loader|


  * Local Scope: Per-server scope parameters. Local scope parameters should be set under `[server_xxx]` blocks within [application configuration](Configuration#writing-an-application-configuration-file) file.
  * App Scope: Parameters for each {master, slaves} pair. These parameters should be set under a  `[server_default]` block within [application configuration](Configuration#writing-an-application-configuration-file) file.
  * Global Scope: Parameters for all {master, slaves} pairs. Global scope parameters are useful only when you manage multiple {master, slaves} pairs from single manager server. These parameters should be set in a [global configuration](Configuration#writing-a-global-configuration-file) file.


### hostname

Hostname or IP address of the target MySQL server. This parameter is mandatory, and must be configured under `[server_xxx]` blocks within [application configuration](Configuration#writing-an-application-configuration-file) file.

### ip

IP address of the target MySQL server. Default is `gethostbyname($hostname)`. MHA Manager and MHA Node internally uses this IP address to connect via MySQL and SSH. Normally you don't need to configure this parameter because it's automatically resolved from hostname parameter.

### port

Port number of the target MySQL server. Default is 3306. MHA connects to MySQL servers by using IP address and port.

### ssh_host
> (Supported from 0.53)

Hostname or IP address of the target MySQL server that is used via SSH. This parameter (and ssh_port parameter) is useful when you are in a multi VLAN configuration: For security reason, the SSH is not permit on the data's VLAN.
Default is same as hostname.

### ssh_ip
> (Supported from 0.53)

IP address of the target MySQL server that is used via SSH. Default is `gethostbyname($ssh_host)`.

### ssh_port
>(Supported from 0.53)

Port number of the target MySQL server used via SSH. Default is 22.

### ssh_connection_timeout
> (Supported from 0.54)

Default is 5 seconds. Before adding this parameter timeout was hard coded.

### ssh_options
> (Supported from 0.53)

Additional SSH command line options.

### candidate_master

You might use different kinds of machines between slaves, and want to promote the most reliable machine to the new master (i.e. promoting a RAID1+0 slave rather than RAID0 slaves).

By setting candidate_master to 1, the server is prioritized to the new master, as long as it meets conditions to be the new master (i.e. binary log is enabled, it does not delay replication significantly, etc). So candidate\_master=1 does not mean that the specified host always becomes new master when the current master crashes, but is helpful to set priority.

If you set candidate_master=1 on multiple servers, priority is decided by sort order by block name (`[server_xxx]`). `[server_1]` will have higher priority than `[server_2]`.

### no_master

By setting no_master=1 on the target server, the server never becomes the new master. This is useful if you have some servers that should not become the new master. For example, you may want to set no_master=1 when you run slaves on unreliable (RAID0) machine, or when you run a slave at a remote data center.
Note that if none of the slaves can be new master, MHA aborts and does not start monitoring/failover.

### ignore_fail

By default, MHA Manager does not start failover if any of the slave server fails (can not connect via MySQL/SSH, SQL thread stops with errors, etc). But in some cases you might want to continue failover if only specific slave servers fail. By setting ignore_fail=1 at specific servers, MHA consinues failover even though these servers fail. By default, it's 0.

### skip_init_ssh_check

Skipping SSH connectivity at initial startup.

### skip_reset_slave
> (Supported from 0.56)

Skipping executing RESET SLAVE (ALL) after master failover.

### user

MySQL administrative database username to the target MySQL server. This should be root because it runs all necessary administrative commands such as STOP SLAVE, CHANGE MASTER, RESET SLAVE. By default, it's root.

### password

MySQL password of the "user" user. By default, it's empty.

### repl_user

MySQL replication username used at CHANGE MASTER TO master_user .. on each slave. This user should have REPLICATION SLAVE privilege on the target master. By default, Master_User from SHOW SLAVE STATUS on the new master (currently running as a slave) will be used.

### repl_password

MySQL password of the "repl_user" user. By default, it's the current replication password. This means current master's password. If you run [online master switch](masterha_master_switch#scheduled-Online_Master_Switch) with setting --orig_master_is_new_slave (which means current master runs as a new slave of the new master), starting a slave will fail without setting repl_password because on the current master default replication password is empty (MHA will execute change master without setting replication password on the orig master, though setting current replication password on other slaves).

### disable_log_bin

When this option is set, when applying differential relay logs to slaves, slaves do not generate binary logs. Internally MHA passes --disable-log-bin to mysqlbinlog command.

### master_pid_file

Setting master's pid file. This might be useful when you are running multiple MySQL instances within single server. See [shutdown_script](#shutdown_script) parameter for details.

### ssh_user

OS user name that MHA Manager and MHA Node use to access to MySQL servers. This is needed for various kinds of purposes such as executing commands remotely (Manager->MySQL), copying differential relay logs from the latest slave to other slaves (MySQL->MySQL), etc.

This user must have at least read privileges on MySQL binary/relay log files and relay_log.info file, and write privileges on logging directory (remote_workdir on each MySQL server).

This user must be able to connect to servers without any interactive operations. It is generally recommended using SSH public key authentication. By default, ssh_user is current OS user on the manager.


### remote_workdir

Working directory full path name that each MHA Node (running on a MySQL server) generates log files. If not exists, MHA Node automatically creates it. If sufficient permissions are not granted, MHA Node aborts. Note that neither MHA Manager nor MHA Node checks available disk space so you need to care about it. By default, remote_workdir is "/var/tmp".

### master_binlog_dir

Directory full path name where MySQL generates binary logs on the master. This parameter is used if dead MySQL master is reachable via SSH, in order to read and copy necessary binary log events. This parameter is needed because there is no way to identify binary log directory automatically if master MySQL is dead.

By default, master_binlog_dir is "/var/lib/mysql,/var/log/mysql". /var/lib/mysql is the default binlog output directory of most of MySQL distributions, and /var/log/mysql is the default binlog output directory of Ubuntu MySQL package. You can set multiple directories separated by commas (i.e /data1,/data2,…).

### log_level

Logging threshold that MHA Manager prints. Default is info and should be fine in most cases. Either debug/info/warning/error can be set.

### manager_workdir

Working directory full path that MHA Manager generates related status files. If not set, /var/tmp is used.

### client_bindir

If MySQL command line utilities are installed under a non-standard directory, use this option to set the directory.

### client_libdir

If MySQL libraries are installed under a non-standard directory, use this option to set the directory.

### manager_log

Full path file name that MHA Manager generates logs. If not set, MHA Manager prints to STDOUT/STDERR. When executing manual failover (interactive failover), MHA Manager ignores manager_log setting and always prints to STDOUT/STDERR.

### check_repl_delay

By default, if a slave behinds master more than 100MB of relay logs (= needs to apply more than 100MB of relay logs), MHA does not choice the slave as a new master because it takes too long time to recover. By setting check_repl_delay=0, MHA ignores replication delay when selecting a new master. This option is useful when you set candidate_master=1 on a specific host and you want to make sure that the host can be new master.

### check_repl_filter

By default, if any of the master and slaves has different binary log / replication filtering rule each other, MHA prints errors and does not start monitoring or failover. This is to avoid unexpected recovery errors such as "Table not exists". If you are 100% sure that different filtering settings don't cause recovery problems, set check_repl_filter = 0. Note that MHA does not check filtering rules when applying differential relay logs, so you might encounter "Table not exists" (or others) errors if you set check_repl_filter = 0. Be very careful if you set this parameter.

### latest_priority

By default, the latest slave (a slave receives the latest binlog events) is prioritized as a new master. If you want to fully control the order of priority (i.e. host2->host3->host4..), setting latest_priority=0 will help. See [FAQ](FAQ#which-host-is-selected-as-a-new-master) for details.

### multi_tier_slave
> (Supported from 0.52)

Starting from MHA Manager version 0.52, multi-master replication configurations are supported.
By default, it is not allowed to set three or more tier replication hosts in a MHA configuration file. For example, suppose that host2 replicates from host1 and host3 replicates from host2. By default, it is not allowed to write host1,2,3 in a configuration file because it's three tier replication, and MHA Manager stops with errors.
By setting multi_tier_slave, MHA Manager does not abort with three tier replication, but simply ignores third tier hosts. If host1 (master) crashes, host2 will be selected as a master, and host3 will continue replication from host2.

### ping_interval

This parameter states how often MHA Manager pings(executes ping SQL statement) the master. After missing three connection intervals in a row, MHA Manager decides that the MySQL master is dead. Thus, the maximum time for discovering a failure through the ping mechanism is four times the ping interval. The default is 3 (3 seconds).

If MHA Manager fails to connect by too many connections or authentication errors, it doesn't count that the master is dead.

### ping_type
> (Supported from 0.53)

By default, MHA establishes a persistent connection to a master and checks master's availability by executing "SELECT 1" (ping_type=SELECT).
But in some cases, it is better to check by connecting/disconnecting every time, because it's more strict and it can detect TCP connection level failure more quickly. Setting ping_type=CONNECT makes it possible.
Starting from 0.56, ping_type=INSERT was added.

### secondary_check_script

In general, it is highly recommended to have two or more network routes to check MySQL master server availability. By default, MHA Manager checks only single route: From Manager to Master. But this is not recommended. MHA can actually have two or more checking routes by calling an external script defined at secondary_check_script parameter. Here is an example configuration.

    secondary_check_script = masterha_secondary_check -s remote_host1 -s remote_host2

masterha_secondary_check is included in the MHA Manager package. Built-in masterha_secondary_check script should be fine in most cases, but you can call any script here.

In the above example, MHA Manager checks MySQL master server activity via Manager-(A)->remote_host1-(B)->master_host and Manager-(A)->remote_host2-(B)->master_host. If connection A was successful and B was unsuccessful in both routes, masterha_secondary_check exits with return code 0 and MHA Manager decides that MySQL master is really dead, and will start failover. If A was unsuccessful, masterha_secondary_check exits with return code 2 and MHA Manager guesses that network problem has happened and it does not start failover. If B was successful, masterha_secondary_check exits with return code 3 and MHA Manager understands that MySQL master server is actually alive, and does not start failover.

Generally speaking, remote_host1 and remote_host2 should be located on different network segments from MHA Manager and MySQL servers.

MHA invokes a script defined at secondary_check_script parameter and passes the following arguments automatically (so you don't need to set below arguments in the config file). masterha_secondary_check should be appropriate in many cases, but you can write any network checking script if you need more functionality.

  * --user=(SSH username of the remote hosts. [ssh_user](#ssh_user) parameter value will be passed)
  * --master_host=(master's hostname)
  * --master_ip=(master's ip address)
  * --master_port=(master's port number)

Note that built-in masterha_secondary_check script depends on IO::Socket::INET Perl package, which was included by default from Perl v5.6.0. masterha_secondary_check script also connects to all remote servers via SSH so SSH public key authentication settings are required.
In addition, masterha_secondary_check script tries to establish TCP connection from remote server (set by -s) to MySQL master. This means max_connections settings in my.cnf does not affect. If the TCP connection succeeds, Aborted_connects status variable on the master will be incremented by 1.

### master_ip_failover_script

In common HA environments, many cases people allocate one virtual IP address on a master. If the master crashes, HA software like Pacemaker takes over the virtual IP address to the standby server.

Another common approach is creating a global catalog database that has all mappings between application names and writer/reader IP addresses (i.e. {app1_master1, 192.168.0.1}, {app_master2, 192.168.0.2}, …), instead of using virtual IP addresses. In this case, you need to update the catalog database when the current master dies.

Both approaches have advantages and disadvantages. MHA does not force one approach, but allow users to use any IP address failover solution. master_ip_failover_script parameter can be used for that purpose. In other words, you need to write a script to make applications transparently connect to the new master, and have to define at master_ip_failover_script parameter. Here is an example.

    master_ip_failover_script= /usr/local/sample/bin/master_ip_failover

A sample script is located under (MHA Manager package)/samples/scripts/master_ip_failover. Sample scripts are included in MHA Manager tarball and GitHub branch.

MHA Manager calls master_ip_failover_script three times. First time is before entering master monitor (for script validity checking), second time is just before calling shutdown_script, and third time is after applying all relay logs to the new master. MHA Manager passes below arguments (you don't need to set these arguments in the config file).

  * Checking phase
    * --command=status
    * --ssh_user=(current master's ssh username)
    * --orig_master_host=(current master's hostname)
    * --orig_master_ip=(current master's ip address)
    * --orig_master_port=(current master's port number)

  * Current master shutdown phase
    * --command=stop or stopssh
    * --ssh_user=(dead master's ssh username, if reachable via ssh)
    * --orig_master_host=(current(dead) master's hostname)
    * --orig_master_ip=(current(dead) master's ip address)
    * --orig_master_port=(current(dead) master's port number)

  * New master activation phase
    * --command=start
    * --ssh_user=(new master's ssh username)
    * --orig_master_host=(dead master's hostname)
    * --orig_master_ip=(dead master's ip address)
    * --orig_master_port=(dead master's port number)
    * --new_master_host=(new master's hostname)
    * --new_master_ip=(new master's ip address)
    * --new_master_port(new master's port number)
    * --new_master_user=(new master's user)
    * --new_master_password(new master's password)

If you use a shared virtual IP address on a master, you may not need to do anything in the master shutdown phase, as long as you power off the machine later in the [shutdown_script](#shutdown_script.md). In the new master activation phase, you can assign the virtual IP on the new master.
If you use a catalog database approach, you may need to delete or update a record of the dead master in the master shutdown phase. In the new master activation phase, you can insert/update a record of the new master.
Additionally, you can do everything you need so that applications can write to the new master. For example, SET GLOBAL read_only=0, creating database user with write privileges, etc.

MHA Manager checks exit code (return code) of the script. If the script exits with return code 0 or 10, MHA Manager continues operations.
If the script exits with return code other than 0 or 10, MHA Manager aborts and it won't continue failover.
Default parameter is empty, so MHA Manager does not invoke anything by default.

### master_ip_online_change_script

This is similar to master_ip_failover_script parameter, but this is not used by master failover command, but by master online change command (masterha_master_switch --master_state=alive).
Passed arguments are the following.

  * Current master write freezing phase
    * --command=stop or stopssh
    * --orig_master_host=(current master's hostname)
    * --orig_master_ip=(current master's ip address)
    * --orig_master_port=(current master's port number)
    * --orig_master_user=(current master's user)
    * --orig_master_password=(current master's password)
    * --orig_master_ssh_user=(from 0.56, current master's ssh user)
    * --orig_master_is_new_slave=(from 0.56, notifying whether the orig master will be new slave or not)

  * New master granting write phase
    * --command=start
    * --orig_master_host=(orig master's hostname)
    * --orig_master_ip=(orig master's ip address)
    * --orig_master_port=(orig master's port number)
    * --new_master_host=(new master's hostname)
    * --new_master_ip=(new master's ip address)
    * --new_master_port(new master's port number)
    * --new_master_user=(new master's user)
    * --new_master_password=(new master's password)
    * --new_master_ssh_user=(from 0.56, new master's ssh user)

MHA executes FLUSH TABLES WITH READ LOCK on the current master just after the Current master write freezing phase. You can write any logic to do [graceful master switch](http://www.slideshare.net/matsunobu/automated-master-failover/44).
On the new master granting write phase, you can do almost the same thing as master_ip_failover_script. For example, creating a privileged user, executing SET GLOBAL read_only=0, updating a catalog database, etc.
If the script exits with return code other than 0 or 10, MHA Manager aborts and it won't continue master switch.

Default parameter is empty, so MHA Manager does not invoke anything by default.

A sample script is located under (MHA Manager package)/samples/scripts/master_ip_online_change. Sample scripts are included in MHA Manager tarball and GitHub branch.

### shutdown_script

You may want to force shutdown the master server so that it never restarts services (node fencing). This is important to avoid split brain. Here is an example.

    shutdown_script= /usr/local/sample/bin/power_manager

A sample script is located at (MHA Manager package)/samples/scripts/power_manager. Sample scripts are included in MHA Manager tarball and GitHub branch.

Before calling shutdown_script, MHA Manager internally checks whether the MySQL master is reachable via SSH or not. If SSH is reachable (OS is alive but mysqld is not running, i.e. data file is corrupt), MHA Manager passes the following arguments.

  * --command=stopssh
  * --ssh\_user=(ssh username so that you can connect to the master)
  * --host=(master's hostname)
  * --ip=(master's ip address)
  * --port=(master's port number)
  * --pid\_file=(master's pid file)

If the master is not reachable via SSH, MHA Manager passes the following arguments.

  * --command=stop
  * --host=(master's hostname)
  * --ip=(master's ip address)

The sample script works as below.
If --command=stopssh was passed, the script kills all mysqld and mysqld_safe processes with -9 on the master via ssh. If --pid_file was also passed, the script tries to kill only the specified process instead of killing all mysqld processes, and if it fails it kills all mysqld and mysqld_safe processes. This is helpful when you run multiple mysqld instances on the master.
If stopping via SSH succeeds, the script exits with return code 10. If exit code is 10, MHA manager later connects to the master via SSH and save necessary binary logs.
If the script fails connecting to master via SSH or --command=stop was passed from MHA Manager, the script tries to power off the machine. Power off command depends on H/W. For HP(iLO), ipmitool or SSL is common. For Dell(DRAC), dracadm is common.
If power off was successful, the script exits with return code 0. Otherwise exits with return code 1.
MHA Manager starts failover processes if exit code is 0. If exit code is other than 0 or 10, MHA Manager aborts failover.
Default parameter is empty, so MHA Manager does not invoke anything by default.

In addition, MHA Manager calls shutdown_script when starting monitoring. The below arguments are passed at that time. You can check script settings here. Controlling power highly depends on H/W, so it is highly recommended checking power status here. If you are something wrong, you can be aware before starting monitoring.

  * --command=status
  * --host=(master's hostname)
  * --ip=(master's ip address)


### report_script

You might want to send a report (i.e. e-mail) when failover has completed or ended with errors. report_script can be used for the purpose. MHA Manager passes the following arguments.

  * --orig_master_host=(dead master's hostname)
  * --new_master_host=(new master's hostname)
  * --new_slave_hosts=(new slaves' hostnames, delimited by commas)
  * --subject=(mail subject)
  * --body=(body)

Default parameter is empty, so MHA Manager does not invoke anything by default.

A sample script is located at (MHA Manager package)/samples/scripts/send\_report. Sample scripts are included in MHA Manager tarball and GitHub branch.

### init_conf_load_script

This script can be used when you do not want to set plain texts in the configuration file (i.e. password and repl_password). By returning "name=value" pairs from this script, you can override the global configuration file parameters. Example script is as below.

    #!/usr/bin/perl
    
    print "password=$ROOT_PASS\n";
    print "repl_password=$REPL_PASS\n";

Default parameter is empty, so MHA Manager does not invoke anything by default.
