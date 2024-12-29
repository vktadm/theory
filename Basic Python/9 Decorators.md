**Декораторы** в Python представляют собой мощный инструмент, который позволяет **изменять или расширять поведение функций и классов** без изменения их исходного кода. Они являются **функциями высшего порядка**, что означает, что они могут принимать другие функции или классы в качестве аргументов и возвращать новые функции или классы с дополнительной функциональностью.

Декораторы **"оборачивают"** функции, позволяя выполнять код до и после вызова обернутой функции. Это делает их идеальными для таких задач, как логирование, контроль доступа, кэширование и другие

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        # Код до вызова функции
        result = func(*args, **kwargs)
        # Код после вызова функции
        return result
    return wrapper

@my_decorator 
def my_function(): 
	print("Hello, World!")
```
##### Использование нескольких декораторов

Можно применять несколько декораторов к одной функции. Они будут применяться в порядке, начиная с самого внутреннего (ближайшего к функции) к самому внешнему.
```python
@decorator1 # 2-ой
@decorator2 # 1-ый
def my_function():
    pass
```
##### Декораторы с параметрами
Декораторы могут также принимать параметры. Это достигается за счет добавления еще одного уровня вложенности:
```python
def decorator_with_params(param):
    def actual_decorator(func):
        def wrapper(*args, **kwargs):
            # Использование параметра
            print(param)
            return func(*args, **kwargs)
        return wrapper
    return actual_decorator

@decorator_with_params("Hello")
def greet():
    print("Greetings!")
```
##### Декораторы широко используются в Python для различных задач:

- **Логирование**: отслеживание вызовов функций и их аргументов.
- **Кэширование**: сохранение результатов функций для повышения производительности.
- **Контроль доступа**: проверка прав пользователя перед выполнением функций.
- **Измерение времени выполнения**: анализ производительности функций