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
popped_genres = genres.pop() # Удаление последнего элемента с возможностью сохранить значени 
popped_genres1 = genres.pop(0)
genres.remove('Ужасы') # Удаление по значению (только первое вхождение)
```
##### Сортировка `.sort(), sorted(), reverse()`
```python
firs_names = ['Катя', 'Маша', 'Ваня', 'Кирилл', 'Анна']

firs_names.sort() # Сортировка по алфавиту
firs_names.sort(reverse=True) # Сортировка в обратном порядке

new_sorted_list = sorted(firs_names) # Сортировка без изменения оригинального списка

cheer_clubs = ['wave', 'no limit', 'zoom', 'abceer', 'victory'] cheer_clubs.reverse() # Изменения порядка на обратный, без сортировки
```
##### Перебор элементов списка
```python
cheer_clubs = ['wave', 'no limit', 'zoom', 'abceer', 'victory']
for cheer_club in cheer_clubs:     
	print(f'{cheer_club.title()} - лучший клуб чирлидинга?')
```
##### Копирование списка
```python
new_smartphones = smartphones[:] # Копирование списка
same_smartphones = smartphones # Связывает новую переменную с существующем списком`
```
##### Сегменты (slices)
```python
smartphones = ['samsung', 'apple', 'nokia', 'xiaomi', 'zte']

smartphones[0:3] 
sorted_smartphones[1::2] # вывод через один элемент
sorted_smartphones[:5:3] 
sorted_smartphones[-5::3]
```