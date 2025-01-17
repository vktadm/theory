```python
for value in range(1,5):
    print(value)

numbers = list(range(1,6)) # Список из чисел от 1 до 5

even_numbers = list(range(2,11,2)) # Список из четных чисел

squares = []
for value in range(1,11):
    squares.append(value**2)

digits = [3, 45, 11, 7, 3, 90, 0, 5, 22, 79, 8]

minimum, maximum, summa = min(digits), max(digits), sum(digits)
```
##### Arrays
```python
mass_2x2 = [[x for x in range(1,4)] for y in range(1,4)]

mass_squares = [[x**2 for x in range(1,11,2)] for y in range(1,11,2)]
```