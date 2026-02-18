# Kruskal_video
https://drive.google.com/file/d/1uhuEfefCMHhSILJaBTQio2UetHRY4gaC/view?usp=drive_link
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Edge {
    int src;
    int dest;
    int weight;
};


int findParent(vector<int> &parent, int node) {
    if (parent[node] != node)
        parent[node] = findParent(parent, parent[node]);
    return parent[node];
}


void unite(vector<int> &parent, vector<int> &rankV, int u, int v) {
    int root1 = findParent(parent, u);
    int root2 = findParent(parent, v);

    if (root1 == root2) return;

    if (rankV[root1] < rankV[root2])
        parent[root1] = root2;
    else if (rankV[root1] > rankV[root2])
        parent[root2] = root1;
    else {
        parent[root2] = root1;
        rankV[root1]++;
    }
}

int main() {
    int V, E;

    cout << "Enter number of vertices: ";
    cin >> V;

    cout << "Enter number of edges: ";
    cin >> E;

    vector<Edge> edges(E);

    cout << "Enter source, destination and weight for each edge:\n";
    for (int i = 0; i < E; i++) {
        cin >> edges[i].src >> edges[i].dest >> edges[i].weight;
        edges[i].src--;   // Convert to zero-based index
        edges[i].dest--;
    }

    
    sort(edges.begin(), edges.end(), [](Edge a, Edge b) {
        return a.weight < b.weight;
    });

    vector<int> parent(V);
    vector<int> rankV(V, 0);

    for (int i = 0; i < V; i++)
        parent[i] = i;

    int mst_weight = 0;
    int edges_used = 0;

    cout << "\nEdges in the Minimum Spanning Tree:\n";

    for (int i = 0; i < E && edges_used < V - 1; i++) {
        int u = edges[i].src;
        int v = edges[i].dest;

        int root1 = findParent(parent, u);
        int root2 = findParent(parent, v);

        
        if (root1 != root2) {
            cout << u + 1 << " -- " << v + 1
                 << " == " << edges[i].weight << endl;

            unite(parent, rankV, root1, root2);
            mst_weight += edges[i].weight;
            edges_used++;
        }
    }

    cout << "Total weight of MST: " << mst_weight << endl;

    return 0;
}
