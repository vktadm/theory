## Разделы

- **Конфигурационные данные**
- **Точка входа (запуск бота)**
- **Middleware**
- **Фильтры**
- **Хэндлеры**
- **Бизнес-логика**
- **Взаимодействие с БД**
- **Взаимодействие с внешними API**
- **Лексикон бота**
- **Клавиатуры**
- **Состояния**
- **Тесты**
- **Обработчики ошибок**
- **Утилиты**
## Базовая конфигурация
```python
import asyncio
import logging
from aiogram import Bot, Dispatcher, types
from aiogram.filters.command import Command

# Включаем логирование, чтобы не пропустить важные сообщения
logging.basicConfig(level=logging.INFO)
# Объект бота
bot = Bot(token="12345678:AaBbCcDdEeFfGgHh")
# Диспетчер
dp = Dispatcher()

# Хэндлер на команду /start
@dp.message(Command("start"))
async def cmd_start(message: types.Message):
    await message.answer("Hello!")

# Запуск процесса поллинга новых апдейтов
async def main():
    await dp.start_polling(bot)

if __name__ == "__main__":
    asyncio.run(main())
```

## Отправка сообщений
```python
@dp.message(Command("answer"))
async def cmd_answer(message: types.Message):
    await message.answer("Это простой ответ")


@dp.message(Command("reply"))
async def cmd_reply(message: types.Message):
    await message.reply('Это ответ с "ответом"')
```

## Фильтры

```python
def my_start_filter(message: Message) -> bool:  
    return message.text == '/start'


@dp.message(my_start_filter)  
async def process_start_command(message: Message):  
    await message.answer(text='Это команда /start')
    
# Или с lambda
@dp.message(lambda msg: msg.text == '/start')  
async def process_start_command(message: Message):  
    await message.answer(text='Это команда /start')
```

##### ContentType

