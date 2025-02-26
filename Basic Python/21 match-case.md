Python 3.10 +

```python
cmd = 'top'  
  
match cmd:  
    case 'top':  
        print('Вверх')  
    case 'lift':  
        print('Влево')  
    case 'right':  
        print('Вправо')  
    case _:  
        print('Другое')  
  
print('Процессы завершены')
```

```python
cmd = 'top'  
  
match cmd:  
    case 'top':  
        print('Вверх')  
    case command:  
        # создает переменную и ссылается на значение cmd  
        # command = cmd        # отрабатывает всегда        # все другие блоки case игнорируются        print(f'команда: {command}')  
  
print('Процессы завершены')
```

```python
"""Проверка типов данных с учетом наследования"""  
cmd = 5  
  
match cmd:  
    case str():  
        print('Строка')  
    case bool():  
        print('Булево значение')  
    case int() as command if 0 <= command < 10:  
        print('Целочисленный тип')  
    case float():  
        print('Значение с плавающей точкой')  
  
print('Процесс закончен')
```
**Оператор if - название guard**

## Кортежи и списки

```python
books = [  
    ("Преступление и наказание", "Федор Достоевский", 671),  
    ("Гордость и предубеждение", "Джейн Остин", 432),  
    ("1984", "Джордж Оруэлл", 328),  
    ("Убить пересмешника", "Харпер Ли", 281),  
    ("Гарри Поттер и философский камень", "Дж. К. Роулинг", 223),  
    ("Властелин колец", "Дж. Р. Р. Толкин", 1178),  
    ("Великий Гэтсби", "Ф. Скотт Фицджеральд", 180),  
    ("Моби Дик", "Герман Мелвилл", 625),  
    ("Одиссея", "Гомер", 384),  
    ("Илиада", "Гомер", 683)  
]  


match books[0]:  
    case (title, author, *_) if len(books[0]) >= 4:  
        # скобки не создают tuple или list - это группирующие скобки  
        # работает и для tuple и для list
        # *_ позволяет распоковать все элементы кортежа, 
        # вне зависимости от их кол-ва
        # но не будет работать, если указано всего 3 эл-а
        print(f'Книга: {title}; Автор: {author}')  
    case title, author, pages, price:  
        # тут кол-во элементов больше, чем в кортеже,
        # кортеж из большего кол-ва элементов не пройдет проверку
        print(f'Книга: {title}; Автор: {author}; Количество страниц: {pages}; Цена: {price}')
    case title, author, pages:    
		print(f'Книга: {title}; Автор: {author}; Количество страниц: {pages}')
	case _:  
        print('Другой тип данных')  
  
print('Процесс закончен')
```

```python
# book = (1, "1984", "Джордж Оруэлл", 328, 2024)
book = ["1984", "Джордж Оруэлл", 328, 2024]  
  
match book:  
    case [_, str() as title, str() as author, int() as pages, _]:  
        print(f'Книга: {title}; Автор: {author}; Количество страниц: {pages}')  
    case (str() as title, str() as author, pages, _):  
        print(f'Книга: {title}; Автор: {author}; Количество страниц: {pages}')  
    case _:  
        print('Другой формат данных')  

# анологичный результат
match book:
	case tuple():  
	    print('Неверный формат данных - tuple')
    case [title, author, pages, _] | [_, title, author, pages, _]:  
        print(f'Книга: {title}; Автор: {author}; Количество страниц: {pages}')  
    case _:  
        print('Другой формат данных')
```

## Словари и множества

Достаточно наличия указанных ключей.

```python
request = {
	'url': 'https://example.com',
	'method': 'GET',
	'timeout': 1000
}

match request:
	case {'url': url, 'method': method}:  
	    print(f'Запрос: url: {url}, метод: {method}') 
    case _:  
        print('Неверный запрос')
```

Сработает только, если значение timeout == 1000 или 2000.
```python
request = {
	'url': 'https://example.com',
	'method': 'GET',
	'timeout': 1000
}

match request:
	case {'url': url, 'method': method, timeout: 1000 | 2000}:  
	    print(f'Запрос: url: {url}, метод: {method}') 
    case _:  
        print('Неверный запрос')
```

Ограничение на кол-во передаваемых именнованных аргументов.

```python
request = {  
    'url': 'https://example.com',  
    'method': 'GET',  
    'timeout': 1000  
}  
  
match request:  
    case {'url': url, 'method': method, **kwargs} if len(**kwargs) < 2:  
        print(f'Запрос: url: {url}, метод: {method}')  
    case _:  
        print('Неверный запрос')
```
