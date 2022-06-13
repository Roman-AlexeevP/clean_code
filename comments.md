## 1

```python
# Классы ЭРИ, для которых высчитывается рад.стойкость
ACTIVE_ERI_CATHEGORY = (...)
```

## 2

```python
class DateInput(forms.DateInput):
    input_type = 'date'

    def __init__(self, attrs=None):
        super().__init__(attrs)
        # Формат переопределен, так как по-другому Джанго 3.0.1 не воспринимает даты из виджета календаря
        self.format = ('%Y-%m-%d')
```

## 3

```python
# Для партий изделий с отсутствующим зав.номером создается только один объект
# Вид заводского номера: Дата создания + номер ведомости
if fabric_numbers == set():
    fab_number = f"Создано {timezone.now():%Y-%m-%d} ведомость №{sheet.number}"
    instance = models.Instance(fab_number=fab_number)
    instance.save()
```

## 4

```python
fabric_number_in = Q(fab_number__in=instances_data.get("fabric_numbers"))
# Фильтр для партий изделий(в них количество может быть больше 1 и заводской номер общий)
unnumbered_instances = Q(count__gt=1)
instances = models.Instance.objects
.filter(final_product=instances_data.get("final_product"),
        product=instances_data.get("product"),
        sheet=sheet,
        order=instances_data.get("order"),
        contract=instances_data.get("contract"))
.filter(fabric_number_in | unnumbered_instances)
```

## 5

```python
# Очистка номеров вида 001, 010 от нулей в начале
numbers = set(map(lambda x: int(x.replace(r'(0+)\d+', '')), numbers))
```

## 6

```python
# поставить у ПИ дату погашения и номер гасящего ИИ, is_active = False у ПИ
query = """update on_notice
           set (cancel_date, cancel_notice, is_active)=(current_date, '{}', false)  
           where id={}""".format(output.get("notice_full_number"), notice.get("second_id"))
```

## 7

```python
# Вторичное предъявление создается только для отклоненных предъявлений или с наличием дефекта
case_condition = Case(When(
    ~Q(Exists(have_secondaries_query)) &
    Q(status__in=(choices.StatusChoices.DEFECT, choices.StatusChoices.DECLINED)) &
    Q(is_secondary=False),
    then=Value(True)),
    default=Value(False),
    output_field=BooleanField())
```