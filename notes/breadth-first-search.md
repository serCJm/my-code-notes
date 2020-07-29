---
title: Breadth-First Search
tags:
  - algorithms
emoji: 💻
link:
---

## Breadth-First Search

```js
/* A Queue object for queue-like functionality over JavaScript arrays. */
var Queue = function() {
    this.items = [];
};
Queue.prototype.enqueue = function(obj) {
    this.items.push(obj);
};
Queue.prototype.dequeue = function() {
    return this.items.shift();
};
Queue.prototype.isEmpty = function() {
    return this.items.length === 0;
};

/*
 * Performs a breadth-first search on a graph
 * @param {array} graph - Graph, represented as adjacency lists.
 * @param {number} source - The index of the source vertex.
 * @returns {array} Array of objects describing each vertex, like
 *     [{distance: _, predecessor: _ }]
 */
var doBFS = function(graph, source) {
    var bfsInfo = [];

    for (var i = 0; i < graph.length; i++) {
        bfsInfo[i] = {
            distance: null,
            predecessor: null };
    }

    bfsInfo[source].distance = 0;

    var queue = new Queue();
    queue.enqueue(source);

    // Traverse the graph
    // As long as the queue is not empty:
    while (!queue.isEmpty()) {
        //  Repeatedly dequeue a vertex u from the queue.
        var current = queue.dequeue();
        for (var j = 0; j < graph[current].length; j++) {

            //  For each neighbor v of u that has not been visited:
            var neighbor = graph[current][j];
            if (bfsInfo[neighbor].distance === null) {
                //     Set distance to 1 greater than u's distance
                bfsInfo[neighbor].distance = bfsInfo[current].distance + 1;
                //     Set predecessor to u
                bfsInfo[neighbor].predecessor = current;
                //     Enqueue v
                queue.enqueue(neighbor);
            }

            //
            //  Hint:
            //  use graph to get the neighbors,
            //  use bfsInfo for distances and predecessors
        }
    }
    return bfsInfo;
};
```