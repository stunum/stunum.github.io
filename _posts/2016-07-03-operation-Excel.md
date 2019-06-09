---
layout: post
title: python操作Excel
date: 2016-07-03 15:43:38
tags:  水滴石穿
---

### 读Excel
###### 整个文件读取
```
# -*- coding: utf-8 -*- 
import sys  
reload(sys)  
sys.setdefaultencoding('utf8')  #python2对中文支持不是很友好，这样可以在py文件中写中文了
import xlrd
def read_file(file_path):
    book = xlrd.open_workbook(file_path) #得到 Excel 文件的 book 对象，实例化对象
    sheet = book.sheet_by_index(0) # 通过 sheet 索引获得 sheet 对象
    ncols = sheet.ncols # 列数
    nrows = sheet.nrows # 行数
    print("row count:{:d}, column count:{:d}".format(nrows, ncols))
    for row in range(nrows):
        print(sheet.row_values(row))
if __name__ == '__main__':
    read_file("test.xls")
```
###### 遍历cell读取
```
# -*- coding: utf-8 -*- 
from __future__ import print_function
import sys  
reload(sys)  
sys.setdefaultencoding('utf8')  
import xlrd
def read_file(file_path):
    book = xlrd.open_workbook(file_path)
    sheet = book.sheet_by_index(0)
    ncols = sheet.ncols
    nrows = sheet.nrows
    for row in range(nrows):
        for col in range(ncols):
            print(sheet.cell_value(row, col), end='')
            print('\t', end='')
        print("")
if __name__ == '__main__':
    read_file("test.xls")
```
### 写Excel
```
# -*- coding: utf-8 -*- 
import sys  
reload(sys)  
sys.setdefaultencoding('utf8')  
import xlwt
def get_data():
    data = []
    data.append(["id", "value"])
    for i in range(10):
        col_data = []
        col_data.append(i)
        col_data.append(i * 10)
        data.append(col_data)
    return data
def write_file(file_path, sheet_name, data):
    book = xlwt.Workbook(encoding = 'utf-8')
    sheet = book.add_sheet(sheet_name)
    row = 0
    for row_data in data:
        col = 0
        for cell_data in row_data:
            sheet.write(row, col, cell_data)
            col += 1
        row += 1
    book.save(file_path)
if __name__ == '__main__':
    data = get_data()
    write_file("test.xls", "test sheet", data)

```
