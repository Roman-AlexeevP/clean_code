# 1

```python
for i in range(len(NOTICE_FIELDS_NAME)):
    dict_result[NOTICE_FIELDS_NAME[i]] = query_result[i]
```
-->
```python
notices_data = dict(zip(NOTICE_FIELDS_NAME, query_result))
```

# 2

```python
def list_to_line_with_sep(some_list, sep):
    if isinstance(some_list, list):
        line = ''
        for i in range(len(some_list)):
            line += some_list[i] + sep
        return line[:-len(sep)]
    else:
        return False
```
-->
```python
def iterable_to_separated_string(iterable: Iterable,
                                 separator: Optional[int, str]) -> str:
    return f"{separator}".join(iterable)
```

# 3

```python
def query_result_to_dict(field_list, query_result):
    list_of_dict = []
    for line in query_result:
        new_dict = {}
        for i in range(len(line)):
            new_dict[field_list[i]] = '-' if line[i] is None else line[i]
        list_of_dict.append(new_dict)
    return list_of_dict
```
-->
```python
def query_result_to_dict(field_names: Iterable,
                         query_result: Iterable,
                         default: Any = "-") -> dict:
    return (
        {field_name: field_value or delault} for field_name, field_value in zip(field_names, field_values)
        for field_values in query_result
    )

```

# 4

```python
for i in range(len(list_of_eri)):
    choices_list.append(list_of_eri[i].pop("short_name_choice"))
```
-->
```python
choices = [eri.pop("short_name_choice") for eri in eri_list]
```

# 5

```python
for i in range(len(fields_product)):
    if fields_product[i] in numeric_fields:
        output[fields_product[i]] = transform_to_number(res[i])
    else:
        output[fields_product[i]] = res[i]
```
-->
```python
requirements_data = ({field: transform_to_number(value) if field in numeric_fields else field}
                     for field, value in zip(fields_product, result_data))

```