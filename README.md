SCD
===

This program is an implementation of the community detection algorithm described in the papers titled

[High quality, scalable and parallel community detection for large real graphs.](http://www.dama.upc.edu/en/publications/fp546prat.pdf) Arnau Prat-Pérez, David Dominguez-Sal, Josep-Lluis Larriba-Pey - WWW 2014.

[Put Three and Three Together: Triangle-Driven Community Detection.](http://dl.acm.org/citation.cfm?id=2775108) Arnau Prat-Prez, David Dominguez-Sal, Josep-M. Brunat, Josep-Lluis Larriba Pey - TKDD.

> This is a slightly refactored version of the original SCD extended with the I/O formats to be natively applicable for the [Clubmark](https://github.com/eXascaleInfolab/clubmark) clustering benchmark.  
Extended by Artem Lutov <artem@exascale.info>


Compile
===

SCD uses CMake 2.8.2 or greater to compile. In order to build SCD, move to SCD directory and type:

```
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
```

This will create a build directory into the SCD folder tree, and configure and build SCD in Release mode.
In order to compile SCD in Debug mode, please replace the last two lines of the snippet above by:

```
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
```

Execution
===

To execute SCD type, move to the build folder and type:

```
./scd -f [network file name]
```

where the [network file name] contains the network with an edge per line, and each edge is represented as a pair of numeric identifiers.
IMPORTANT: the input network should be unweighted and undirected.

> Formally, a simple [nsL (nse, nsa) format](https://github.com/eXascaleInfolab/PyNetConvert#nsl) is supported, which is a generalization of ncol and snap formats. The duplicated edges / arcs are skipped, the weights are also skipped showing the respecting WARNING to stderr.

The next snipped shows a valid network file:

```
1 2
4 5
3 4
1 3
```

As an example, type:

```
./scd -f ./network.dat
```

This will run the program, and will output the communities found at network.dat to "./communities.dat", which contains
a community per line, represented as a list of identifiers. network.dat contains a network composed by two cliques of size 5 linked by a single edge. Therefore, communities.dat outputs:

```
1 2 3 4 5
6 7 8 9 10
```

Now, we summarize the options of the program:

  *  -f [netork file name] :    Specifies the network file.
  *  -o [output file name] :    Specifies the output file name (DEFAULT="communities.dat").
  *  -n [number of threads]:    Specifies the number of threads to run the algorithm (DEFAULT=maximum available cores).
  *  -l [lookahead]:            Specifies the number of lookahead iterations to look before terminating the optimization process (DEFAULT=5).
  *  -p [partition file name]:  Specifies the initial partition to refine from (Optional).


Tools
===

Into the build folder, a folder named tools is automatically created and contains useful tools. The list of tools currently available are the following:
  * wcc: Computes, given a graph and a partition, the WCC of the partition.
      * Usage: wcc -f [graph file name] -p [partition file name]
  * f1score: Computes the f1score between two partitions.
      * Usage: f1score [partition file name 1] [partition file name 2]


Related Projects
===

- [xmeasures](https://github.com/eXascaleInfolab/xmeasures)  - Extrinsic quality (accuracy) measures evaluation for the overlapping clustering on large datasets: family of mean F1-Score (including clusters labeling), Omega Index (fuzzy version of the Adjusted Rand Index) and standard NMI (for non-overlapping clusters).
- [GenConvNMI](https://github.com/eXascaleInfolab/GenConvNMI) - Overlapping NMI evaluation that is compatible with the original NMI (unlike the `onmi`).
- [OvpNMI](https://github.com/eXascaleInfolab/OvpNMI) - Another method of the NMI evaluation for the overlapping clusters (communities) that is not compatible with the standard NMI value unlike GenConvNMI, but it is much faster and yields exact results unlike probabilistic results with some variance in GenConvNMI.
- [Clubmark](https://github.com/eXascaleInfolab/clubmark) - A parallel isolation framework for benchmarking and profiling clustering (community detection) algorithms considering overlaps (covers).
- [ExecTime](https://bitbucket.org/lumais/exectime/)  - A lightweight resource consumption (RSS RAM, CPU, etc.) profiler.
