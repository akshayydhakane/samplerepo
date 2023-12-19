<h2>On server to be monitered</h2>

- <h3>install nrpe service and plugins</h3>
```
sudo apt-get update
sudo apt-get install nagios-nrpe-server nagios-plugins sysstat dc build-essential
```
- <h3>add additional plugins</h3>
- <h3>if there no backup of plugins
- <h3>download and extract</h3>
- <h3>create dummy file named installed.firewall to directly run A-subcomponents script</h3>
```
wget https://assets.nagios.com/downloads/nagiosxi/agents/linux-nrpe-agent.tar.gz && tar -xvf linux-nrpe-agent.tar.gz
```

```
cd .../linux-nrpe-agent
touch installed.firewall
```

```
./A-subcomponents # it will install all additional script does not matter if there are some errors
```
- <h3>edit /etc/nagios/nrpe.cfg
- <h3>Add monitoring server
```
allowed_hosts=127.0.0.1, 52.0.173.209
```
- <h3>Allowing Agguement

<h3>Note: 0 disallow to 1 allow

```
dont_blame_nrpe=1
```

command[check_total_procs]=/usr/lib/nagios/plugins/check_procs -w 150 -c 200   # add below lines according to requirement of service to monitered

these lines are nothing but har coded command definitions which will be executed by nagios server

```
command[check_procs]=/usr/lib/nagios/plugins/check_procs -w 250 -c 350
command[check_init_service]=/usr/local/nagios/libexec/check_init_service ssh
#command[check_disk]=/usr/local/nagios/libexec/check_disk -w 20% -c 10% -p /
#command[check_mongo_disk]=/usr/lib/nagios/plugins/check_mongo_disk -w 20% -c 10% -p /var/lib/mongo
command[check_apt]=/usr/lib/nagios/plugins/check_apt -U
command[check_cpu_stats]=/usr/local/nagios/libexec/check_cpu_stats.sh -w 85 -c 95
command[check_mem]=/usr/local/nagios/libexec/custom_check_mem -w 20 -c 10
command[check_open_files]=/usr/local/nagios/libexec/check_open_files.pl -w 30 -c 50
command[check_disk]=/usr/local/nagios/libexec/check_disk $ARG1$
```
Command configuration for new server ideal for dyanmic and threshold is provided by nagios-server
```
command[check_load]=/usr/lib/nagios/plugins/check_load $ARG1$
command[check_procs]=/usr/lib/nagios/plugins/check_procs $ARG1$
command[check_init_service]=/usr/local/nagios/libexec/check_init_service $ARG1$
command[check_mongo_disk]=/usr/lib/nagios/plugins/check_mongo_disk $ARG1$
command[check_apt]=/usr/lib/nagios/plugins/check_apt $ARG1$
command[check_cpu_stats]=/usr/local/nagios/libexec/check_cpu_stats.sh $ARG1$
command[check_mem]=/usr/local/nagios/libexec/custom_check_mem $ARG1$
command[check_open_files]=/usr/local/nagios/libexec/check_open_files.pl $ARG1$
command[check_disk]=/usr/local/nagios/libexec/check_disk $ARG1$
command[check_users]=/usr/lib/nagios/plugins/check_users $ARG1$
```
save file
Note:
```
if backup is available then simple copy the scripts to some folder which has ownership of nagios
also change path of the harcoded scripts
```
- <h3>Additional Info</h3>

    install additional packages for scripts

    for "check_cpu_stats.sh" if encounters "UNKNOWN: iostat not found or is not executable by the nagios user."

    apt-get install sysstat

    for "custom_check_mem" in service goes critical state even numbers are normal also it shows % in results

    apt-get install dc

- <h3>Restart the nrpe service</h3>

    service nagios-nrpe-server status

- <h3>on server containing nagios server</h3>
    goto server and add .cfg file for the server
    
    restart services
- <h3>Allow All ICMP - IPv4 and All ICMP - IPv6 on client instance</h3>

- <h3>Test command for to check manually if any error
```
/usr/local/nagios/libexec/check_nrpe -H localhost -c check_disk
```    






