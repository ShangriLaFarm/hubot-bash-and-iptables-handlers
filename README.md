# hubot-bash-and-iptables-handlers
GitHub HUBOT handler to run arbitrary bash/shell command and an specific handler for IPTABLES

First, [RTFM](https://hubot.github.com/)

Then install [hubot-script-shellcmd](https://www.npmjs.com/package/hubot-script-shellcmd/)

Now you can use the [run](https://github.com/ShangriLaFarm/hubot-bash-and-iptables-handlers/blob/master/run) script simply by put it in hubot_dir/bash/handlers and use it like
```bash
  shellcmd run date
```

To use the ban [ban](https://github.com/ShangriLaFarm/hubot-bash-and-iptables-handlers/blob/master/ban) script you need to add an IPTABLES chain with the name "DOOMED"
```bash
  iptables -N DOOMED
```

then you can talk to HUBOT for "ban" a CIDR
```bash
  shellcmd ban 85.21.47.65/32
```

or to show and save IPTABLES rules to /etc/firewall.conf
```bash
  shellcmd ban save
```

In our case we add some stuff to the DOOMED chain you can do that if you want functionality like "log&drop"
```bash
  iptables -A DOOMED -j LOG --log-prefix "DOOMED:" --log-level 4
  iptables -A DOOMED -j DROP
```
in this way packets are logged and them dropped.

To intercept packets logged in this way we add an /etc/rsyslog.d/iptables-doomed.conf file with these content
```bash
  :msg, contains, "DOOMED:" /var/log/iptables-doomed.log
  & ~
```
and the restart rsyslog
```bash
  /etc/init.d/rsyslog restart
```

For direct support click [here](http://hubs.ly/H01ldz50)
