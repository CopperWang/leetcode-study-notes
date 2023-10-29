# 2877. Create a DataFrame from List

### 题干

编写一个解决方案，从一个名为 `student_data` 的二维列表中创建一个 `DataFrame`。这个二维列表包含了一些学生的 ID 和年龄。

`DataFrame` 应该有两列，分别是 `student_id` 和 `age`，并且与原始的二维列表的顺序相同。

### 分析

这道题目要求从一个二维列表（`student_data`）创建一个DataFrame。这个二维列表包含学生的ID和年龄。

### 涉及的知识点和技能

1. **`pandas`库**：`panda` 是 `Python中` 的一个开源数据分析工具库，提供了大量数据结构和数据分析工具。其中，`DataFrame` 是 `pandas` 中一个重要的数据结构，它是一个二维标记数据结构，具有行索引和列索引，可以看作是一个 `Excel` 工作表。
2. **列表操作**：需要从2D列表中提取数据。
3. **创建 `DataFrame`**：使用 `pandas` 的 `DataFrame` 构造函数从列表创建 `DataFrame`。

### Python代码实现

```python
import pandas as pd

def createDataFrame(self, student_data: List[List[int]]) -> pd.DataFrame:
    # 从student_data提取数据，生成DataFrame
    df = pd.DataFrame(student_data, columns=['student_id', 'age'])
    return df
```

上述代码首先导入了pandas库，并使用别名pd。在`createDataFrame`方法中，我们使用`pd.DataFrame`构造函数从`student_data`创建一个DataFrame，并指定列名为'student_id'和'age'。最后，返回创建的DataFrame。

# 2878. Get the Size of a DataFrame

### 题干：
```
DataFrame players:
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| player_id   | int    |
| name        | object |
| age         | int    |
| position    | object |
| ...         | ...    |
+-------------+--------+
```
编写一个解决方案来计算并显示 `player` 的行数和列数。

将结果返回为一个数组：

[行数, 列数]

### 涉及的知识点和技能

1. **pandas库**：使用pandas库操作DataFrame。
2. **DataFrame属性**：`shape` 属性可以获取DataFrame的形状，返回的是一个元组，其中包含行数和列数。

Python代码实现：

```python
import pandas as pd

class Solution:
    def getShape(self, players: pd.DataFrame) -> List[int]:
        # 使用shape属性获取DataFrame的行数和列数
        rows, columns = players.shape
        return [rows, columns]
```

上述代码中，我们直接利用pandas DataFrame的 `shape` 属性来获取其形状（行数和列数）。`shape` 返回一个元组，其中第一个元素是行数，第二个元素是列数。我们直接将这两个元素放入一个列表中并返回。

# 2879. Display the First Three Rows
```
DataFrame: employees
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| employee_id | int    |
| name        | object |
| department  | object |
| salary      | int    |
+-------------+--------+
```

### 题干

编写一个解决方案来显示此 DataFrame 的前三行。

### 涉及的知识点和技能

1. **pandas库**：使用pandas库操作DataFrame。
2. **DataFrame切片**：可以使用行索引进行切片来获取DataFrame的部分行。

Python代码实现：

```python
import pandas as pd

class Solution:
    def displayFirstThreeRows(self, employees: pd.DataFrame) -> pd.DataFrame:
        # 使用切片获取前三行
        return employees.iloc[:3]
```

在上述代码中，我们使用了pandas DataFrame的 `iloc` 属性进行基于行号的索引。`iloc[:3]` 可以帮助我们获取前三行的数据。

# 2880. Select Data
### 题干
```
DataFrame students
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+

```

编写一个解决方案来选择 student_id 为 101 的学生的姓名和年龄。

### 涉及的知识点和技能

1. **pandas库**：使用pandas库操作DataFrame。
2. **条件选择**：使用条件选择从DataFrame中选择指定的行。
3. **列选择**：选择DataFrame的指定列。

Python代码实现：

