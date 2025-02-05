```python
a = (x ** 2 for x in range(0, 5))
# <generator object <genexpr> at 0x1022bd700>

next(a) # 0
next(a) # 1
```

Генератор можно итеировать только 1 раз.
```python
gen = (x ** 2 for x in range(6))
list(gen)

gen = (x ** 2 for x in range(6))
set(gen)

gen = (x ** 2 for x in range(6))
sum(gen) # 55
```

Генератор не хранит в памяти все значения, а переходит к следующему по мере необходимости.
```python
gen = (x for x in range(pow(10,10)))

for itm in gen:
	print(itm)
	if itm >= pow(10,5):
		break
# 0 ... 100000

gen = (x for x in range(10))
len(gen) # TypeError: object of type 'generator' has no len()
```

## Оператор `yield`

'Замораживает' состояние функции до следующего вызова.

```python
def get_list():
	for x in [1, 2, 3, 4, 5]:
		yield x

a = get_list()
print(a) # <generator object get_list at 0x1064fe200>
print(next(a)) # 1
print(next(a)) # 2
print(next(a)) # 3
```

```python
def get_list():  
    """Вычисляет среднее арифметическое для последовательности."""  
    for i in range(1, 10):  
        a = range(i, 11)  
        yield sum(a) / len(a)  
  
  
a = get_list()  
print(list(a))

# [5.5, 6.0, 6.5, 7.0, 7.5, 8.0, 8.5, 9.0, 9.5]
```