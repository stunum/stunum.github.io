---
layout: post
title: python数据结构之单链
date: 2018-02-04 19:20:51
tags:  水滴石穿
---
python实现单链的数据结构

```
class SingleChain(object):
    def __init__(self):
        self._header = None

    def add(self, data):
        newnode = Node(data)
        if self._header == None:
            self._header = newnode
        else:
            node = self._header
            print(node.next)
            while node.next != None:
                node = node.next
            node.next = newnode

    def length(self):
        if self._header == None:
            return 0
        node = self._header
        le = 1
        while node.next != None:
            le += 1
            node = node.next
        return le

    def get(self, index):
        leth = self.length()
        if index > 0 and index <= leth - 1:
            le = 0
            node = self._header
            if index == 0:
                return node.data
            while 1:
                le += 1
                if node.next != None:
                    node = node.next
                    if index == le:
                        return node.data
                else:
                    break
        elif index < 0 and abs(index) <= leth:
            le = 0
            node = self._header
            leth = self.length()
            if index == -leth:
                return node.data
            while 1:
                le += 1
                inx = le - leth
                if node.next != None:
                    node = node.next
                    if index == inx:
                        return node.data
                else:
                    break

    def pop(self):
        if self._header == None:
            print('list is empty!')
            return False

        nnode = self._header
        anode = nnode
        while 1:
            if nnode.next == None and nnode is anode:
                self._header = None
                return nnode.data
            elif nnode.next != None:
                anode = nnode.next
                if anode.next == None:
                    nnode.next = None
                    return anode.data
                nnode = anode.next
        return 0

if __name__=="__main__":
    sc = SingleChain()
    sc.add(1)
    sc.add(3)
    sc.add(5)
    sc.add(7)
    print(sc.length())
    print(sc.get(2))
    print(sc.pop())
    print(sc.length())

>>> 4
>>> 5
>>> 7
>>> 3
```



