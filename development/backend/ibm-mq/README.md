# MQ setup

## Basic setup for development

```
define listener(TCP.LISTENER) trptype(tcp) control(qmgr) port(1414)
start listener(TCP.LISTENER)

define channel(SYSTEM.ADMIN.SVRCONN) chltype(SVRCONN) REPLACE

set CHLAUTH(*) TYPE(BLOCKUSER) USERLIST('nobody','*MQADMIN')
set CHLAUTH(SYSTEM.ADMIN.*) TYPE(BLOCKUSER) USERLIST('nobody')

set CHLAUTH(SYSTEM.DEF.SVRCONN) TYPE(ADDRESSMAP) ADDRESS(*) USERSRC(CHANNEL)
set CHLAUTH(SYSTEM.DEF.*) TYPE(BLOCKUSER) USERLIST('nobody')


ALTER AUTHINFO(SYSTEM.DEFAULT.AUTHINFO.IDPWOS) AUTHTYPE(IDPWOS) +
CHCKCLNT(OPTIONAL)
REFRESH SECURITY TYPE(CONNAUTH)
REFRESH SECURITY
```


https://www.ibm.com/support/pages/commands-create-queue-manager-and-configure-it-remote-administration-mq-explorer


```

setmqaut -m qm_prm9ww -n '**' -t queue -g mqm +alladm +browse
setmqaut -m qm_prmss -n '**' -t queue -g mqm +alladm +browse

```