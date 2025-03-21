Extended Event: query_pre_execution_showplan

Physical Operation: happens in storage system
Logical Operation: you indicate in SQL to be done

Show plan operators
* scan vs seek
* parallelism
* implicit conversion

---

SET STATISTICS IO, TIME ON
GO

---



# 

Extended Event: query_pre_execution_showplan

Physical Operation: happens in storage system
Logical oepration: you indicate in SQL to be done

Show Operators
* scan (cs seek)
* parallelism
* implicit conversion

Logical reads = page reads = 8KB per page = div 1024 in MB

DBCC FREEPROCCACHE = clean cache

Consulting: Parallel, MAXDOP, CTOP = Cost Threshold of Parallelism (20), Unused Index, Missing Index, Index Hint, Implicit Conversion (hyerarchy of datatypes)

Seek Predicates = WHERE clause

Nested Loop Joins = outer is small, inner = large indexed

MErge Join = both input are sorted, large

Hash Join = large data with set matching operations

## Tools

SSMS - Performance Tuning Dashboard
SQL Server BEst PRaacites Analyzed
PErfmon
PAL Tool (pal.codeplex.com)
SQL Server Map Tool
SQL Diag

DBCC SQLPERF('sys.dm_os_wait_stats', CLEAR)
go

Thread Scheduling: 
* each CPU core has a scheduler
* sys.dm_os_schedulers (runnable_tasks.count = runnable queue)

Thread Quantum = 4ms

Threads.SUSPENDED   = waiting resouces, waiter list, receive signal
Threads.RUNNABLE    = waiting in the queue
Threads.RUNNING =   in processor

sys.dm_os_waiting_tasks = all threads suspended
sys.dm_os_wait_stats    = stats for waits (past)

Extended Events
* sqlos_wait_info
* sqlos.wait_info_external

Latch = sync mechanosm between threads, to access in memory data
    EX = exclusive = to change
    SH = share = to read (nobody can change)

Resource Governor
DBCC DROPCLEANBUFFERS = drop all pages in memory

---

Latches

PAGEIOLATCH_XX (SH EX): page to be read from disk to memory
PAGELATCH_XX (SH EX): access in memory data file
LATCH_XX (SH EX): access to all other structures

sys.dm_os_latch_stats = non page latch, latch classes

Spinlock = lighter latch
spinlock backoff = sleep for spinning on spinlock

sys.dm_os_spinlocks_stats = SQLPERF CLEAR

EEvent: 
    sqlserver.latch_suspend_begin
    sqlserver.latch_suspend_end
    sqlos.spinlock_backoff

CXPACKET: coordinator of parallel operation
Seu trabalho é desigual, quem terminou gera CXPACKET também

Investigating Latch Contention
Investigating Spinlock Contention

---

DBCC PAGE

PAGEIOLATCH + CXPACKET = parallel scans
* implicit conversions
* buffer pool memory pressure  and page life expectancy (paginas sendo liberadas da memoria rapida, contenção memória pelo Wndows)
* i/o subsystem
* NC indexes reduce scans
* update statistics
* faster IO, higher data volume

ASYNC NETWORK IO: 
* not network, but client application
* network latency

WRITELOG: writing to transaction log buffer to flush to disk (limit = 32)
* trx log file IO
* IO latency
* LOGBUFFER = internal contention for log buffers
* size of txns (smalls are bad)
* frequent page splits (FILLFACTOR)
* NC indexes not used (FILLFACTOR)

PAGELATCH
* access in memory data file page
* analyze which pages
* page splits, fragmentation, indexes too
* tempdb contention = + tempdb data files (1 per core, 4 per core max)

PAGELATCH_EX 
* IDENTITY
* avoid update index (dummy to size fit)
* spread CI + random field or partition

LCK_M_XX
* waiting lock because other thread holds incomp lock
* follow chain of block, report blocked process
* lock escalations (partition level, not table level)
* large updates
* snapshot isolation, lock hints
* mirroring, network issues

SOS SCHEDULER YEILD
* too much CPU usage
* exhausted quantum, spinning in a spinlock

OLEDB
* ole db apps, DMs, DBCC CHECKDB, linked servers

PREEMPTIVE_OS_XX: sufix is a windows API
* callout the OS to do something
* thread
* +200 types
BACKUPXX
* BUFFER: waiting for data or buffer for data
* IO: ra=eading from database files
* THREAD: waiting for some IO to completeon another thread (zero ini of data/log file)
Analyze:
* where saving backups? slow? (type, network)
* IO slow?
* PREEMPTIVE OS WRITEFILEGATHER (look into it)

DBMIRRORXX: waits on database mirroring

HADRXX: availability groups

TRACEWRITE e SQLTRACE_XX: sql trace (too much tracing)

LATCH_XX: 
* non page latch
* sys.dm_os_latch_stats , check class latch (latches category)

ACCESS_METHODS_XX: scans, parallel scans, page splits (FILLFACTOR)

FGCB_ADD_REMOVE: filegroup shrink, grow, use EEvents (auto growth)

DBCC_XX: running DBCC (do not avoid running CHECKDB)

ASYNC IO COMPLETION: non data IO to complete, like write to backup media

---

SQLSKILLS.com/help/waits

Index Fragmentation
* 0 - 10 = do nothing
* 10 - 30 = reorg
* +30 = rebuild

sys.dm_os_index_physical_stats = function, will calculate to get results
* LIMITED: fast
* DETAILED: all index stats (page density)
* SAMPLED: 1% sample of leaf pages


Low Page Density = quanto % uma pagina contem dados, row é muito grande e deixa espaõ vazio

average fragmentation in percent
avg space used
page count
logical fragmentation
page density
page count

allocation unit type desc = IN_ROW_DATA

Alterar FILL_FACTOR exige fazer rebuild, INS ou UPD nao faz nada

REBUILD: multi CPU, update stats, FILL FACTOR, ON or OFF, all or nothing

REORGANIZE: single thread, ONLINE, can interrupt

sys.dm_exec_requests

wait_on_low_priority


