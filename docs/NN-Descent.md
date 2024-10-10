## Summary
The Nearest Neighbor Descent (NN-Descent) algorithm is based on an intuitive hypothesis—“the neighbors of neighbors are likely to also be neighbors.” The algorithm starts with a randomly generated low-quality k-nearest neighbor graph and gradually improves the quality of the graph through an iterative process. In each iteration, distances between nodes in the neighbor lists are computed pairwise, and attempts are made to insert each other into the respective neighbor lists; this step is known as "local-join." Suppose node i has two neighbor nodes u and v; after local-join, two new directed edges, <u, v> and <v, u>, will be created. If the new edge <u, v> is shorter than the longest edge in node u's k-nearest neighbor list, then node v can be inserted into node u's k-nearest neighbor list, and vice versa. As new edges are continuously inserted, the quality of the k-nearest neighbor graph gradually improves until convergence.

## Explain in detail

1. init a random k-NN graph
2. NN-Descent (local-join & sampling)

## C++ implementation key points

1. RE-organize the `Node` 

Since the k-NN points should be sorted by distances, and has a `new/old` attribute for local-join, the basic `Node` can be defined as below.

```cpp
#include <iostream>
#include <limits>

struct Node {
    float dist;
    unsigned idx;
    bool is_new;

    // Default constructor
    Node() : dist(std::numeric_limits<float>::max()), idx(0), is_new(true) {}

    // Comparison operators
    bool operator<(const Node& other) const { return dist < other.dist; }
    bool operator>(const Node& other) const { return dist > other.dist; }
    
    // Updated operator<= to check for equal distance and smaller index
    bool operator<=(const Node& other) const {
        return (dist == other.dist && idx < other.idx) || (dist < other.dist);
    }

    bool operator>=(const Node& other) const {
        return (dist == other.dist && idx > other.idx) || (dist > other.dist);
    }
    
    bool operator==(const Node& other) const {
        return dist == other.dist && idx == other.idx;
    }

    // Setter methods
    void setNew() { is_new = true; }
    void setOld() { is_new = false; }

    // Check if it's new
    bool isNew() const { return is_new; }
};

int main() {
    Node a;
    Node b;
    b.dist = 10.0f;
    b.idx = 1;

    std::cout << "Node a is new: " << a.isNew() << std::endl;
    std::cout << "Node b distance: " << b.dist << std::endl;
    std::cout << "Comparison a < b: " << (a < b) << std::endl;
    std::cout << "Comparison a <= b: " << (a <= b) << std::endl;

    return 0;
}
```

2. Randomly generate a *k*-NN graph

   ```cpp
   //* Given N and k, assign a part of memory to host the k-NN graph *//
       const unsigned N = 5; // Example number of points
       const unsigned k = 3; // Number of neighbors
   
       // Create a 2D vector to hold the kNN graph with Node type
       std::vector<std::vector<Node>> knnGraph(N, std::vector<Node>(k));
   
   // Fill the k-NN graph with random id.
       std::srand(static_cast<unsigned>(std::time(0)));
   
       for (unsigned i = 0; i < N; ++i) {
           for (unsigned j = 0; j < k; ++j) {
               unsigned random_id;
               do {
                   random_id = std::rand() % N; // Generate a random index
               } while (random_id == i); // Ensure it's not equal to i
               
               float distance = calculateDistance(i, random_id); 
               // Calculate distance between i and random_id
               
               knnGraph[i][j] = Node{distance, random_id, true}; 
               // Assign node with calculated distance and random id
           }
       }
   
   // Sort the knn neighbors by distances
   	for (unsigned i = 0; i < N; ++i) {
           sort(knnGraph[i].begin(), knnGraph[i].end());
       }
   ```

3. Sampling

   If all $k$ neighbors 

   ```cpp 
   ```

   

4. Local-join

## Memory Consumption Analysis 