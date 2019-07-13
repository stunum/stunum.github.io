---
layout: post
title: python数据结构之二叉树
date: 2018-02-03 12:52:01
tags:  水滴石穿
---

python实现二叉树的数据结构
```
class TreeNode(object):
    def __init__(self, root):
        self._root = root

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

>>> n1 = TreeNode(5)
>>> n2 = TreeNode(4)
>>> n3 = TreeNode(6)
>>> n1.leftnode(n2)
>>> n1.rightnode(n3)
>>> n1.leftnodeval
4
>>> n1.rightnodeval
6
```