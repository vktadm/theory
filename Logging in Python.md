**Logging** или логирование (журналирование) - это автоматическое ведение записей о тех или иных событиях в хронологическом порядке с целью либо убедиться в том, что все идет по плану, либо упростить отлавливание нежелательного поведения программного обеспечения.

Обычно логи, помимо какого-то информационного сообщения о том, что что-то произошло или наоборот, не произошло, содержат дополнительную служебную информацию:

- Время события
- Модуль, при исполнении кода в котором произошло событие
- Уровень серьезности сообщения лога
- Номер строки кода в модуле
- Пойманное исключение
- Любая другая полезная информация

То есть, когда нам нужно во время работы программы вывести в консоль и/или в файл какую-то информацию о работе программы, мы просто в нужном месте кода вставляем конструкцию вида:

```python
logging.<метод_отвечающий_за_уровень_логирования>('<сообщение>')
```

Основными ключевыми понятиями в библиотеке `logging` являются:

- **Логгеры**. Предоставляют интерфейс пользователю для создания логов в коде.
- **Уровни логирования**. Определяют степень серьезности информации, выводимой в логах.
- **Хэндлеры**. Определяют место вывода логов (например, консоль или файл).
- **Форматтеры.** Определяют то, как логи будут выглядеть.
- **Фильтры.** Позволяют гибко настраивать то, какие логи выводить.

```python
import logging

logging.debug('Это лог уровня DEBUG')
logging.info('Это лог уровня INFO')
logging.warning('Это лог уровня WARNING')
logging.error('Это лог уровня ERROR')
logging.critical('Это лог уровня CRITICAL')
```

В консоли вы получите вывод:

```no-highlight
WARNING:root:Это лог уровня WARNING
ERROR:root:Это лог уровня ERROR
CRITICAL:root:Это лог уровня CRITICAL
```

По умолчанию вывод логов начинается только с уровня `WARNING`, то есть уровни `DEBUG` и `INFO` игнорируются. Это поведение, разумеется, можно менять.

Методы логгеров, создающие лог нужного уровня называются так же, как и сами уровни, только строчными буквами. Уровень `DEBUG` - метод `debug`, уровень `WARNING` - метод `warning` и т.п.

Основных уровней логирования в библиотеке `logging` пять: от самого низкого `DEBUG` до самого высокого `CRITICAL`:

1. `DEBUG` (10)
2. `INFO` (20)
3. `WARNING` (30)
4. `ERROR` (40)
5. `CRITICAL` (50)

Здесь 10, 20, ... , 50 - это просто числовой способ записи этих же уровней, чтобы можно было легко сравнивать уровни между собой и понимать какой из них выше, а какой ниже.

```python
import logging

print(logging.DEBUG)
print(logging.INFO)
print(logging.WARNING)
print(logging.ERROR)
print(logging.CRITICAL)
```

```no-highlight
10
20
30
40
50
```

##### Настраиваем уровень `DEBUG`, как минимальный уровень, с которого требуется вывод логов.

```python
import logging

logging.basicConfig(level=logging.DEBUG)

logging.debug('Это лог уровня DEBUG')
logging.info('Это лог уровня INFO')
logging.warning('Это лог уровня WARNING')
logging.error('Это лог уровня ERROR')
logging.critical('Это лог уровня CRITICAL')
```

И, соответственно, теперь получаем в консоли сообщения всех уровней:

```no-highlight
DEBUG:root:Это лог уровня DEBUG
INFO:root:Это лог уровня INFO
WARNING:root:Это лог уровня WARNING
ERROR:root:Это лог уровня ERROR
CRITICAL:root:Это лог уровня CRITICAL
```

##### Логгеры

Логгеров в проекте может быть сколько угодно, но обычно по одному в каждом модуле проекта, а также корневой логгер (`root`).

```python
import logging

logger = logging.getLogger()

print(logger)
```

Результат работы этого кода:

