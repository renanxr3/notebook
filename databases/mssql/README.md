

# SQL Server Performance Tuning Notes

## Table of Contents

- [Extended Events](#extended-events)
- [Operations](#operations)
- [Show Plan Operators](#show-plan-operators)
- [Logical Reads](#logical-reads)
- [Consulting Topics](#consulting-topics)
- [Join Types](#join-types)
- [Tools](#tools)
- [Thread Scheduling](#thread-scheduling)
- [Latches](#latches)
- [Latch Types](#latch-types)
- [Spinlocks](#spinlocks)
- [CXPACKET](#cxpacket)
- [Investigating Contention](#investigating-contention)
- [DBCC PAGE](#dbcc-page)
- [Wait Types](#wait-types)
- [Indexes](#indexes)
- [DMVs](#dmvs)

## Extended Events

- `query_pre_execution_showplan`

## Operations

- **Physical operation**: happens in storage system
- **Logical operation**: indicated in SQL to be done

## Show Plan Operators

- Scan vs seek
- Parallelism
- Implicit conversion

---

```sql
SET STATISTICS IO, TIME ON
GO
```

---

## Logical Reads

- Page reads = 8KB per page (divide by 1024 for MB)
- `DBCC FREEPROCCACHE` cleans cache

## Consulting Topics

- Parallelism, MAXDOP, CTOP (Cost Threshold of Parallelism, e.g., 20)
- Unused index, missing index, index hint
- Implicit conversion (hierarchy of datatypes)
- Seek predicates = WHERE clause
- Nested loop joins: outer is small, inner is large and indexed


## Join Types

- Merge join: both inputs are sorted, large
- Hash join: large data with set matching operations

## Tools

- SSMS Performance Tuning Dashboard
- SQL Server Best Practices Analyzer
- Perfmon
- PAL Tool ([pal.codeplex.com](http://pal.codeplex.com))
- SQL Server Map Tool
- SQL Diag

```sql
DBCC SQLPERF('sys.dm_os_wait_stats', CLEAR)
GO
```

## Thread Scheduling

- Each CPU core has a scheduler
- `sys.dm_os_schedulers` (runnable_tasks.count = runnable queue)
- Thread quantum = 4ms

Thread states:

- SUSPENDED: waiting resources, waiter list, receive signal
- RUNNABLE: waiting in the queue
- RUNNING: in processor

- `sys.dm_os_waiting_tasks`: all threads suspended
- `sys.dm_os_wait_stats`: stats for waits (past)

## Extended Events

- `sqlos_wait_info`
- `sqlos.wait_info_external`

## Latches

- Latch: synchronization mechanism between threads to access in-memory data
    - EX: exclusive (to change)
    - SH: share (to read, nobody can change)
- Resource Governor
- `DBCC DROPCLEANBUFFERS`: drop all pages in memory

---

### Latch Types

- `PAGEIOLATCH_XX` (SH/EX): page read from disk to memory
- `PAGELATCH_XX` (SH/EX): access in-memory data file
- `LATCH_XX` (SH/EX): access to other structures
- `sys.dm_os_latch_stats`: non-page latch, latch classes

## Spinlocks

- Spinlock: lighter latch
- Spinlock backoff: sleep for spinning on spinlock
- `sys.dm_os_spinlocks_stats` (SQLPERF CLEAR)

### Extended Events

- `sqlserver.latch_suspend_begin`
- `sqlserver.latch_suspend_end`
- `sqlos.spinlock_backoff`

## CXPACKET

- Coordinator of parallel operation
- Uneven work distribution; completed threads also generate CXPACKET

---

## Investigating Contention

- Latch contention
- Spinlock contention

---

## DBCC PAGE

- `PAGEIOLATCH` + `CXPACKET` = parallel scans
- Implicit conversions
- Buffer pool memory pressure and page life expectancy (pages released from fast memory, memory contention by Windows)
- I/O subsystem
- NC indexes reduce scans
- Update statistics
- Faster I/O, higher data volume

## Wait Types

The following table summarizes common SQL Server wait types and their descriptions:

| Wait Type              | Description |
|------------------------|-------------|
| ASYNC NETWORK IO       | - Not network, but client application<br>- Network latency |
| WRITELOG               | - Writing to transaction log buffer to flush to disk (limit = 32)<br>- Log file I/O<br>- I/O latency<br>- LOGBUFFER: internal contention for log buffers<br>- Size of transactions (small transactions are bad)<br>- Frequent page splits (FILLFACTOR)<br>- NC indexes not used (FILLFACTOR) |
| PAGELATCH              | - Access in-memory data file page<br>- Analyze which pages<br>- Page splits, fragmentation, indexes<br>- Tempdb contention: add tempdb data files (1 per core, max 4 per core) |
| PAGELATCH_EX           | - IDENTITY<br>- Avoid update index (dummy to size fit)<br>- Spread clustered index + random field or partition |
| LCK_M_XX               | - Waiting lock because another thread holds incompatible lock<br>- Follow chain of block, report blocked process<br>- Lock escalations (partition level, not table level)<br>- Large updates<br>- Snapshot isolation, lock hints<br>- Mirroring, network issues |
| SOS SCHEDULER YIELD    | - Too much CPU usage<br>- Exhausted quantum, spinning in a spinlock |
| OLEDB                  | - OLE DB apps<br>- DMs<br>- DBCC CHECKDB<br>- Linked servers |
| PREEMPTIVE_OS_XX       | - Suffix is a Windows API<br>- Call out to OS to do something<br>- Thread<br>- 200+ types |
| BACKUPXX               | - BUFFER: waiting for data or buffer for data<br>- IO: reading from database files<br>- THREAD: waiting for some IO to complete on another thread (zero init of data/log file)<br>- Analyze:<br>&nbsp;&nbsp;- Where saving backups? Slow? (type, network)<br>&nbsp;&nbsp;- IO slow?<br>&nbsp;&nbsp;- PREEMPTIVE OS WRITEFILEGATHER (look into it) |
| DBMIRRORXX             | - Waits on database mirroring |
| HADRXX                 | - Availability groups |
| TRACEWRITE/SQLTRACE_XX | - SQL trace (too much tracing) |
| LATCH_XX               | - Non-page latch<br>- `sys.dm_os_latch_stats`, check class latch (latches category) |
| ACCESS_METHODS_XX      | - Scans<br>- Parallel scans<br>- Page splits (FILLFACTOR) |
| FGCB_ADD_REMOVE        | - Filegroup shrink<br>- Grow<br>- Use Extended Events (auto growth) |
| DBCC_XX                | - Running DBCC (do not avoid running CHECKDB) |
| ASYNC IO COMPLETION    | - Non-data IO to complete<br>- Like write to backup media |

[SQLSKILLS.com/help/waits](https://www.sqlskills.com/help/waits)

## Indexes

### Fragmentation

- 0–10%: do nothing
- 10–30%: reorganize
- \>30%: rebuild

### Stats

- `sys.dm_os_index_physical_stats` function calculates results:
    - LIMITED: fast
    - DETAILED: all index stats (page density)
    - SAMPLED: 1% sample of leaf pages

### Low Page Density

- Percentage of page containing data; large rows leave empty space

### Metrics

- Average fragmentation in percent
- Average space used
- Page count
- Logical fragmentation
- Page density
- Allocation unit type desc = IN_ROW_DATA

### FILL_FACTOR

- Changing FILL_FACTOR requires rebuild; INSERT or UPDATE does nothing

### REBUILD

- Multi-CPU, updates stats, FILL_FACTOR, ON or OFF, all or nothing

### REORGANIZE

- Single thread, ONLINE, can interrupt

## DMVs

The following table groups Dynamic Management Views (DMVs) by their functional category with sample SQL scripts:

| Category | DMV | Purpose | Sample Script |
|----------|-----|---------|----------------|
| **Scheduling** | `sys.dm_os_schedulers` | Retrieve scheduler information for each CPU core | `SELECT scheduler_id, cpu_id, runnable_tasks_count FROM sys.dm_os_schedulers WHERE status = 'VISIBLE ONLINE'` |
| **Scheduling** | `sys.dm_os_waiting_tasks` | Find all threads in SUSPENDED state | `SELECT * FROM sys.dm_os_waiting_tasks WHERE session_id > 50` |
| **Scheduling** | `sys.dm_os_tasks` | Monitor task execution and scheduling | `SELECT * FROM sys.dm_os_tasks WHERE state = 'SUSPENDED'` |
| **Wait Statistics** | `sys.dm_os_wait_stats` | Query historical wait statistics (use CLEAR to reset) | `SELECT TOP 10 wait_type, waiting_tasks_count, wait_time_ms FROM sys.dm_os_wait_stats ORDER BY wait_time_ms DESC` |
| **Wait Statistics** | `sys.dm_exec_requests` | View currently executing requests and their wait information | `SELECT session_id, command, cpu_time, total_elapsed_time, wait_type FROM sys.dm_exec_requests WHERE session_id > 50` |
| **Requests & Sessions** | `sys.dm_exec_sessions` | View active sessions and connection details | `SELECT session_id, login_name, status, cpu_time FROM sys.dm_exec_sessions WHERE session_id > 50` |
| **Requests & Sessions** | `sys.dm_exec_sql_text` | Retrieve SQL text for a request/batch | `SELECT t.text FROM sys.dm_exec_requests r CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) t WHERE r.session_id > 50` |
| **Locks** | `sys.dm_tran_locks` | Monitor all locks held in the database | `SELECT * FROM sys.dm_tran_locks WHERE request_session_id > 50` |
| **Latches** | `sys.dm_os_latch_stats` | Monitor non-page latch statistics and latch classes | `SELECT TOP 10 latch_class, waiting_requests_count, wait_time_ms FROM sys.dm_os_latch_stats ORDER BY wait_time_ms DESC` |
| **Spinlocks** | `sys.dm_os_spinlock_stats` | Analyze spinlock statistics | `SELECT TOP 10 spinlock_name, spins, spins_per_collision FROM sys.dm_os_spinlock_stats ORDER BY collisions DESC` |
| **Memory** | `sys.dm_os_memory_clerks` | Monitor memory usage by components | `SELECT TOP 10 type, memory_node_id, pages_allocated_count FROM sys.dm_os_memory_clerks ORDER BY pages_allocated_count DESC` |
| **Memory** | `sys.dm_os_memory_nodes` | View memory allocation per NUMA node | `SELECT memory_node_id, virtual_address_space_reserved_mb, visible_target_kb FROM sys.dm_os_memory_nodes` |
| **Performance** | `sys.dm_os_performance_counters` | Access performance counters | `SELECT object_name, counter_name, cntr_value FROM sys.dm_os_performance_counters WHERE counter_name = 'Page life expectancy'` |
| **I/O** | `sys.dm_io_virtual_file_stats` | Monitor I/O statistics per file | `SELECT db_id, file_id, SUM(num_of_reads) AS total_reads, SUM(num_of_writes) AS total_writes FROM sys.dm_io_virtual_file_stats(NULL, NULL) GROUP BY db_id, file_id` |
| **I/O** | `sys.dm_io_pending_io_requests` | View pending I/O requests | `SELECT * FROM sys.dm_io_pending_io_requests` |
| **Indexes** | `sys.dm_db_index_usage_stats` | Track index usage statistics | `SELECT TOP 10 db_id, object_id, index_id, user_seeks, user_scans FROM sys.dm_db_index_usage_stats ORDER BY user_seeks DESC` |
| **Indexes** | `sys.dm_db_missing_indexes` | Identify missing indexes | `SELECT migs.avg_user_impact * (migs.user_seeks + migs.user_scans) AS improvement FROM sys.dm_db_missing_indexes mid JOIN sys.dm_db_missing_index_groups mig ON mid.index_handle = mig.index_handle JOIN sys.dm_db_missing_index_groups_stats migs ON mig.index_group_id = migs.group_handle` |
| **Indexes** | `sys.dm_os_index_physical_stats` | Analyze index fragmentation and physical statistics | `SELECT * FROM sys.dm_db_index_physical_stats(DB_ID(), NULL, NULL, NULL, 'LIMITED') WHERE avg_fragmentation_in_percent > 10` |
| **Query Stats** | `sys.dm_exec_query_stats` | Monitor query execution statistics | `SELECT TOP 10 qt.text, qs.execution_count, qs.total_cpu_time FROM sys.dm_exec_query_stats qs CROSS APPLY sys.dm_exec_sql_text(qs.sql_handle) qt ORDER BY qs.total_cpu_time DESC` |
| **Query Stats** | `sys.dm_exec_procedure_stats` | Monitor stored procedure execution | `SELECT TOP 10 object_name, execution_count, total_cpu_time FROM sys.dm_exec_procedure_stats ORDER BY total_cpu_time DESC` |

### Notes
- Use `DBCC SQLPERF('sys.dm_os_wait_stats', CLEAR)` to reset wait statistics
- Use `sys.dm_os_index_physical_stats` with LIMITED (fast), DETAILED (comprehensive), or SAMPLED (1% sample) modes
- Filter by `session_id > 50` to exclude system sessions
- Combine DMVs for comprehensive performance analysis and cross-reference for deeper insights