```python
import pandas as pd

class Solution:
    def selectData(self, students: pd.DataFrame) -> pd.DataFrame:
        # 使用条件选择获取student_id为101的学生，并选择其name和age列
        return students[students['student_id'] == 101][['name', 'age']]
```

在上述代码中，我们首先使用条件选择来获取student_id为101的学生：`students['student_id'] == 101` 会返回一个布尔值Series，其中对应student_id为101的行的值为True，其他行的值为False。接下来，我们使用这个布尔值Series对DataFrame进行索引，以选择满足条件的行。最后，我们使用列名列表 `['name', 'age']` 来选择这两列。

# 2881. Create a New Column
```
DataFrame employees
+-------------+--------+
| Column Name | Type.  |
+-------------+--------+
| name        | object |
| salary      | int.   |
+-------------+--------+
```
### 题干

一家公司计划为其员工提供奖金。

编写一个解决方案，创建一个名为 "bonus" 的新列，该列包含薪资列的双倍值。


### 涉及的知识点和技能

1. **pandas库**：使用pandas库操作DataFrame。
2. **新增列**：向DataFrame添加新列并计算其值。
3. **列运算**：对DataFrame的某一列进行算术运算。

Python代码实现：

```python
import pandas as pd

class Solution:
    def createNewColumn(self, employees: pd.DataFrame) -> pd.DataFrame:
        # 为bonus列赋值，其值为salary列的两倍
        employees['bonus'] = employees['salary'] * 2
        return employees
```

在上述代码中，我们直接使用 `employees['salary'] * 2` 计算每个员工的奖金，然后将这些值赋给新的 `bonus` 列。这利用了pandas的向量化操作，可以一次性计算所有行的值，而不需要使用循环。

# 2882. Drop Duplicate Rows
### 题干
```
DataFrame customers
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| customer_id | int    |
| name        | object |
| email       | object |
+-------------+--------+
```

在基于 `email` 列的DataFrame中存在一些重复的行。

编写一个解决方案，以移除这些重复的行并仅保留**第一次**出现的行。

### 涉及的知识点和技能

1. **pandas库**：使用pandas库操作DataFrame。
2. **删除重复值**：使用 `drop_duplicates` 函数删除重复行。
3. **基于某列删除重复值**：使用 `drop_duplicates` 的 `subset` 参数基于特定列删除重复值。

### Python代码实现

```python
import pandas as pd

class Solution:
    def dropDuplicateRows(self, customers: pd.DataFrame) -> pd.DataFrame:
        # 基于email列删除重复行，仅保留第一次出现的行
        return customers.drop_duplicates(subset='email', keep='first')
```

在上述代码中，我们使用 `drop_duplicates` 函数和它的 `subset` 参数来指定我们希望基于哪一列删除重复行。`keep='first'` 参数意味着我们希望保留每个重复值的第一次出现，这也是默认行为，但为了清晰，我们在这里明确地指定了它。

# 2883. Drop Missing Data
### 题干
```
DataFrame students
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+
```

在 `name` 列中，有些行的值是缺失的。

编写一个解决方案，以移除那些含有缺失值的行。

### 涉及的知识点和技能

1. **pandas库**：使用pandas库操作DataFrame。
2. **处理缺失数据**：理解如何使用 `dropna` 函数删除具有缺失值的行。
3. **基于某列处理缺失数据**：使用 `dropna` 的 `subset` 参数基于特定列删除具有缺失值的行。

### Python代码实现

```python
import pandas as pd

class Solution:
    def dropMissingData(self, students: pd.DataFrame) -> pd.DataFrame:
        # 基于name列删除具有缺失值的行
        return students.dropna(subset=['name'])
```

在上述代码中，我们使用了 `dropna` 函数和它的 `subset` 参数来指定我们希望基于哪一列删除具有缺失值的行。这样，只有那些在 `name` 列中具有缺失值的行才会被删除，其他列的缺失值不会影响到结果。

# 2884. Modify Columns
### 题干
```
DataFrame employees
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| name        | object |
| salary      | int    |
+-------------+--------+
```
一家公司打算给其员工加薪。

