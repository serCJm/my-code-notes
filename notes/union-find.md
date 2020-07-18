---
title: Union Find (Disjoint Sets)
tags:
  - data-structures
emoji: 🌲
link: https://www.youtube.com/watch?v=ibjEGG7ylHk
---

## Description
Disjoint sets are sets with nothing in common.
Find is an operation that determines weather an element belongs to a particular set.
Union connects two components/sets together.

Also, helps to find if there's a cycle present in a graph. When uniting components together into sets, one by one, if both components are already present in a particular set, that would form a cycle, so don't unite them.

### JS Implementation

```js
function UnionFind(count) {
        this.roots = Array(count);
        this.ranks = Array(count);

        for(let i=0; i<count; ++i) {
            this.roots[i] = i;
            this.ranks[i] = 0;
        }
    }

    UnionFind.prototype.find = function(x) {
        let root = x;

        // Find the root of the component
        const roots = this.roots;
        while(roots[root] !== root) {
            root = roots[root]
        }

        // Compress the path leading back to the root
        // "Path compression"
        // Gives amortized constant time complexity
        while(root !== x) {
            const next = roots[x]
            roots[x] = root
            x = next;
        }
        return root;
    }

    UnionFind.prototype.union = function(x, y) {
        const xr = this.find(x);
        const yr = this.find(y);
        if (xr === yr) {
            return;
        }
        const ranks = this.ranks;
        const roots = this.roots;
        const xd = ranks[xr];
		const yd = ranks[yr];
		// Merge two components/sets together
		// Merge smaller set into larger one
        if(xd < yd) {
			roots[xr] = yr;
        } else if(xd > yd) {
            roots[yr] = xr;
        } else {
            roots[yr] = xr;
            ++ranks[xr];
        }
    }
```