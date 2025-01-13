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

## Диплинки

Можно сформировать ссылку вида `t.me/bot?start=xxx` и пре переходе по такой ссылке пользователю покажут кнопку «Начать», при нажатии которой бот получит сообщение `/start xxx`. Т.е. в ссылке зашивается некий дополнительный параметр, не требующий ручного ввода.

```python
import re

from aiogram import F
from aiogram.types import Message
from aiogram.filters import Command, CommandObject, CommandStart

@dp.message(Command("help"))
@dp.message(CommandStart(deep_link=True, magic=F.args == "help"))
async def cmd_start_help(message: Message):
    await message.answer("Это сообщение со справкой")


@dp.message(CommandStart(deep_link=True, magic=F.args.regexp(re.compile(r'book_(\d+)'))))
async def cmd_start_book(message: Message, command: CommandObject):
    book_number = command.args.split("_")[1]
    await message.answer(f"Sending book №{book_number}")
```

## Сервисные (служебные) сообщения

Сообщения в Telegram делятся на текстовые, медиафайлы и служебные (они же — сервисные).

```python
# Отправка приветственного сообщения вошедшему участнику
@dp.message(F.new_chat_members)
async def somebody_added(message: Message):
    for user in message.new_chat_members:
        # проперти full_name берёт сразу имя И фамилию 
        await message.reply(f"Привет, {user.full_name}")
```

## Кнопки

То, что цепляется к низу экрана вашего устройства, будем называть **обычными** кнопками, а то, что цепляется непосредственно к сообщениям, назовём **инлайн**-кнопками.

```python
# Обычные кнопки, расположенные вертикально
@dp.message(Command("start"))
async def cmd_start(message: types.Message):
    kb = [
        [types.KeyboardButton(text="С пюрешкой")],
        [types.KeyboardButton(text="Без пюрешки")]
    ]
    keyboard = types.ReplyKeyboardMarkup(keyboard=kb)
    await message.answer("Как подавать котлеты?", reply_markup=keyboard)
```
По умолчанию «кнопочная» клавиатура должна занимать на смартфонах столько же места, сколько и обычная буквенная. Для уменьшения кнопок к объекту клавиатуры надо указать дополнительный параметр `resize_keyboard=True`.
```python
# Обычные кнопки, расположенные горизонтально
# `input_field_placeholder`, заменит текст в пустой строке ввода, когда активна обычная клавиатура
@dp.message(Command("start"))
async def cmd_start(message: types.Message):
    kb = [
        [
            types.KeyboardButton(text="С пюрешкой"),
            types.KeyboardButton(text="Без пюрешки")
        ],
    ]
    keyboard = types.ReplyKeyboardMarkup(
        keyboard=kb,
        resize_keyboard=True,
        input_field_placeholder="Выберите способ подачи"
    )
    await message.answer("Как подавать котлеты?", reply_markup=keyboard)
```
##### Обработка кнопок
```python
from aiogram import F

@dp.message(F.text.lower() == "с пюрешкой")
async def with_puree(message: types.Message):
    await message.reply("Отличный выбор!")

@dp.message(F.text.lower() == "без пюрешки")
async def without_puree(message: types.Message):
    await message.reply("Так невкусно!")

# Удалить кнопки ReplyKeyboardRemove
@dp.message(F.text.lower() == "без пюрешки")
async def without_puree(message: types.Message):
    await message.reply("Отличный выбор!", reply_markup=types.ReplyKeyboardRemove())
```
##### Keyboard Builder

Для более динамической генерации кнопок можно воспользоваться сборщиком клавиатур.
- `add(<KeyboardButton>)` — добавляет кнопку в память сборщика;
- `adjust(int1, int2, int3...)` — делает строки по `int1, int2, int3...` кнопок;
- `as_markup()` — возвращает готовый объект клавиатуры;
- `button(<params>)` — добавляет кнопку с заданными параметрами, тип кнопки (Reply или Inline) определяется автоматически.

```python
from aiogram.utils.keyboard import ReplyKeyboardBuilder

# Пронумерованная клавиатура размером 4×4
@dp.message(Command("reply_builder"))
async def reply_builder(message: types.Message):
    builder = ReplyKeyboardBuilder()
    for i in range(1, 17):
        builder.add(types.KeyboardButton(text=str(i)))
    builder.adjust(4)
    await message.answer(
        "Выберите число:",
        reply_markup=builder.as_markup(resize_keyboard=True),
    )
```
## Инлайн кнопки

В отличие от обычных кнопок, инлайновые цепляются не к низу экрана, а к сообщению, с которым были отправлены.
##### URL-кнопки
Самые простые инлайн-кнопки относятся к типу URL, т.е. «ссылка». Поддерживаются только протоколы HTTP(S) и tg://
```python
from aiogram.utils.keyboard import InlineKeyboardBuilder

@dp.message(Command("inline_url"))
async def cmd_inline_url(message: types.Message, bot: Bot):
    builder = InlineKeyboardBuilder()
    builder.row(types.InlineKeyboardButton(
        text="GitHub", url="https://github.com")
    )
    builder.row(types.InlineKeyboardButton(
        text="Оф. канал Telegram",
        url="tg://resolve?domain=telegram")
    )

    # Чтобы иметь возможность показать ID-кнопку,
    # У юзера должен быть False флаг has_private_forwards
    user_id = 1234567890
    chat_info = await bot.get_chat(user_id)
    if not chat_info.has_private_forwards:
        builder.row(types.InlineKeyboardButton(
            text="Какой-то пользователь",
            url=f"tg://user?id={user_id}")
        )

    await message.answer(
        'Выберите ссылку',
        reply_markup=builder.as_markup(),
    )
```
##### Колбэки
У колбэк-кнопок есть специальное значение (data), по которому ваше приложение опознаёт, что нажато и что надо сделать. И выбор правильного data **очень важен**!

