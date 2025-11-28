# Міністерство освіти і науки України
## Національнийй технічний університет "Харківський політехнічний інститут" Кафедра Автоматики та управління в технічних системах
## *Лабораторна робота з дисципліни: "Прикладне програмування на Python"*


Виконав: 
студент групи ІКМ-722а 
**Москальчук О. О.**

**Перевірив:**  
**д.т.н. Караман Д.Г.**



**Харків – 2025**


### Мета роботи: Ознайомитися зі способами створення та використання підпрограм, модулів і пакетів у Python. Навчитися структурувати програму шляхом поділу її на функціональні частини та модулі. Закріпити вміння застосовувати стандартні модулі мови (наприклад, statistics).

# Блок схема файлу stats_module.py

![блок схема](images/stats_module.png)

# Блок схема файлу main.py
![блок схема](images/main.png)

# Блок схема файлу _init_.py
![блок схема](images/init.png) 

---
# Файл stats_module.py

## Імпорт модуля

```python
import statistics
```

## Функція повертає середнє значення

```python
def get_average(values):
    average = sum(values) / len(values)
    return average
```

## Функція повертає мінімальне значення

```python
def get_min(values):
    minimum = min(values)
    return minimum
```

## Функція повертає максимальне значення

```python
def get_max(values):
    maximum = max(values) 
    return maximum
```

## Функція повертає медіану

```python
def get_median(values):
    median = statistics.median(values) 
    return median
```

## Функція знаходить різкі перепади між сусідніми значеннями

```python
def find_jumps(values, threshold):
    jumps = {"index": [], "value1": [], "value2": []}
    for i in range(1, len(values)-1):
        if abs(values[i+1] - values[i]) > threshold:
            jumps["index"].append(i)
            jumps["value1"].append(values[i])
            jumps["value2"].append(values[i+1])
    return jumps
```

## Функція виводить таблицю значень

```python
def show_table(data_dict, title):
    print (f"\nТаблиця показників: {title}")
    print ("Мітка часу | Значення")
    for time_label, value in data_dict[title].items():
        print (f"   {time_label}   \t |  {value}")
```

## Функція перетворює текстовий ввід на список чисел

```python
def parse_input(input_str):
    data = input_str.split()
    dataFloat = [float(value) for value in data]
    return dataFloat
```

## Функція формує словник даних

```python
def create_data_dict(temp_list, hum_list, pres_list):
    data_dict = {
        "temperature": {},
        "humidity": {},
        "pressure": {}
    }
    for i in range(len(temp_list)):
        time = f"T{i+1}"
        data_dict["temperature"][time] = temp_list[i]
        data_dict["humidity"][time] = hum_list[i]
        data_dict["pressure"][time] = pres_list[i]

    return data_dict
```

-----

# Головний файл (main.py)

## Імпорт та функція обробки

```python
import stats_module as sm

def process_measurements(title, data_dict, threshold):
    print (f"=== Обробка показників для:{title}===")
    value = list(data_dict[title].values())
    sm.show_table(data_dict, title)
    
    avg = sm.get_average(value)
    print (f"Середні значення: {avg:.2f}")
    
    min_val = sm.get_min(value)
    print (f"Мінімальні значення: {min_val:.2f}")
    
    max_val = sm.get_max(value)
    print (f"Максимальні значення: {max_val:.2f}")
    
    med = sm.get_median(value)
    print (f"Медіанні значення: {med:.2f}")
    
    jumps = sm.find_jumps(value, threshold)
    if jumps["index"]:
        print("Виявлені стрибки:")
        for i in range(len(jumps["index"])):
            print(f"  Між T{jumps['index'][i]+1} і T{jumps['index'][i]+2}: "
                  f"{jumps['value1'][i]} → {jumps['value2'][i]}")
    else:
        print("Стрибків не виявлено.")
```

## Головна функція

