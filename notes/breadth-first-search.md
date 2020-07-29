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
            //  Hint:
            //  use graph to get the neighbors,
            //  use bfsInfo for distances and predecessors
        }
    }
    return bfsInfo;
};
```

## Finding Shortest Path

[Castle On The Grid](https://www.youtube.com/watch?v=OM5hC6rOIM8)
[Breadth First Search grid shortest path](https://www.youtube.com/watch?v=KiCBXu4P-2Y)

```js
function minimumMoves(grid, startRow, startCol, goalRow, goalCol) {
	// initialize a 2d matrix to track visited nodes and its parents
	const visited = Array(grid.length)
		.fill(false)
		.map((_) =>
			Array(grid[0].length)
				.fill(false)
				.map((_) => ({ visited: false, parent: null }))
		);

	visited[startRow][startCol].visited = true;
	// console.log(visited);

	const rowQueue = [startRow];
	const colQueue = [startCol];

	let moves = 0;
	let parent = null;
	while (rowQueue.length !== 0) {
		const row = rowQueue.shift();
		const col = colQueue.shift();
		if (row === goalRow && col === goalCol) {
			parent = visited[row][col].parent;
			break;
		}
		exploreNeighbours(row, col);
	}

	// console.log(visited);
	// backtrack to count moves
	while (parent !== null) {
		// console.log(parent);
		moves++;
		parent = visited[parent[0]][parent[1]].parent;
	}

	return moves;

	function exploreNeighbours(row, col) {
		// north, south, east, west direction vectors
		const dc = [0, 0, -1, +1];
		const dr = [-1, +1, 0, 0];

		for (let i = 0; i < 4; i++) {
			let cc = col;
			let rr = row;
			while (true) {
				cc += dc[i];
				rr += dr[i];

				// stop at out of bounds
				if (cc < 0 || rr < 0) break;
				if (rr > grid.length - 1 || cc > grid[0].length - 1) break;

				// stop visited or blocked cells
				if (visited[rr][cc].visited) continue;
				if (grid[rr][cc] === "X") break;

				colQueue.push(cc);
				rowQueue.push(rr);
				visited[rr][cc].visited = true;
				visited[rr][cc].parent = [row, col];
			}
		}
	}
}
```