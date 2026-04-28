# Manhattan distance

**Keywords**: manhattan distance,l1,taxicab

---

**Manhattan distance** (also called L1 distance or taxicab distance) **measures the distance between two points by summing absolute differences of coordinates**, named after Manhattan's grid layout where movement only occurs along streets rather than diagonally.

**What Is Manhattan Distance?**

- **Definition**: Distance equals sum of absolute coordinate differences.
- **Formula**: d = Σ|aᵢ - bᵢ| for all dimensions
- **Name Origin**: Manhattan taxi can only drive along streets (grid)
- **Geometry**: Forms diamond shape (vs Euclidean's circle)
- **Computation**: Simple and fast (no square root needed)

**Why Manhattan Distance Matters**

- **Computational Efficiency**: O(n) operations, no square root
- **High Dimensions**: More stable than Euclidean in high-D spaces
- **Grid Problems**: Natural fit for grid-based navigation
- **Outlier Robustness**: Less sensitive to outliers than L2 distance
- **Interpretability**: Easy to understand (blocks, steps, moves)
- **Practical**: Used in recommendation, clustering, pathfinding

**Mathematical Formula**

**2D Case**:
Manhattan distance = |x₁ - x₂| + |y₁ - y₂|

**Example**: From (0,0) to (3,4)
Distance = |3-0| + |4-0| = 3 + 4 = **7 blocks**

**N-Dimensional**:
d(A, B) = Σ|aᵢ - bᵢ| for i = 1 to n

**Visual Comparison**:
```
Taxi Path (Manhattan):     Direct Path (Euclidean):
(0,0) → (3,4)            (0,0) → (3,4)
Distance = 7 blocks       Distance = 5 units
```

**Python Implementation**
```python
import numpy as np
from scipy.spatial.distance import cityblock

def manhattan_distance(a, b):
    """Calculate Manhattan distance."""
    return np.sum(np.abs(a - b))

# Example
point1 = np.array([1, 2, 3])
point2 = np.array([4, 6, 8])
distance = manhattan_distance(point1, point2)
# = |1-4| + |2-6| + |3-8| = 3 + 4 + 5 = 12

# Using scipy
distance = cityblock(point1, point2)  # Same result
```

**When to Use Manhattan Distance**

**✅ Excellent For**:
- Grid-based problems (chess, pathfinding)
- High-dimensional data (NLP, images)
- Sparse vectors (text embeddings)
- Integer coordinates (taxi routing)
- Robustness to outliers
- Computational constraints

**❌ Not Ideal For**:
- Continuous geometric spaces
- Circular/radial patterns
- Rotation-invariant applications
- Smooth distance function needs

**Use Cases**

**1. Path Finding & Routing**
```python
def path_heuristic(current, goal):
    """Manhattan distance heuristic for A* pathfinding."""
    return abs(current[0] - goal[0]) + abs(current[1] - goal[1])

# A* algorithm uses this to guide search
# More efficient than Euclidean for grid-based movement
```

**2. Recommendation Systems**
```python
# User preference vectors
user1_ratings = np.array([5, 3, 4, 2, 5])
user2_ratings = np.array([4, 4, 3, 3, 4])

# Manhattan distance between preferences
difference = manhattan_distance(user1_ratings, user2_ratings)
# Smaller = more similar preferences
similarity = 1 / (1 + difference)
```

**3. Image Processing**
```python
# Color difference in RGB space
color1 = np.array([255, 0, 0])   # Red
color2 = np.array([0, 255, 0])   # Green
difference = manhattan_distance(color1, color2)
# = 255 + 255 + 0 = 510 (very different)
```

**4. Outlier Detection**
```python
from sklearn.neighbors import NearestNeighbors

# Find outliers using Manhattan distance from center
nn = NearestNeighbors(metric='manhattan')
nn.fit(data)
distances, indices = nn.kneighbors(data, n_neighbors=5)

# Points far from neighbors are outliers
outliers = data[distances[:, 0] > threshold]
```

**5. Anomaly Detection in Time Series**
```python
# Detect unusual pattern changes
window1 = np.array([100, 102, 101, 103, 102])
window2 = np.array([100, 105, 115, 120, 119])  # Spike!

anomaly_score = manhattan_distance(window1, window2)
# High score detects anomaly
```

**Machine Learning Applications**

**K-Nearest Neighbors (KNN)**
```python
from sklearn.neighbors import KNeighborsClassifier

# Use Manhattan distance instead of Euclidean
knn = KNeighborsClassifier(n_neighbors=5, metric='manhattan')
knn.fit(X_train, y_train)
predictions = knn.predict(X_test)

# Often works better in high dimensions!
```

**K-Medians Clustering**
```python
from sklearn_extra.cluster import KMedoids

# K-Means uses L2, K-Medians uses L1 (Manhattan)
kmedoids = KMedoids(n_clusters=3, metric='manhattan')
labels = kmedoids.fit_predict(data)

# More robust to outliers than K-Means
```

**Pairwise Distance Matrix**
```python
from scipy.spatial.distance import pdist, squareform

points = np.array([[1,2], [3,4], [5,6]])

# Calculate all pairwise Manhattan distances
distances = pdist(points, metric='cityblock')
distance_matrix = squareform(distances)

# Efficient for clustering, similarity analysis
```

**Mathematical Properties**

**Distance Axioms**:
1. **Non-negative**: d(a,b) ≥ 0
2. **Identity**: d(a,a) = 0
3. **Symmetry**: d(a,b) = d(b,a)
4. **Triangle inequality**: d(a,c) ≤ d(a,b) + d(b,c)

**Relationships**:
- Manhattan ≥ Euclidean (always)
- Manhattan ≤ Chebyshev × √n (differs by dimension)
- Manhattan useful when grid structure exists

**Computational Properties**:
- **Time**: O(n) linear in dimensions
- **Space**: O(1) to compute (no storage needed)
- **Parallelizable**: Yes, embarrassingly parallel
- **Differentiable**: No (absolute value|·|)

**Advantages**
✅ Fast computation (no sqrt)
✅ Interpretable (grid steps)
✅ Robust to outliers
✅ Works in high dimensions
✅ Sparse data friendly

**Disadvantages**
❌ Not rotation invariant
❌ Not differentiable at zero
❌ Assumes grid movement
❌ Grid-biased

**Optimization Tips**
```python
# Vectorize for speed
def manhattan_matrix(X, Y):
    """Fast pairwise Manhattan distances."""
    return np.sum(np.abs(X[:, np.newaxis, :] - Y[np.newaxis, :, :]), axis=2)

# Much faster than Python loops!
```

**Real-World Example: Warehouse Routing**
```python
# Robot at origin needs to visit items
items = [(3, 4), (2, 1), (5, 5)]

# Calculate Manhattan distance to each
distances = [abs(x) + abs(y) for x, y in items]
# = [7, 3, 10]

# Visit closest item first
closest_idx = np.argmin(distances)
print(f"Visit item {items[closest_idx]} first")
# Output: "Visit item (2, 1) first"
```

Manhattan distance is **fundamental for grid-based problems and high-dimensional ML** — its computational simplicity, interpretability, and robustness make it indispensable for pathfinding, clustering, outlier detection, and applications where Euclidean distance overestimates true dissimilarity.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/manhattan-distance) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
