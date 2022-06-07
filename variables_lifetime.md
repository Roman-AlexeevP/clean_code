## 1

```python
today = datetime.today()
month_begin = date(today.year, today.month, 1)
context["time_period"] = f"{month_begin:%d.%m.%Y}-{today:%d.%m.%Y}"
```

-->

```python
context["time_period"] = utils.get_current_month_period()
```

Логика вынесена в локальный метод, чтобы не было возможности неправильно вручную задать период

## 2

```python
stats_info = selector.get_all_stats()
context.update(stats_info)
```

-->

```python
context.update(selector.get_all_stats())
```

Значение сразу передается в обновление словаря, чтобы не изменить лишний раз переменную

## 3

```python
fab_number_alias = self.request.POST.get('fabric_numbers').lower()
presentation = self.get_object()
presentation.is_repair = form.cleaned_data.get("is_repair", False)
presentation.save()
form.cleaned_data.update({"fab_number_alias": fab_number_alias})
```

-->

```python
presentation = self.get_object()
presentation.is_repair = form.cleaned_data.get("is_repair", False)
presentation.save()

fab_number_alias = self.request.POST.get('fabric_numbers').lower()
form.cleaned_data.update({"fab_number_alias": fab_number_alias})
```

Сгруппированы методы в одной подгруппе обновление предъявления, в другой добавление переменной в словарь

## 4

```python
def get_initial(self):
    initial = super().get_initial()
    presentation = self.get_object()
    if not self.extra_context:
        self.extra_context = {"presentation": presentation}
    self.extra_context.update({"presentation": presentation})

    if presentation.presented_instances.count() > 0:
        instance = presentation.presented_instances.first()
        initial_values = {
            "final_product": instance.final_product,
            "product": instance.product,
            "sheet_number": instance.sheet.number,
            "count_instances": presentation.instances_count,
            "fabric_numbers": presentation.fabric_numbers_alias,
            "order": instance.order,
            "contract": presentation.contract,
            "contract_part": instance.contract_part,
            "is_repair": presentation.is_repair
        }
        initial.update(initial_values)
    return initial
```

-->

```python
def get_initial(self):
    presentation = self.get_object()
    self.update_extra_context(presentation=presentation)

    selector = self.get_selector()
    initial = super().get_initial()
    initial.update(selector.get_initial_for_form(presentation))

    return initial
```

Логика по методам вынесена, обращение к переменной `initial` сделано после ее инициализации, работа
с `extra_context` вынесенав локальный метод

## 5

```python
def get_context_data(self, **kwargs):
    context = super().get_context_data(**kwargs)
    status_form = forms.ChangeStatusForm(initial={"status": self.object.status})
    status_form.fields["instances"].queryset = self.object.presented_instances.all()
    context["status_form"] = status_form
    return context
```

-->

```python
def get_context_data(self, **kwargs):
    status_form = forms.ChangeStatusForm(initial={"status": self.object.status})
    status_form.fields["instances"].queryset = self.object.presented_instances.all()

    context = super().get_context_data(**kwargs)
    context["status_form"] = status_form
    return context
```

Вызов инициализации контекста смещен к его фактическому обновлению

## 6

```python
queryset = selector.get_defect_operations(defectPTK)
return queryset
```

-->

```python
return selector.get_defect_operations(defectPTK)
```

Убрано лишнее использование переменной

## 7

```python
document = create_final_document(record_d4=record_d4)

f = BytesIO()
document.save(f)
path_to_d4 = TEMP_FILES + '{} Д4.docx'.format(record_d4.dec_number)
document.save(path_to_d4)

record_d4.docx_file = '{} Д4.docx'.format(record_d4.dec_number)
record_d4.save()

length = f.tell()
f.seek(0)
file_name = escape_uri_path('{} Д4.docx'.format(record_d4.dec_number))
response = HttpResponse(
    f.getvalue(),
    content_type='application/vnd.openxmlformats-officedocument.wordprocessingml.document'
)
response['Content-Disposition'] = "attachment; filename=" + file_name
response['Content-Length'] = length
return response
```

-->

```python
document = create_final_document(record_d4=record_d4)
docx_file = save_document_to_path(record_d4)
return generate_docx_file_response(docx_file)
```

Сгруппированы обращения к record_d4 по отдельным методам, вся логика убрана в методы

## 8

```python
pid = request.POST.get('passport_id', None)
passport_this = PassportD4.objects.get(id=pid)
if username == passport_this.owner:
    passport_this.status = 4
    passport_this.dec_number = '{}-del'.format(passport_this.dec_number)
    passport_this.save()
```

-->

```python
delete_passport(passport_id=request.POST.get('passport_id', None))
```

Вся работа с паспортом убрана во внутренний метод, чтобы не трогать базу
и объект паспорта из контроллера

## 9

```python
passport_this = PassportD4.objects.get(id=pid)
dec_number = passport_this.dec_number

if dec_number.find('-') != -1:
    new_dec_number = dec_number.split('-')[0] + '-XX'
else:
    new_dec_number = dec_number + '-XX'

passport_this.id = None
passport_this.owner = username
passport_this.status = 1
passport_this.created = datetime.now()
passport_this.docx_file = None
passport_this.dec_number = new_dec_number
passport_this.save()
new_passport_id = passport_this.id
```

-->

```python
new_passport = get_copied_passport(passport_id=passport_id)
```

Работа с объектом паспорта внесена на более локальный уровень в отдельную функцию

## 10

