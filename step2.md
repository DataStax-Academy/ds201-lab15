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

<div class="step-title">Observe a Write Path</div>

✅ Shutdown the second replica node for the *cassandra* partition:

Wait for the node to shutdown.

✅ Restart the first node you shut down:

✅ Start *cqlsh* and connect to a running node:

What do you think will happen when you try to retrieve the *cassandra* partition from the *videos_by_tag* table now?

<details class="katapod-details">
  <summary>Answer</summary>
  The query should return *no data* since you deleted the *data* files for the replica you started and the other replica is not running.
</details>
<br>

✅ Run the query:
```cql
SELECT * FROM killrvideo.videos_by_tag WHERE tag = 'cassandra';
```

✅ Quit *cqlsh*:
```cql
QUIT
```

✅ Restart the other replica node:

✅ Start *cqlsh*: 
```
./node1/bin/cqlsh
```

✅ Set the consistncy level to `TWO`:
<details class="katapod-details">
  <summary>Solution</summary>

```cql
CONSISTENCY TWO;
```

</details>
<br>

A consistency level of `TWO` will cause the coordinator to read both replicas, calculate the checksum of the results, and then check if the data is not in sync between the two replica nodes. If not in sync, Cassandra will then invoke a Write Path to replace the parts of the data on the nodes that has the oldest data writetime, or that lacks data completely (especially if someone deleted the data directory).

✅ Execute the following query:
```cql
SELECT * FROM killrvideo.videos_by_tag WHERE tag = 'cassandra';
```
It should return the rows in the *cassandra* partition:

✅ Quit *cqlsh*:
```cql
QUIT
```

✅ Shutdown the replica node whose data files you **did not delete**:

✅ Start *cqlsh*:

✅ Verify that the consistency level is `ONE`:
```cql
CONSISTENCY;
```

✅ Execute the following query:
```cql
SELECT * FROM killrvideo.videos_by_tag WHERE tag = 'cassandra';
```

This time we get our data because the previous invocation of the query caused a Write Path  writing data to the *videos_by_tag* table for the node with the deleted data files!

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"step1"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
  <a href='command:katapod.loadPage?[{"step":"finish"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>

</div>
