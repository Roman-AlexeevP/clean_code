## 1

```python
context["product_choices"] = db.get_all_products_for_choice()  # Получить список всех изделий для селекта
```

->

```python
context["product_choices"] = db.get_all_products_for_choice()
```

П.4 шум: Убран очевидный избыточный комментарий

## 2

```python
# db.resource_param(request=request, product_id=product_id, product_selected=product_selected,
#                   context=context, initial_values=initial_values, parsed_eri=parsed_eri,
#                   general_resume=general_resume)
product_id = int(product_id)
```

-->

```python
product_id = int(product_id)
```

П.11 закомментрированный код: убран закомментированный код

## 3

```python
# check_user(req=request, ctx=context, just_for_registered_users=False, just_for_super=False)
```

П.11 закомментрированный код: убран закомментированный код

## 4

```python
# Срок службы ЭРИ будет сравне со сроком службы изделия
# Если срок службы изделия не с даты ввода в эксплуатацию, а с даты изготовления или приемки,
# то в срок службы сравнения надо включить срок хранения
if initial_values.get('start_life_code') == 'date_vp_producer':
    product_selected["work_end"] = start_life + max(life_entered, term_of_keeping)
    initial_values["comment"] = "от изготовления"
elif moment_name == "just_work":
    product_selected["work_end"] = start_life + life_entered
elif moment_name == "with_keeping":
    product_selected["work_end"] = start_life + term_of_keeping + life_entered
    initial_values["comment"] = "c учетом хранения"
```

-->

```python
eri_life_parameters: dict = get_eri_life_data()
instance_life_parameters: dict = get_instance_life_data()
work_end_date = calculate_work_end_date(eri_life_parameters, instance_life_parameters)
```

П. 12 функции вместо комментариев: логика разобрана по функциям и описана во внутренней реализации

## 5

```python
# Форма фильтрации записей
filter_form = InstPartFilterForm()
```

->

```python
filter_form = InstPartFilterForm()
```

п.4 шум: убран очевидный комментарий

## 6

```python
id_2_export = request.GET.get("id", None)
record_d4 = PassportD4.objects.get(id=id_2_export)
# Шрифты и размеры для генерации документа описаны в модуле on.generation.d4_doc_settings.py
if username == record_d4.owner:
    document = create_final_document(record_d4=record_d4)
```

-->

```python
id_2_export = request.GET.get("id", None)
record_d4 = PassportD4.objects.get(id=id_2_export)
if username == record_d4.owner:
    document = create_final_document(record_d4=record_d4)
```

П.9 нелокальная информация: информация по генерации документа убрана из комментария в документацию функции

## 7

```python
# Форма хранится в контексте в формате:
# Поле: [[Значение, Примечание], [Значение, Примечание], ...], аналогично хранятся и поля в БД для показателей надежности
add_requirement_form_to_context(gid=gid, passp_id=passport_id, ctx=context)
```

-->

```python
# Форма хранится в контексте в формате:
# Поле: [[Значение, Примечание], [Значение, Примечание], ...]
add_requirement_form_to_context(gid=gid, passp_id=passport_id, ctx=context)
```

П.9 нелокальная информация: информация по БД в данном комментарии нелокальна и убрана

## 8

```python
# создание записи об элементе Д4 в таблицу ElementD4
# SUBBLOCK_FIELDS = ['id', 'dec_number', 'sub_block_name', 'sub_block_count', 'comment',
#                       'block_id', 'item_xml_id', 'pos_name', 'is_hidden', 'removed']
new_el_id, new_elem_created = db.get_or_create_elementd4_record(sublock_item)
```

-->

```python
new_el_id, new_elem_created = db.get_or_create_elementd4_record(sublock_item)
```

П.4 шум: Убран громоздкий комментарий с очевидной информацией, информация по поля указана в
get_or_create_elementd4_record()

## 9

```python
# Поиск ЭРИ по imbase и потом по наименованию и ТУ
if imbase_number is not None and len(imbase_number) > 5:
    query = "select id from on_eri where imbase='" + imbase_number + "'"
    self.cursor.execute(query)
    eri_record = self.cursor.fetchone()
if eri_record is None and eri_ty is not None and eri_name is not None:
    query = "select id from on_eri where long_name='" + eri_name + "' and ty = '" + eri_ty + "'"
    self.cursor.execute(query)
    eri_record = self.cursor.fetchone()
# Конец поиска
if eri_record is not None:
    return eri_record[0], False
else:
    ...
следующий
код
```

-->

```python
eri_data = search_by_imbase_number(imbase_number, eri_data)
if eri_data is not None:
    return eri_data
...
следующий
код
```

п.6(п.12) комментарии для границ и вместо кода: комментарии убраны из-за добавления новых функций с инкапсуляции логики
последовательного поиска внутри

## 10

```python
# Соответствующие названия полей в on_elementd4
field_alias = [...]
```

->

```python
element_d4_field_names = [...]
```

п.12 комментарий вместо имени переменной: изменено имя переменной и комментарий перестает быть необходимым

## 11

```python
# Очистка номеров вида 001, 010 от нулей в начале
numbers = set(map(lambda x: int(x.replace(r'(0+)\d+', '')), numbers))
```

-->

```python
numbers = clear_zeros_from_numbers(numbers)
```

п.12 комментарий вместо функции: Добавлена функция для большей наглядности и убран комментарий

## 12

```python
# el.ntd_selected
ntd_selected = json.loads(item[6])
for param in item[5]:
    for param_selected in ntd_selected:
        if param.get("key") == param_selected.get("key"):
            param["ntd_selected"] = param_selected.get("value")
            break

# el.scheme
scheme = json.loads(item[7])
for param in item[5]:
    for param_scheme_value in scheme:
        if param.get("key") == param_scheme_value.get("key"):
            param["scheme"] = param_scheme_value.get("value")
            break
```

-->

```python
parameters.update(get_ntd_selected(item[NTD_SELECTED_INDEX]))
parameters.update(get_scheme_values(item[SCHEME_VALUES_INDEX]))
```

п.1 и п.12 неочевидный комментарий вместо кода: реализация убрана в функции и комментарий убран

## 13

```python
# удаление нежелательных символов
for key in items_to_update.keys():
    item = items_to_update.get(key)
    if item.get("pos_name") is not None:
        item["pos_name"] = item.get("pos_name").replace("\n", "").replace("\r", "")
        .replace("\t", "").replace(u'\xa0', "").replace("\"", "").replace("\'", "")
        .replace("...'", "-").replace("..'", "-").replace("['", "(").replace("]'", ")").replace(" ", "")
```

-->

```python
remove_special_symbols(items_to_update, "pos_name") 
```

п.12 комментарий вместо кода: реализация убрана в функции и комментарий убран

## 14

```python
# Проверка есть ли паспорт с таким id
query = """select id from on_passportd4 where id={}""".format(passp_id)
self.cursor.execute(query)
result = self.cursor.fetchone()
if result is None or len(result) == 0:
    return False
```
-->
```python
exist_passport = exists_passport(passport_id)
if exist_passport:
    ... следующий код
```
п.12 комментарий вместо кода: реализация убрана в функции и комментарий убран
## 15

```python
# context[MSG] = calc_gamma_decorated(g, g_0, life, unit)
```
п.11 закомментрированный код: убран закомментированный код