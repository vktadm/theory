`lst.sort(reverse, )`

## list

```python
cities = [ "Москва", "Санкт-Петербург", "Новосибирск", "Екатеринбург", "Казань", "Нижний Новгород", "Челябинск", "Омск", "Ростов-на-Дону", "Уфа" ]

sorted(cities)
# return ['Екатеринбург', 'Казань', 'Москва', 'Нижний Новгород', 'Новосибирск', 'Омск', 'Ростов-на-Дону', 'Санкт-Петербург', 'Уфа', 'Челябинск']

cities.sort()
# ['Екатеринбург', 'Казань', 'Москва', 'Нижний Новгород', 'Новосибирск', 'Омск', 'Ростов-на-Дону', 'Санкт-Петербург', 'Уфа', 'Челябинск']
```

# tuple, str

```python
rivers = ("Волга", "Енисей", "Лена", "Амур", "Обь", "Кама", "Дон", "Томь", "Печора", "Сура")

sort_rivers = sorted(rivers)
# ['Амур', 'Волга', 'Дон', 'Енисей', 'Кама', 'Лена', 'Обь', 'Печора', 'Сура', 'Томь']

sorted('python') # ['h', 'n', 'o', 'p', 't', 'y']
```

## dict

```python
dictionary = {
			  "hello": "привет",
			  "world": "мир",
			  "python": "питон",
			  "computer": "компьютер",
			  "programming": "программирование"
			  }


sorted(dictionary) # ['computer', 'hello', 'programming', 'python', 'world']
sorted(dictionary.values()) # ['компьютер', 'мир', 'питон', 'привет', 'программирование']

sorted(dictionary.items()) # сортировка все-равно по ключу
# [('computer', 'компьютер'), ('hello', 'привет'), ('programming', 'программирование'), ('python', 'питон'), ('world', 'мир')]

dict(sorted(dictionary.items()))
# {'computer': 'компьютер', 'hello': 'привет', 'programming': 'программирование', 'python': 'питон', 'world': 'мир'}
```

## key

```python
def id_even(num):  
    return num % 2  
  
  
lst = [4, 5, -7, -34, 10, 2, -8, -9, 3]  
# sort_lst = sorted(lst, key=id_even)  
sort_lst = sorted(lst, key=lambda num: num % 2)  
# [4, -34, 10, 2, -8, 5, -7, -9, 3]

cities = [ "Москва", "Санкт-Петербург", "Новосибирск", "Екатеринбург", "Казань", "Нижний Новгород", "Челябинск", "Омск", "Ростов-на-Дону", "Уфа" ]

sort_lst = sorted(cities, key=len)
# ['Уфа', 'Омск', 'Москва', 'Казань', 'Челябинск', 'Новосибирск', 'Екатеринбург', 'Ростов-на-Дону', 'Санкт-Петербург', 'Нижний Новгород']

sort_lst = sorted(cities, key=lambda x: x[-1])
# ['Москва', 'Уфа', 'Санкт-Петербург', 'Екатеринбург', 'Нижний Новгород', 'Новосибирск', 'Челябинск', 'Омск', 'Ростов-на-Дону', 'Казань']
```

Сортировка по значению в кортеже.

```python
books = [("Мастер и Маргарита", "Михаил Булгаков", 550), ("1984", "Джордж Оруэлл", 499), ("Гордость и предубеждение", "Джейн Остин", 620), ("Убить пересмешника", "Харпер Ли", 580), ("Преступление и наказание", "Фёдор Достоевский", 650)]

sort_books = sorted(books, key=lambda item: item[-1])
# [('1984', 'Джордж Оруэлл', 499), ('Мастер и Маргарита', 'Михаил Булгаков', 550), ('Убить пересмешника', 'Харпер Ли', 580), ('Гордость и предубеждение', 'Джейн Остин', 620), ('Преступление и наказание', 'Фёдор Достоевский', 650)]
```

Отсортировать по шаблону.

```python
notes = 'доремифасольляси'  
  
# lst_in = input().split()  
lst_in = 'до фа соль до ре фа ля си'.split()  
  
print(*sorted(lst_in, key=notes.find))
# до до ре фа фа соль ля си
```

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