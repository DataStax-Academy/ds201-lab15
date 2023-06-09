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

<div class="step-title">Watch the <i>commit log</i> directory as you insert rows</div>

✅ Check that the node is up and running:

```
nodetool status
```

✅ In *terminal-two*, execute the following command:

---
**Note:** Clicking on the command below will automatically execute it in *terminal-two*. 

---

```
### term2
watch -n 1 -d "ls -lh node/data/commitlog"
```

This will refresh the contents of the commit log directory every second so that you can see what happens to the commit log files.


Don’t do it now, but keep in mind you’ll need to press *CTRL-C* to exit the watch.

✅ In *terminal-one*, run *cassandra-stress* to write several hundreds of thousands of rows to our node:
```
./node/tools/bin/cassandra-stress write no-warmup n=250000 -port native=9042 -rate threads=1
```

There are a few things to watch for in *terminal-two* while *cassandra-stress* inserts rows:

*	The total size will continue to increase.
*	The timestamp will change for the current segment being written.
*	You may get additional commit log files as well.

✅ When cassandra-stress completes, terminate the watch on the second terminal by pressing CTRL-C.

```
### {"terminalId": "term2", "macrosBefore": ["ctrl_c"]}
echo "ctrl-c"
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
