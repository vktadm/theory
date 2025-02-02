```python
open(file [, mode='r', encoding=None, ...])
# 'r' - чтение, 'w' - запись
# по умолчанию открывается на чтение
```
- D:
	- parent
		- prt.data
	- out.txt
	- app
		- images
			- img.txt
		- **example.py**
		- my_file.txt

```python
open('example.py')
open('d:/app/my_file.txt')

open('imges/img.txt')

open('../out.txt')

open('../parent/prt.data')
```

## Чтение файла
Спецификация (Byte Order Mark (BOM)) - это специальный маркер в начале текстового файла, который указывает кодировку файла. В UTF-8 спецификация представлена последовательностью байтов 0xEF, 0xBB, 0xBF.

```python
file = open('file.txt')
encode_file = open('encode_file.txt', encoding='Windows-1251')

file.read(10) # прочитать первые 10 символов (выведит 9 из-за #FEFF)
file.read(10) # прочитать следующие 10 символов 
```


**#FEFF** Lorem ipsum odor amet, consectetuer adipiscing elit.  
Eu pellentesque ultricies felis torquent adipiscing dictumst dapibus magna.  
Dignissim mus eget netus et porta magnis mattis. **EOF** (End Of File)

## Управление файловой позицией

```python
file.seek(offset[, from_what])
# offset - значкение для установки файловой позиции
```

```python
file = open('file.txt')

file.read(10) # Lorem ipsu
file.tell() # 10 # возвращает текущую файловую позицию
file.seek(0)
file.read(5) # Lorem

file.readline() # построчное чтение (с сохранение \n)
# Lorem ipsum odor amet, consectetuer adipiscing elit.\n

file.readlines() # возвращает массив строк # лучше не использовать, тк может возникнуть переполнение памяти

file.close()
```

## try-except-finally

```python
try:  
    file = open('file.txt', encoding='utf-8')  
    try:  
        data = file.readlines()  
        int(data)  
        print(data)  
    finally:  
        file.close()  
except FileNotFoundError:  
    print('Невозможно прочитать файл!')  
except:  
    print('Ошибка при работе с файлом!')

# Ошибка при работе с файлом!
# finally выполниться в любом случае
```

## Файловый менеджер контекста

**Автоматически закрывает в файл.**

```python
with open('file.txt', encoding='utf-8') as file:
	data = file.readlines()
	print(data)

file.closed # True
```

## Запись в файл

```python
file = open('out.txt', 'w') # удаляется содержимое

file.write('Hello World!')  
file.close()
```

- 'a' - добавляем данные;
- 'a+' - добавляем и считываем данные;
- 'w' - только заменяем;
- 'r' - только читаем.
```python
try:  
    with open('out.txt', 'a+') as file:  
        file.write('Hello!\n')  
        file.write('How are you?\n')  
        file.seek(0) # файловая позиция для чтение, но не для записи
        data = file.readlines()  
        print(data)
except:  
    print('Ошибка при работе с файлом!')

# ['Hello!\n', 'How are you?\n', 'Hello!\n', 'How are you?\n']
```

## Запись в бинарном виде

```python
import pickle  
  
books = [  
    ("Лев Толстой", "Война и мир", 1225),  
    ("Федор Достоевский", "Преступление и наказание", 430),  
    ("Антон Чехов", "Три сестры", 120),  
    ("Михаил Булгаков", "Мастер и Маргарита", 400),  
    ("Александр Пушкин", "Евгений Онегин", 300),  
    ("Джордж Оруэлл", "1984", 328),  
    ("Рэй Брэдбери", "451 градус по Фаренгейту", 256),  
    ("Габриэль Гарсиа Маркес", "Сто лет одиночества", 417),  
    ("Харуки Мураками", "Норвежский лес", 400),  
    ("Джейн Остин", "Гордость и предубеждение", 432)  
]  
  
  
file = open("out_bin.txt", "wb")  
pickle.dump(books, file)  
file.close()
```

## Чтение бинарной информации

```python
import pickle  
  
books = [  
    ("Лев Толстой", "Война и мир", 1225),  
    ("Федор Достоевский", "Преступление и наказание", 430),  
    ("Антон Чехов", "Три сестры", 120),  
    ("Михаил Булгаков", "Мастер и Маргарита", 400),  
    ("Александр Пушкин", "Евгений Онегин", 300),  
    ("Джордж Оруэлл", "1984", 328),  
    ("Рэй Брэдбери", "451 градус по Фаренгейту", 256),  
    ("Габриэль Гарсиа Маркес", "Сто лет одиночества", 417),  
    ("Харуки Мураками", "Норвежский лес", 400),  
    ("Джейн Остин", "Гордость и предубеждение", 432)  
]  
  
  
file = open("out_bin.txt", "rb")  
b_data = pickle.load(file)  
print(b_data)  
file.close()
```
