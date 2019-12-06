# Graphs
- 비선형 자료구조
- 노드와 노드들 사이에 링크로 이루어짐
- 트리와 달리 어떤 패턴으로든 연결될 수 있음
- 그래프는 비어있을 수 있음

### Undirected Graph
- `edges`는 방향이 없다 (오고가고 가능)

#### undirected state graphs
**example** : Graph for game state  
*Three Coins in a line*  
```
Rule :
1. You may flip the middle coin anytime
2. You may flip one of the end coins
    only if the other two coins are the same as each other

ex ) H H H
   1 H H T (rule 2)
   2 T H H (rule 2)
   3 H T H (rule 1)
```

### Directed Graph
- `edges`는 방향이 있다  

## More Graph Terminology
`Loop`  
: 자기 자신 `vertex`에 연결되어있는 `edge`  

`path`  
: `vertex` 들의 `sequence`


`Cycle`  
: `path` P1, P2, ... Pm 가 있으면 P1=Pm인 `path`

`Multiple Edges`  
: 같은 vertex에 같은 방향으로 두개이상 연결된 `edge`가 있음

`Simple Graph`  
: no `loop`, no `multiple edge`


## Graph Implementation
### With Adjacency Matrix (인접행렬)
- 간선의 boolean값 또는 가중치(weight)값이 들어있다

### With Adjacency List  
- n개의 linked list(n : 정점의 개수)  
- 각 array cell은 그래프의 정점을 나타낸다.
- each array cell [i] has a pointer to a linked list of allvertices directly connected from i

### With Edge Sets
- n개의 set배열
- 각 배열은 그래프의 정점을 나타낸다.
- each array cell[i] has a set object set<dataType> of all vertices directly connected from i

### With Weighted Vertices
```cpp
template<class Item>
class graph{
    public:
        static const std::size_t MAX=20;

        void add_vertex(const Item & label);
        void add_edge(size_t source, size_t target);
        void remove_edge(size_t source, size_t target);
        Item& operator [] (size_t vertex);
        size_t size();
        bool is_edge(size_t source, size_t target);
        set<size_t> neighbors(size_t vertex) const;
    private:
        bool edges[MAX][MAX];
        Item labels[MAX];
        std::size_t many_vertices;
}
```
#### Constructor
정점 간선이 모두 없는 그래프 object를 생성한다.
#### Operator[ ]
특정 정점에 대한 reference를 return 한다. 
```cpp
template <class Item>
Item & graph<Item>::operator [] (std::size_t vertex){
    assert(vertex<size());
    return labels[vertex];
}

// 사용
graph<double> g;
g[2]=7.67;
```

#### add_vertex
```cpp
template<class Item>
void graph<Item>::add_vertex(const Item& label){
    size_t new_vertex_number;
    size_t vertex;
    assert(size()<MAX);
    new_vertex_number =many_vertices;
    many_vertices++;
    for(vertex=0;vertex<many_vertices;vertex++){
        edges[vertex][new_vertex_number]=false;
        edges[new_vertex_number][vertex] =false;
    }
    labels[new_vertex_number]= label;
}
``` 

#### add_edge
```cpp
template <class Item>
void graph<Item>::add_edge(std::size_t source, std::size_t target){
    assert(source<size());
    assert(target<size());
    edges[source][target]= true;
}
```
#### remove_edge
```cpp
template <class Item>
void graph<Item>::remove_edge(size_t source, size_t target){
    assert(source<size());
    assert(target<size());
    edges[source][target]=false;
}
```

#### is_edge
```cpp
template <class Item>
bool graph<Item>::is_edge (std::size_t source, std::size_t target) const{
    assert(source<size());
    assert(target<size());
    return edges[source][target];
}
```

#### neighbors
```cpp
template<class Item>
set<size_t> graph<Item>::neighbors (size_t vertex) const{
    set<size> answer;
    assert(vertex<size());
    for(size_t i=0; i<size();i++){
        if(edges[vertex][i])
            answer.insert(i);
    }
    return answer;
}
```