```python
def parse_input(input_str):
    data = input_str.split()
    dataFloat = []
    for value in data:
        dataFloat.append(float(value))
    return dataFloat

def create_data_dict(temp_list, hum_list, pres_list):
    data_dict = {
        "temperature": {},
        "humidity": {},
        "pressure": {}
    }
    for i in range(len(temp_list)):
        time = f"T{i+1}"
        data_dict["temperature"][time] = temp_list[i]
        data_dict["humidity"][time] = hum_list[i]
        data_dict["pressure"][time] = pres_list[i]

    return data_dict

def process_measurements(title, data_dict, threshold):
    print (f"=== Обробка показників для:{title}===")
    value = list(data_dict[title].values())
    sm.show_table(data_dict, title)
    avg = sm.get_average(value)
    print (f"Середні значення: {avg:.2f}")
    min = sm.get_min(value)
    print (f"Мінімальні значення: {min:.2f}")
    max = sm.get_max(value)
    print (f"Максимальні значення: {max:.2f}")
    med = sm.get_median(value)
    print (f"Медіанні значення: {med:.2f}")
    
    jumps = sm.find_jumps(value, threshold)
    if jumps["index"]:
        print("Виявлені стрибки:")
        for i in range(len(jumps["index"])):
            print(f"  Між T{jumps['index'][i]+1} і T{jumps['index'][i]+2}: "
                  f"{jumps['value1'][i]} → {jumps['value2'][i]}")
    else:
        print("Стрибків не виявлено.")
    
def main():
    print("=== Обробка показів системи 'Розумний будинок' ===")
    temperature_input = input("Введіть показники температури (через пробіл): ")
    humidity_input = input("Введіть показники вологості (через пробіл): ")
    pressure_input = input("Введіть показники тиску (через пробіл): ")
    temp_list = parse_input(temperature_input)
    hum_list = parse_input(humidity_input)
    pres_list = parse_input(pressure_input)
    data_dict = create_data_dict(temp_list, hum_list, pres_list)
    process_measurements("temperature", data_dict, threshold=7)
    process_measurements("humidity", data_dict, threshold=20)
    process_measurements("pressure", data_dict, threshold=5000)
    

if __name__ == "__main__":
```
# Файл sensors/ __ init __ .py
 забезпечує використання модулів у складі пакета sensors дозволяє отримувати доступ до функцій модуля stats_module напряму без необхідності використання import sensors.stats_module, а потім sensors.stats_module.get_average(...) або import sensors.stats_module as sm, а потім sm.get_average(...) також можна: from sensors import stats_module → stats_module.get_average(...) також можна: from sensors.stats_module import get_average → get_average(...)
 ``` python
from .stats_module import (
    get_average,
    get_min,
    get_max,
    get_median,
    find_jumps,
    show_table
)
```

 визначає, що можна імпортувати, а що - заборонено (службові елементи) при використанні варіанту конструкції: from sensors import *

``` python
__all__ = [
    "get_average",
    "get_min",
    "get_max",
    "get_median",
    "find_jumps",
    "show_table"
]
```
# Приклад роботи програми
```
=== Обробка показів системи 'Розумний будинок' ===
Введіть показники температури (через пробіл): 20 10 19 18 20
Введіть показники вологості (через пробіл): 12 20 29 15 16
Введіть показники тиску (через пробіл): 1320 1456 1345 1567 1678
=== Обробка показників для:temperature===

Таблиця показників: temperature
Мітка часу | Значення
   T1            |  20.0
   T2            |  10.0
   T3            |  19.0
   T4            |  18.0
   T5            |  20.0
Середні значення: 17.40
Мінімальні значення: 10.00
Максимальні значення: 20.00
Медіанні значення: 19.00
Виявлені стрибки:
  Між T1 і T2: 20.0 → 10.0
  Між T2 і T3: 10.0 → 19.0
=== Обробка показників для:humidity===

Таблиця показників: humidity
Мітка часу | Значення
   T1            |  12.0
   T2            |  20.0
   T3            |  29.0
   T4            |  15.0
   T5            |  16.0
Середні значення: 18.40
Мінімальні значення: 12.00
Максимальні значення: 29.00
Медіанні значення: 16.00
Стрибків не виявлено.
=== Обробка показників для:pressure===

Таблиця показників: pressure
Мітка часу | Значення
   T1            |  1320.0
   T2            |  1456.0
   T3            |  1345.0
   T4            |  1567.0
   T5            |  1678.0
Середні значення: 1473.20
Мінімальні значення: 1320.00
Максимальні значення: 1678.00
Медіанні значення: 1456.00
Стрибків не виявлено.
``` 
### Висновок: ознайомилися зі способами створення та використання підпрограм, модулів і пакетів у Python. Навчитися структурувати програму шляхом поділу її на функціональні частини та модулі. Закріпити вміння застосовувати стандартні модулі мови (наприклад, statistics).

### Github репозиторій:
[Посилання](https://github.com/)