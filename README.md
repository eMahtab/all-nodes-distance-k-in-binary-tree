# All nodes distance k in binary tree
All nodes distance k in a binary tree

## https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree

Given the root of a binary tree, the value of a target node target, and an integer k, return an array of the values of all nodes that have a distance k from the target node.

**You can return the answer in any order.**

![All nodes distance k in binary tree](all-nodes-distance-k-in-binary-tree.jpg?raw=true)

## Approach : Create graph from the given tree and BFS
We will first create a graph from the given binary tree.
Once we have the graph, all we need to do is, to find all the nodes which have distance k from the targetNode using BFS (start BFS from targetNode).

# Implementation :
```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    TreeNode sourceNode;
    public List<Integer> distanceK(TreeNode root, TreeNode target, int k) {
        List<Integer> result = new ArrayList<>();
        if(root == null)
            return result;
        Map<Integer,List<TreeNode>> graph = new HashMap<>();
        // Create a graph representation from given binary tree
        dfs(root, graph, null, target);
        // Next do BFS traversal to find all the nodes distance k from target node
        Queue<TreeNode> q = new ArrayDeque<>();
        Set<Integer> visited = new HashSet<>();
        // Start the BFS from given target node
        q.add(sourceNode);
        visited.add(sourceNode.val);
        int distance = 0;
        while(!q.isEmpty() && distance <= k) {
            int size = q.size();
            for(int i = 0; i < size; i++) {
                TreeNode node = q.remove();
                if(distance == k)
                    result.add(node.val);
                List<TreeNode> neighbors = graph.get(node.val);
                // add neighbors to the queue
                for(TreeNode neighbor : neighbors) {
                    if(!visited.contains(neighbor.val)) {
                        q.add(neighbor);
                        visited.add(neighbor.val);
                    }  
                }
            }
            distance++;
        }
        return result;
    }
    
    private void dfs(TreeNode node, Map<Integer, List<TreeNode>> graph, TreeNode parent, TreeNode target) {
        if(node == null)
            return;
        if(node.val == target.val)
            sourceNode = node;
        graph.putIfAbsent(node.val, new ArrayList<TreeNode>());
        List<TreeNode> neighbors = graph.get(node.val);
        if(node.left != null)
            neighbors.add(node.left);
        if(node.right != null)
            neighbors.add(node.right);
        if(parent != null) {
            neighbors.add(parent);
        }
        dfs(node.left, graph, node, target);
        dfs(node.right, graph, node, target);
    }
}
```

## Complexity Analysis :
```
Time Complexity: O(N) where N is the number of nodes in the given input tree.

Space Complexity: O(N), the size of the graph.
```

# References :
https://github.com/eMahtab/closest-leaf-in-a-binary-tree

# Similar Question :
Closest leaf in a binary tree https://leetcode.com/problems/closest-leaf-in-a-binary-tree
