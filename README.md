Smart City / Smart Campus Scheduling — Assignment 4
What This Project Is About

This project implements several graph algorithms to handle dependencies between tasks in a city or campus system.
The goal is to identify cycles, establish a valid execution order, and compute the minimum and maximum time needed to complete chains of tasks.

The core algorithms used here:

Strongly Connected Components (SCC) — Tarjan's algorithm

Topological Sorting — Kahn's algorithm

Shortest and Longest Paths in a DAG — dynamic programming based on topological order

Project Structure
```plaintext
PrimMST/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   ├── model/          # Graph data models
│   │   │   ├── graph/
│   │   │   │   ├── scc/        # SCC algorithms
│   │   │   │   ├── topo/       # Topological sort
│   │   │   │   └── dagsp/      # DAG shortest/longest paths
│   │   │   ├── util/           # Metrics and utilities
│   │   │   └── org/example/    # Main class
│   │   └── resources/
│   └── test/
│       └── java/               # JUnit tests
├── data/                        # Test datasets
├── pom.xml                       # Maven configuration
└── README.md

Requirements

Java 21+

Maven 3.6+

Build and Run
Build:
mvn clean compile

Run:
mvn exec:java -Dexec.mainClass="org.example.Main" -Dexec.args="data/tasks.json"

Alternative run:
mvn compile
java -cp target/classes:$(mvn dependency:build-classpath -q -Dmdep.outputFile=/dev/stdout) org.example.Main data/tasks.json

Tests:
mvn test

Dataset generation:
mvn compile exec:java -Dexec.mainClass="util.GraphGenerator"


Datasets are already in /data/.

Dataset Info

The /data/ folder contains graphs of different sizes.
Some contain cycles, some are pure DAGs, some consist of multiple SCCs.

Edge weights represent the cost or time of the dependency between tasks.

Algorithms
1. SCC (Tarjan's Algorithm)

Finds groups of nodes forming cycles.
Runs in O(V + E).

TarjanSCC tarjan = new TarjanSCC(graph);
List<List<Integer>> sccs = tarjan.findSCCs();

2. Condensation Graph

Each SCC becomes a single node.
The resulting graph is always a DAG.

3. Topological Sort (Kahn)

Determines a valid task execution order if no cycles exist.
Detects cycles if they are present.

TopologicalSort topo = new TopologicalSort(dag);
List<Integer> order = topo.kahnTopologicalSort();

4. Shortest Paths in DAG

Computes minimum cost chains of tasks.

DAGShortestPath sp = new DAGShortestPath(dag, source);
double[] distances = sp.getDistances();

5. Longest Paths (Critical Path)

Finds the chain of tasks that determines the total execution time.

DAGLongestPath lp = new DAGLongestPath(dag);
double length = lp.getLongestPathLength();

Metrics

Each algorithm tracks:

Execution time

Edge traversals

Queue operations

Relaxation steps

Used to analyze performance.

Input Format
{
  "directed": true,
  "n": 8,
  "edges": [
    {"u": 0, "v": 1, "w": 3.0}
  ],
  "source": 0,
  "weight_model": "edge"
}

Output

The program prints:

SCCs and their sizes

Condensed DAG

Topological order

Shortest path distances

Critical path and length

Performance metrics

Summary

This project shows how to reason about dependent tasks, find the execution order, and identify bottlenecks.
Useful for project planning, logistics, city management systems, and any scenario with dependency chains.