```python
@dp.message(Command("random"))
async def cmd_random(message: types.Message):
    builder = InlineKeyboardBuilder()
    builder.add(types.InlineKeyboardButton(
        text="Нажми меня",
        callback_data="random_value")
    )
    await message.answer(
        "Нажмите на кнопку, чтобы бот отправил число от 1 до 10",
        reply_markup=builder.as_markup()
    )

# Хэндлер на обработку нажатия кнопки
@dp.callback_query(F.data == "random_value")
async def send_random_value(callback: types.CallbackQuery):
    await callback.message.answer(str(randint(1, 10)))
    await callback.answer(text="Спасибо, что воспользовались ботом!", show_alert=True) # чтобы на кнопки не появлялись часики
```
## Фабрика колбэков

```python
from typing import Optional
from aiogram.filters.callback_data import CallbackData

# Cоздаётся объекты типа `CallbackData`, указывается префикс, 
# описывается структуру, а дальше фреймворк самостоятельно собирает 
# строку с данными колбэка
# Наш класс обязательно должен наследоваться от `CallbackData` и 
# принимать значение префикса. Префикс — это общая подстрока в 
# начале, по которой фреймворк будет определять, какая структура 
# лежит в колбэке.
class NumbersCallbackFactory(CallbackData, prefix="fabnum"):
    action: str
    value: Optional[int] = None

# Функция генерации клавиатуры
# В качестве аргумента `callback_data` вместо строки будем указывать 
# экземпляр нашего класса `NumbersCallbackFactory`
def get_keyboard_fab():
    builder = InlineKeyboardBuilder()
    builder.button(
        text="-2", callback_data=NumbersCallbackFactory(action="change", value=-2)
    )
    builder.button(
        text="-1", callback_data=NumbersCallbackFactory(action="change", value=-1)
    )
    builder.button(
        text="+1", callback_data=NumbersCallbackFactory(action="change", value=1)
    )
    builder.button(
        text="+2", callback_data=NumbersCallbackFactory(action="change", value=2)
    )
    builder.button(
        text="Подтвердить", callback_data=NumbersCallbackFactory(action="finish")
    )
    # Выравниваем кнопки по 4 в ряд, чтобы получилось 4 + 1
    builder.adjust(4)
    return builder.as_markup()

# Методы редактирования сообщения
async def update_num_text_fab(message: types.Message, new_value: int):
    with suppress(TelegramBadRequest):
        await message.edit_text(
            f"Укажите число: {new_value}",
            reply_markup=get_keyboard_fab()
        )
# Метод отправки сообщения
@dp.message(Command("numbers_fab"))
async def cmd_numbers_fab(message: types.Message):
    user_data[message.from_user.id] = 0
    await message.answer("Укажите число: 0", reply_markup=get_keyboard_fab())

# Обработка колбэков
@dp.callback_query(NumbersCallbackFactory.filter())
async def callbacks_num_change_fab(
        callback: types.CallbackQuery, 
        callback_data: NumbersCallbackFactory
):
    # Текущее значение
    user_value = user_data.get(callback.from_user.id, 0)
    # Если число нужно изменить
    if callback_data.action == "change":
        user_data[callback.from_user.id] = user_value + callback_data.value
        await update_num_text_fab(callback.message, user_value + callback_data.value)
    # Если число нужно зафиксировать
    else:
        await callback.message.edit_text(f"Итого: {user_value}")
    await callback.answer()```
Конкретизируем наши хэндлеры и сделаем отдельный обработчик для числовых кнопок и для кнопки «Подтвердить». Фильтровать будем по значению `action` и в этом нам помогут «магические фильтры» aiogram 3.x.
```python
from magic_filter import F

# Нажатие на одну из кнопок: -2, -1, +1, +2
@dp.callback_query(NumbersCallbackFactory.filter(F.action == "change"))
async def callbacks_num_change_fab(
        callback: types.CallbackQuery, 
        callback_data: NumbersCallbackFactory
):
    # Текущее значение
    user_value = user_data.get(callback.from_user.id, 0)

    user_data[callback.from_user.id] = user_value + callback_data.value
    await update_num_text_fab(callback.message, user_value + callback_data.value)
    await callback.answer()


# Нажатие на кнопку "подтвердить"
@dp.callback_query(NumbersCallbackFactory.filter(F.action == "finish"))
async def callbacks_num_finish_fab(callback: types.CallbackQuery):
    # Текущее значение
    user_value = user_data.get(callback.from_user.id, 0)

    await callback.message.edit_text(f"Итого: {user_value}")
    await callback.answer()
```
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
![[Pasted image 20250113112728.png]]
1. Диспетчер — корневой роутер.
2. Хэндлеры цепляются к роутерам.
3. Роутеры могут быть вложенными, но между ними только однонаправленная связь.
4. Порядок включения (и, соответственно, проверки) роутеров явно определён.
![[Pasted image 20250113112808.png]]
