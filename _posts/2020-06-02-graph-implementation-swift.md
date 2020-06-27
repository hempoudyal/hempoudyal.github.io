---
layout: post
title:  Graph Algorithms implementation in Swift - Avengers Example
date:   2020-06-02 14:01:35 +0300
image:  './../../../assets/img/graph/graphMain.jpeg'
tags:   [DataStructure, Swift, iOS]
---
Graphs are the widely used data structure in Computer Science. Basic understanding of Graphs can help to develop knowledge about how modern technologies are mapping complex data and using it for required functionalities.

The relationship mapping of people in Social Media can aid to suggest mutual friend. Similarly, relationship mapping of flights between cities can help to carry out operations such as to find cheapest flight or shortest route to any city.

In this tutorial, we’ll implement a basic graph structure in Swift, and explore some of its properties.

---

## Graph

A graph is made up of a set of vertices paired with a set of edges.

![](/assets/img/graph/graph1.png)

In the graph above, **Vertices** are the circles, and the lines between them are **Edges**. A vertex connects to other vertices by edges.

## Directed and Undirected Graphs

A graph can have direction, edges have direction to respective vertices. It depends on the structure of data, the **flight** mapping between cities are usually **directed**, as a flight may not return through same route.

A graph can also be undirected graph, where there is no direction. **Friendship** mapping in social media is **undirected**, as both are friend of each other.

## Adjacency List

Each Vertex will hold an adjacency list, described by outgoing edges. The list can even help us to visualize the data being hold by adjacent vertices.

## Implementing Graphs in Swift

![](/assets/img/graph/graph2.png)

As in the above image we are going to create a graph of Avengers. I assume, most of you adore the superheroes and are aware of them. Here, superheroes and the movies they were in are Vertices. They are linked with each other based on the movies they were together through Edges. Superheroes those were ally in particular movies, such as SpiderMan and IronMan in Civil war are also linked with each other.

The relationships between superheroes are undirected, if two heroes are ally with each other, both are connected by undirected edge.

## Implementing Vertex, Edge and Graphable Protocol

Let’s start by creating a new Swift playground.

![](/assets/img/graph/graph3.png)

First we need a **vertex** for the graph. We have to create a struct named Vertex, that holds a generic type data.

Now to connect every vertex to another vertex, we need an edge between them. Create a struct named **Edge** with two properties **source** and **destination**. We also need an enum of **EdgeType** to describe whether an edge between two vertices is a directed path or undirected path.

Since we are storing a vertex as a custom key in a dictionary, it has to conform to the **Hashable** protocol. Hashable inherits from the Equatable protocol, we must also add an equal-to operator for the custom type.

The **CustomStringConvertible** protocol allows us to define a custom output. We are going to use it to print index and data of Vertex.

Now, we have the most important part, we have to define Graphable protocol. The protocol requires an **associatedType** called **Element**. This allows the protocol to be generic, to hold any type.

**createVertex(data:)** provides a utility method to create a vertex

**addDirectedEdge(from: to:)** allows to add direct edge from source to destination

similarly **addUndirectedEdge(between: and:)** allows for undirected edge

**add(_:from:to)** provides a utility to get an edge between two vertices

**edges(from:)** provides a utility to retrieve all the edges that source vertex connects to

## Adjacency List

Let’s implement the adjacency list, in this tutorial, we will create adjacency list using dictionary of arrays. Each key in the dictionary is a vertex, and each value is the corresponding array of edges. We will use a dictionary adjacencies to store our graph.

![](/assets/img/graph/graph4.png)

We are going to use the methods that we declared previously in the Graph protocol.

* Use **createVertex** to create a Vertex.

* Next, we will implement **addDirectedEdge** that creates an Edge, retrieve the array of edges affiliated with source vertex and append the edge to array.

* An undirected graph can be treated as a directed graph that goes both ways, let’s call **addDirectedEdge** twice, but with source and destination swapped.

* **add(_:from:to)** checks to see if the type is directed or undirected, creates the correct edge.

* To retrieve information, loop through each edge.

## Visualize the adjacency list

![](/assets/img/graph/graph5.png)