```python
part_edit_form = None
part = None
if part_id and part_id.isnumeric():
    part = InstPart.objects.get(id=part_id)
    part_edit_form = EditInstPartForm(instance=part)
    context["product"] = part.instance.product
    context["instance"] = part.instance
    context["part_id"] = part_id
    context["form"] = part_edit_form
```

-->

```python
if part_id and part_id.isnumeric():
    context.update(get_part_info_by_id(part_id))
```

Работа с переменными `part`, `part_edit_form` инкапсулирована в методе-селекторе
и работа с ними сгруппирована там же

## 11

```python
fid = int(fid)
context["fid"] = fid
fail = Fail.objects.get(id=fid)
fail_instance = fail.fail_instance
fail_part = fail.fail_part
product = fail_instance.product
add_file_form = UploadFileForFail()
add_file_form.fields.get("fid").initial = fid
add_file_form.fields.get("file").disabled = True
context["fid"] = str(fail.id)
context['product'] = product
context['instance'] = fail_instance
context['part'] = fail_part
context['fail'] = fail
context['add_file_form'] = add_file_form
```

-->

```python
add_file_form = generate_fail_form(fid)
fail_data = get_fail_data_by_id(fail_id)
context.update(add_file_form, fail_data)
```

Сгруппированы методы для получения формы, информации по отказу и заполнению контекста

## 12

```python
new_notice_form = EditNoticeForm()
...
код
if request.method == 'GET':
    ...
    куча
    кода, действительно
    огромная
    new_notice_form.fields.get("per_page").initial = per_page
    ...
    аналогичная
    куча
    кода
    context["new_notice_form"] = new_notice_form
    return response(context=context)
```

-->

```python
if request.method == 'GET':
    new_notice_form = EditNoticeForm(initial={"per_page": per_page})
    context["new_notice_form"] = new_notice_form
    return response(context=context)
```

Уменьшено окно работы с формой и контекстом и переменной `per_page`

## 13

```python
params = {}
db = RirvonDatabase()
query = 'SELECT distinct test_kind FROM rad_radprotocol where test_kind is not null'
db.cursor.execute(query)
params["test_kind"] = tuple(map(lambda x: (x[0], x[0]), db.cursor.fetchall()))
query2 = 'SELECT distinct eri_type FROM rad_radprotocol where eri_type is not null'
db.cursor.execute(query2)
params["eri_type"] = tuple(map(lambda x: (x[0], x[0]), db.cursor.fetchall()))
db.close_connection()

```
-->
```python
rad_parameters = {
    "eri_type": get_eri_type(),
    "test_kind": get_test_kind()
}
```

Все инкапсулировано по методам, словарь создается и наполняется в одной строчке

## 14

```python
gamma_form = GammaProForm()
gamma_form.fields["life"].initial = 20
gamma_form.fields["gamma"].initial = 99
gamma_form.fields["gamma_0"].initial = 50
gamma_form.fields["unit"].initial = 'y'
context['gamma_form'] = gamma_form
if request.method == 'POST':
    life = request.POST.get('life')
    gamma_form.fields["life"].initial = life
    g = request.POST.get('gamma')
    gamma_form.fields["gamma"].initial = g
    g_0 = request.POST.get('gamma_0')
    gamma_form.fields["gamma_0"].initial = g_0
    unit = request.POST.get('unit')
    gamma_form.fields["unit"].initial = unit
    context['gamma_form'] = gamma_form
    # context[MSG] = calc_gamma_decorated(g, g_0, life, unit)
    answer = calc_gamma_decorated(gamma=g.replace(',', '.'), gamma_0=g_0.replace(',', '.'),
                                  delta_t=life.replace(',', '.'), unit=unit)
    if answer is not False:
        context["msg"] = answer
```
-->
```python
gamma_form = GammaProForm(initial=GAMMA_INITIAL)
context['gamma_form'] = gamma_form
if request.method == 'POST':
    gamma_form = GammaProForm(initial=request.POST)
    life = request.POST.get('life')
    gamma = request.POST.get('gamma')
    gamma_0 = request.POST.get('gamma_0')
    unit = request.POST.get('unit')

    result = calc_gamma_decorated(gamma=gamma.replace(',', '.'),
                                  gamma_0=gamma_0.replace(',', '.'),
                                  delta_t=life.replace(',', '.'),
                                  unit=unit)
    context['gamma_form'] = gamma_form
    context["result"] = result
```

Обновление контекста сгруппировано, поправлены обращения к полям на нативный способ, вместо ручного установления,
добавлена константа для инициализации формы

## 15

```python
def __init__(self):
    super().__init__()
    idb = InstanceDB()
    group_choice = [('-', '-')]
    group_choice.extend(idb.get_groups_list())
    self.fields.get("group_id").choices = tuple(group_choice)
    product_choice = [('-', '-')]
    product_choice.extend(idb.get_product_list())
    self.fields.get("product_id").choices = tuple(product_choice)
    year_choice = [('-', '-')]
    query = """select DISTINCT(to_char(fail_message_date_904, 'YYYY')) as yr from on_fail where fail_message_date_904 is not null order by yr DESC;"""
    idb.cursor.execute(query)
    years = idb.cursor.fetchall()
    if years is not None and len(years) > 0:
        year_choice.extend([(x[0], x[0]) for x in years])
        self.fields.get("year").choices = tuple(year_choice)
    idb.close_connection()
```
-->
```python
def __init__(self):
    super().__init__()
    self.fields.get("group_id").choices = get_group_choices()
    
    self.fields.get("product_id").choices = get_product_choices()

    self.fields.get("year").choices = get_years_choices()
```
Работа с каждым конкретным полем формы сгруппирована в отдельных методах, убран прямой запрос в БД