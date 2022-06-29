# 1 python常用代码片段

### 字符串反转

```python
my_string = 'ABCDE'
reversed_string=my_string[::-1]
print(reversed_string)
```

### 首字母大写

```python
new_string = my_string.title()
print(new_string) 
```

### 在字符串中查找唯一元素

```python
my_string = "aavvccccddddeee"

# converting the string to a set
temp_set = set(my_string)

# stitching set into a string using join
new_string = ''.join(temp_set)

print(new_string)

```

### 列表推导式

```python
 # Multiplying each element in a list by 2

  original_list = [1,2,3,4]

  new_list = [2*x for x in original_list]

  print(new_list)
  # [2,4,6,8]

```

### 变量交换

```python
  a = 1
  b = 2

  a, b = b, a

  print(a) # 2
  print(b) # 1

```

### 将字符串拆分为子字符串列表

```python
  string_1 = "My name is Chaitanya Baweja"
  string_2 = "sample/ string 2"

  # default separator ' '
  print(string_1.split())
  # ['My', 'name', 'is', 'Chaitanya', 'Baweja']

  # defining separator as '/'
  print(string_2.split('/'))
  # ['sample', ' string 2']

```

### 将字符串列表组合成单个字符串

```python
  list_of_strings = ['My', 'name', 'is', 'Chaitanya', 'Baweja']

  # Using join with the comma separator
  print(','.join(list_of_strings))

  # Output
  # My,name,is,Chaitanya,Baweja

```

### 检查回文字符串

```python
  my_string = "abcba"

  if my_string == my_string[::-1]:
      print("palindrome")
  else:
      print("not palindrome")

  # Output
  # palindrom

```

### 列表中元素统计

这样做有多种方法，但是我最喜欢的是使用 Python Counter 类。
Python 计数器跟踪容器中每个元素的频率， Counter（）返回一个字典，其中元素作为键，频率作为值。
我们还使用 most_common（）函数来获取列表中的 most_frequent 元素。

```python
 # finding frequency of each element in a list
  from collections import Counter

  my_list = ['a','a','b','b','b','c','d','d','d','d','d']
  count = Counter(my_list) # defining a counter object

  print(count) # Of all elements
  # Counter({'d': 5, 'b': 3, 'a': 2, 'c': 1})

  print(count['b']) # of individual element
  # 3

  print(count.most_common(1)) # most frequent element
  # [('d', 5)]

```

### 使用枚举获取索引 / 值对

```python
 my_list = ['a', 'b', 'c', 'd', 'e']

 for index, value in enumerate(my_list):
      print('{0}: {1}'.format(index, value))

  # 0: a
  # 1: b
  # 2: c
  # 3: d
  # 4: e

```

### 合并两个字典

```python
 dict_1 = {'apple': 9, 'banana': 6}
 dict_2 = {'banana': 4, 'orange': 8}

  combined_dict = {**dict_1, **dict_2}

  print(combined_dict)
  # Output
  # {'apple': 9, 'banana': 4, 'orange': 8}

```

### 执行一短代码所需时间

```python
  import time

  start_time = time.time()
  # Code to check follows
  a, b = 1,2
  c = a+ b
  # Code to check ends
  end_time = time.time()
  time_taken_in_micro = (end_time- start_time)*(10**6)

  print(" Time taken in micro_seconds: {0} ms").format(time_taken_in_micro)

```

### 展平列表清单

```python
from iteration_utilities import deepflatten

  # if you only have one depth nested_list, use this
  def flatten(l):
    return [item for sublist in l for item in sublist]

  l = [[1,2,3],[3]]
  print(flatten(l))
  # [1, 2, 3, 3]

  # if you don't know how deep the list is nested
  l = [[1,2,3],[4,[5],[6,7]],[8,[9,[10]]]]

  print(list(deepflatten(l, depth=3)))
  # [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

```

### 从列表中随机取样

```python
 import random

  my_list = ['a', 'b', 'c', 'd', 'e']
  num_samples = 2

  samples = random.sample(my_list,num_samples)
  print(samples)
  # [ 'a', 'e'] this will have any 2 random values


  import secrets                            # imports secure module.
  secure_random = secrets.SystemRandom()    # creates a secure random object.

  my_list = ['a','b','c','d','e']
  num_samples = 2

  samples = secure_random.sample(my_list, num_samples)

  print(samples)
  # [ 'e', 'd'] this will have any 2 random values

```

### 数字化

以下代码段会将整数转换为数字列表。

```python
  num = 123456

  # using map
  list_of_digits = list(map(int, str(num)))

  print(list_of_digits)
  # [1, 2, 3, 4, 5, 6]

  # using list comprehension
  list_of_digits = [int(x) for x in str(num)]

  print(list_of_digits)
  # [1, 2, 3, 4, 5, 6]


```

### 检查唯一性

以下函数将检查列表中的所有元素是否唯一。

```python
  def unique(l):
      if len(l)==len(set(l)):
          print("All elements are unique")
      else:
          print("List has duplicates")

  unique([1,2,3,4])
  # All elements are unique

  unique([1,1,2,3])
  # List has duplicates

```















```python

```



