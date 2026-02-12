# Annoy-style Random Projection Tree

Minimal implementation of **Approximate Nearest Neighbor (ANN)** search using **random projection trees**, inspired by Spotify’s Annoy library.

---

## How It Works

1. **Tree Construction**
   - Recursively split the dataset using **random hyperplanes**.
   - At each internal node:
     - Pick **two random points** `p1` and `p2`.
     - Compute the **normal vector**: `w = p1 - p2`.
     - Compute the **threshold**: `b = w · ((p1 + p2)/2)` (midpoint projection).
     - Partition points into two halves:
       - `w · x <= b` → left child
       - `w · x > b` → right child
   - Stop recursion when the number of points in a node ≤ `leaf_size`.

2. **Query**
   - Traverse the tree to reach leaf nodes based on the hyperplane decisions.
   - Collect candidate points from one or multiple trees.
   - Compute distances to candidates.
   - Return top-k nearest neighbors.

3. **Multiple Trees**
   - Building multiple random trees reduces randomness.
   - Improves the probability of including true neighbors in the candidate set.

---

## Why It Works

- Random hyperplanes avoid axis-alignment problems in high dimensions (unlike KD-trees).  
- Similar points are likely to fall in the same leaf nodes.  
- Multiple trees improve search accuracy without scanning all points.  
- Approximate search provides a **speed-accuracy tradeoff**, making it suitable for large datasets.

---

## Node Structure

- **Internal node:** stores `normal` vector and `threshold`, plus `left` and `right` children.  
- **Leaf node:** stores indices of points in that leaf.

---

## Visualization (2D)

- Hyperplanes can be drawn as lines.  
- Leaves correspond to regions of space bounded by these hyperplanes.  
- Example:  

<img width="689" height="701" alt="изображение" src="https://github.com/user-attachments/assets/475bbe8d-5f64-4eb6-be3d-a8949daa9987" />



<img width="1024" height="793" alt="изображение" src="https://github.com/user-attachments/assets/f6604458-42ab-45dc-b091-f488122ee3e9" />

<img width="1024" height="793" alt="изображение" src="https://github.com/user-attachments/assets/ee34631e-34b2-4933-ba96-0b1755a88d67" />

