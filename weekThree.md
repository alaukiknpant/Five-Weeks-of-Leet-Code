## Week 3 - Non-Linear Data Structures#

<p>The focus of week 3 is on non-linear data structures 
like trees, graphs and heaps. You should be familiar 
with the various tree traversal (in-order, pre-order, 
post-order) algorithms and graph traversal algorithms such
as breadth-first search and depth-first search. In my experience,
using more advanced graph algorithms (Dijkstra's and Floyd-Warshall)
is quite rare and usually not necessary.</p>

- [Validate Binary Search Tree](https://leetcode.com/problems/validate-binary-search-tree/)
    - Recursive Solution
        ```python
        class Solution:
          def isValidBST(self, root: TreeNode) -> bool:
            
            def helper(node, lower= float('-inf'), upper = float('inf')):
                
                if not node:
                    return True
                
                val = node.val
                
                if (val >= upper or val <= lower):
                    return False
                
                if not helper(node.right, val, upper):
                    return False
                if not helper(node.left, lower, val):
                    return False
                return True
            return(helper(root))
        ```
     - DFS Solution
         ```python
            class Solution:
                def isValidBST(self, root: TreeNode) -> bool:
                    
                    if not root:
                        return True
                
                    stack = [(root, float('-inf'), float('inf'))]
                
                    while stack:
                        node, lower, higher = stack.pop()
                        
                        if not node:
                            continue
                        
                        val = node.val
                        
                        if val >= higher or val <= lower:
                            return False
                        
                        stack.append((node.left, lower, val))
                        
                        stack.append((node.right, val, higher))
                                      
                    return True
        ```
- [Invert/Flip Binary Tree]()
   - Recursive
       ```python
          class Solution:
            def invertTree(self, root: TreeNode) -> TreeNode:
                def invert(node):
                    if not node:
                        return None      
                    node.left, node.right = invert(node.right), invert(node.left)
                    return node   
                return invert(root) 
        ```
   
   - [Iterative BFS](https://leetcode.com/problems/invert-binary-tree/submissions/)
   
       ```python
          class Solution:
            def invertTree(self, root: TreeNode) -> TreeNode:
                if not root:
                    return None        
                queue = collections.deque()
                queue.append(root)
                while queue:
                    node = queue.popleft()      
                    if not node:
                        continue
                    left = node.left
                    right = node.right
                    node.left, node.right = right, left
                    queue.append(left)
                    queue.append(right)    
                return root
        ```
- [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

    ```python
        class Solution:
            def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
                if not intervals:
                    return 0
                intervals = sorted(intervals, key = operator.itemgetter(1))
                print(intervals)
                last = intervals[0]
                picked = 0
                for i in intervals[1:]:
                    if i[0] < last[1]:
                        picked += 1
                    else:
                        last = i
                return picked
    ```
        
       
    - Strategy: Sort by ending interval
       
- [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/submissions/)

    ```python
        class Codec:
        
            def serialize(self, root):
                if not root:
                    return "null"
                data = root.val
                
                return "" + str(data) + ","  + str(self.serialize(root.left)) + "," + str(self.serialize(root.right))
            
            
            def deserialize(self, data):
                
                if data == "null":
                    return None
                dataList = data.split(",")
                   
                queue = collections.deque(dataList)
               
                def helper(queue):
                    ele = queue.popleft()
                    if ele == 'null':
                        return None
                    root = TreeNode(ele)
                    root.left = helper(queue)
                    root.right = helper(queue)
                    return root
                
                return helper(queue)
    ```
- [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)
    ```python
    class Solution:
        def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
            node = None
            if inorder: 
                val = preorder.pop(0)
                node = TreeNode(val)
                idx = inorder.index(val)
                node.left = self.buildTree(preorder, inorder[:idx])
                node.right = self.buildTree(preorder, inorder[idx+1:])
            return node
    ```
- [Top K Frequent Elements]()
    ```python
    class Solution:
        def topKFrequent(self, nums: List[int], k: int) -> List[int]:
            FrequencyMap = collections.Counter(nums)
            
            elememtList = []
            for key in FrequencyMap:
                elememtList.append((-FrequencyMap[key], key))
            heapq.heapify(elememtList)
            ans = []
            for i in range(k):
                ans.append(heapq.heappop(elememtList)[1])
            return ans
    ```
- [Clone Graph](https://leetcode.com/problems/clone-graph/submissions/)
    ```python
    class Solution:
        def cloneGraph(self, node: 'Node') -> 'Node':
            
            if not node:
                return
            
            queue = collections.deque()
            val = node.val
            deepCopy = Node(val)
            dic = {node: deepCopy}
            
            queue.append(node)
            while queue:
                curNode = queue.pop()
                for neighbour in curNode.neighbors:
                    if neighbour not in dic:
                        val = neighbour.val
                        deepCopyNeighbour = Node(val)
                        dic[neighbour] = deepCopyNeighbour
                        dic[curNode].neighbors.append(deepCopyNeighbour)
                        queue.append(neighbour)
                    else:
                        dic[curNode].neighbors.append(dic[neighbour])
            return deepCopy
    ```
- [Course Schedule](https://yangshun.github.io/tech-interview-handbook/best-practice-questions/)
    ```python
    class Solution:
        def canFinish(self, numCourses: int, prerequisites: List[List[int]]) -> bool:
            
            preMap = dict()
            for course, prereq in prerequisites:
                preMap[course] = preMap.get(course, []) + [prereq]
            visited = set()
            
            
            def hasLoop(course, courseSet):
                boole = True
                if course in visited or course not in preMap:
                    return True
                if course in courseSet:
                    return False
                preReq = preMap[course]
                for pre in preReq:
                    boole = boole and hasLoop(pre, courseSet|{course})
                return boole
            
            for i in range(numCourses):
                if not hasLoop(i, set()):
                    return False
                visited.add(i)
            return True
    ```
- [Binary Tree Maximum Path Sum](https://leetcode.com/problems/binary-tree-maximum-path-sum/submissions/)
    ```python
    class Solution:
        def maxPathSum(self, root: TreeNode) -> int:
            self.maxi = float('-inf')
            def dfs(root):
                if not root:
                    return 0
                left =max(0, dfs(root.left))
                right = max(0, dfs(root.right))

                #Remove the line below to calculate max sum from root
                self.maxi = max(self.maxi, left + right + root.val)
                
                return max(left, right) + root.val
            dfs(root)
            return self.maxi
    ```
    
- [Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)
    ```python
    class Solution(object):
        def maxDepth(self, root):
            def maxi(root):
                if not root:
                    return 0
                left = 1 + maxi(root.left)
                right = 1 + maxi(root.right)
                return max(left, right)
            return maxi(root)
     ```
