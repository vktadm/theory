Type hints в Python, введенные в версии 3.5.
##### Основные концепции типизации
1. **Базовые типы**, такие как `int`, `float`, `str`, и `bool` для аннотации переменных.
```python
age: int = 20
name: str = "Alice"
is_active: bool = True
```
2. **Типы для параметров функций и их возвращаемые значения.**
```python
def greet(name: str) -> str:
    return "Hello, " + name
```
1. **Композитные типы**:
	Для более сложных структур данных, таких как списки и словари, используются классы из модуля `typing`.
	
	Позволяет **явно указать**, какие **типы данных** ожидаются в списке.
	
	С Python 3.9 можно использовать встроенный тип `list` с аннотацией типов.
```python
from typing import List

def sum_numbers(numbers: List[int]) -> int:
    return sum(numbers)

# python 3.9
def process_items(items: list[str]) -> None:
    for item in items:
        print(item)
```
4. **Опциональные типы**:
	С помощью `Optional` можно указать, что переменная может быть либо определенного типа, либо `None`.
	
	С Python 3.10 можно использовать оператор `|` для обозначения того, что переменная может быть одного из нескольких типов
```python
from typing import Optional

def find_student(student_id: int) -> Optional[dict[str, str]]:
    # Логика поиска студента
    return None  # Если студент не найден

# python 3.10
def get_user_age(user_id: int) -> int | None:
    return None
```
5. **Пользовательские типы**:
```python
class Student:
    def __init__(self, name: str):
        self.name = name

def get_student_name(student: Student) -> str:
    return student.name
```
6. **Аннотирование с метаданными**:
	В Python 3.9 добавлена возможность использования `Annotated` для добавления дополнительной информации к типам
```python
from typing import Annotated

def say_hello(name: Annotated[str, "This is just metadata"]) -> str:
    return f"Hello {name}"
```
