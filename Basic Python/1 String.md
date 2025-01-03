**Конкатенация** (от лат. concatenatio, «присоединение, сцепление») - объединение нескольких строк в одну.
```python
str_sum = 'Привет, ' + 'мир!'
# >> Привет мир!
str_multiply = 'ха' * 3 
# >> хахаха num = 56
```
##### Перевод регистра
```python
st1 = "This is a string."

st1.title()
st1.upper()
st.lower()
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