## Связывание во время выполнения программы

```python
config = load_config("bot.ini")

storage = MemoryStorage()
bot = Bot(token=config.tg_bot.token)
dp = Dispatcher(bot, storage=storage)
```

В данном случае нам лучше всего связывать на уровне выполненния программы,
потому что таким образом предоставляется гибкая настройка бота через файл конфигурации
и в коде не хранятся в открытую подобные данные как токен бота.

## Связывание во время запуска программы

```python
def create_log_info(user: user_models.User,
                    service: str,
                    message: str,
                    level_number: int = choices.LevelNumberChoices.READ) -> models.Log:
    """
    Shortcut function for creating INFO log object
    Info about args in create_log()
    """
    return create_log(user=user,
                      service=service,
                      message=message,
                      level_number=level_number,
                      level=choices.LevelChoices.INFO)
```

Связывание происходит с именованной текстовой константой, которая хранится в Enum, здесь это наиболее эффективное
решение,
так как нет необходимости каждый раз менять название уровня лога(оно общепринято) или обращаться в файл. Если будет
необходимость, то всегда можно открыть файл
с константами и изменить поля.

## Связывание во время каждой новой инизиализации объекта

```python
def __init__(self):
    super().__init__()
    self.fields.get("group_id").choices = get_group_choices()
```

Связывание происходит наиболее в поздний момент: в каждый момент инициализации объекта формы,
так как содержимое может меняться в зависимости от добавления/удаления записей о группах пользователями