return 100 - self.get_percent(declined_by_vp_presentations, PresentationSB.objects.count())
-->
declined_presentations_percent = self.get_percent(declined_by_vp_presentations, PresentationSB.objects.count())
if declined_presentations_percent > 100:
    return -1
return 100 - declined_presentations_percent

\\ Добавил условие на случай неправильного процента отклоненых предъявлений


is_accepted = Q(status=StatusChoices.ACCEPTED)
is_final = Q(is_final=True)
confirmed_instances_count = PresentationSB.objects.filter(is_final_condition & is_accepted_condition)

\\ Сделаны переменные для логических условий: принятые и финишные предъявления

is_accepted = Q(status=StatusChoices.ACCEPTED)
is_first = Q(is_secondary=False)
first_presentation_count = PresentationSB.objects \
            .filter(is_first_condition) \
            .count()

\\ Сделаны переменные для логических условий: первичные и принятые предъявления

if defect_presentation.fabric_numbers_alias != "б/н":
->
if defect_presentation.fabric_numbers_alias != EMPTY_FABRIC_NUMBER

\\ Добавлена константа для обозначения отсутствующего заводского номера

if unit == 'y':
...
elif unit == 'h':
...
->
class UnitChoices(TextChoices):
    YEAR = 'y', 'year'
    HOUR = 'h', 'hour'

\\ Добавлено перечисление для строковых констант выбора единицы измерения

if len(questions_and_choices) < 3:
    return await ctx.send('Need at least 1 question with 2 choices.')
elif len(questions_and_choices) > 21:
    return await ctx.send('You can only have up to 20 choices.')
-->
MIN_USER_CHOICES_COUNT = 3
MAX_USER_CHOICES_COUNT = 21

\\ Добавлены константы для максимального и минимального количества вариантов с вопросами для опросов

gamma = float('{0:.1f}'.format(gamma))
gamma_0 = float('{0:.1f}'.format(gamma_0))
-->
gamma = float('{0:.2f}'.format(gamma))
gamma_0 = float('{0:.2f}'.format(gamma_0))

\\ Изменена точность для расчетов гаммы-показателей

form_number = "null" -> NULL_STRING_VALUE = "null"

\\ Добавлена константа для магической строки с нулл-значением

Локализация -> для каждого поля или модели добавлены атрибут verbose_name и если необходимо verbose_name_plural, для 
корректного отображения на русском языке, для обычного отображения используются оригинальные имена моделей/полей

if notice.get("status") == "Сформировано" and not (context.get("is_chief") or
                                                   context.get("is_owner") or
                                                   context.get("is_archivist"))
-->
has_not_permissions_to_edit = not (context.get("is_chief") or
                                   context.get("is_owner") or
                                   context.get("is_archivist")

\\ Выделено логическое условие по отсутствию разрешений на редактирование записи в отдельную переменную

if notice.get("status") == "Утв. архив" or not notice.get("is_active")
-->
is_unactive = notice.get("status") == "Утв. архив" or not notice.get("is_active")

\\ Выделена логическая переменная для неактивных извещений

if notice.get("status") == "Резерв" and context.get("is_privileged")
-->
can_take_reserve = notice.get("status") == "Резерв" and context.get("is_privileged")

\\ Выделена логическая переменная для определения наличия прав на извлечение из резерва извещения

address.find("127.0.0") --> address.find(LOCALHOST_ADDRESS)

\\ Добавлена константа для адреса локалхоста