## Итератор и итерируемый объект

Позволяет только 1 раз пройти по списку. 
```python
lst = [value for value in range(10, 0, -1)] # [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]

it = iter(lst) # Итератор
next(it) # 10
next(it) # 9
# ...
next(it) # StopIteration
```

Работает со всеми типами данных.
```python
s = 'Python'

it = iter(s)
next(it) # 'P'
next(it) # 'y'
# ...
next(it) # StopIteration
```

Для `range`
```python
r = range(10)
type(r) # <class 'range'>

it = iter(r)
next(it) # 0
```

## Comprehension

List comprehension (генераторы списков) в Python — это удобный и лаконичный способ создания списков на основе других итерируемых объектов. Этот инструмент позволяет сократить код, сделать его более читаемым и повысить производительность.

```python
n = 10
lst = [value for value in range(n)]

lst = ['value'] * 10 # ['value', 'value', 'value', 'value', 'value', 'value', 'value', 'value', 'value', 'value']

str_lst = [word for word in 'Python'] # ['P', 'y', 't', 'h', 'o', 'n']

lst = [value in range(-10, 10) if abs(value) % 2 == 1] # [-9, -7, -5, -3, -1, 1, 3, 5, 7, 9]

lst = ['Нечетное' if value % 2 == 1 else 'Четное' for value in range(-3, 4)] 
# ['Нечетное', 'Четное', 'Нечетное', 'Четное', 'Нечетное', 'Четное', 'Нечетное']

lst = ['Нечетное' if value % 2 == 1 else 'Четное' for value in range(-3, 4) if value > 0]
# ['Нечетное', 'Четное', 'Нечетное']
```
##### Вложенные comprehensions
```python
n = 3
lst = [(i, j) for i in range(n) for j in range(n)]
# (0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]

matrix = [[0, 1, 2, 3], [10, 11, 12, 13], [20, 21, 22, 23]]

lst = [value for row in matrix for value in row]
# [0, 1, 2, 3, 10, 11, 12, 13, 20, 21, 22, 23]

m, n = 3, 4
matrix = [[a for a in range(m)] for b in range(n)]
# [[0, 1, 2], [0, 1, 2], [0, 1, 2], [0, 1, 2]]

# Транспонирование матрицы A
A = [[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]]
A = [[row[i] for row in A] for i in range(len(A[0]))]
# [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]

[y / 2 for y in [x ** 2 for x in range(5)]]
# [0.0, 0.5, 2.0, 4.5, 8.0]
```
