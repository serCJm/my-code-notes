---
title: Kruskal
tags:
  - algorithms
emoji: 💻
link:
---

## Kruskal

[Union Find Series](https://www.youtube.com/watch?v=ibjEGG7ylHk)

[Disjoint Sets](https://www.youtube.com/watch?v=wU6udHRIkcc&t)

[Kruskal](https://www.youtube.com/watch?v=5xosHRdxqHA&t)

```js
function kruskals(gNodes, gFrom, gTo, gWeight) {
	// Need union find (disjoint set data-structure for the solution)
	function UnionFind(count) {
		this.roots = Array(count);
		this.ranks = Array(count);

		for (let i = 0; i < count; ++i) {
			this.roots[i] = i;
			this.ranks[i] = 0;
		}
	}

	UnionFind.prototype.find = function (x) {
		let root = x;

		// Find the root of the component
		const roots = this.roots;
		while (roots[root] !== root) {
			root = roots[root];
		}

		// Compress the path leading back to the root
		// "Path compression"
		// Gives amortized constant time complexity
		while (root !== x) {
			const next = roots[x];
			roots[x] = root;
			x = next;
		}
		return root;
	};

	UnionFind.prototype.union = function (x, y) {
		const xr = this.find(x);
		const yr = this.find(y);
		if (xr === yr) {
			return;
		}
		const ranks = this.ranks;
		const roots = this.roots;
		const xd = ranks[xr];
		const yd = ranks[yr];
		if (xd < yd) {
			roots[xr] = yr;
		} else if (xd > yd) {
			roots[yr] = xr;
		} else {
			roots[yr] = xr;
			++ranks[xr];
		}
	};

	// have a list of edges with weights
	const tree = [];
	gFrom.forEach((el, i) => {
		const edge = {};
		edge.v1 = el;
		edge.v2 = gTo[i];
		edge.weight = gWeight[i];
		tree.push(edge);
	});

	// sort the edges in ascending order
	tree.sort((a, b) => a.weight - b.weight);

	const djSet = new UnionFind(gNodes);
	let totalWeight = 0;

	tree.forEach((edge) => {
		// for each edge, find a root
		const r1 = djSet.find(edge.v1);
		const r2 = djSet.find(edge.v2);
		// if edges don't belong to the same set
		// unite them and increase total weight
		// to find minimum spanning tree
		if (r1 !== r2) {
			totalWeight += edge.weight;
			djSet.union(r1, r2);
		}
	});

	return totalWeight;
}
```