This will help us to print out the vertex, and all the vertices it’s connected to by an edge.

The adjacency list is complete, we have succesfully built our graph data structure.

## Testing graph with Avengers Superheroes

![](/assets/img/graph/graph6.png)

The code above creates the association graph based on the relationship of the Avengers using the adjacency list.

Swift playground should show the following output.

![](/assets/img/graph/graph7.png)

Complete code, you can run in Swift Playground:


{% highlight ruby %}

enum EdgeType {
    case directed
    case undirected
}

struct Vertex<T> {
    let data: T
    let index: Int
}

struct Edge<T> {
    let source: Vertex<T>
    let destination: Vertex<T>
}

extension Vertex: Hashable where T: Hashable {}
extension Vertex: Equatable where T: Equatable {}

extension Vertex: CustomStringConvertible {

    var description: String{
        return "\(index):\(data)"
    }

}

protocol Graph {
    
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>)
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>)
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
    
}

class AdjacencyList <T: Hashable>: Graph {
    
    private var adjancencies: [Vertex<T>: [Edge<T>]] = [:]
    
    init() {}
    
    func createVertex(data: T) -> Vertex<T> {
        
        let vertex = Vertex(data: data, index: adjancencies.count)
        adjancencies[vertex] = []
        return vertex
        
    }
    
    func addDirectedEdge(from source: Vertex<T>, to destination: Vertex<T>) {
        
        let edge = Edge(source: source, destination: destination)
        adjancencies[source]?.append(edge)
        
    }
    
    
    func addUndirectedEdge(between source: Vertex<T>, and destination: Vertex<T>) {
        
        addDirectedEdge(from: source, to: destination)
        addDirectedEdge(from: destination, to: source)
        
    }
    
    func add(_ edge: EdgeType, from source: Vertex<T>, to destination: Vertex<T>) {
        
        switch edge {
        case .directed:
            addDirectedEdge(from: source, to: destination)
        case .undirected:
            addUndirectedEdge(between: source, and: destination)
        }
        
    }
    
    func edges(from source: Vertex<T>) -> [Edge<T>] {
        return adjancencies[source] ?? []
    }
    
}

extension AdjacencyList: CustomStringConvertible {
    public var description: String {
        var result = ""
        for (vertex, edges) in adjancencies { 
            var edgeString = ""
            for (index, edge) in edges.enumerated() { 
                if index != edges.count - 1 {
                    edgeString.append("\(edge.destination), ")
                } else {
                    edgeString.append("\(edge.destination)")
                }
            }
            result.append("\(vertex) ---> [ \(edgeString) ]\n") 
        }
        return result
    }
    
}

let graph = AdjacencyList<String>()

let spiderMan = graph.createVertex(data: "Spider Man")
let ironMan = graph.createVertex(data: "Iron Man")
let captainAmerica = graph.createVertex(data: "Captain America")
let antMan = graph.createVertex(data: "Ant Man")
let civilWar = graph.createVertex(data: "Civil War")
let avengers = graph.createVertex(data: "Avengers")
let thor = graph.createVertex(data: "Thor")
let ragnarok = graph.createVertex(data: "Ragnarok")
let hulk = graph.createVertex(data: "Hulk")

graph.add(.undirected, from: spiderMan, to: ironMan)
graph.add(.undirected, from: spiderMan, to: civilWar)
graph.add(.undirected, from: spiderMan, to: avengers)
graph.add(.undirected, from: ironMan, to: avengers)
graph.add(.undirected, from: ironMan, to: civilWar)
graph.add(.undirected, from: captainAmerica, to: civilWar)
graph.add(.undirected, from: captainAmerica, to: avengers)
graph.add(.undirected, from: captainAmerica, to: antMan)
graph.add(.undirected, from: antMan, to: civilWar)
graph.add(.undirected, from: antMan, to: avengers)
graph.add(.undirected, from: thor, to: avengers)
graph.add(.undirected, from: thor, to: ragnarok)
graph.add(.undirected, from: hulk, to: avengers)
graph.add(.undirected, from: hulk, to: ragnarok)

print(graph)

{% endhighlight %}