---
layout: posts
title: python使用openpyxl库操作Excel
categories: python学习笔记
tags: 
    - python
    - openpyxl
---
## 一、加载表格
```
wb = openpyxl.load_workbook("abc.xlsx")#加载工作簿
ws = wb["Sheet2"]#加载工作表
```
## 二、读取单元格数值
```
ws["A1"].value#读取A1单元格的值
```
## 三、单元格填充颜色
```
fill = PatternFill(fill_type="solid", fgColor="54FF9F")
ws["A1"].fill = fill
wb.save("abc.xlsx")
```
注：操作一次就执行一次`save()`方法，否则可能只保留最后一次的操作
