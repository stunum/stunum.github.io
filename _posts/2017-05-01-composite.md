---
layout: post
title: python设计模式之组合模式
date: 2017-05-01 13:27:16
tags:  水滴石穿
---
### 组合模式

##### 意义：
将对象组合成树形结构以表示“部分-整体”的层次结构。C o m p o s i t e 使得用户对单个对象和组合对象的使用具有一致性。 
##### 适用性：
- 你想表示对象的部分-整体层次结构。
- 你希望用户忽略组合对象与单个对象的不同，用户将统一地使用组合结构中的所有对象。

##### 举例：
```
class ComponentBases(object):
    def __init__(self, name):
        slef.name = name

    def add(self, obj):
        pass

    def remove(self, obj):
        pass

    def display(self, number):
        pass


class Node(ComponentBases):

    def __init__(self, name, duty):
        self.name = name
        self.duty = duty
        self.children = []

    def add(self, obj):
        self.children.append(obj)

    def remove(self, obj):
        self.children.remove(obj)

    def display(self, number=1):
        print(f"部门：{self.name} 级别：{number} 职责：{self.duty}")
        n = number+1
        for obj in self.children:
            obj.display(n)


if __name__ == '__main__':
    root = Node("总经理办公室", "总负责人")
    node1 = Node("财务部门", "公司财务管理")
    root.add(node1)
    node2 = Node("业务部门", "销售产品")
    root.add(node2)
    node3 = Node("生产部门", "生产产品")
    root.add(node3)
    node4 = Node("销售事业一部门", "A产品销售")
    node2.add(node4)
    node5 = Node("销售事业二部门", "B产品销售")
    node2.add(node5)
    root.display()
```

输出结果：
```
部门：总经理办公室 级别：1 职责：总负责人
部门：财务部门 级别：2 职责：公司财务管理
部门：业务部门 级别：2 职责：销售产品
部门：销售事业一部门 级别：3 职责：A产品销售
部门：销售事业二部门 级别：3 职责：B产品销售
部门：生产部门 级别：2 职责：生产产品
```