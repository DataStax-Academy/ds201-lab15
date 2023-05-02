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
 <a href='command:katapod.loadPage?[{"step":"intro"}]'
   class="btn btn-dark navigation-top-left">⬅️ Back
 </a>
<span class="step-count"> Step 1 of 2</span>
 <a href='command:katapod.loadPage?[{"step":"step2"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>

<!-- CONTENT -->

<div class="step-title">Shut down a node an delete it's data files</div>

✅ Check that the node is up and running:

```
nodetool status
```

✅ Use `nodetool getendpoints` to determine which nodes hold the replicas for the cassandra partition tag value in the *videos_by_tag* table:
```
./node1/bin/nodetool getendpoints killrvideo videos_by_tag 'cassandra'
```

✅ Shut down one of the replica nodes for the *cassandra* partition by running `nodetool stopdaemon`:

<details class="katapod-details">
  <summary>Solution</summary>

Make sure that you only shut down only one replica node!
<table class="katapod-table">
  <tr>
    <th>Node (IP)</th>
    <th>Shutdown command</th>
  </tr>
  <tr>
    <td>127.0.0.1</td>
    <td>

```
./node1/bin/nodetool stopdaemon
``` 
</td>
<tr>

  <tr>
    <td>127.0.0.2</td>
    <td>

```
./node2/bin/nodetool stopdaemon
``` 
</td>
<tr>
    
<tr>

  <tr>
    <td>127.0.0.3</td>
    <td>

```
./node3/bin/nodetool stopdaemon
``` 
</td>
<tr>

</table>

Keep track of which node you shut down.
</details>
<br>

✅ Navigate to the *data* directory of the node you shut down:

(In the command below *node**X*** is the name of the node you shut down.)
```
### {"execute": false}
cd ./nodeX/data/data/killrvideo

```

✅ Use `ls` to find the name of the directory for the videos_by_tag table, and then delete the directory:
```
ls -l
```

Output
```
### {"execute": false}
videos_by_tag-d90a364078aa11eabb5ad5e1d0ea70cd
```

✅ Delete the *videos_by_tag...* directory:
```
### {"execute": false}
rm -rf videos_by_tag-d90a364078aa11eabb5ad5e1d0ea70cd
```

✅ Start *cqlsh* on one of the running nodes:

✅ Verify that the consistency level is `ONE`:
```cql
CONSISTENCY;
```
What do you think will happen when you try to retrieve the *cassandra* partition from the *videos_by_tag* table?

<details class="katapod-details">
  <summary>Answer</summary>
  The query should succeed since the consistency level is <code>ONE</code> and there is one active node with the data in the cluster.
</details>
<br>

✅ Run the query:
```cql
SELECT * FROM killrvideo.videos_by_tag WHERE tag = 'cassandra';
```

✅ Quit *cqlsh*
```cql
QUIT
```

<!-- NAVIGATION -->
<div id="navigation-bottom" class="navigation-bottom">
 <a href='command:katapod.loadPage?[{"step":"intro"}]'
   class="btn btn-dark navigation-bottom-left">⬅️ Back
 </a>
  <a href='command:katapod.loadPage?[{"step":"step2"}]' 
    class="btn btn-dark navigation-top-right">Next ➡️
  </a>
</div>