[❗️Документация по типам контента](https://docs.aiogram.dev/en/dev-3.x/api/enums/content_type.html#module-aiogram.enums.content_type)

```python
ContentType.AUDIO == 'audio'                                # True
ContentType.TEXT == 'text'                                  # True
ContentType.PHOTO == 'photo'                                # True
ContentType.STICKER == 'sticker'                            # True
ContentType.CONTACT == 'contact'                            # True
ContentType.LOCATION == 'location'                          # True
ContentType.POLL == 'poll'                                  # True
ContentType.SUCCESSFUL_PAYMENT == 'successful_payment'      # True
ContentType.VOICE == 'voice'                                # True
ContentType.WEB_APP_DATA == 'web_app_data'                  # True
```

```python
# Этот хэндлер будет срабатывать на тип контента "photo"
@dp.message(F.content_type == ContentType.PHOTO)
async def process_send_photo(message: Message):
    await message.answer(text='Вы прислали фото')


# Этот хэндлер будет срабатывать на тип контента "photo"
@dp.message(F.content_type == 'photo')
async def process_send_photo(message: Message):
    await message.answer(text='Вы прислали фото')


# Этот хэндлер будет срабатывать на тип контента "photo"
@dp.message(F.photo)
async def process_send_photo(message: Message):
    await message.answer(text='Вы прислали фото')
```

```python
# Этот хэндлер будет срабатывать на тип контента "voice", "video" или "text"
@dp.message(F.content_type.in_({'voice', 'video', 'text'}))
async def process_send_vovite(message: Message):
    await message.answer(text='Вы прислали войс, видео или текст')


# Этот хэндлер будет срабатывать на тип контента "voice", "video" или "text"
@dp.message(F.content_type.in_({ContentType.VOICE,
                                ContentType.VIDEO,
                                ContentType.TEXT}))
async def process_send_vovite(message: Message):
    await message.answer(text='Вы прислали войс, видео или текст')
```

##### Встроенные фильтры [❗️Документация по встроенным фильтрам](https://docs.aiogram.dev/en/dev-3.x/dispatcher/filters/index.html#builtin-filters)

##### Магические фильтры

```python
from aiogram import F
F.photo                                    # Фильтр для фото
F.voice                                    # Фильтр для голосовых сообщений
F.content_type.in_({ContentType.PHOTO,
                    ContentType.VOICE,
                    ContentType.VIDEO})    # Фильтр на несколько типов контента
F.text == 'привет'                         # Фильтр на полное совпадение текста
F.text.startswith('привет')                # Фильтр на то, что текст сообщения начинается с 'привет'
~F.text.endswith('bot')                    # Инвертирование результата фильтра
```

**Примеры**

```python
# Фильтр, который будет пропускать только апдейты от пользователя с ID = 173901673
lambda message: message.from_user.id == 173901673
F.from_user.id == 173901673

# Фильтр, который будет пропускать только апдейты от админов из списка 193905674, 173901673, 144941561
lambda message: message.from_user.id in {193905674, 173901673, 144941561}
F.from_user.id.in_({193905674, 173901673, 144941561})

# Фильтр, который будет пропускать апдейты текстового типа, кроме тех, которые начинаются со слова "Привет"
lambda message: not message.text.startswith('Привет')
~F.text.startswith('Привет')

# Фильтр, который будет пропускать апдейты любого типа, кроме фото, видео, аудио и документов
lambda message: not message.content_type in {ContentType.PHOTO, ContentType.VIDEO, ContentType.AUDIO, ContentType.DOCUMENT}
~F.content_type.in_({ContentType.PHOTO, ContentType.VIDEO, ContentType.AUDIO, ContentType.DOCUMENT})
```

##### Магический метод __call__

```python
class MyClass:
    def __init__(self) -> None:
        pass

    def __call__(self) -> str:
        return 'Результат вызова экземпляра класса'


my_class_1 = MyClass()
my_class_2 = MyClass()

print(my_class_1()) # Результат вызова экземпляра класса
print(my_class_2()) # Результат вызова экземпляра класса
```

##### Собственные фильтры

```python
from aiogram.filters import BaseFilter

# Это список администраторов бота
admin_ids: list[int] = [173901673, 178876776, 197177271]

class IsAdmin(BaseFilter):
    def __init__(self, admin_ids: list[int]) -> None:
        self.admin_ids = admin_ids

    async def __call__(self, message: Message) -> bool:
        return message.from_user.id in self.admin_ids
```

**❗️Ваш ID можно узнать из любого апдейта от вас вашему боту - `print(message.from_user.id)`**

##### Комбинирование фильтров

1. Если фильтры указать через запятую, то между ними будет проверяться условие "И"
```python
@dp.message(F.photo, F.from_user.id == 173901673) # True and True == True
@dp.message(F.photo, F.voice) # True and False == False
```
2. Если нужно, чтобы фильтры работали по условию "ИЛИ", то есть хэндлер срабатывал бы тогда, когда хотя бы один фильтр из цепочки возвращал `True` - нужно декорировать хэндлер столько раз, сколько фильтров в цепочке.
```python
@dp.message(F.content_type == ContentType.VIDEO) @dp.message(F.text.lower().startswith('привет'))
```
3. Если нужно инвертировать результат работы фильтра - используется знак `~` перед фильтром
```python
@dp.message(~F.text.startswith('привет'))
```
4. Магические фильтры можно комбинировать, используя операции побитового сравнения И/ИЛИ (`&` / `|`)
```python
@dp.message((F.text | F.photo) & F.from_user.id == 173901673)
```
5. В качестве способа объединения фильтров можно использовать функции `and_f()`, `or_f()`, `invert_f()`, импортируемые из `aiogram.filters`
```python
from aiogram.filters import and_f

# ...

@dp.message(and_f(F.text.endswith('bot'), F.from_user.id == 173901673))
```

## Абсолютный и относительный импорт

В Python существуют абсолютные и относительные импорты.

- **Абсолютный импорт:** Использует полный путь к модулю, начиная от корневой папки проекта.
- **Относительный импорт:** Использует относительный путь к модулю, начиная от текущего модуля. Существует два типа относительных импортов: явный и неявный (не поддерживается в Python 3). В относительном импорте с помощью точек можно дойти только до директории, содержащей запущенный из командной строки скрипт.

Относительные импорты использовать не рекомендуется, и самое оправданное их использование - внутри файлов `__init__.py`.

Можно контролировать доступные для импорта объекты с помощью переменной `__all__`.
##### Некоторые аспекты, связанные с импортами:

1. Абсолютные импорты отсчитываются от директории, в которой находится исполняемый файл, то есть файл, который в процессе выполнения, имеет имя `__main__`.
    
2. В исполняемом файле - точке входа в проект, не может, по умолчанию, быть относительных импортов - только абсолютные.
    
3. Относительными импортами, без "хаков" и "костылей" нельзя подниматься на уровень, в котором находится исполняемый файл - интерпретатор выдаст ошибку.
    
4. Относительные импорты могут быть оправданы внутри пакетов, но надежнее использовать абсолютные импорты, если вы пока не "набили руку".
    
5. Чтобы модули были доступны на уровне пакетов - используйте инициализатор пакета `__init__.py`, в котором пропишите импорты этих модулей.
    
6. Импорты со звездочкой оправданы, когда надо получить доступ к объектам модулей, подпакетов, модулей в подпакетах и т.д. на уровне пакета.
    
7. Контролировать то, что можно импортировать из модуля, при использовании импорта со звездочкой, можно либо называя скрытые объекты с нижнего подчеркивания, либо перечисляя доступные объекты в переменной `__all__`.
    
8. Чтобы избежать перекрытий имен объектов - помните о порядке поиска модулей и пакетов интерпретатором. Что первое будет найдено, то и импортируется.
    
    - Сначала идет поиск модулей и пакетов во встроенных модулях (`builtin_module_names`)
        
    - Затем поиск в корне проекта (в той же директории, что и исполняемый файл)
        
    - Затем идет обращение к переменной `PYTHONPATH`
        
    - Далее идет поиск в модулях стандартной библиотеки (`stdlib_module_names`)
        
    - В самую последнюю очередь поиск модулей и пакетов происходит среди установленных сторонних библиотек в папке `site-packages`
        
9. Не допускайте циклических импортов - интерпретатор выдаст ошибку.

## Точка входа в приложение
**`bot.py`**
```python
import asyncio
from aiogram import Bot, Dispatcher


# Запуск бота
async def main():
    bot = Bot(token="TOKEN")
    dp = Dispatcher()

    # Запускаем бота и пропускаем все накопленные входящие
    # Да, этот метод можно вызвать даже если у вас поллинг
    await bot.delete_webhook(drop_pending_updates=True)
    await dp.start_polling(bot)


if __name__ == "__main__":
    asyncio.run(main())
```

## Роутеры

### main.py

```python
import asyncio

from aiogram import Bot, Dispatcher
from config_data.config import Config, load_config
from handlers import other_handlers, user_handlers


# Функция конфигурирования и запуска бота
async def main():

    # Загружаем конфиг в переменную config
    config: Config = load_config()
    
    # Инициализируем бот и диспетчер
    bot = Bot(token=config.tg_bot.token)
    dp = Dispatcher()

    # Регистриуем роутеры в диспетчере
    dp.include_router(user_handlers.router)
    dp.include_router(other_handlers.router)

    # Пропускаем накопившиеся апдейты и запускаем polling
    await bot.delete_webhook(drop_pending_updates=True)
    await dp.start_polling(bot)


asyncio.run(main())
```

### user_handlers.py

```python
from aiogram import Router
from aiogram.filters import Command, CommandStart
from aiogram.types import Message
from lexicon.lexicon import LEXICON_RU

# Инициализируем роутер уровня модуля
router = Router()


# Этот хэндлер срабатывает на команду /start
@router.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(text=LEXICON_RU['/start'])


# Этот хэндлер срабатывает на команду /help
@router.message(Command(commands='help'))
async def process_help_command(message: Message):
    await message.answer(text=LEXICON_RU['/help'])
```

### other_handlers.py

```python
from aiogram import Router
from aiogram.types import Message
from lexicon.lexicon import LEXICON_RU

# Инициализируем роутер уровня модуля
router = Router()


# Этот хэндлер будет срабатывать на любые ваши сообщения,
# кроме команд "/start" и "/help"
@router.message()
async def send_echo(message: Message):
    try:
        await message.send_copy(chat_id=message.chat.id)
    except TypeError:
        await message.reply(text=LEXICON_RU['no_echo'])
```

```python
router.message.filter(KnownUser())


@router.message(filter_1, filter_2)
async def handler_1(message: Message):
    # ...


@router.message(filter_3)
async def handler_2(message: Message):
    # ...


@router.message(filter_4, filter_5)
async def handler_3(message: Message):
    # ...


@router.message(filter_6)
async def handler_4(message: Message):
    # ...
```

## Кнопки

Два основных вида кнопок: обычные (reply) и инлайн (inline).
##### Обычные (reply) кнопки

Для работы с обычными кнопками из `aiogram.types` понадобятся следующие классы:

- `ReplyKeyboardMarkup` - для создания объекта клавиатуры ([ссылка](https://core.telegram.org/bots/api#replykeyboardmarkup) на документацию Telegram Bot API)
- `KeyboardButton` - для создания кнопок клавиатуры ([ссылка](https://core.telegram.org/bots/api#keyboardbutton) на документацию Telegram Bot API)
- `ReplyKeyboardRemove` - для удаления клавиатуры ([ссылка](https://core.telegram.org/bots/api#replykeyboardremove) на документацию Telegram Bot API)

```python
# Создаем объекты кнопок
button_1 = KeyboardButton(text='Собак 🦮')
button_2 = KeyboardButton(text='Огурцов 🥒')

# Создаем объект клавиатуры, добавляя в него кнопки
keyboard = ReplyKeyboardMarkup(
    keyboard=[[button_1, button_2]],
    resize_keyboard=True,
    one_time_keyboard=True
)
```

## KeyboardBuilder ("строитель клавиатур")

##### Метод row()

```python
# Инициализируем билдер
kb_builder = ReplyKeyboardBuilder()

# Создаем список с кнопками
buttons: list[KeyboardButton] = [
    KeyboardButton(text=f'Кнопка {i + 1}') for i in range(10)
]

# Распаковываем список с кнопками в билдер, указываем, что
# в одном ряду должно быть 4 кнопки
kb_builder.row(*buttons, width=4)


# Этот хэндлер будет срабатывать на команду "/start"
# и отправлять в чат клавиатуру
@dp.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(
        text='Вот такая получается клавиатура',
        reply_markup=kb_builder.as_markup(resize_keyboard=True)
    )
```

Метод `row` у класса `ReplyKeyboardBuilder` позволяет расположить кнопки клавиатуры автоматически, в зависимости от параметра `width` - желаемого количества кнопок в ряду.
##### Метод add()

В отличие от метода `row()` метод `add()` добавляет кнопки с нового ряда только если в предыдущем ряду для новых кнопок уже нет места. Причем, методу `add` все равно какой там у вас был параметр `width` до этого. Кнопки будут добавляться в ряд пока их там не станет 8 и только потом начнут заполнять новый ряд. Тоже до 8 штук.

```python
# Инициализируем билдер
kb_builder = ReplyKeyboardBuilder()

# Создаем первый список с кнопками
buttons_1: list[KeyboardButton] = [
    KeyboardButton(text=f'Кн. {i + 1}') for i in range(5)
]
# Создаем второй список с кнопками
buttons_2: list[KeyboardButton] = [
    KeyboardButton(text=f'Кн. {i + 6}') for i in range(10)
]
# Распаковываем список с кнопками в билдер методом row,
# указываем, что в одном ряду должно быть 4 кнопки
kb_builder.row(*buttons_1, width=4)

# Распаковываем второй список с кнопками методом add
kb_builder.add(*buttons_2)


# Этот хэндлер будет срабатывать на команду "/start"
# и отправлять в чат клавиатуру
@dp.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(
        text='Вот такая получается клавиатура',
        reply_markup=kb_builder.as_markup(resize_keyboard=True)
    )
```

##### Метод adjust()

Чтобы указать какое количество кнопок должно быть в каждом ряду - нужно передать в метод `adjust` целые числа (от 1 до 8), начиная с первого ряда. Причем данный метод будет игнорировать параметр `width`, если кнопки были добавлены в билдер методом `row`.

Создадим клавиатуру, добавив 8 кнопок методом `add` и расставим их так, чтобы в 1-м ряду была одна кнопка, во втором - 3, а остальные расставились бы автоматически.

```python
# Инициализируем билдер
kb_builder = ReplyKeyboardBuilder()

# Создаем первый список с кнопками
buttons_1: list[KeyboardButton] = [
    KeyboardButton(text=f'Кнопка {i + 1}') for i in range(8)
]
# Распаковываем список с кнопками методом add
kb_builder.add(*buttons_1)

# Явно сообщаем билдеру сколько хотим видеть кнопок в 1-м и 2-м рядах
kb_builder.adjust(1, 3)


# Этот хэндлер будет срабатывать на команду "/start"
# и отправлять в чат клавиатуру
@dp.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(
        text='Вот такая получается клавиатура',
        reply_markup=kb_builder.as_markup(resize_keyboard=True)
    )
```


Также у метода `adjust` есть параметр `repeat`, который по умолчанию равен `False`. Если сделать его `True`, то значения количества кнопок по рядам будут повторяться для новых рядов с кнопками.

Создадим клавиатуру с 10 кнопками, переданными в билдер методом `add` и методом `adjust` разместим их по две в каждом нечетном ряду и по 1 в каждом четном.

```python
# Инициализируем билдер
kb_builder = ReplyKeyboardBuilder()

# Создаем первый список с кнопками
buttons_1: list[KeyboardButton] = [
    KeyboardButton(text=f'Кнопка {i + 1}') for i in range(10)
]
# Распаковываем список с кнопками методом add
kb_builder.add(*buttons_1)

# Явно сообщаем билдеру сколько хотим видеть кнопок в 1-м и 2-м рядах,
# а также говорим методу повторять такое размещение для остальных рядов
kb_builder.adjust(2, 1, repeat=True)


# Этот хэндлер будет срабатывать на команду "/start"
# и отправлять в чат клавиатуру
@dp.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(
        text='Вот такая получается клавиатура',
        reply_markup=kb_builder.as_markup(resize_keyboard=True)
    )
```

## Метод as_markup()

Метод `as_markup` превращает билдер в объект `ReplyKeyboardMarkup`, передавая ему в качестве аргументов клавиатуру, которую мы в билдере построили как массив массивов с кнопками, а также другие дополнительные аргументы.

https://core.telegram.org/bots/api#replykeyboardmarkup

## Специальные обычные кнопки

Существует 6 типов специальных обычных кнопок:

- Для отправки своего телефона (параметр `request_contact`)
- Для отправки своей геопозиции (параметр `request_location`)
- Для создания опроса/викторины (параметр `request_poll`)
- Для запуска Web-приложений прямо в Телеграм (параметр `web_app`)
- (параметр `request_user`)
- (параметр `request_chat`)

## Инлайн-кнопки

За создание инлайн-кнопок отвечают следующие классы из `aiogram.types`:

- `InlineKeyboardMarkup` - объект клавиатуры, принимающий список рядов кнопок (объектов `InlineKeyboardButton`)
- `InlineKeyboardButton` - объект инлайн-кнопки

Кнопки календаря из библиотеки `aiogram_calendar`: 

![[aiogram_calendar.avif]]
##### URL-кнопки

URL-кнопки - это такие инлайн-кнопки, нажатие на которые переводит нас в браузер по ссылке, связанной с этой кнопкой, или на какой-то внутренний ресурс самого Телеграм (канал, группу и т.п.) тоже по ссылке. За них отвечает атрибут `url` класса `InlineKeyboardButton`. Давайте посмотрим, как такие кнопки работают.

```python
# Создаем объекты инлайн-кнопок
url_button_1 = InlineKeyboardButton(
    text='Курс "Телеграм-боты на Python и AIOgram"',
    url='https://stepik.org/120924'
)
url_button_2 = InlineKeyboardButton(
    text='Документация Telegram Bot API',
    url='https://core.telegram.org/bots/api'
)

# Создаем объект инлайн-клавиатуры
keyboard = InlineKeyboardMarkup(
    inline_keyboard=[[url_button_1],
                     [url_button_2]]
)


# Этот хэндлер будет срабатывать на команду "/start"
# и отправлять в чат клавиатуру c url-кнопками
@dp.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(
        text='Это инлайн-кнопки с параметром "url"',
        reply_markup=keyboard
    )
```

##### Callback-кнопки

```python
# Создаем объекты инлайн-кнопок
big_button_1 = InlineKeyboardButton(
    text='БОЛЬШАЯ КНОПКА 1',
    callback_data='big_button_1_pressed'
)

big_button_2 = InlineKeyboardButton(
    text='БОЛЬШАЯ КНОПКА 2',
    callback_data='big_button_2_pressed'
)

# Создаем объект инлайн-клавиатуры
keyboard = InlineKeyboardMarkup(
    inline_keyboard=[[big_button_1],
                     [big_button_2]]
)


# Этот хэндлер будет срабатывать на команду "/start"
# и отправлять в чат клавиатуру с инлайн-кнопками
@dp.message(CommandStart())
async def process_start_command(message: Message):
    await message.answer(
        text='Это инлайн-кнопки. Нажми на любую!',
        reply_markup=keyboard
)
```

```python
from aiogram import F
from aiogram.types import CallbackQuery

# ...

# Этот хэндлер будет срабатывать на апдейт типа CallbackQuery
# с data 'big_button_1_pressed' или 'big_button_2_pressed'
@dp.callback_query(F.data.in_(['big_button_1_pressed',
                               'big_button_2_pressed']))
async def process_buttons_press(callback: CallbackQuery):
    await callback.answer()

# ...
```

Считается хорошим тоном отвечать на каждое нажатие callback-кнопки, чтобы бот не "зависал в задумчивости". Для этого, как в нашем примере, достаточно в конце хэндлера вызывать `callback.answer()`, даже если никаких действий на нажатие кнопки не предполагается.

```python
# ...

# Этот хэндлер будет срабатывать на апдейт типа CallbackQuery
# с data 'big_button_1_pressed'
@dp.callback_query(F.data == 'big_button_1_pressed')
async def process_button_1_press(callback: CallbackQuery):
    if callback.message.text != 'Была нажата БОЛЬШАЯ КНОПКА 1':
        await callback.message.edit_text(
            text='Была нажата БОЛЬШАЯ КНОПКА 1',
            reply_markup=callback.message.reply_markup
        )
    await callback.answer()


# Этот хэндлер будет срабатывать на апдейт типа CallbackQuery
# с data 'big_button_2_pressed'
@dp.callback_query(F.data == 'big_button_2_pressed')
async def process_button_2_press(callback: CallbackQuery):
    if callback.message.text != 'Была нажата БОЛЬШАЯ КНОПКА 2':
        await callback.message.edit_text(
            text='Была нажата БОЛЬШАЯ КНОПКА 2',
            reply_markup=callback.message.reply_markup
        )
    await callback.answer()

#...
```

##### Принцип работы с инлайн-кнопками с параметром `callback_data`:

1. Создаем инлайн-кнопки с текстом, который будет отображаться на кнопке, и текстом в `callback_data`, который будет приходить в апдейте типа `CallbackQuery` в поле `data`.
2. Создаем объект инлайн-клавиатуры и добавляем в него массив массивов кнопок.
3. Отправляем инлайн-клавиатуру вместе с текстом сообщения пользователю (параметр `reply_markup`).
4. Методом `callback_query` у диспетчера (роутера) ловим апдейт типа `CallbackQuery`, фильтруем его по полю `data` и направляем в соответствующий хэндлер.
5. В хэндлере либо модифицируем сообщение (текст и/или кнопки), либо отправляем пустой ответ `callback.answer()`, чтобы у пользователя не было ощущения, что бот завис в задумчивости.
6. Повторяем сначала - столько раз, сколько необходимо.
##### `callback.answer()`дополнительные параметры:

- `text` - отвечает за текст всплывающего на пару секунд окна нотификации или алерта (до 200 символов)
- `show_alert` - отвечает за превращение всплывающего и самостоятельно исчезающего окошка нотификации в алерт, требующий закрытия, если установить равным `True`. По умолчанию - `False`.
- `url` - ссылка, которая будет открыта клиентом пользователя (поддерживаются не любые ссылки, а только специфические)
- `cache_time` - отвечает за максимальное время (в секундах) кэширования результата запроса обратного вызова клиентом пользователя.
## InlineKeyboardBuilder ("строитель инлайн-клавиатур")

1. В рамках класса доступны основные методы:
    
    - `row()` - для добавления в билдер списка кнопок. Параметр `width` отвечает за то, сколько кнопок будет в одном ряду, когда клавиатура будет отправлена пользователю
        
    - `add()` - этим методом кнопки добавляются в последний ряд клавиатуры, если их там еще меньше 8. Если в последнем ряду уже итак 8 кнопок - кнопки добавляются в новый ряд. Метод `add()` игнорирует параметр `width` метода `row()`, если он вызывался ранее. То есть кнопки в ряду заполняются до 8, не зависимо от того. что указано в `width` метода `row()`
        
    - `adjust()` - метод, позволяющий расположить кнопки, добавленные методом `row()`, в кастомной конфигурации. Метод игнорирует параметр `width` метода `row()`
        
    - `copy()` - метод, позволяющий получить полную копию билдера. Например, если вы хотите создать похожую клавиатуру, модифицировав старую, но не меняя саму старую клавиатуру
        
    - `export()` - метод, позволяющий получить список списков с кнопками из билдера
        
    - `as_markup()` - метод, превращающий билдер в объект инлайн-клавиатуры `InlineKeyboardMarkup`

## Особенности работы с фабрикой коллбэков

При работе с фабрикой коллбэков нужно держать в голове некоторые особенности/ограничения:

1. Длина `callback_data` для инлайн-кнопок ограничена 64 байтами. Это не очень много, но в целом, достаточно для реализации многих задумок.
2. В качестве разделителя данных в `callback_data`, по умолчанию используется двоеточие. Но можно заменить на другой символ или группу символов. За это отвечает параметр `sep`. Имеет смысл менять разделитель в том случае, если у вас могут оказаться двоеточия в данных, из которых вы строите `callback_data`.
3. Если вы формируете инлайн-клавиатуру как список списков кнопок, то экземпляр класса вашей фабрики необходимо упаковать в строку с помощью метода `pack()`.
4. Если вы формируете инлайн-клавиатуру с помощью билдера, добавляя аргументы в метод `button()` - упаковывать экземпляр класса фабрики в строку не надо.
5. Использование незащищенной фабрики коллбэков - это потенциальная угроза безопасности для вашего сервиса, потому что недобросовестные пользователи могут подменять `callback_data`, отправляя вашему боту запросы через сервера телеграм с данными кнопок, которые вы пользователю не отправляли.

## Middleware

**Middleware** - это от слов _middle_ (середина) и _ware_ (изделие). В контексте IT - это промежуточное программное обеспечение. Какой-то код, который вклинивается в основной процесс взаимодействия (пайплайн). С целью модификации или обогащения данных, валидации их, отклонения дальнейшей их обработки и т.п.

##### Middleware как класс

```python
from typing import Any, Awaitable, Callable, Dict

from aiogram import BaseMiddleware
from aiogram.types import TelegramObject


class SomeMiddleware(BaseMiddleware):
    async def __call__(
        self,
        handler: Callable[[TelegramObject, Dict[str, Any]], Awaitable[Any]],
        event: TelegramObject,
        data: Dict[str, Any]
    ) -> Any:

        # ...
        # Здесь выполняется код на входе в middleware
        # ...

        result = await handler(event, data)

        # ...
        # Здесь выполняется код на выходе из middleware
        # ...

        return result
```

 **Важно соблюдать следующие требования:**
- Пользовательские миддлвари должны наследоваться от класса `BaseMiddleware`.
- У пользовательской миддлвари должен быть обязательно реализован асинхронный метод `__call__`.
- Метод `__call__` всегда принимает 4 обязательных аргумента:
    - `self` - ссылка на экземпляр класса. Это ООП, друзья.
    - `handler` - объект хэндлера. Для outer middleware он не определен, потому что пока не пройдены фильтры мы не знаем есть ли вообще подходящий для данного апдейта хэндлер.
    - `event` - `TelegramObject` - тип события, которое хотим обработать (`Update`, `Message`, `CallbackQuery`...).
    - `data` - словарь с данными, ассоциированными с текущим апдейтом.

Для подключения миддлварей используется следующая конструкция:
```
<имя_роутера>.<тип_события>.<тип_миддлвари>(<имя_миддлвари>())
```

**Пример 1.** Регистрируем внешнюю миддлварь, в которую будут попадать все апдейты.

```python
dp.update.outer_middleware(SomeOuterMiddleware())
dp.update.middleware(SomeOuterMiddleware())
```
**Пример 2.** Подключаем внешнюю миддлварь к корневому роутеру для событий типа `Message`.

```python
dp.message.outer_middleware(SomeOuterMiddleware())
```

**Пример 3.** Подключаем к роутеру `some_router` внутреннюю миддлварь на апдейты типа `CallbackQuery`.

```python
some_router.callback_query.middleware(SomeInnerMiddleware())
```

**Пример 4.** Подключаем внешнюю миддлварь к диспетчеру на события типа `CallbackQuery`, а внутреннюю на события типа `Message` к роутеру `some_router`.

```python
dp.callback_query.outer_middleware(SomeOuterMiddleware()) some_router.message.middleware(SomeInnerMiddleware())
```

##### Типовые задачи, решаемые миддлварями
1. Обращение в базу данных для получения роли пользователя
2. Теневой бан
```python
from typing import Any, Awaitable, Callable, Dict

from aiogram import BaseMiddleware
from aiogram.types import TelegramObject, User

CACHE = {
    'banned': [254443334, 214454432, 112221212],
}


class ShadowBanMiddleware(BaseMiddleware):

    async def __call__(
        self,
        handler: Callable[[TelegramObject, Dict[str, Any]], Awaitable[Any]],
        event: TelegramObject,
        data: Dict[str, Any]
    ) -> Any:
        
        user: User = data.get('event_from_user')
        if user is not None:
            if user.id in CACHE.get('banned'):
                return

        return await handler(event, data)
```

```python
dp.update.middleware(ShadowBanMiddleware())
```
1. Передача коннекта к базе данных в фильтры и хэндлеры
2. Определение языка пользователя для подготовки переводов
3. Определение часового пояса пользователя
4. Замер времени выполнения для поиска узких мест
5. Кеширование
6. Троттлинг
```python
from typing import Any, Awaitable, Callable, Dict

from aiogram import BaseMiddleware
from aiogram.types import TelegramObject, User
from cachetools import TTLCache

CACHE = TTLCache(maxsize=10_000, ttl=5)  # Максимальный размер кэша - 10000 ключей, а время жизни ключа - 5 секунд

class ThrottlingMiddleware(BaseMiddleware):

    async def __call__(
            self,
            handler: Callable[[TelegramObject, Dict[str, Any]], Awaitable[Any]],
            event: TelegramObject,
            data: Dict[str, Any],
    ) -> Any:
        user: User = data.get('event_from_user')

        if user.id in CACHE:
            return

        CACHE[user.id] = True

        return await handler(event, data)
```

```python
dp.update.middleware(ThrottlingMiddleware())
```
