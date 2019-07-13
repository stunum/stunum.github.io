---
layout: post
title: python广度优先算法
date: 2018-01-25 23:06:51
tags:  水滴石穿
---
![](https://media.stunum.com/2ctree.png)


```
#节点类
class TreeNode(object):
    def __init__(self, root):
        self._root = root
        self._left=None
        self._right=None

    @property
    def rootnode(self):
        return self._root

    @property
    def leftnode(self):
        if hasattr(self, '_left'):
            return self._left
        return None

    @property
    def leftnodeval(self):
        if hasattr(self, '_left'):
            return self._left.rootnode
        return None

    @leftnode.setter
    def leftnode(self, lnode):
        self._left = lnode

    @property
    def rightnode(self):
        if hasattr(self, '_right'):
            return self._right
        return None

    @property
    def rightnodeval(self):
        if hasattr(self, '_right'):
            return self._right.rootnode
        return None

    @rightnode.setter
    def rightnode(self, rnode):
        self._right = rnode

```
广度优先算法
```
def breadthfirstsearch(node):
    rlist=[]
    llist=[]
    llist.append(node)
    def bfs():
        bt=llist.pop()



```