编写一个解决方案，将“salary”列中的每个薪水乘以2来修改该列。

### 涉及的知识点和技能

1. **pandas库**：这是一个用于数据处理和分析的Python库，可以很容易地操作数据表（如DataFrame）。
2. **DataFrame的列操作**：知道如何选择和修改DataFrame的特定列。
3. **元素级操作**：理解如何在DataFrame的列上执行元素级的操作。

### Python代码实现

```python
import pandas as pd

class Solution:
    def modifyColumns(self, employees: pd.DataFrame) -> pd.DataFrame:
        # 将salary列的每个值乘以2
        employees['salary'] = employees['salary'] * 2
        return employees
```

在上述代码中，我们直接选择 `salary` 列并将其乘以2。这会将整列的每个值都乘以2。最后，返回已修改的 `employees` DataFrame。

# 2885. Rename Columns
### 题干
```
DataFrame students
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| id          | int    |
| first       | object |
| last        | object |
| age         | int    |
+-------------+--------+
```

编写一个解决方案，按照以下方式重命名列：

- 将 "id" 改为 "student_id"
- 将 "first" 改为 "first_name"
- 将 "last" 改为 "last_name"
- 将 "age" 改为 "age_in_years"

### 涉及的知识点和技能

1. **pandas库**：这是一个用于数据处理和分析的Python库。
2. **DataFrame列重命名**：理解如何使用pandas的rename方法来重命名DataFrame的列。

### Python代码实现

```python
import pandas as pd

def renameColumns(students: pd.DataFrame) -> pd.DataFrame:
    # 使用rename方法来重命名列
    students = students.rename(columns={
        "id": "student_id",
        "first": "first_name",
        "last": "last_name",
        "age": "age_in_years"
    })
    return students
```

在上述代码中，我们使用了DataFrame的 `rename` 方法，为其提供了一个字典，其中键是原始的列名，值是新的列名。这样，`rename` 方法会按照提供的映射关系对列进行重命名。然后，返回已重命名的 `students` DataFrame。

# 2886. Change Data Type
### 题干
```
DataFrame students
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
| grade       | float  |
+-------------+--------+
```

编写一个解决方案来纠正以下错误：

`grade` 列中的数据以浮点数形式存储，将其转换为整数。

这道题目要求我们将`students` DataFrame中的`grade`列从浮点数类型转换为整数类型。

### 涉及的知识点和技能

1. **pandas库**：这是一个用于数据处理和分析的Python库。
2. **DataFrame数据类型转换**：理解如何使用pandas的`astype`方法来更改DataFrame中某一列的数据类型。

### Python代码实现

```python
import pandas as pd

def changeDatatype(students: pd.DataFrame) -> pd.DataFrame:
    # 使用astype方法将grade列的数据类型转换为int
    students['grade'] = students['grade'].astype(int)
    return students
```

在上述代码中，我们使用了列的`astype`方法来将`grade`列的数据类型从浮点数转换为整数。然后返回已更改数据类型的`students` DataFrame。

# 2887. Fill Missing Data
### 题干
```
DataFrame products
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| name        | object |
| quantity    | int    |
| price       | int    |
+-------------+--------+
```

编写一个解决方案，用 `0` 填充 `quantity` 列中的缺失值。

### 涉及的知识点和技能

1. **pandas库**：这是一个用于数据处理和分析的Python库。
2. **DataFrame缺失值处理**：理解如何使用pandas的`fillna`方法来填充DataFrame中的缺失值。

### Python代码实现

```python
import pandas as pd

def fillMissingValues(products: pd.DataFrame) -> pd.DataFrame:
    # 使用fillna方法填充quantity列中的缺失值为0
    products['quantity'] = products['quantity'].fillna(0).astype(int)
    return products
```

在上述代码中，我们首先使用`fillna`方法将`quantity`列中的缺失值填充为0。接下来，为了确保数据类型的一致性，我们使用`astype`方法将其转换为整数类型。最后，我们返回已填充缺失值的`products` DataFrame。

