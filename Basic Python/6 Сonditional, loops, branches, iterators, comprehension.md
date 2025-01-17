##  Сonditional `if - elif - else`

```python
x = int(input())

if 0 <= x < 20:
	print(1)
elif x >= 20:
	print(2)
else:
	print(3)

if x:
	print('True, значение не 0 и не False')
```
##  Функция `range()`, `reversed()`
Синтаксис:`range(start, stop, step)`
- **`start`** — начальное значение последовательности. Если этот параметр не указан, по умолчанию используется 0.
- **`stop`** — конечное значение последовательности. Этот параметр является обязательным и указывает, до какого числа будет продолжаться последовательность. `stop` не включается в итоговый диапазон.
- **`step`** — шаг, с которым будет увеличиваться значение. Если этот параметр не указан, по умолчанию используется шаг 1.
```python
around_zero = range(-3, 3)
# >> -3, -2, -1, 0, 1, 2 
for i in reversed(range(1, 13)):     
	print(i, 'бомм!') print('С тновым годом!')`

list(range(0, 10, 2)) # [0, 2, 4, 6, 8]

smartphones = ['samsung', 'apple', 'nokia', 'nokia', 'xiaomi', 'zte', 'redmi', 'samsung', 'apple']

for key, value in enumerate(smartphones):
	print(f'key: {key}\nvalue: {value}', end='\n\n')
```
## Loops
Для выполнения повторяющихся, однотипных операций в программировании используются циклы. В Python таких циклов два:
- **for** – счетный цикл, повторяется определенное **количество раз**;
- **while** – условный цикл, повторяется до выполнения определенного **условия**.
##### `for` и `while`
```python
for value_name in range(quantity):
	body

for i in range(start, stop, stap):
	body

# Перебор элементов какой-либо коллекции:
for value_name in clliction_name:
	body

for ...:
	if condition:
		break # завершить досрочно

for ...:
	if condition:
		continue # переходит к следующей итерации при обнаружении элемента

# Без использования переменной
for _ in range(int(input())):
	k, v = input().split(': ')
	mydict[k.capitalize()] = v.title()

flag = True
while flag:
	if condition:
		break
	elif condition2:
		continue
	else:
		action
else:
	# Блок else выполнится после штатного завершения цикла
	action
```

**Пример с else**
```python
max_val = 10
i = 0

print('Для завершения введите "q"')
while i <= max_val:
	s = input('Введите любой символ: ')
	if s == "q":
		break
	i += 1
else:
	print('Цикл завершился в штатном режиме!')
```

## Тернарный условный оператор
```python
<значение 1> if <условие> else <значение 2>

val = True
[[1, 2, 3] if val else [2, 3, 4]]

a, b, c = 4, 6, 2
if a > b:
	if a > c:
		print('a -> max')
	else:
		print('c -> max')
else:
	if b > c:
		print('b -> max')
	else:
		print('c -> max')

# Эквивалентный тернарный условный оператор
max_value = (a if a > c else c) if a > b else (b if b > c else c)
```

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
