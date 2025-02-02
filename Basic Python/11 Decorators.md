## Замыкания

Замыкания в Python представляют собой мощный механизм, позволяющий функциям сохранять доступ к переменным из внешней области видимости, даже после завершения выполнения внешней функции. Это достигается за счет создания вложенных функций, которые могут обращаться к переменным внешней функции, сохраняя их состояние.

```python
def outer(value):
	def inner():
		print(f'Print: {value}')
	return
	
f = outer('Hello')
f()
```
**Удаление пробелов и др. символов из строки**
```python
def strip_string(strip_chars=" "):
    def do_strip(string):
        return string.strip(strip_chars)
 
    return do_strip

strip1 = strip_string()
strip2 = strip_string(" !?,.;")

print(strip1(" hello python!.. ")) # hello python!..
print(strip2(" hello python!.. ")) # hello python
```

## Декораторы

**Декораторы** в Python представляют собой мощный инструмент, который позволяет **изменять или расширять поведение функций и классов** без изменения их исходного кода. Они являются **функциями высшего порядка**, что означает, что они могут принимать другие функции или классы в качестве аргументов и возвращать новые функции или классы с дополнительной функциональностью.

Декораторы **"оборачивают"** функции, позволяя выполнять код до и после вызова обернутой функции. Это делает их идеальными для таких задач, как логирование, контроль доступа, кэширование и другие

**Не возвращает результат.**
```python
def func_decorator(func):  
    def wrapper(*args, **kwargs):  
        print("----Действие до функции----")  
        func(*args, **kwargs)  
        print("----Действие после функции")  
  
    return wrapper

# Прописываем декорирование через замыкание  
# some_func = func_decorator(some_func)


@func_decorator  
def some_func(title, tag):  
    print(f"title = {title}, tag = {tag}")


some_func("Python forever!", "h1")

```

**Возвращает значение.**
```python
def func_decorator_return(func):  
    def wrapper(*args, **kwargs):  
        print("----Действие до функции----")  
        result = func(*args, **kwargs)  
        print("----Действие после функции")  
        return result  
  
    return wrapper


@func_decorator_return  
def some_func_return(title, tag):  
    print(f"title = {title}, tag = {tag}")  
    return f"<{tag}>{title}</{tag}>"


# Прописываем декорирование через замыкание  
# some_func_return = func_decorator_return(some_function_return)


func_result = some_func_return("New line", "h2")  
print("\n" + func_result)
```
## Использование нескольких декораторов

Можно применять несколько декораторов к одной функции. Они будут применяться в порядке, начиная с самого внутреннего (ближайшего к функции) к самому внешнему.
```python
@decorator1 # 2-ой
@decorator2 # 1-ый
def my_function():
    pass
```
## Декораторы с параметрами

Декораторы могут также принимать параметры. Это достигается за счет добавления еще одного уровня вложенности:
```python
import math  
  
  
# Декоратор функции с параметром  
def df_decorator(dx=0.01):  
    def func_decorator(func):  
        def wrapper(x, *args, **kwargs):  
            res = (func(x + dx, *args, *kwargs) - func(x, *args, *kwargs)) / dx  
            return res  
  
        return wrapper  
    return func_decorator  
  
  
@df_decorator(dx=0.000001)  
def sin_df(x):  
    return math.sin(x)  
  
  
if __name__ == '__main__':  
    df = sin_df(math.pi / 3)  
    print(df)
    
```

 **Декораторы широко используются в Python для различных задач:**
 
- **Логирование**: отслеживание вызовов функций и их аргументов.
- **Кэширование**: сохранение результатов функций для повышения производительности.
- **Контроль доступа**: проверка прав пользователя перед выполнением функций.
- **Измерение времени выполнения**: анализ производительности функций