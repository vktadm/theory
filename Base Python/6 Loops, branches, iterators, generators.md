##### Функция `range()`, `reversed()`
Синтаксис:`range(start, stop, step)`
- **`start`** — начальное значение последовательности. Если этот параметр не указан, по умолчанию используется 0.
- **`stop`** — конечное значение последовательности. Этот параметр является обязательным и указывает, до какого числа будет продолжаться последовательность. `stop` не включается в итоговый диапазон.
- **`step`** — шаг, с которым будет увеличиваться значение. Если этот параметр не указан, по умолчанию используется шаг 1.
```python
around_zero = range(-3, 3)
# >> -3, -2, -1, 0, 1, 2 
for i in reversed(range(1, 13)):     
	print(i, 'бомм!') print('С тновым годом!')`
```
##### Циклы
Для выполнения повторяющихся, однотипных операций в программировании используются циклы. В Python таких циклов два:
- **for** – счетный цикл, повторяется определенное **количество раз**;
- **while** – условный цикл, повторяется до выполнения определенного **условия**.
##### `for` и while
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
	# Блок else выполнится, если flag == False
	action
```
##### Генераторы
```python
my_list = [x for x in range(quantity) if condition else value]
```