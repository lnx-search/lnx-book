# Optimising
Part of what makes lnx so powerful and performance is it's fine grain thread
pool controls, this lets you adjust where your system resources go on a runtime
and per index basis.

### Why Do I Want To Do This?
Thread pools are expensive, the more threads you allocate the higher the
general CPU and memory usage is while this is generally a minor difference
it can make a significant difference if you naively set the limits.

For example if we have a server with `12` logical cpus (threads) available
and we want to index a large dataset we want to allocate as many threads
as we can to the index writer. However, if we allocate `12` threads we 
must be aware that this will **_negatively affect our readers_** when we
start adding docs.<br/>
This is because our program can (essentially) only do 12 things in parallel 
MAX, so when we start adding more threads the OS will start switching resources
across the threads so we will naturally see a decrease in performance across 
all our threads.
 
