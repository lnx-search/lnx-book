# Understanding Your Workload
Not every set of data is going to have the same optimum configuration, there
are a few points you probably want to be aware of when choosing your settings.

### Large Datasets (millions+ of documents)
Searching large datasets can benefit from having a larger number of reader
threads and a lower max concurrency set, having a low number of reader threads
and a high concurrency has the negative affect of trying to run many searches
at once very slowly vs a few searches very quickly. 

There is also a hard capped limit on how much your system can actually handle.
If you set the max concurrency beyond what your system can handle you will
seg fault at high loads which is a un-recoverable error. 

### Small Datasets (thousands+ of documents)
Smaller datasets often cannot make good use of a larger number of reader threads,
this is because the data is not split into enough segments to make full use of
the parallel computation.

Often with smaller datasets you're better having a higher concurrency and a lower
number of reader threads.

For example, the movies.json dataset as part of the demo app is roughly most
optimised at 1 or 2 reader threads and `n * 3` max concurrency where `n` is the
number of cpu cores on the machine. 
This can still change though depending on the clock speed of your CPU so still 
benchmark and test before sticking with a configuration.
