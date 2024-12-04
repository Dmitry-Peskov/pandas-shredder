# data-shredder
Пакет, предоставляющий интерфейс для нарезки Pandas.DataFrame на отдельные файлы с заданным количеством строк в каждом из них.  

Предположим у вас есть файл _**big_data.csv**_ на **7 000 000** строк, расположенный в _**C:\\Documents**_.  
И у вас существует потребность сделать из него несколько файлов формата **xlsx** на **500 000** строк каждый.

_**Shredder**_ поможет Вам реализовать эту задумку в несколько строк кода.

Создадим DataFrame на основе данных из нашего файла _**C:\\Documents\\big_data.csv**_ и инициализируем Shredder

```python
import pandas as pd
from pandas_shredder import Shredder


data = pd.read_csv("C:\\Documents\\big_data.csv")

shredder = Shredder(
    directory="C:\\Documents\\result",
    extension="xlsx",
    lines_per_serving=500_000
)
```

Запустим измельчитель вызвав `run()`, передав в него набор данных и имя для выходных файлов.
```python
shredder.run(
    dataframe=df,
    file_name="small_data"
)
```
В результате выполнения кода, в каталоге _**C:\\Documents\\result**_ вас будут ожидать файлы:
- 1_small_data.xlsx
- 2_small_data.xlsx
- 3_small_data.xlsx
- ...

Для сохранения данных в файлы, используются стандартные методы **pandas**. Вы можете сконфигурировать процесс сохранения через именованные аргументы переданные в `run()` вместо `**kwargs`.  
Например:
```python
shredder.run(
    dataframe=df,
    file_name="small_data",
    index=False,
    header=False,
    sheet_name="data"
)
```
Возможные именованные аргументы зависят от выбранного в `extension` расширения. Ознакомьтесь с таблицей ниже, что бы узнать больше.

| **extension** | **pandas method**                                                                                       |
|---------------|---------------------------------------------------------------------------------------------------------|
| csv           | [to_csv()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_csv.html)     |
| xlsx          | [to_excel()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_excel.html) |
| html          | [to_html()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_html.html)   |
| json          | [to_json()](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.to_json.html)   |
| xml           | [to_xml()](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_xml.html)                                                                                            |
