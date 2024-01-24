---
title: DS Algo Interview Questions
date: 2024-01-13
lang: en
duration: 10min
type: blog
---

## Time Complexity
- sort(): O(nlogn)
- heaps
    - top(): O(1)
    - insert(): O(logn)
    - remove(): O(logn)
    - heapify(): O(n)  # convert a list into a heap
- binary search: O(logn)
- hashmaps:
    - search: O(1)
    - insert: O(1)
    - remove: O(1)


## 6.Top k values, k closest points to origin, network delay time, graph algos, shortest path algos, prims algo, min cost to connect all points
- minheap, maxheap


## 5.Sliding window


## 4.Binary search: search for a number, search a 2d matrix
- sorted array, start from halfway point, eleminate one side


## 3.DFS, BFS: number of islands
- trees, graphs, matrices

BFS in a tree (level order traversal):
use a queue
- add root's the children to the queue
- remove first element from the queue, add its children to the queue


DFS in a tree:
- use any: inorder (left, root, right), preorder (root, left, right), postorder (left, right, post)


DFS in a cyclic graph:
- as you go, mark visited nodes. once reached the deepest end, backtrack back to the start


Topological sort:
- directed acyclic graph (DAG): eg: tasks that depend on each other
Topological sort = convert a DAG into a linear graph: where all the dependencies are listed in a order
using dfs:
use a stack:
start from one node, traverse to the deepest end, while backtracking, add them to a stack
the stack top to bottom is one topological sort. can have many



## 2.Recusrion: combination sum, n-queens
backtracking, dp, memoization


## 1.Hashmaps: two sum



## 1.two pointers
- sorted array, keep each pointer at each end, decrease the right if the sum is > expected, increase the left if sum is < expected


## 2.divide and conquer
- used in merge sort

## 3.k way merge
- divide arrays into single arrays and



## Write the code for a Binary Search Tree

```python
class Node:
    def __init__(self, key):
        self.val = key
        self.left = None
        self.right = None


class Tree:
    def __init__(self):
        self.root = None

    def insert(self, key):
        if not self.root:  # Add the first node
            self.root = Node(key)
        else:  # Add it somewhere below the root
            self._insert_recursive(self.root, key)

    def _insert_recursive(self, node, key):
        """Insert the given key somewhere below the given node"""

        if key > node.val:  # Choose left or right side
            if not node.right:  # If leaf, add the node, else keep going in
                node.right = Node(key)
            else:
                self._insert_recursive(node.right, key)
        else:
            if not node.left:
                node.left = Node(key)
            else:
                self._insert_recursive(node.left, key)

    def inorder(self, node):
        """Print the value of the node after going left, then go right"""

        if node:
            self.inorder(node.left)
            print(node.val, ' ')
            self.inorder(node.right)


tree = Tree()
tree.insert(5)
tree.insert(3)
tree.insert(7)
tree.insert(1)
tree.insert(4)
tree.insert(6)

tree.inorder(tree.root)
```

## Write the code for Least Recently Used Cache
Implement a cache that stores a limited number of items and evicts the least recently used item when it exceeds its capacity.

```python
class Node:
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.next = None  # Doubly linked list connections
        self.prev = None

class Cache:
    def __init__(self, capacity):
        self.head = Node(0,0)  # Special nodes
        self.tail = Node(0,0)
        self.head.next = self.tail  # Connecting them
        self.tail.prev = self.head

        self.capacity = capacity
        self.nodes = {}  # Our Cache DB {key: node}

    def put(self, key, val):
        if len(self.nodes) == self.capacity: # remove last node and add new node
            self._remove_node(self.head.next)
            del self.nodes[key]

        if key in self.nodes: # remove it
            self._remove_node(self.nodes[key])

        self._add_node(Node(key, val))
        self.nodes[key] = Node(key, val)

    def get(self, key):
        if key in self.nodes: # remove and add again
            self._remove_node(self.nodes[key])
            self._add_node(self.nodes[key])
            return self.nodes[key].val
        else:
            return -1

    def _add_node(self, node):
        """Add node between tail and tail.prev"""
        newest_node = self.tail.prev

        newest_node.next = node
        node.prev = newest_node

        node.next = self.tail
        self.tail.prev = node

    def _remove_node(self, node):
        """Connect node.prev with node.next"""
        node.prev.next = node.next
        node.next.prev = node.prev
```

## Write the code for pagerank algorithm
Parse a folder of html pages, get all a links from it, use networkx's pagerank algorithm

```py
from bs4 import BeautifulSoup
import os
import networkx as nx

def extract_links_from_html(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        contents = file.read()
    soup = BeautifulSoup(contents, 'html.parser')
    links = [a['href'] for a in soup.find_all('a', href=True)]
    return links

def create_graph_from_folder(folder_path):
    graph = nx.DiGraph()

    for filename in os.listdir(folder_path):
        if filename.endswith('.html'):
            file_path = os.path.join(folder_path, filename)
            links = extract_links_from_html(file_path)
            for link in links:
                graph.add_edge(filename, link)

    return graph

def calculate_page_rank_from_folder(folder_path):
    graph = create_graph_from_folder(folder_path)
    pagerank = nx.pagerank(graph)
    return pagerank
```

## Reference Resources
- https://www.youtube.com/watch?v=ft0owvS5tQA  # top 6 coding interview questions
- https://youtu.be/guKRryH1yho?si=yXwZF6hGAf-7Wi1E  # Top 7 Algorithms for Coding Interviews
- https://www.youtube.com/@tryexponent/playlists
- https://seanprashad.com/leetcode-patterns/
- https://github.com/donnemartin/system-design-primer
- https://github.com/checkcheckzz/system-design-interview
- https://github.com/jwasham/coding-interview-university
- https://github.com/alex/what-happens-when
- https://github.com/prasadgujar/low-level-design-primer