# 2888. Reshape Data: Concatenate
### 题干
```
DataFrame df1
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+

DataFrame df2
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| student_id  | int    |
| name        | object |
| age         | int    |
+-------------+--------+
```
编写一个方案，将这两个DataFrame在**垂直方向**上拼接成一个DataFrame。

### 涉及的知识点和技能

1. **pandas库**：这是Python的一个数据处理和分析库。
2. **DataFrame的连接**：要知道如何使用pandas的`concat`方法垂直连接两个或多个DataFrame。

### Python代码实现

```python
import pandas as pd

def concatenateTables(df1: pd.DataFrame, df2: pd.DataFrame) -> pd.DataFrame:
    # 使用concat方法垂直连接两个DataFrame
    result = pd.concat([df1, df2], ignore_index=True)
    return result
```

在上述代码中，我们使用`concat`方法将`df1`和`df2`垂直连接。`ignore_index=True`参数的目的是重新生成索引，以确保结果DataFrame的索引是连续的。最后，我们返回连接后的`result` DataFrame。

# 2889. Reshape Data: Pivot
### 题干
```
DataFrame weather
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| city        | object |
| month       | object |
| temperature | int    |
+-------------+--------+
```
编写一个解决方案，将数据**透视**，使得每一行代表一个特定月份的温度，每个城市都是一个单独的列。

### 涉及的知识点和技能

1. **pandas库**：Python的一个数据处理和分析库。
2. **数据透视**：pandas的`pivot`方法可以帮助我们基于给定的值和索引来重塑DataFrame。

### Python代码实现

```python
import pandas as pd

def pivotTable(weather: pd.DataFrame) -> pd.DataFrame:
    # 使用pivot方法将数据透视，将月份设为行索引，城市设为列，温度为值
    result = weather.pivot(index='month', columns='city', values='temperature')
    return result
```

在上述代码中，我们使用`pivot`方法透视`weather` DataFrame。我们指定`month`作为行索引，`city`作为列，`temperature`作为值。这样，我们就得到了一个每行代表一个月份，每列代表一个城市的DataFrame。每个单元格的值是该城市在指定月份的温度。

# 2890. Reshape Data: Melt
### 题干
```
DataFrame report
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| product     | object |
| quarter_1   | int    |
| quarter_2   | int    |
| quarter_3   | int    |
| quarter_4   | int    |
+-------------+--------+
```
编写一个解决方案，以便**重塑**数据，使得每一行代表某一特定季度的产品销售数据。


```python
import pandas as pd

def meltTable(report: pd.DataFrame) -> pd.DataFrame:
    # 使用pandas的melt方法将report数据框从宽格式转换为长格式
    result = pd.melt(report, id_vars=['product'], value_vars=['quarter_1', 'quarter_2', 'quarter_3', 'quarter_4'], 
                     var_name='quarter', value_name='sales')
    
    # 使用 categorical 类型确保排序按照输入的季度和产品顺序
    result['quarter'] = pd.Categorical(result['quarter'], categories=['quarter_1', 'quarter_2', 'quarter_3', 'quarter_4'], ordered=True)
    result['product'] = pd.Categorical(result['product'], categories=report['product'].unique(), ordered=True)
    
    # 为了保持输出的顺序与题目描述相匹配，我们首先按照季度，然后按照产品进行排序
    result = result.sort_values(by=['quarter', 'product']).reset_index(drop=True)
    
    return result    
```

# 2891. Method Chaining
### 题干
```
DataFrame animals
+-------------+--------+
| Column Name | Type   |
+-------------+--------+
| name        | object |
| species     | object |
| age         | int    |
| weight      | int    |
+-------------+--------+
```
编写一个解决方案，列出体重**严格超过**`100`公斤的动物的名称。

按重量**降序**返回这些动物。

```python
import pandas as pd

def findHeavyAnimals(animals: pd.DataFrame) -> pd.DataFrame:
    return animals[animals['weight'] > 100].sort_values(by = 'weight', ascending = False)[['name']]
```

