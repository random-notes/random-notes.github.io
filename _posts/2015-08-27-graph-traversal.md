---
layout: post
title:  "Graph Traversal -- DFS, BFS, and BFS From Both Sides"
date:   2015-08-27
categories: coding
tags: leetcode dfs bfs
---
# Depth First Search
The key point of DFS is keep tracking all visited nodes and make sure check the end condition correctly. Usually the end condition is reaching certain length or certain node.

{% gist b942e1b8b4a9edcd05ec %}

# Breadth First Search
The key point of BFS is also keep tracking visited nodes. Unlike DFS which uses recursion, BFS uses a queue to do layer-by-layer traversal.

{% gist 542f78c54f77f26fb819 %}

In this implementation, we do not keep the lavel, or depth, of the nodes we visited, a simple way to do that is insert an indicator to indicate the end of a layer:

```cpp
deque<Node*> frontier;
vector<int> visited(GRAPH_SIZE, false);
frontier.push_back(startNode);
visited[startNode->label] =   ;
int level = 0; // start node is at level 0
while(frontier.size() > 0) {
  // push a nullptr into the queue as a label, all nodes before that belongs to the same level
  frontier.push_back(nullptr);
  while(frontier.front() != nullptr) {
    /*Do normal works, push_back a new neighbor that we did not visited*/
    frontier.pop_back(); // remove current node
  }
  frontier.pop_back(nullptr); // now we reach the end of the level, add level by 1
  ++level;
}
```

### Breath First Search with Two Frontiers

The following code is for the **Word Ladder** problem. I used BFS with two frontiers to solve this problem. The details can be seen in the code comment

{% gist 2f026be77fa8fcc2ef73 %}