```bash
<RootLogger root (WARNING)>
```

Имя логгера может быть любым (любой строкой, если быть точнее), но согласно конвенции (неформальной договоренности разработчиков) принято называть логгеры по имени модуля, в котором они создаются, а за имя модуля, как вы знаете, отвечает магическая переменная `__name__`:

```python
import logging

logger = logging.getLogger(__name__)

print(logger)
```

Результат работы кода:

```bash
<Logger __main__ (WARNING)>
```

**Логгеры выстраиваются в иерархию. У каждого логгера, кроме `root`, есть предки.** 

```python
import logging

logger = logging.getLogger()

print(logger.parent)

logger = logging.getLogger(__name__)

print(logger.parent)
```

Результат:

```bash
None
<RootLogger root (WARNING)>
```

То есть, для `root`- логгера в атрибуте `parent` записано значение `None`, а для логгера `__main__` предком является как раз `root`- логгер. Иерархию логгеров можно выстраивать с помощью точечной нотации в имени:

```python
import logging

logger_1 = logging.getLogger('one.two')

print(logger_1.parent)

logger_2 = logging.getLogger('one.two.three')

print(logger_2.parent)
```

Результат:

```bash
<RootLogger root (WARNING)>
<Logger one.two (WARNING)>
```

##### Ключевые моменты:
1. Логгеров с одинаковым именем не может быть больше одного
2. Логгеры следует называть по имени модуля, в котором они создаются, передавая в функцию `getLogger` аргумент `__name__`.
3. Корневым логгером является `root`-логгер
4. У каждого логгера, кроме `root`, есть предки
5. Если в каком-то логгере не заданы его настройки - берутся настройки его предка, вплоть до `root`-логгера.

## Форматтеры

- `%(asctime)s` - время создания лога в виде, понятном человеку. По умолчанию выглядит так - **2023-12-31 11:29:31,689**
- `%(filename)s` - имя модуля, в котором сработал вызов лога
- `%(funcName)s` - имя функции, в которой произошел вызов лога
- `%(levelname)s` - уровень, на котором был вызван данный лог (`DEBUG`, `INFO` и т.п.)
- `%(lineno)d` - номер строки кода, на которой произошел вызов лога
- `%(name)s` - имя логгера
- `%(message)s` - сообщение, которое должно быть выведено вместе с логом

Комбинируя эти объекты форматирования, можно гибко настраивать то как будут выглядеть ваши логи. Мы в курсе будем использовать вот такой вид:

```python
format='[%(asctime)s] #%(levelname)-8s %(filename)s:'
       '%(lineno)d - %(name)s - %(message)s'
```

И выглядеть логи, форматированные таким образом, будут так:

```no-highlight
[2023-12-29 22:56:02,062] #INFO     dispatcher.py:172 - aiogram.event - Update id=563099722 is handled. Duration 272 ms by bot id=6341710373
[2023-12-29 22:56:08,331] #DEBUG    intent_middleware.py:67 - aiogram_dialog.context.intent_middleware - Loading context for intent: `pCh8w8`, stack: ``, user: `173901673`, chat: `173901673`
[2023-12-29 22:56:08,332] #DEBUG    dialog.py:121 - aiogram_dialog.dialog - Dialog render (Dialog 'FSMMainMenu')
```

Самый простой способ - указать форматирование один раз при базовой конфигурации логирования, например, в точке входа в проект:

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format='[%(asctime)s] #%(levelname)-8s %(filename)s:'
           '%(lineno)d - %(name)s - %(message)s'
)

logger = logging.getLogger(__name__)

logger.debug('Лог уровня DEBUG')
```

Результат работы кода:

```no-highlight
[2024-01-02 16:42:22,074] #DEBUG    test_5.py:11 - __main__ - Лог уровня DEBUG
```

Чтобы ее избежать и воспользоваться возможностью форматирования через фигурные скобки, при настройке форматирования нужно еще явно задать значение параметра `style`. По умолчанию в качестве значения этого параметра стоит `'%'`, поэтому и форматирование в первом случае работает, как ожидается. А во втором нужно явно указать `style='{'` и тогда тоже все будет работать:

```python
import logging

