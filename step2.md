<!-- TOP -->
<div class="top">
  <img class="scenario-academy-logo" src="https://datastax-academy.github.io/katapod-shared-assets/images/ds-academy-2023.svg" />
  <div class="scenario-title-section">
    <span class="scenario-title">Write Path</span>
    <span class="scenario-subtitle">ℹ️ For technical support, please contact us via <a href="mailto:academy@datastax.com">email</a>.</span>
  </div>
</div>

<!-- NAVIGATION -->
<div id="navigation-top" class="navigation-top">
 <a href='command:katapod.loadPage?[{"step":"step1"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 2 of 2</span>
 <a href='command:katapod.loadPage?[{"step":"finish"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>

</div>

<!-- CONTENT -->

<div class="step-title">Observe the <i>memtables</i> and <i>commit log</i> before and after a <i>flush</i></div>

✅ Execute the following *nodetool* command:
```
nodetool tablestats keyspace1.standard1
```
*cassandra-stress* created *keyspace1* and the *standard1* table and populated its data. Running *nodetool tablestats* gives you the statistics for that table. 

Output
```
### {"execute":false}
Total number of tables: 59
----------------
Keyspace : keyspace1
        Read Count: 0
        Read Latency: NaN ms
        Write Count: 250000
        Write Latency: 0.013440336 ms
        Pending Flushes: 0
                Table: standard1
                SSTable count: 2
                Space used (live): 59575885
                Space used (total): 59575885
                Space used by snapshots (total): 0
                Off heap memory used (total): 699116
                SSTable Compression Ratio: -1.0
                Number of partitions (estimate): 252290
                Memtable cell count: 1180
                Memtable data size: 319780
                Memtable off heap memory used: 388020
                Memtable switch count: 11
                ...
```

Check that the *Write Count* matches the number of rows we told *cassandra-stress* to insert. *nodetool tablestats* also reports the number of SSTables, space used, and bloom filter statistics.

✅ Make note of the Memtable statistics in the nodetool tablestats output. Specifically, the memtable for the *keyspace1.standard1* table is not empty.

✅ Execute the following *nodetool* command to flush the memtable content of all tables in all keyspaces to disk:
```
nodetool flush
```

✅ Check the statistics for the *keyspace1.standard1* table again by executing:
```
nodetool tablestats keyspace1.standard1
```

Confirm that the Memtable statistics have zeroed out because we flushed the previous memtable to disk.

Output
```
### {"execute":false}
      ...
      Memtable cell count: 0
      Memtable data size: 0
      Memtable off heap memory used: 0
      Memtable switch count: 12
      ...
```

✅ Run the following commands to shut down the node and clear out the system logs:
```
nodetool stopdaemon
rm -f ./node/logs/*.log
```

✅ Start up the node again using the following command:
```
cassandra
```

Wait for the node to finish starting up before continuing.

✅ Open the *system.log* file, located at ~/node/logs/system.log, using a command-line editor. 
```
nano ~/node/logs/system.log
```

✅ In the system.log, look for messages that includes the word *CommitLog.java*. 

```
### {"execute":false}
... CommitLog.java:194 - Replaying /home/ubuntu/node/data/commit-log/CommitLog-680-1586320327793.log, /home/ubuntu/node/data/commit-log/CommitLog-680-1586320327794.log
... CommitLog.java:196 - Log replay complete, 37 replayed mutations
```

If there are existing commit log files while Cassandra is starting up, it will replay the mutations in those logs into memtables and then automatically flush the memtables to disk.

If there happen to be no commit log segments found during start up, then no replay needs to be done. 



<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step1"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
  <a href='command:katapod.loadPage?[{"step":"finish"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>

</div>
