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

# 2 python操作Jenkins调用

```python
# 1、初始化
jk = jenkins.Jenkins(url='Jenkins地址', username='用户名', password='授权令牌')

# 2、构建项目
jk.build_job(name='构建的job名称')

# 3、参数化构建项目
jk.build_job(name='构建的job名称', parameters='构建的参数，字典类型')

# 4、停止一个正在运行的项目
jk.stop_build('job名称', '构建编号ID')

# 5、激活项目状态为可构建
jk.enable_job('job名称')

# 6、变更项目状态为不可构建
jk.disable_job('job名称')

# 7、删除项目
jk.delete_job('job名称')

# 8、获取项目当前构建的最后一次编号
last_build_number = jk.get_job_info('job名称')['lastBuild']['number']

# 9、通过构建编号获取任务状态
status = jk.get_build_info('job名称', last_build_number)['result']
# 状态有4种：SUCCESS|FAILURE|ABORTED|pending

# 10、获取项目控制台日志
result = jk.get_build_console_output(name='job名称', number=last_build_number)

# 11、获取项目测试报告
result = jk.get_build_test_report(name='job名称', number=last_build_number)
```



# 3 Python操作Jira提交BUG

jira Python文档https://jira.readthedocs.io/en/latest/

安装：pip install jira

认证：Jira的访问是有权限的，在访问Jira项目时首先要进行认证，Jira Python库提供了3种认证方式：

> 通过Cookis方式认证（用户名，密码）
> 通过Basic Auth方式认证（用户名，密码）
> 通过OAuth方式认证

认证方式只需要选择一种即可，以下代码为使用Cookies方式认证。

```python
from jira import JIRA
jira=JIRA(server='http://jira.xxx.com/jira',auth=('账号','密码'))
```

返回的jira对象便可以对Jira进行操作：

### 项目（Project）

项目对象的主要属性及方法如下：

> key: 项目的Key
> name: 项目名称
> description: 项目描述
> lead: 项目负责人
> projectCategory: 项目分类
> components: 项目组件
> versions: 项目中的版本
> raw: 项目的原始API数据

```python
# 访问权限的项目列表
print(jira.projects())

# 项目key
print(jira.project("KB").key)

# 项目名称
print(jira.project("KB").name)

# 项目描述
print(jira.project("KB").description)

# 项目负责人
print(jira.project("KB").lead)

# 项目模块
print(jira.project("KB").components)

# 项目版本
print(jira.project("KB").versions)

# 项目的原始API数据
print(jira.project("KB").raw)
```

### 问题（Issue）

Issue是Jira的核心，Jira中的任务，用户Story，Bug实质上都是一个Issue。

单个问题对象可以通过jira.issue("问题的Key")得到，问题的主要属性和方法如下：

> id: 问题的id
> key: 问题的Key
> permalink(): 获取问题连接
> fields: 问题的描述，创建时间等所有的配置域
> raw: 问题的原始API数据

```python
#问题的id
print(jira.issue('KB-18900').id)

#问题的Key
print(jira.issue('KB-18900').key)

#问题的描述，创建时间等所有的配置域
print(jira.issue('KB-18900').permalink())

#问题的原始API数据
print(jira.issue('KB-18900').raw)
```

### 配置域（Fields）

一般问题的ields中的属性分为固定属性和自定义属性，自定义属性格式一般为类似customfield_10012这种。常用的问题的Fields有：

> assignee：经办人
> created: 创建时间
> creator: 创建人
> labels: 标签
> priority: 优先级
> progress:
> project: 所示项目
> reporter: 报告人
> status: 状态
> summary: 问题描述
> worklog: 活动日志
> updated: 更新时间
> watches: 关注者
> comments: 评论
> resolution: 解决方案
> subtasks: 子任务
> issuelinks: 连接问题
> lastViewed: 最近查看时间
> attachment

```python
#经办人
print(jira.issue('CB-18900').fields.assignee)

#创建人
print(jira.issue('CB-18900').fields.creator)

#报告人
print(jira.issue('CB-18900').fields.reporter)

#责任人
print(jira.issue('CB-18900').fields.customfield_10316)

#创建时间
print(jira.issue('CB-18900').fields.created)

#标签
print(jira.issue('CB-18900').fields.labels)

#优先级
print(jira.issue('CB-18900').fields.priority)

#问题类型
print(jira.issue('CB-18900').fields.issuetype)

#所示项目
print(jira.issue('CB-18900').fields.project)

#状态
print(jira.issue('CB-18900').fields.status)

#问题描述
print(jira.issue('CB-18900').fields.summary)

#活动日志
print(jira.issue('CB-18900').fields.worklog)

#更新时间
print(jira.issue('CB-18900').fields.updated)

#关注着
print(jira.issue('CB-18900').fields.watches)
```

### 关注者/评论/附件

> jira.watchers(): 问题的关注者
> jira.add_watcher(): 添加关注者
> jira.remove_watcher(): 移除关注者
> jira.comments(): 问题的所有评论
> jira.comment(): 某条评论
> jira.add_comment()：添加评论
> comment.update()/delete(): 更新/删除评论
> jira.add_attachment(): 添加附件

```python
issue = jira.issue('JRA-1330')

print(jiaa.watchers(issue)) # 所有关注者
jira.add_watcher(issue, 'username') # 添加关注者

print(jira.comments(issue)) # 所有评论
comment = jira.comment(issue, '10234') # 某条评论
jira.add_comment(issue, 'new comment') # 新增评论
comment.update(body='update comment') # 更新评论
comment.delete() # 删除该评论

print(issue.fields.attachment) # 问题附件
jira.add_attachment(issue=issue, attachment='/some/path/attachment.txt') # 添加附件

```
### 创建/分配/转换问题

> jira.create_issue(): 创建问题
> jira.create_issues(): 批量创建问题
> jira.assign_issue(): 分配问题
> jira.transitions(): 获取问题的工作流
> jira.transition_issue(): 转换问题

```python
# 创建问题
issue_dict = {
  'project': {'id': 123},
  'summary': 'New issue from jira-python',
  'description': 'Look into this one',
  'issuetype': {'name': 'Bug'},
}
new_issue = jira.create_issue(fields=issue_dict)

# 批量创建问题
issue_list = [
{
  'project': {'id': 123},
  'summary': 'First issue of many',
  'description': 'Look into this one',
  'issuetype': {'name': 'Bug'},
},
{
  'project': {'key': 'FOO'},
  'summary': 'Second issue',
  'description': 'Another one',
  'issuetype': {'name': 'Bug'},
},
{
  'project': {'name': 'Bar'},
  'summary': 'Last issue',
  'description': 'Final issue of batch.',
  'issuetype': {'name': 'Bug'},
}]
issues = jira.create_issues(field_list=issue_list)

# 分配问题
jira.assign_issue(issue, 'newassignee')

# 转换问题
jira.transition_issue(issue, '5', assignee={'name': 'pm_user'}, resolution={'id': '3'})
```
### 提交bug

```python
# 提交BUG
issue_dict = {
  'project': {'id': 10202},#项目id
  'summary': '测试',#BUG概要
  'description': '测试',#BUG详情
   'priority': {'name':'Low'},#bug优先级
  'assignee':{'name':'chengzi@x.com'},#分配人
  'customfield_10316':{'name':'chengzi@x.com'},#责任人
  'labels': ['大大项目'],#所属项目
  'issuetype': {'id': 10004}#问题类型-故障
}
new_issue = jira.create_issue(issue_dict)

```



```python

```



