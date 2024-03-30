<center>pyexcel文档操作</center>





[toc]







## pyexcel文档操作

> pyexcel: [pyexcel](https://github.com/pyexcel/pyexcel)用于读取、操作和写入 csv、ods、xls、xlsx 和 xlsm 文件中的数据的单一 API





### 1. 安装

> 官网：[pyexcel](https://docs.pyexcel.org/en/latest/)

```shell
pip install pyexcel

# 依赖
pip install pyexcel-xls
```









### 2. 使用

> * 存入数据到表格
>
> blog: [知乎](https://zhuanlan.zhihu.com/p/686434634)

```python
import pyexcel

a_list_of_dictionaries = [
    {
        "Name": 'Adam',
        "Age": 28
    },
    {
        "Name": 'Beatrice',
        "Age": 29
    },
    {
        "Name": 'Ceri',
        "Age": 30
    },
    {
        "Name": 'Dean',
        "Age": 26
    }
]


pyexcel.save_as(records=a_list_of_dictionaries, dest_file_name="aa.xls")
```



> * 获取表格数据

```python
import pyexcel as p

records =p.iget_records(file_name="aa.xls")

for record in records:
    print("%s is aged at %d" % (record['Name'], record['Age']))

p.free_resources()
```







### 4. 高级功能

> * 自定义数据格式
>
> PyExcel 允许用户自定义 Excel 数据的格式，包括样式、字体、颜色等。

```python
import pyexcel

data = [['Name', 'Age', 'Gender'],
        ['Alice', 30, 'Female'],
        ['Bob', 25, 'Male']]
settings = {
    'font': {'bold': True},
    'color': {'foreground': 'blue'}
}
pyexcel.save_as(array=data, dest_file_name='example.xlsx', **settings
```





> * 大数据处理
>
> PyExcel 提供了针对大型数据集的优化处理方法，使得处理大数据集的速度更快更高效。

```python
import pyexcel

data = pyexcel.get_sheet(file_name='aa.xls', use_iterators=True)
for row in data:
    print(row)
```

> * 数据格式转换
>
>   PyExcel 允许用户对 Excel 文件中的数据进行格式转换，如将日期格式转换为字符串格式、将数字格式转换为文本格式等。

```python
import pyexcel

data = pyexcel.get_sheet(file_name='example.xlsx')
data.format(int, 'Age')
data.save_as('formatted_data.xlsx')
```

