Модуль random управляет генерацией случайных чисел. Его основные функции:
```python
import random as rd
# from random import randint

# генерирует случайное число от 0.0 до 1.0
n = 10
number = rd.random() * n

# возвращает случайное число из определенного диапазона
number = rd.randint(20, 50)

# возвращает случайное число из определенного набора чисел
number = rd.randrange(10)  # значение от 0 до 10 не включая
number = rd.randrange(2, 10)  # значение в диапазоне 2, 3, 4, 5, 6, 7, 8, 9
number = random.randrange(2, 10, 2)  # значение в диапазоне 2, 4, 6, 8

numbers = [1, 2, 3, 4, 5, 6, 7, 8]

# перемешивает список
rd.shuffle(numbers)

# возвращает случайный элемент списка
random_number = rd.choice(numbers)


```