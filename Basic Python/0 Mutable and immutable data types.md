##### Изменяемые объекты (Mutable)

Изменяемые объекты могут изменять свое содержимое после создания. Это означает, что вы можете изменять, добавлять или удалять элементы без создания нового объекта. К изменяемым типам данных в Python относятся:
- **Списки (list)**: Позволяют хранить упорядоченные коллекции элементов, которые можно изменять.
- **Словари (dict)**: Хранят пары "ключ-значение" и позволяют изменять их содержимое.
- **Множества (set)**: Неупорядоченные коллекции уникальных элементов, которые также можно изменять.
- **Байтовые массивы (bytearray)**: Позволяют хранить изменяемые последовательности байтов.

 **Важные аспекты**
1. **Передача по ссылке**: Изменяемые объекты передаются по ссылке, что означает, что несколько переменных могут указывать на один и тот же объект. Изменение объекта через одну переменную отразится на всех остальных.
2. **Создание копий**: Чтобы избежать нежелательных изменений, можно создавать копии изменяемых объектов:

```python
original_list = [1, 2, 3]
copied_list = original_list[:]  # Создание копии списка
```
##### Неизменяемые объекты (Immutable)

Неизменяемые объекты не могут быть изменены после их создания. Если вы попытаетесь изменить такой объект, будет создан новый объект с новым значением. К неизменяемым типам данных в Python относятся:

- **Целые числа (int)**: Не могут быть изменены; при присвоении нового значения создается новый объект.
- **Числа с плавающей точкой (float)**: Аналогично целым числам.
- **Строки (str)**: Неизменяемы; любые операции с ними создают новые строки.
- **Кортежи (tuple)**: Упорядоченные коллекции, которые нельзя изменить после создания.
- **Байты (bytes)**: Неизменяемая последовательность байтов.
- **frozenset**: Неизменяемая версия множества.