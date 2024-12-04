### SQL statements that are consuming high CPU in Oracle 19c:

1. Identify Sessions with High CPU Usage:

```SQL
SELECT v.sid, v.serial#, v.username, v.program, 
       v.status, v.schemaname, v.osuser, 
       v.machine, v.terminal, v.module, 
       v.action, v.logon_time, 
       ROUND(v.value / 100) "CPU Usage (Seconds)"
FROM v$session s, v$sesstat v, v$statname n
WHERE v.sid = s.sid
  AND v.statistic# = n.statistic#
  AND n.name = 'CPU used by this session'
  AND v.value > 0
ORDER BY v.value DESC;
```


2. Identify SQL Statements with High CPU Usage:

```SQL
SELECT sql_id, 
       ROUND(elapsed_time/1000000, 2) "Elapsed Time (s)", 
       ROUND(cpu_time/1000000, 2) "CPU Time (s)", 
       executions, 
       sql_text
FROM v$sql
WHERE cpu_time > 0
ORDER BY cpu_time DESC;
```
