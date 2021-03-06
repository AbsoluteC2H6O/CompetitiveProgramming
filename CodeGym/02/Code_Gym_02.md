# Data structures that are not supported directly in the standard library.

Many problems in competitive programming are solved using data structures that does not
have out of the box support in the C++ STL. This folder mention some of them and exposes
ways to implementate them.

# Graphs

The graph allows to modelate a wide range of competitive programming problems.
Is necesary that each contendor or team implements their own bug-free graph structure
in order to compete.

We present some of the common ways to represent a graph in C++, with some terminology first.
Some references are appended for futher readings. You will find there all the formalism related to the topic.

#### ¿What is a graph?

A graph is a collection of two things: nodes and edges.

![grafo1](/CodeGym/02/images/grafo1.png)

In above image, nodes are the circles with integers inside. The edges are the lines joining the circles. ¿What is usefull a bunch of circles and edges?. They allow to modelate relations between objects. For example, each circle may represent a user from a social network and the lines between the relations maintained by two users (friend, wife, pet, conditional freedom officer, etc...)

#### Path
A path leads from node **a** to node **b** throught some edges of the graph. The **lenght of the path** is the number of edges in it. In the Image example, a path in it is: 1->3->4->5 with a lenght of three edges.

A path is a **cycle** if the first and last node are the same. 1->2->4->3->1 is an example of a cycle.

A path is **simple** if each node appears at most once.

#### Connectivity

A graph is connected if there is a path between any two nodes.
The following graph is not connected, because it is not possible to get from
node 4 to any other node:

![grafo2](/CodeGym/02/images/grafo2.png)

#### Edge directions.

A graph is **directed if the edges can be traversed in one direction only**. For
example, the following graph is directed:

![grafo3](/CodeGym/02/images/grafo3.png)

#### Edge weights

In a weighted graph, each edge is assigned a weight. The weights are often
interpreted as edge lengths. **The length** of a path in a weighted graph is the sum of
the edges weights inside the path .For example, the following graph is weighted, and the lenght of the path 1->2->5 is 12:

![grafo4](/CodeGym/02/images/grafo4.png)

#### Neighbors and degrees

Two nodes are **neighbors or adjacent if there is an edge between them**. The
**degree of a node is the number of its neighbors**. For example, in the previous image, the neighbors of node 2 are 1, 4 and 5, so its degree is 3.

We will say a graph is **complete** if the degree of every node is **n-1**, where n is the number of nodes. The graph contains all posible edges between the nodes.

## Graph Representation

As we mention earlier, the main purpose of this folder is to expose common ways to implement these data structures in C++.

For a graph, there is several ways of implementations. Usually, the conditional to choose one or another is the size of the graph and the way the algorithms processes the graph.

In the examples, we'll representing the next graph:

![grafo5](/CodeGym/02/images/grafo5.png)

### Adjacency List

In the adjacency list representation, there is a vector **K** of **V** size (V is the number of nodes in the graph). And for each **K[i]** there is another vector that contains **pairs** of nodes that are neighbors to the **i** node in the form: **(neighbor, weight of the edge to that neighbor)**.

If the graph is unweighted, the second element of the pair can be setted to zero or just droped.

The next little program ejemplifies this approach:
~~~
#include <vector>
#include <utility>
using namespace std;

int main() {

  // We need a Vector of size 4 or more to save the graph.
  vector<pair<int, int>> graph[5];

  graph[1].push_back({2,5});
  graph[2].push_back({3,7});
  graph[2].push_back({4,6});
  graph[3].push_back({4,5});
  graph[4].push_back({1,2});

  return 0;
}
~~~

Why is usefull the adjacency list approach? Because alloud us to easily find all the neighbors for a given node, just by getting that node from the vector and then iterating throught the adjacency list of size **k** associated.

We have **O(n) = O(1) + O(K) = O(k)**. If the graph is small, linear complexity is pretty good and andjacenty list is preferable.

## Adjacency Matrix

In competitive programming, usually the size **V** of a graph is given. So, we can represent the graph using old school C-Style 2D-Array to represent a matrix of **VxV** dimensions.

Each value of the matrix, **matrix[a][b]** indicates whether the graph contains an edge from node a to node b. If the edge is included in the graph, then **matrix[a][b]=1**, and otherwise **matrix[a][b]=0**.

If we have a weighted graph, the value of **matrix[a][b]** should be the weight of the edge joining two nodes.


~~~
#include <vector>
#include <utility>
using namespace std;

int main() {

  int graph[V+1][V+1] = {0};

  graph[1][2] = 5;
  graph[2][3] = 7;
  graph[2][4] = 6;
  graph[3][4] = 5;
  graph[4][1] = 2;

  return 0;
}
~~~

Above's code gives this representation of the graph: ![matrix](/CodeGym/02/images/matrixW.png)

The obvios drawback of the adjacency matrix representation is that the matrix
contains **nxn** elements, and usually most of them are zero. For this reason, the
representation cannot be used if the graph is large.

## Edges list

An edge list contains all edges of a graph in some order. This is a convenient
way to represent a graph if the algorithm processes all edges of the graph and it
is not needed to find edges that start at a given node.

The edge list can be stored in a vector of pairs, where each pair denotes the two nodes (**a**,**b**) joined by an edge.

For our example, we have a weighted graph, so we can extend the edge list to a list of tuples in the form (**a**, **b**; **w**),
where we have a edge between node **a** and **b** with a weight of **w**

~~~
#include <vector>
#include <utility>
#include <tuple>
using namespace std;

int main() {

  // We need a Vector of size 4 or more to save the graph.
  vector<tuple<int, int, int>> edges;

  edges.push_back({1,2,5});
  edges.push_back({2,3,7});
  edges.push_back({2,4,6});
  edges.push_back({3,4,5});
  edges.push_back({4,1,2});

  return 0;
}
~~~