logging.basicConfig(
    level=logging.DEBUG,
    format='[{asctime}] #{levelname:8} {filename}:'
           '{lineno} - {name} - {message}',
    style='{'
)

logger = logging.getLogger(__name__)

logger.debug('Лог уровня DEBUG')
```

Результат:

```no-highlight
[2024-01-02 16:51:30,084] #DEBUG    test_5.py:12 - __main__ - Лог уровня DEBUG
```

В общем случае, форматтеры можно создавать так:

```python
import logging

format_1 = '#%(levelname)-8s [%(asctime)s] - %(filename)s:'\
           '%(lineno)d - %(name)s - %(message)s'
format_2 = '[{asctime}] #{levelname:8} {filename}:'\
           '{lineno} - {name} - {message}'

formatter_1 = logging.Formatter(fmt=format_1)
formatter_2 = logging.Formatter(
    fmt=format_2,
    style='{'
)
```

## Хэндлеры

Хэндлеры логов отвечают за то, **куда** какие логи отправлять:
- в stdout
- в stderr
- в файл
- в базу данных
- в специальный сервис хранения и анализа логов
- старшему главному начальнику на телефон
- куда угодно

##### Просмотр хэндлеров логгера

```python
import logging

logger = logging.getLogger(__name__)

print(logger.handlers)
```

##### Хэндлеры для вывода логов в `stderr` и `stdout`

```python
import logging
import sys

logger = logging.getLogger(__name__)

stderr_handler = logging.StreamHandler()
stdout_handler = logging.StreamHandler(sys.stdout)

logger.addHandler(stdout_handler)
logger.addHandler(stderr_handler)

print(logger.handlers)

logger.warning('Это лог с предупреждением!')
```

Результат работы кода:

```bash
[<StreamHandler <stdout> (NOTSET)>, <StreamHandler <stderr> (NOTSET)>]
Это лог с предупреждением!
Это лог с предупреждением!
```
##### Хэндлеры для вывода логов в `stderr` и `stdout` с разным форматированием

Чтобы добавить форматтер к хэндлеру - нужно воспользоваться методом `setFormatter`.

```python
import logging
import sys

# Определяем первый вид форматирования
format_1 = '#%(levelname)-8s [%(asctime)s] - %(filename)s:'\
           '%(lineno)d - %(name)s - %(message)s'
# Определяем второй вид форматирования
format_2 = '[{asctime}] #{levelname:8} {filename}:'\
           '{lineno} - {name} - {message}'

# Инициализируем первый форматтер
formatter_1 = logging.Formatter(fmt=format_1)
# Инициализируем второй форматтер
formatter_2 = logging.Formatter(
    fmt=format_2,
    style='{'
)

# Создаем логгер
logger = logging.getLogger(__name__)

# Инициализируем хэндлер, который будет перенаправлять логи в stderr
stderr_handler = logging.StreamHandler()
# Инициализируем хэндлер, который будет перенаправлять логи в stdout
stdout_handler = logging.StreamHandler(sys.stdout)

# Устанавливаем форматтеры для хэндлеров
stderr_handler.setFormatter(formatter_1)
stdout_handler.setFormatter(formatter_2)

# Добавляем хэндлеры логгеру
logger.addHandler(stdout_handler)
logger.addHandler(stderr_handler)

# Создаем лог
logger.warning('Это лог с предупреждением!')
```

Результат работы:

```no-highlight
[2024-01-02 18:18:27,987] #WARNING  test_5.py:34 - __main__ - Это лог с предупреждением!
#WARNING  [2024-01-02 18:18:27,987] - test_5.py:34 - __main__ - Это лог с предупреждением!
```

##### Хэндлер для записи логов в файл

```python
import logging

