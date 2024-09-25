## Summary
The Nearest Neighbor Descent (NN-Descent) algorithm is based on an intuitive hypothesis—“the neighbors of neighbors are likely to also be neighbors.” The algorithm starts with a randomly generated low-quality k-nearest neighbor graph and gradually improves the quality of the graph through an iterative process. In each iteration, distances between nodes in the neighbor lists are computed pairwise, and attempts are made to insert each other into the respective neighbor lists; this step is known as "local-join." Suppose node i has two neighbor nodes u and v; after local-join, two new directed edges, <u, v> and <v, u>, will be created. If the new edge <u, v> is shorter than the longest edge in node u's k-nearest neighbor list, then node v can be inserted into node u's k-nearest neighbor list, and vice versa. As new edges are continuously inserted, the quality of the k-nearest neighbor graph gradually improves until convergence.

## Explain in detail

1. init a random k-NN graph
2. NN-Descent (local-join & sampling)

## C++ implementation key points

1. Basic C++ Struct of a Node 

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
3. Sampling 
4. Local-join

## Memory Consumption Analysis 