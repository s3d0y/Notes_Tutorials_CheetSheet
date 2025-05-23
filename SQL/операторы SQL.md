#sql 

#### Основные операторы сравнения:

| Оператор                 | Описание                                                 | Пример                                             |
| ------------------------ | -------------------------------------------------------- | -------------------------------------------------- |
| `=`                      | Равенство                                                | `WHERE id = 10`                                    |
| `<>`/`!=`                | Неравенство                                              | `WHERE status <> 'active'`                         |
| `>`, `<`                 | Больше/меньше                                            | `WHERE price > 100`                                |
| `>=`, `<=`               | Больше/меньше или равно                                  | `WHERE age >= 18`                                  |
| `BETWEEN`, `NOT BETWEEN` | "Между" (включительно) т.е. в диапазоне / не в диапазоне | `WHERE date BETWEEN '2023-01-01' AND '2023-12-31'` |
#### **Операторы проверки множеств**

| <br><br>Оператор | Название           | Описание                                                          | Пример                                      |
| ---------------- | ------------------ | ----------------------------------------------------------------- | ------------------------------------------- |
| `IN`             | Входит в список    | Проверяет, совпадает ли значение с хотя бы одним элементом списка | `WHERE id IN (1, 2, 3)`                     |
| `NOT IN`         | Не входит в список | Проверяет, что значение отсутствует во всех элементах списка      | `WHERE status NOT IN ('deleted', 'banned')` |
| `= ANY()`        | Аналог `IN`        | Альтернативный синтаксис (особенно полезно с подзапросами)        | `WHERE id = ANY(ARRAY[1, 2, 3])`            |
| `<> ALL()`       | Аналог `NOT IN`    | Более безопасная версия `NOT IN` (корректно обрабатывает NULL)    | `WHERE id <> ALL(ARRAY[1, 2, 3])`           |

#### **Специальные операторы для работы с NULL**

Для сравнения с NULL всегда использовать `IS NULL`/`IS NOT NULL` (не `= NULL`!)

| Оператор           | Описание                | Пример                         |
| ------------------ | ----------------------- | ------------------------------ |
| `IS NULL`          | Проверка на NULL        | `WHERE phone IS NULL`          |
| `IS NOT NULL`      | Проверка на не-NULL     | `WHERE email IS NOT NULL`      |
| `IS DISTINCT FROM` | Сравнение с учётом NULL | `WHERE col IS DISTINCT FROM 0` |

#### Строковые операторы

| Оператор    | Симв. опаторы | Название                 | Пример                                       | Описание                                                 |
| ----------- | ------------- | ------------------------ | -------------------------------------------- | -------------------------------------------------------- |
| `\|`        |               | Конкатенация             | `SELECT 'Hello' \| ' World'` → `Hello World` | Объединяет строки                                        |
| `LIKE`      | ~~            | Соответствие шаблону     | `WHERE name LIKE 'Jon%'`                     | `%` — любая строка, `_` — один символ (регистрозависимо) |
| `ILIKE`     | ~~*           | Без учёта регистра       | `WHERE name ILIKE 'jon%'`                    | Аналог `LIKE`, но регистронезависимый                    |
| `NOT LIKE`  | !~~           | Отрицание                | `WHERE name NOT LIKE '%Test%'`               | Не соответствует шаблону                                 |
| `NOT ILIKE` | !~~*          | Отрицание (без регистра) | `WHERE name NOT ILIKE '%test%'`              |                                                          |
 
#### Регулярные выражения

|Оператор|Эквивалент|Пример|Описание|
|---|---|---|---|
|`~`|Регулярное выражение|`WHERE text ~ '^[A-Z][a-z]+'`|Проверка по regex (регистрозависимо)|
|`~*`|Regex без регистра|`WHERE text ~* '^[a-z]+'`|Без учёта регистра|
|`!~`|NOT REGEXP|`WHERE text !~ '\d'`|Не содержит цифр|
|`!~*`|NOT REGEXP (без регистра)|`WHERE text !~* '[A-Z]'`|Не содержит заглавных букв|

#### Основные логические операторы

| Оператор                   | Название                  | Пример                                     | Описание                                           |
| -------------------------- | ------------------------- | ------------------------------------------ | -------------------------------------------------- |
| `AND`                      | Логическое И              | `WHERE age > 18 AND status = 'active'`     | Возвращает TRUE, если оба условия истинны          |
| `OR`                       | Логическое ИЛИ            | `WHERE role = 'admin' OR role = 'manager'` | Возвращает TRUE, если хотя бы одно условие истинно |
| `NOT`                      | Логическое НЕ             | `WHERE NOT is_deleted`                     | Инвертирует условие                                |
| `()`                       | Группировка               | `WHERE (a OR b) AND c`                     | Определяет порядок выполнения логических операций  |
| `IS [NOT] TRUE/FALSE/NULL` | Проверка булевых значений | `WHERE is_active IS TRUE`                  |                                                    |
1. **Приоритет операторов**:
    
    - `NOT` имеет наивысший приоритет
        
    - Затем `AND`
        
    - Затем `OR`
        
    - Используйте скобки `()` для явного указания порядка




