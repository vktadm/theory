```python
list = [3, 5, 8, -1, 5, 8, 10, 2]

sort_list = sorted(iterable, *, key=None, reverse=False)
list.sort(*, key=None, reverse=False) # сортировка на месте
```
Параметр `key` позволяет вам указать функцию, которая будет применена к каждому элементу итерируемого объекта перед сортировкой.
```python
# Сортировка списка строк по их длине
words = ["apple", "banana", "cherry", "date"]
sorted_words = sorted(words, key=len)
# >> ['date', 'apple', 'banana', 'cherry']

# Сортировка по абсолютному значению
numbers = [-10, 1, -5, 2, -3]
sorted_numbers = sorted(numbers, key=abs)
# >> [1, 2, -3, -5, -10]

# Сортировка списка словарей (JSON)
students = [ {"name": "John", "age": 25},
			{"name": "Jane", "age": 22},
			{"name": "Dave", "age": 23} ]
sorted_students = sorted(students, key=lambda student: student["age"])

# Сортировка списка кортежей
tuples = [(1, 'one'), (3, 'three'), (2, 'two')]
sorted_tuples = sorted(tuples, key=lambda x: x[1])

# Сортировка без учета регистра
words = ["Apple", "banana", "Cherry", "date"]
sorted_words = sorted(words, key=str.lower)

# Проверка на None
data = [None, "apple", "banana"]
sorted_data = sorted(data, key=lambda x: (x is None, x))

# Приведение к одному типу
data = [1, "apple", 3]
sorted_data = sorted(data, key=str)

# Сортировка сложных данных
data = [ {"name": "John", "details": {"age": 25}},
		{"name": "Jane", "details": {"age": 22}},
		{"name": "Dave", "details": {"age": 23}} ]
sorted_data = sorted(data, key=lambda x: x["details"]["age"])

# Сортировка с учетом нескольких критериев
students = [ {"name": "John", "age": 25},
			{"name": "Jane", "age": 22},
			{"name": "Dave", "age": 23},
			{"name": "Dave", "age": 22} ]
sorted_students = sorted(students, 
						 key=lambda student: (student["age"], student["name"]))
```