## Graph Traversals
- different from tree traversals since there might be cycles in a graph
- need to **mark** each vertex as it is processed in order to avoid repetitive visits
- there are two common ways
  - Depth-Frist Search (DFS)
  - Breadth-First Search (BFS)


### Depth-First Search
- search proceeds from the start verteex to one of its neighbors, then to one of the neighbor's neighbors, and so on.
- search has to back up if it cannot proceed further
- could be implemented usin a stack or recursively

#### Depth-First Search : Implementation
- we will implementation a `recursive version` of DFS
- depth_first(processFuction, aGraph, startingVertex)
- it will use a boolean array marked[aGraph.MAX] to keep track of the visited vertices
- depth_first(...) will call a recursive function rec_dfs to actually carry out the search

Pseudocode for depth_first(...)
```
1. check that the start vertex is a valid vertex number of the graph
2. Set all the components of `marked[]` to false
3. Call a separate recursive function `rec_dfs` to actually carry out the search
```
```cpp
template<class Process, class Item, class SizeType>
void depth_first(Process f, graph<Item> & g, SizeType start){
    bool marked[g.MAX];
    assert(start<g.size());
    fill_n(marked, g.size(),false);
    rec_dfs(f,g,start, marked);
}
```

#### rec_dfs
```cpp
template <class Process , class Item , class SizeType>
void rec_dfs(Process f, graph<Item> & g, SizeType v, bool marked[]){
    set<size_t> connections = g.neighbors(v);
    set<size_t>::iterator it;
    marked[v]=true;
    f(g[v]);

    for(it=connections.begin();it!=connections.end();++it){
        if(!marked[*it])
            rec_dfs(f,g,*it,marked);
    }
}
```
### Breath-First Search
- search proceeds from the start vertex to each of its neighbors
- after processing all neighbors, search proceeds to the neighbors' neighbors, and so on 
- search ends when all reachable vertices have been processed
- could be implemented using a queue

#### Breadth-First Search : Implementation
- a queue of vertex numbers will be used
- breadth_first(processFunction, aGraph, startingVertex)
- it will use a boolean array marked[aGraph.MAX] to keep track of the visited vertices

```cpp
template <class Process , class Item, class SizeType>
void breadth_first(Process f, graph<Item>& g, SizeType start){
    bool marked[g.MAX];
    set<size_t> connections;
    set<size_t>::iterator it;
    queue<size_t> vertex_queue;
    
    assert(start<g.size());
    fill_n(marked, g.size(),false);
    marked[start]= true;
    f(g[start]);
    vertex_queue.push(start);
    do{
        connections =g.neighbors(vertex_queue.front());
        vertex_queue.pop();
        for(it= connetions.begin();it!=connections.end();++it){
            if(!marked[*it]){
                marked[*it]=true;
                f(g[*it]);
                vertex_queue.push(*it);
            }
        }
    }while(!vertex_queue.empty());
}
```

### Path Algorithms
Determining Whether a Path Exists
- either DFS or BFS could be used to determine whether a path exists between two vertices `u` and `v`

1. Use `u` as the start vertex of the search and proceed with a DFS of BFS
2. If `v` is ever visited, then there is a path between `u` and `v`
3. If `v` is never visited, then there is no path between `u` and `v`

Graphs with Weighted Edges
- the edges of a graph might have weights representing "costs", "distances", etc.
- the **weight of a path** is the **total sum of the weights** of all edges in the path
- the **shortest path** between `u` and `v` is a path between `u` and `v` with the **smallest weight**


Shortest Distance Algorithm
- Given a vertex start of a graph `g`, find the shortest distance to each vertex `v` of the graph.  
-> distance[v] = the weight of the shortest path from start to `v`


#### Dijkstra's Algorithm
Single-Source Shortest Path  
**input** : 방향그래프  
**output** : 시작노드로부터 최단거리가 채워진 distance 배열  
**variables**   
- distance[ ] : 사이즈 n 짜리 int 배열
- allowed_vertices : 정점들의 집합

