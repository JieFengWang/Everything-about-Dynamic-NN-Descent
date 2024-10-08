## Summary of *k*-NN Graph
Without loss of generality, consider a dataset C = \{x_i | x_i \in \mathbb{R}^d\} containing n points, where n is the size of the dataset and d is its dimensionality. The k-nearest neighbor graph G of dataset C is a directed graph, where each node corresponds to a d-dimensional vector data point. In this graph, each node is connected by k directed edges to its k nearest nodes under the distance metric m(·, ·) . Using an adjacency list storage method, graph G contains n k-nearest neighbor lists, with each list corresponding to a node's k neighbors. G[i] represents the k-nearest neighbor list of the i-th node, which contains k nodes sorted in ascending order of their distance to node i . These nodes are referred to as neighbors of node i , and the list G[i] defines the neighborhood of node i . For dataset C , the time complexity of constructing the k-nearest neighbor graph using a brute force method is O(d \cdot n^2) .

## Illustration of *k*-NN Graph

![](./../figs/real_knn.jpg)

## Representation by 2D C++ Vector

*k*-NN graph can be represented by a 2D vector with C++. 

```cpp
#include <iostream>
#include <vector>

using Node = unsigned; // Define Node as an alias for unsigned

int main() {
    Node numPoints = 5; // Example number of points
    Node k = 3; // Number of neighbors

    // Create a 2D vector to hold the kNN graph
    std::vector<std::vector<Node>> knnGraph(numPoints, std::vector<Node>(k));

    // Example: Filling the kNN graph with dummy indices
    for (Node i = 0; i < numPoints; ++i) {
        for (Node j = 0; j < k; ++j) {
            knnGraph[i][j] = (i + j + 1) % numPoints; // Dummy neighbors
        }
    }

    // Print the kNN graph
    for (Node i = 0; i < numPoints; ++i) {
        std::cout << "Neighbors of point " << i << ": ";
        for (Node j = 0; j < k; ++j) {
            std::cout << knnGraph[i][j] << " ";
        }
        std::cout << std::endl;
    }

    return 0;
}
```