## Комментарии TODO

### 1

```python
def get_sheet_names_by_zip(self) -> list:
    """
     Возвращает название страниц в xlsx файле, быстрее во много раз чем  через чтение pandas
    """
    # TODO: Не используется в данный момент из-за проблем с совместимостью на сервере с ZipFile, необходимо починить
    with ZipFile(self.file) as zipped_file:
        summary = zipped_file.open(r'xl/workbook.xml').read()
    soup = BeautifulSoup(summary, 'xml')
    return [sheet.get('name') for sheet in soup.find_all("sheet")]
```

### 2

```python
# TODO: необходимо оптимизировать и основной запрос и данный подзапрос для использования на сайте
# В данный момент слишком длительное время выполнения, если его добавлять к запросу
"""
elementsd4_amount_subquery = Subquery(ElementD4.objects.filter(reference_id=OuterRef('pk'))
                                       .values('reference_id')
                                       .annotate(count=Count('pk'))
                                       .values('count'))
"""
```

### 3

```python
# TODO: необходимо исправить все .get() запросы на .get_object_or_404() до 22.06.22
presentationsb = models.PresentationSB.objects.get(pk=self.kwargs.get("pk"))
```

### 4

```python
# Добавление пользователя происходит не через ORM
# Создается raw SQL из-за отсутствия поддержки m2m в PostgreSQL <= 10.X
# TODO: После обновления БД исправить на ORM-query
user = User.objects.get(pk=request.POST.get('user_id', None))
group = Group.objects.get(pk=request.POST.get('group_id', None))
insert_user_in_group(user.id, group.id)
```

## Информативный комментарий

### 5

```python
# добавление пользователю прав для использования журнала извещений из-за специфики БД происходит подобным образом:
# Создается объект для модели Developer и пользователь заносится в группу "Журнал извещений"
developer = Developer.objects.create(**user_data)
insert_user_in_group(user_data.get('id'), Group.objects.get(name='Журнал извещений').id)
```

### 6

```python
fabric_number_in = Q(fab_number__in=instances_data.get("fabric_numbers"))
# Фильтр для партий изделий(в них количество может быть больше 1 и заводской номер общий)
unnumbered_instances = Q(count__gt=1)
```

->

```python
fabric_number_in = Q(fab_number__in=instances_data.get("fabric_numbers"))
# Для изделий без заводских номеров в БД хранится запись с автоматически сгенерированным зав.номером
# и значением в поле count > 0, поэтому создается отдельный фильтр для совместного учета
unnumbered_instances = Q(count__gt=1)
```

### 7

```python
# Поиск времени в формате "в/на hh:mm или H утра/вечера/ночи"
regex_delta_absolute = r"\b(?:в|на) (\d\d:\d\d|\d?(?: вечера| утра| ночи)?)\b"
```

## Усиление

### 8 

```python
# ВАЖНО! Иначе цикл проверки не запустится без прогрузки бота
@check_reminders.before_loop
async def before_checking(self):
    await self.bot.wait_until_ready()
```

## Прояснение 

### 9

```python
def get_count_positional_numbers(positional_numbers_string):
# Виды возможного ввода:
# Строка вида "DA28-DA30, DA31.1 - DA31.3, DA31.5, DA44-DA46, DA47, DA87-DA90, DA91-DA94, DA95-DA97"
```

### 10

```python
# Форма хранится в контексте в формате:
# Поле: [[Значение, Примечание], [Значение, Примечание], ...], аналогично хранятся и поля в БД для показателей надежности
add_requirement_form_to_context(gid=gid, passp_id=passport_id, ctx=context)
```

## Информативные 

### 11

```python
id_2_export = request.GET.get("id", None)
record_d4 = PassportD4.objects.get(id=id_2_export)
# Шрифты и размеры для генерации документа описаны в модуле on.generation.d4_doc_settings.py
if username == record_d4.owner:
    document = create_final_document(record_d4=record_d4)
```

### 12

```python

# ПОГАШЕНИЕ ПИ = создание ИИ
# Гасящее ИИ к не первой части ПИ наследует номер от первого ИИ, гасящего какую-либо часть этого ПИ
# Берем номер, год, тип и проверям нет ли гасящего ИИ
query = """select cancel_notice from on_notice where number='{}' and year_number='{}' 
                and kind='ПИ' and cancel_date is not null 
                order by id DESC limit 1""".format(pi_number, new_notice.get("year_number"))
```