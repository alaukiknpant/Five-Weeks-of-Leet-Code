## Concurrent Algorithms in Scala

# [Peterson Lock in a tree](https://ilyasergey.net/YSC3248/_static/resources/programming-02-mex.pdf)

  
  
  To solve this question, we first calculate the depth of a tree required for ```n``` threads trying to access locks, which is given by ```log_2(n)```. 
  We then create an array representation of the tree of locks. The size of the array is going to be within the bounds of ```2^Depth```. Note that this array can 
  be used to represent a perfect Binary Tree, in which all the internal nodes have two children and all leaf nodes are at the same level.
  
  After creating an array representation of the tree, we work on doing a binary encoding of the leaf nodes as shown in the following image.
  
![alt text][logo]

[logo]: https://www.cs.princeton.edu/courses/archive/spr01/cs126/assignments/prefix.gif "Logo Title Text 2"

Note that in the above figure, if the path is ```1010```, we go to the right child of the root(1) and then the left child(0) of the next node accesed and then the right child and then the left child.


### Locking

Locking requires us to do a binary encoding of the thread T(id between 0 and n-1 where n is the number of threads) trying  acquire the lock. `000` would refer to the left most node, for example, in a tree of depth three. Then we calculate the index in the array of the thread under consideration using its binary encoding using the ```getInitIndex``` function. We lock the ```PetersonNode``` in this index.  Once we find the index of leaf node, we access its parents each time using the formula given in the code and unlock the ```PetersonNode``` in the index of the parent. The way to find the index of a node is inspired by the way we find indices in heaps. We keep accesing the parent and lock it until we reach the root node.

### Unlocking

Unlocking works in a similar fashion. We find the binary encoding of the thread under consideration and keep unlocking the nodes that are in the path from the root to the leaf node.  Note that while unlocking, we unlock starting from the root (we start locking from the leaf node).


### Reason this code is deadlock free, starvtion free and mutually exclusive

We know Peterson lock satisfies all these conditions. To ensure the entire tree of Peterson locks follows these condition, we need to make sure that no more than  2 node whose IDs are encoded as 1 and 0 can access one PetersonNode. This is ensured by the binary encoding of the paths.

