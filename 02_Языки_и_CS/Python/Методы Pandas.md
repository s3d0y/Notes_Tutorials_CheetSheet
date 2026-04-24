
 - **.loc[]** — это метод, который принимает только индексные метки и возвращает строку или фрейм данных, если индексная метка существует в вызывающем фрейме данных.
 
``` Python

df.loc[6, 'char']
# Пример вывода
	'a6'

df.loc[6:17:2, 'char']
# Пример вывода
6 a6
8 a8
10 a10
12 a12
14 a14
16 a16
Name: char, dtype: object

df.loc[df.char == 'test']

```

- **.iloc** — это метод, который позволяет находить и извлекать строки и столбцы на основе их целочисленных позиций.Чтобы выполнить нарезку с помощью iloc[], нужно указать индексы строк и столбцов, которые нужно включить в фрагментированный фрейм данных.Синтаксис аналогичен традиционной нарезке массива, что делает его интуитивно понятным для пользователей Python.

``` Python
df.iloc[1:5, 2:4]

# извлекает строки 2–5 и столбцы 3–4 из фрейма данных.
```

можно присваивать новые значения \

```Python
df.iloc[6:17:2, 1] = 'test'

# Пример вывода
6     test
8     test
10    test
12    test
14    test
16    test
Name: char, dtype: object
```

- **.query()**  - позволяет извлекать определенные строки из фрейма на основе определенных условий.
```Python
df.query('char == "test"')
```


- **.read_csv()** -  чтение данных из CSV файла

```Python
import pandas as pd

wage = pd.read_csv('wage-1-ee13a6b1-605e-44fe-83a6-85d3d7a0b3ae-2873619c-9efe-44e2-932a-f6fed7be056b.csv')

print(wage)
```

```python 
#перебрать строки в ДатаФрэйме int для того чтобы сделать  итог интовым
for i, row in wage.iterrows():
    print(int(row['gender']))
```


- **apply()**  - метод позволяет применить функцию к строкам или столбцам кадра данных pandas. 

 **lambda x** - анонимная безымянная функция в отличии от def. 
 
 синтаксис:  
 ```  Python
 lambda аргументы: выражение
 ```


 [про лямбда функции](https://habr.com/ru/companies/piter/articles/674234/)
 
```Python
#для всех столбцов дата фрейма
df = df.apply ( lambda x: x\* 2 )

#для одного столбца дата фрейма
df.loc[:, 'points'] = df.points.apply ( lambda x: x\* 2 )

#для выбранных столбцов дата фрейма
df[['points', 'rebounds']] = df[['points', 'rebounds']].apply( lambda x: x\* 2 )

#добавили новый столбец и заполнили его 
df['status'] = df['points'].apply ( lambda x: ' Bad ' if x < 20 else ' Good')

#изменили значение
df['points'] = df['points'].apply ( lambda x: x/2 if x < 20 else x\*2)
 
```


- **.read_csv**  - чтение в ДатаФрэйм из csv файла (импорт)
[чтение файлов csv в пандас](https://www.codecamp.ru/blog/pandas-read-csv/)
```python
#import CSV file as DataFrame
df = pd.read_csv('data.csv')

#import only specific columns name from CSV file
df = pd.read_csv('data.csv', usecols=['playerID', 'points'])
#import only specific columns index from CSV file 
	df = pd.read_csv('data.csv', usecols=[ 0 , 1 ])

#import from CSV file and specify that header starts on second row (строка / заголовок)
df = pd.read_csv('data.csv', header= 1 )

#import from CSV file and skip second row
df = pd.read_csv('data.csv', skiprows=[ 1 ] )

#import from CSV file and skip second and third rows
df = pd.read_csv('data.csv', skiprows=[ 1, 2 ] )

#import from CSV file and specify delimiter to use
df = pd.read_csv('data.csv', sep='_')
```

- **.to_csv** 
``` Python
#экпорт в csv без индесов 
df_full.to_csv('total_wage.csv', index = False)
```


- **.groupby()** и  **.agg**() -  группировка и агрегация

```Python

#`dataframe.groupby(Название_колонки_для_группировки)[Перечисление_колонок_для_агрегации].функция_агрегации()`

df_wage.groupby('gender')['wage'].mean()

# сгруппировали  по гендеру и сагрегировали среднее и медиану по тоталу 
df_full.groupby('gender').agg({'total':['mean','median']})
```

- **.value_counts()** - подсчет частоты уникальных значений в серии pandas

[полная статья](https://www.codecamp.ru/blog/pandas-value_counts/)

``` Python
# покажет все
df_wage.value_counts()

# покажет первые 10 значений ?
df_wage.value_counts()[0:10]
```

По умолчанию **функция value_counts()** не показывает частоту значений NaN. Используйте аргумент **dropna** для отображения частоты значений NaN:

```python
my_series. value_counts (dropna= False )
```


- **.drop_duplicates** - удалить дубликат строки
по умолчанию сохраняет первый дубликат.

```python
	#удалить дубликат строки и перезапить в ДатаФрэйм
df = df.drop_duplicates()

#сохранить второй дубликат
df = df.drop_duplicates (keep='last')


#drop duplicates from column 'c' in DataFrame
df = df.drop_duplicates (subset=['c'])
```


- **.describe** - создание описательной статистики фрэйма(frame - кадр). 

```python
# по умолчанию только числовые
df.describe ()

# а так все включит 
df.describe (include='all')

# описание конкретных столбцов
df['points']. describe ()
```

- **plot()** - создает график

``` Python
df_wage['wage'].plot(kind='hist')
```

[Соединение датафрэймов](https://www.codecamp.ru/blog/pandas-join-vs-merge/)

-  **join()**  - объединяет два датафрэйма по индексу.

```python
#use join() function to join together two DataFrames
df1.join(df2)
```

- **.merge** - объединяет два датафрэйма любому указанному столбцу
[про виды соединения ](https://pythonru.com/uroki/osnovy-pandas-3-vazhnye-metody-formatirovanija-dannyh)

```python
#какой то сокращенный вариант. но главное что по колонке. Вероятно итог записался в Дф1
df1.merge(df2, on='name')

# тут новый фрейм - How - вид соединения
pd.merge(df_wage, df_bonus, on='person_id',how='outer')
```

- **fillna** - замены значений NaN в дата фрейме данных pandas.

```python
#replace NaN values in one column
df['col1'] = df['col1']. fillna (0)

#replace NaN values in multiple columns
df[['col1', 'col2']] = df[['col1', 'col2']]. fillna (0) 

#replace NaN values in all columns
df = df.fillna(0)
```