logger = logging.getLogger(__name__)

file_handler = logging.FileHandler('logs.log')

logger.addHandler(file_handler)

print(logger.handlers)

logger.warning('Это лог с предупреждением!')
```

Выполнив этот код, получаем результат в консоли:

```bash
[<FileHandler /<здесь_полный_путь_до_файла_с_логами>/logs.log (NOTSET)>]
```

А в файле **logs.log**:

```no-highlight
Это лог с предупреждением!
```

## Фильтры

 ##### Фильтр, который будет передавать в хэндлеры только логи уровня `ERROR`, в которых есть слово "важно", написанное в любом регистре.

```python
import logging


# Определяем свой фильтр, наследуюясь от класса Filter библиотеки logging
class ErrorLogFilter(logging.Filter):
    # Переопределяем метод filter, который принимает `self` и `record`
    # Переменная рекорд будет ссылаться на объект класса LogRecord
    def filter(self, record):
        return record.levelname == 'ERROR' and 'важно' in record.msg.lower()


# Инициализируем логгер
logger = logging.getLogger(__name__)

# Создаем хэндлер, который будет направлять логи в stderr
stderr_handler = logging.StreamHandler()

# Подключаем фильтр к хэндлеру
stderr_handler.addFilter(ErrorLogFilter())

# Подключаем хэндлер к логгеру
logger.addHandler(stderr_handler)

logger.warning('Важно! Это лог с предупреждением!')
logger.error('Важно! Это лог с ошибкой!')
logger.info('Важно! Это лог с уровня INFO!')
logger.error('Это лог с ошибкой!')
```

Результатом работы кода будет:

```no-highlight
Важно! Это лог с ошибкой!
```

##### Фильтр, который будет передавать в хэндлеры логи, только если значение счетчика в цикле `for` будет четным числом.

```python
import logging


# Определяем свой фильтр, наследуюясь от класса Filter библиотеки logging
class EvenLogFilter(logging.Filter):
    def filter(self, record):
        return not record.i % 2


# Инициализируем логгер
logger = logging.getLogger(__name__)

# Создаем хэндлер, который будет направлять логи в stderr
stderr_handler = logging.StreamHandler()

# Подключаем фильтр к хэндлеру
stderr_handler.addFilter(EvenLogFilter())

# Подключаем хэндлер к логгеру
logger.addHandler(stderr_handler)

for i in range(1, 5):
    logger.warning('Важно! Это лог с предупреждением! %d', i, extra={'i': i})
```

Результат работы кода:

```no-highlight
Важно! Это лог с предупреждением! 2
Важно! Это лог с предупреждением! 4
```

## Логирование исключений

##### Лог вместе с трейсбэком ошибки через передачу методу логгера аргумента `exc_info=True`.

```python
import logging

logger = logging.getLogger(__name__)

try:
    print(4 / 2)
    print(2 / 0)
except ZeroDivisionError:
    logger.error('Тут было исключение', exc_info=True)
```

И теперь информация об исключении также записана в лог:

```no-highlight
2.0
Тут было исключение
Traceback (most recent call last):
  File "/<тут_путь_к_файлу>/test_5.py", line 40, in <module>
    print(2 / 0)
          ~~^~~
ZeroDivisionError: division by zero
```

##### Лог вместе с трейсбэком ошибки через вызов метода `exception` у логгера.

```python
import logging

logger = logging.getLogger(__name__)

try:
    print(4 / 2)
    print(2 / 0)
except ZeroDivisionError:
    logger.exception('Тут было исключение')
```

И результат такой же:

```no-highlight
2.0
Тут было исключение
Traceback (most recent call last):
  File "/<тут_путь_к_файлу>/test_5.py", line 40, in <module>
    print(2 / 0)
          ~~^~~
ZeroDivisionError: division by zero
```

