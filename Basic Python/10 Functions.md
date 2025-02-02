**Функции также являются объектами в Python.**
Имя функции - это ссылка на объект функции.
```python
f = print
f('Hello') # Hello

PARAMETR = True  
  
if PARAMETR:  
    def get_rect(a, b):  
       return f'Периметр: {2 * (a + b)}'  
else:  
    def get_rect(a, b):  
       return f'Площадь: {a * b}'  
  
print(get_rect(5, 6)) # Периметр: 22
```

**Позиционные параметры** — это параметры, которые принимают значения на основе их позиции в списке аргументов при вызове функции. Когда вы вызываете функцию, значения передаются в том порядке, в котором параметры были определены.

```python
def add(x, y):
	return x + y 

result = add(add(10, 12), 5) # 27
```
- Порядок важен: значения передаются в соответствии с порядком параметров.
- Если не указать все необходимые позиционные аргументы, будет вызвано исключение `TypeError`.

**Именованные параметры** (или ключевые аргументы) позволяют передавать значения в функцию, указывая имя параметра. Это делает код более читаемым и позволяет передавать аргументы в любом порядке.
```python
def print_person(name, age):     
	print(f"Name: {name}, Age: {age}")

print_person(age=30, name="Alice")  # Параметры могут быть переданы в любом порядке
```
- Можно передавать аргументы в любом порядке.
- Удобно использовать с функциями, имеющими множество параметров.
- Можно комбинировать позиционные и именованные аргументы, но именованные должны следовать после позиционных.

**Вы можете использовать как позиционные, так и именованные параметры в одной функции:**

```python
def calculate(base, height, *, area=True):
	if area:
		return (base * height) / 2  # Площадь треугольника
	else:
		return base + height  # Периметр 

print(calculate(10, 5))  # Вывод: 25.0 (площадь)
print(calculate(10, 5, area=False))  # Вывод: 15 (периметр)
```
- В примере выше `*` указывает на то, что все последующие параметры должны быть именованными.
- Это позволяет избежать путаницы при вызове функции с множеством параметров.

```python
def add_value(value, lst=[]):
	lst.append(value)
	return lst

l = add_value(1) # [1]
l = add_value(2) # [1, 2]

def add_value(value, lst=None):
	if lst is None:
		lst = []
	lst.append(value)
	return lst

l = add_value(1) # [1]
l = add_value(2) # [2]	
l = add_valer(3, l) # [2, 3]
```

## Функции с произвольным числом параметров
`*args` - оператор упаковки и распаковки аргументов
`**kwargs` - оператор упаковки и распаковки произвольного кол-ва **именованных** аргументов

```python
def fun(fact, *args, formal_arg=False, **kwargs):
	# фактические аргументы должны идти перед *args
	# формальные аргументы должны идти перед **лцфкпы
	print(args)
	print(kwargs)

fun('fact_arg', 1, {3, 4}, 'str', 'f': [1, 2, 3], ('str1', 'str2'): {1, 2})
```

## Операторы упаковки и распаковки `* **`

```python
x, *s = (1, 2, 3, 4) # x = 1, s = [2, 3, 4]
*x, s = (1, 2, 3, 4) # x = [1, 2, 3], s = 4

l = [1, 2, 3, 4]
t = (*l,) # (1, 2, 3, 4)

d = (-5, 5)
list(range(*d)) # [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4]
# Автоматическая распаковка итерируемово объекта
[*range(*d)] # [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4]

[*range(*d), *(True, False), 1, 2, 3]
# [-5, -4, -3, -2, -1, 0, 1, 2, 3, 4, True, False, 1, 2, 3]

english = {'рука': 'hand',
		   'нога': 'leg',
		   'хвост': 'tail',
		   'питон': 'python',
		   'бэкенд-разработчик': 'back-end developer'}

{*english} # {'бэкенд-разработчик', 'хвост', 'питон', 'нога', 'рука'}
{*english.values()} # {'leg', 'tail', 'back-end developer', 'python', 'hand'}
{*english.items()}
# {('хвост', 'tail'), ('рука', 'hand'), ('бэкенд-разработчик', 'back-end developer'), ('нога', 'leg'), ('питон', 'python')}

english_new = {'свинья': 'pig',
			  'кружка': 'cup',
			  'заметка': 'note'}

english_full = {**english, **english_new} # объединит 2 словаря
# {'рука': 'hand', 'нога': 'leg', 'хвост': 'tail', 'питон': 'python', 'бэкенд-разработчик': 'back-end developer', 'свинья': 'pig', 'кружка': 'cup', 'заметка': 'note'}
```

## Global and nonlocal

```python
def outer():
	x = 1
	def inner():
		x = 2
		print(f'Inner: {x}')
	
	inner()
	print(f'Outer: {x}')

outer()
print(f'Global: {x}')
# Inner: 2
# Outer: 1
# Global: 0
```

```python
def outer():
	x = 1
	def inner():
		nonlocal x
		x = 2
		print(f'Inner: {x}')
	
	inner()
	print(f'Outer: {x}')

outer()
print(f'Global: {x}')
# Inner: 2
# Outer: 2
# Global: 0
```

```python
def outer():
	global x
	x = 1
	def inner():
		x = 2
		print(f'Inner: {x}')
	
	inner()
	print(f'Outer: {x}')

outer()
print(f'Global: {x}')
# Inner: 2
# Outer: 1
# Global: 1
```