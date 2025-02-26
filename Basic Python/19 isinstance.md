`isinstance(object, class)` - принадлежность объекта классу с учетом иерархии объектов.

```python
a = 45
isinstance(a, int) # True

b = True
isinstance(b, int) # True

type(b) == int # False
type(b) is bool # True

type(b) in (bool, float, str) # True

c = -4
isinstance(c, (int, float)) # True
isinstance(c, (int, bool)) # True
isinstance(c, (str, bool)) # False
# эквивалентна
isinstance(с, str) or isinstance(c, bool)
```

**`bool` наследуется от `int`**

```python
data = (4.5, True, 'book', 8, 7, -3.3, [True, False], 10.99)

s = 0
for itm in data:
	if isinstance(itm, float):
		s += itm

print(s) # 12.190000000000001

s = sum(filter(lambda itm: isinstance(itm, float), data)) # 12.190000000000001

s = sum(filter(lambda itm: isinstance(itm, int), data)) # 16 (прибавилось True)
# чтобы избежать, используем строгую проверку
s = sum(filter(lambda itm: type(itm) is int, data)) # 15
```