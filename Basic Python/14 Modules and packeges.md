Cписок стандартных библиотек: https://docs.python.org/3/library/index.html

Во время импорта модуля - интерпретатор переводит скрипт в байт-код и выполняет его

```bash
pip install packege_name

pip freeze > requerements.txt # сохранение всех установленных модулей
```

## Пакеты

Пакет в Python — это каталог, который содержит один или несколько модулей и файл `__init__.py`. Этот файл может быть пустым или содержать код, который выполняется при импорте пакета.

![[package.png]]

```python
from courses import NAME, python

from .php import *  # импортируются все функции, указанные в __all__
```
