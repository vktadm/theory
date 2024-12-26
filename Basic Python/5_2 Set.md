**Множество** — неупорядоченная коллекция, состоящая из уникальных элементов.

Одно из важных отличий множества от других коллекций — все элементы множества должны быть **уникальны**, в множестве не может быть двух одинаковых элементов.

Это ещё одна особенность сетов: в памяти компьютера элементы сета **хранятся в неупорядоченном виде** и выводятся в случайном порядке.

К элементу сета **нельзя обратиться по порядковому номеру**, по индексу — ведь один и тот же индекс будет указывать то на один элемент, то на другой.
```python
person_new = {} # Пустая коллеккция
concert_songs = ['Ничего на свете лучше нету',
				 'Мы к вам заехали на час',
				 'Рок-колыбельная',
				 'Луч Солнца Золотого',
				 'Ничего на свете лучше нету',
				 'Куда ты, тропинка, меня завела',
				 'А как известно, мы народ горячий']

set(songs_list) # вернет коллекцию без дубликатов
```
##### Перебор элементов множества в цикле
```python
for song in unique_songs:
	print(song)
```
##### Добавление и удаление элемента множества `.add(), .discard()`
```python
playlist = {'Venus',
			'Yesterday',
			'Fireball',
			'Time',
			'Poison'}
			
playlist.add('Thunderstruck')`
playlist.discard('Venus')
```
##### Объединение элементов множеств `.union()`
```python
playlist_1 = {'Три белых коня',
			  'Happy new year',
			  'Снежинка'}
playlist_2 = {'Last christmas',
			  'Снежинка',
			  'Happy new year'}
			  
playlist_3 = playlist_1.union(playlist_2)
```
##### Поиск различий в двух множествах `.difference()`
```python
playlist_1 = {'Голубой вагон', 'Облака', 'Yesterday', 'Наше лето'}
playlist_2 = {'Наше лето', 'Голубой вагон', 'Облака'}

playlist_3 = playlist_1.difference(playlist_2) 
# >> {'Yesterday'}
```
#### Поиск одинаковых элементов в двух множествах `.intersection()`
```python
films_1 = {'Форсаж', 'Достучаться до небес', 'Мстители: война бесконечности'} films_2 = {'Мстители: война бесконечности', 'Форсаж', 'Матрица'}

films_3 = films_1.intersection(films_2) 
# >> {'Форсаж', 'Мстители: война бесконечности'}`
```