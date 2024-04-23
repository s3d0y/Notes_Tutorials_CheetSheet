Основы работы с Git. 

Хорошее решение - пройти курс на Яндекс.Практикум. [Бесплатный курс - Основы работы с Git](https://practicum.yandex.ru/git-basics/?from=catalog)

Ниже я оставлю шпаргалку с полезной информацией и заодно сам лучше запомню ее. 

 ### Общие моменты
1. В любой непонятной ситуации ищи ответы на свои вопросы в интернете или с помощью Chat GPT
2. Конспектируй все что проходишь или по крайней мере многое.
3. Практикуйся и тестируй себя
4. Не сдавайся 

GIT - система контроля версий.  VCS  - version control system 
или SCM -  source control managment

Премущества:
- Контроль -  кто что когда где зачем 
- Безопасные изменения - вытекает из первого
- Возможность восстановить - можно откатиться на старую версию, и еще и оставить проблемную в одтельном месте для испрвлений
- Командная  работа - можно одновременно вести разработку и помещать изменения сообща 

Основные функции:
- хранение истории изменений
- манипуляция историей: изменение порядка ревизий, удаление версий, возвращение назад в истории
- анализ изменений: кто, что, когда и как часто вносил

 ### Команды терминала Bash

GIT 
git version версия ГИТ. Если версия есть все хорошо, если версии нет - нужно поставить - см. Гугл )
git confing -- global user.name 123 задать имя
git confing -- global user.email 123@123.com задать почту 
git confing -- list  - посмотреть запись в конфиге
git init -  инициализация Git репозитория там где вы находитесь в тукущий момент и начнет отслеживать все изменения. Так же создаст скрытую папку git - системаная штука 
"разгитить папку" - просто удили папку .git rm -rf 
git status -  проверить состояние репозитория

`s3d0y@s3d0y-VM:~/Documents/GitCheatSheet$ git status` 
`On branch main` название текущей ветки
`Your branch is up to date with 'origin/main'.`

`Changes not staged for commit:` - изменения не на сцене
  `(use "git add <file>..." to update what will be committed)`
  `(use "git restore <file>..." to discard changes in working directory)`
	`modified:   README.md` - файлы которые менялись 

`no changes added to commit (use "git add" and/or "git commit -a")`- на комит ничего не добавлено 
`s3d0y@s3d0y-VM:~/Documents/GitCheatSheet$ ``


git add --all  добавить на сцену - подготовить к комиту. 
git commit -m 'coment'
git push 
git commit --amend - работает только с последним комитом HEAD

ssh ключи 
и еще как то можно сращивать репозитории 

# Шпаргалка. Базовые команды в консоли (Command Line Interface)

Чтобы вам было удобнее взаимодействовать с командной строкой, мы подготовили шпаргалку. В ней собраны все команды, о которых мы рассказали в уроках, и их полезные вариации.

### Помощь
- `man`   — manual. Совместно с именем команды выведет инструкуцию. Например `man ls` 

### Навигация

- `pwd` (от англ. _**p**rint **w**orking **d**irectory_, «показать рабочую папку») — покажи, в какой я папке;
- `ls` (от англ. _**l**i**s**t directory contents_, «отобразить содержимое директории») — покажи файлы и папки в текущей папке; 
- `ls -a` — покажи также скрытые файлы и папки, названия которых начинаются с символа `.`;
- `ls -l` —  вывод подробной информации о содержимом каталога в виде списка;
- `ls -  al` —  сразу два ключа. так тоже можно
- `cd first-project` (от англ. _**c**hange **d**irectory_, «сменить директорию») — перейди в папку `first-project`;
- `cd first-project/html` — перейди в папку `html`, которая находится в папке `first-project`;
- `cd ..` — перейди на уровень выше, в родительскую папку;
- `cd ~` — перейди в домашнюю директорию (`/Users/Username`);
- `cd /` — перейди в корневую директорию.

### Работа с файлами и папками

**Создание**

- `touch index.html` (англ. _touch,_ «коснуться») — создай файл `index.html` в текущей папке;
- `touch index.html style.css script.js` — если нужно создать сразу несколько файлов, можно напечатать их имена в одну строку через пробел;
- `mkdir second-project` (от англ. _**m**a**k**e **dir**ectory_, «создать директорию») — создай папку с именем `second-project` в текущей папке.

**Копирование и перемещение**

- `cp file.txt ~/my-dir` (от англ. _**c**o**p**y_, «копировать») — скопируй файл в другое место;
- `mv file.txt ~/my-dir` (от англ. _**m**o**v**e_, «переместить») — перемести файл или папку в другое место.

**Чтение**

- `cat file.txt` (от англ. _con**cat**enate and print_, «объединить и распечатать») — распечатай содержимое текстового файла `file.txt`.

**Удаление**

- `rm about.html` (от англ. _**r**e**m**ove_, «удалить») — удали файл `about.html`;
- `rmdir images` (от англ. _**r**e**m**ove **dir**ectory_, «удалить директорию») — удали папку `images`;
- `rm -rf second-project` (от англ. _**r**e**m**ove,_ «удалить» + _**r**ecursive_, «рекурсивный» + **f**orce, "силой/заставить/принудить" ) — удали папку `second-project` и всё, что она содержит.


### Полезные возможности

- Команды необязательно печатать и выполнять по очереди. Можно указать их списком — разделить двумя амперсандами (`&&`).
- У консоли есть собственная память — буфер с несколькими последними командами. По ним можно перемещаться с помощью клавиш со стрелками вверх (**`↑`**) и вниз (**`↓`**).
- Чтобы не вводить название файла или папки полностью, можно набрать первые символы имени и дважды нажать `Tab`. Если файл или папка есть в текущей директории, командная строка допишет путь сама.
    
    Например, вы находитесь в папке `dev`. Начните вводить `cd first` и дважды нажмите `Tab`. Если папка `first-project` есть внутри `dev`, командная строка автоматически подставит её имя. Останется только нажать `Enter`.