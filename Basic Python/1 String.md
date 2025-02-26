❗️ **Строки - неизменяемый тип данных.**
##### `print(sep, end)`
**sep** - разделитель между данными
**end** - завершающий символ или строка

**Конкатенация** (от лат. concatenatio, «присоединение, сцепление») - объединение нескольких строк в одну.
```python
str_sum = 'Привет, ' + 'мир!'
# >> Привет мир!
str_multiply = 'ха' * 3 
# >> хахаха num = 56

'ab' in 'fsabdhee'
# True

'кит' > 'кот'
# True
'Кот' > 'кот'
# False
ord('Л') # код символа
# 1051
```
В ASCII заглавные символы идут перед строчными.
##### Срезы
```python
'panda'[3]
# d

s = 'This is string'
s[2:4] # 'is'
s[2:8:2] # 'i s'
s[:9] # 'This is s'

s[-3] # 't'
s[2:-3] # 'is is str'
s[::-1] # 'gnirts si sihT'


a = s[:] # копирует, при этом id совпадают

```
##### Перевод регистра
```python
st1 = "This is a string."

st1.title()
st1.upper()
st.lower()
```
##### Методы строк
```python
msg = """Lorem ipsum odor amet, consectetuer adipiscing elit. 
Conubia semper curabitur pellentesque vulputate lectus."""

s = "abrakadabra"

# .count(sub, start, end) 
# подсчет кол-ва вхождение подстроки в строке
s.count('ra') # 2
s.count('ra', 4) # 1
s.count('ra', 4, 10) # 0
s.count('ra', 4, 11) # 0
```
##### `.find(sub, start, end), .rfind(sub, start, end)`

Поиск подстроки в строке (возвращает индекс первого эл-та подстроки в строке)
```python
msg.find('pellentesque') # 79
msg.find('Lorem', 0, 20) # 0
msg.find('Abracadabra') # -1 если подстрока не найдена

# -"- .find, но начинает поиск с конца
s.rfind('ra') # 9
```
##### `.index(sub, start, end)`

Идентичен find, но возвращает error если не найдет подстроку в строке.
##### `.replace(old, new, count)`

count - максимальное кол-во замен. Если count = -1, то заменяет все вхождения подстрки
```python
s.replace('ra', 'po', -1) # 'abpokadabpo'
s.replace('ra', '', -1) # 'abkadab'
```
##### `.isalpha, .isdigit`
```python
'adf'.isalpha() # True
s = 'Hi wourld'
s.isalpha() # False

nums = '12345678'
nums.isdigit() # True
'35.5'.isdigit() # False
```
##### `value.rjust(число_символов, 'заполнитель')`

```python
value = 'hello'
value.rjust(10) # '     hello'

num = '15'
num.rjust(4, '0') # '0015'

num.ljust(4, '0') # '1500'
```
##### `.split(sep, maxsplit=-1)`, `' '.join()`

```python
'Иванов Иван Иванович'.split(' ') # ['Иванов', 'Иван', 'Иванович']

lst = ['Иванов', 'Иван', 'Иванович']
' '.join(lst) # 'Иванов Иван Иванович'
```
##### Выведение строк с пропусками `f'<строка> {}`'
```python
first_name = "kate" last_name = "vinston" full_name = f"{first_name} {last_name}"
```
##### Удаление пропусков `.rstrip()`, `.lstrip()`, `strip()`
```python
st1 = "   Пропуски   "

st1.rstrip()
st1.lstrip()
st1.strip()
```

**Строка — упорядоченная коллекция**, иначе каждый раз при печати выводилась бы нечитаемая чепуха, где все буквы перепутаны.
```python
string = 'Вышел месяц из тумана, вынул ножик из кормана. Буду резать, буду бить - все-равно тебе водить!'

new_list = list(string)
new_set = set(string)

# К элементу строки можно обращаться по индексу.
index_list = [1, 5, 6, -5, 12] 
for i in index_list:     
	print(string[i])
````
##### Разобрать строку на слова: метод `.split()`
```python
milk_str = 'молоковоз'
counter_str = 'Раз-два-три-четыре-пять, вышел зайчик погулять'

new_list = milk_str.split('о')
counter_list = counter_str.split('-')
```

Если вызвать метод `split()` **без аргументов**, с пустыми скобками — он точно так же разобьёт строку **по пробелам**.
#### Создать строку из коллекции: метод `.join()`
```python
words_list = ['раз', 'два', 'три', 'четыре', 'пять', 'вышел зайчик погулять']

new_string = '-'.join(words_list)`
```

#### Сырые строки

```python
import re

raw_string = r"C:\Users\YourName\Documents\file.txt"

pattern = r"\d{3}-\d{2}-\d{4}"  # Шаблон для поиска формата XXX-XX-XXXX
text = "My number is 123-45-6789."
match = re.search(pattern, text)
print(match.group())  # Вывод: 123-45-6789
```

**Ограничения:**
Сырые строки не могут заканчиваться на обратный слэш, так как это приведет к синтаксической ошибке.
```python
# Ошибка: EOL while scanning string literal
invalid_raw_string = r"C:\Users\YourName\Documents\"

valid_raw_string = r"C:\Users\YourName\Documents\\" # Два обратных слэша
```