Список - упорядоченная коллекция. Мутабельный тип данных.
```python
lst = []
lst = list()

new_lst = lst[:] # Копия списка, разные id
lst = list([True, False]) # Создаст новый список, независимый от изночального
lst = [True, False]
new_lst = list(lst)

list('python') # ['p', 'y', 't', 'h', 'o', 'n']

```
##### Объединение списков
```python
genres = ['Драмма', 'Ужасы', 'Триллер', 'Комедия']
new_genres = ['Мультфильмы', 'Аниме']

# merge с дубликатами
full_genres = genres + new_genres
full2_genres = [*genres, *new_genres]
genres.extend(new_genres)

# remove дубликатов
full_genres = set(full_genres)
```
##### Смена регистра, удаление пропусков
```python
genres[0].title()
genres[1].upper()
genres[2].lower())

genres[0].rstrip()
genres[1].lstrip()
genres[2].strip()

message = f'Мой любимый жанр фильмов {genres[3].lower()}!'
```
##### Добавление элемента `.append(), .insert()`
```python
genres.append('Мюзикл') # Добавление элемента в конец списка
genres.insert(1, 'Мелодрамма') # Добовление элемента в определеннную позицию списка
```
##### Удаление элемента `del name_list[index], .pop(), .remove()`
```python
del genres[3] # Удаление элемента с определенным индексом
genres.clear() # Очистка всего списка

popped_genres = genres.pop() # Удаление последнего элемента с возможностью сохранить значени 
popped_genres1 = genres.pop(0)

genres.remove('Ужасы') # Удаление по значению (только первое вхождение)
lst = [1, 'string', 0, False, 23, True]
lst.remove(True) # Удалит 1, тк приведет "1" к bool
lst.remove(False) # Удалит 0
lst.remove('no exists') # ValueError: list.remove(x): x not in list
```
##### Сортировка `.sort(obj, key, reverse), sorted(obj, key, reverse), reverse()`

**reverse** ‒ если True, отсортированный список переворачивается (или сортируется в порядке убывания). 
**key** ‒ функция, которая служит ключом для сравнения сортировок.  


```python
firs_names = ['Катя', 'Маша', 'Ваня', 'Кирилл', 'Анна']

firs_names.sort() # Сортировка по алфавиту
firs_names.sort(reverse=True) # Сортировка в обратном порядке

new_sorted_list = sorted(firs_names) # Сортировка без изменения оригинального списка

cheer_clubs = ['wave', 'no limit', 'zoom', 'abceer', 'victory'] cheer_clubs.reverse() # Изменения порядка на обратный, без сортировки

# key
numbers = [12, 123, 1234, 1, 12345, 11, 111, 22222]
string_list = list(map(str, numbers))

string_list.sort(key=len) # ['1', '12', '11', '123', '111', '1234', '12345', '22222']

```
##### Перебор элементов списка
```python
cheer_clubs = ['wave', 'no limit', 'zoom', 'abceer', 'victory']
for cheer_club in cheer_clubs:     
	print(f'{cheer_club.title()} - лучший клуб чирлидинга?')
```
##### Копирование списка
```python
new_smartphones = smartphones.copy() # Копирование списка
new_smartphones = smartphones[:] # Копирование списка
same_smartphones = smartphones # Связывает новую переменную с существующем списком
```
##### Сегменты (slices)
```python
smartphones = ['samsung', 'apple', 'nokia', 'xiaomi', 'zte', 'redmi']

smartphones[0:3] 
smartphones[1::2] # вывод через один элемент
smartphones[:5:3] 
smartphones[-5::3]

smartphones[:2:-1] # ['redmi', 'zte', 'xiaomi']
```

##### Подсчет элесентов `.count(value, start, end), index(value, start, end)`

```python
smartphones = ['samsung', 'apple', 'nokia', 'nokia', 'xiaomi', 'zte', 'redmi', 'samsung', 'apple']

smartphones.count('samsung') # 2
smartphones.index('apple') # 1 (первое вхождение)
smartphones.index('apple', 2, -1) # ValueError: 'apple' is not in list
smartphones.index('apple', 2) # 8
```

#### Численные списки

```python
for value in range(1,5):
    print(value)

numbers = list(range(1,6)) # Список из чисел от 1 до 5

even_numbers = list(range(2,11,2)) # Список из четных чисел

squares = []
for value in range(1,11):
    squares.append(value**2)

digits = [3, 45, 11, 7, 3, 90, 0, 5, 22, 79, 8]

minimum, maximum, summa = min(digits), max(digits), sum(digits)
```
##### Массивы
```python
mass_2x2 = [[x for x in range(1,4)] for y in range(1,4)]

mass_squares = [[x**2 for x in range(1,11,2)] for y in range(1,11,2)]
```