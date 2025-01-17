- **Упорядоченная, неизменяемая коллекция.**
- Может быть ключом в словаре.
- Занимают меньше памяти, чем списки.
- Кортежи могут содержать изменяемые типы данных (list, dict, set), которые можно изменять внутри кортежа. (Неизменными являются только ссылки на эл-ы)

```python
lst = [1, 2, 3]
lst.__sizeof__() # 72
t = (1, 2, 3)
t.__sizeof__() # 48

t = ()
t = tuple()
```

```python
nums = 1, 2, 3 # (1, 2, 3)
nums = 1, # (1, )
nums = (3, )

human = ('head', 'leg', 'body', 'hand')
new_human = human[:] # id(new_human) == id(human) ❗️

nums[0] # 3

key = ('head', 'leg', 'body', 'hand')
d = {}
d[key] = 'body' # {('head', 'leg', 'body', 'hand'): 'body'}
```
##### Распаковка
```python
a, b, c = 1, 2, 3
a, b, c = (1, 2, 3)

a, b, c = 'map' # a = 'm', b = 'a', c = 'p') 
# кол-во эл-в должно быть ==
```
##### Объединение
```python
a = ()
a = a + (1, 2, 3) # (1, 2, 3)

b = (0, ) * 10 # (0, 0, 0, 0, 0, 0, 0, 0, 0, 0)
```
##### Методы
```python
t = (1, 2, 3, 3, 2, "a", "b", "c", "c", "b")
t.count(2) # 2
t.count("hello") # 0

t.index("a") # 5
t.index("c", 8, 9) # 8
t.index(1, 2) # ValueError: tuple.index(x): x not in tuple
```