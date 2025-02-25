##### Настройка
```bash
$ git config --global user.name "User Namovich" 
$ git config --global user.email username@yandex.ru
```
##### Информация о конфигурации
```bash
$ cat ~/.gitconfig # вывести содержимое файла конфигурации Git.
$ git config --list # тоже самое.

```
##### Инициализация репозитория
```bash
$ git init # создать репозиторий.
$ rm -rf .git # "разгитить" директорию.
```
**-rf:**
* ключ -r (от англ. **_r_**_ecursive_ — «рекурсивно») позволяет удалять папки вместе с их содержимым;
* ключ -f (от англ. **_f_**_orce_ — «заставить») избавит вас от вопросов вроде «Вы точно хотите удалить этот файл? А этот? И этот тоже?».
#####  Состояние репозитория
```bash
$ git status # status — «статус», «состояние») — она показывает текущее состояние репозитория.
$ git add --all # Ключ, или флаг, --all позволяет подготовить к сохранению все файлы в репозитории.
$ git add . # добавить всю текущую папку.

$ git rm -r --cached [name_dir]/ # staged -> untracked для директории
$ git rm --cached [name_file] # staged -> untracked для файла
```
##### Коммиты
```bash
$ git commit -m "Name_Commit" # m - (message — «сообщение»), который присваивает коммиту сообщение.
$ git log # log — «журнал записей», просмотр истории кооммитов.
$ git log --oneline # сокращенный лог.
```
##### SSH
Один из наиболее распространённых сетевых протоколов — **SSH** (от англ. **_S_**_ecure_ **_Sh_**_ell Protocol_).
```bash
$ ls -la .ssh/ # вывели список созданных ключей.
$ ssh-keygen -t ed25519 -C "e-mail"

# скопировать содержимое ключа в буфер обмена:
$ pbcopy < ~/.ssh/id_ed25519.pub
```
##### Добавление удаленного репозитория
```bash
$ git remote add # (от англ. remote — «удалённый» и add — «добавить»).
$ git remote add origin git@github.com:%ИМЯ_АККАУНТА%/first-project.git
$ git remote -v # проверить, что репозитории связаны.
```
**origin** (англ. «источник») — стандартный псевдоним, с помощью которого можно обращаться к главному удалённому репозиторию.
**-v** — короткая форма флага **--verbose** (англ. «подробный»). Он позволяет показать больше информации в выводе
##### Push, pull
```bash
$ git push -u origin master # отправка изменений в удаленный репозиторий.
$ git push # последующие пуши
```
##### Статусы untracked, tracked, staged, modified
* untracked («неотслеживаемый»)
* staged («подготовленный»)
* tracked («отслеживаемый»)
* modified («изменённый»)
##### Файл HEAD

Один из служебных файлов папки `.git`. Он указывает на коммит, который сделан последним (то есть на самый новый).
##### LGS (сокращение от англ. **_l_**_o_**_g_**_istic_**_s_** — «логистика»)
**Jira-ID**, а после — текст сообщения.
```bash
$ git commit -m "LGS-239: Дополнить список пасхалок новыми числами"
```
**Conventional Commits**
Conventional Commits предлагает такой формат коммита: `<type>: <сообщение>`. Первая часть type — это тип изменений. Таких типов достаточно много. 

Вот два примера:
* feat (сокращение от англ. _feature_) — для новой функциональности;
* fix (от англ. «исправить», «устранить») — для исправленных ошибок.
```bash
$ git commit -m "feat: добавить подсчёт суммы заказов за неделю"
```

GitHub можно использовать не только для хранения файлов проекта, но и для ведения списка **задач** (англ. _issue_) этого проекта. Если коммит «закрывает» или «решает» какую-то задачу, то в его сообщении удобно указывать ссылку на неё. Для этого в любом месте сообщения нужно указать #<номер задачи>. Например, вот так.
```bash
$ git commit -m "Исправить #334, добавить график температуры"
```
 
 *** TODO
### Изменения в коммитах

```$ git commit --amend --no-edit # дополнить коммит новыми файлами 
git restore --staged . # «сбросить» все файлы из staged обратно в untracked/modified 
$ git commit --amend -m "Новое сообщение" # изменить сообщение последнего коммита git reset --hard <commit hash> # «откатить» коммит
```

#### Будьте осторожны с командой git reset --hard! При удалении коммитов можно потерять что-то нужное.

`$ git restore <file> # «откатить» изменения, которые не попали ни в staging, ни в коммит`

### Просмотр изменений

`$ git diff # сравнить последнюю закоммиченную версию файла с текущей (изменённой) версией $ git diff --staged # просмотреть изменения в staged файлах $ git diff <хэш коммита> <хэш-коммита> # сравнить изменения в двух коммитах`

`git diff A B` выводит список инструкций: как превратить состояние A в состояние B. Если поменять A и B местами (git diff B A), то и инструкции будут обратные: как превратить B в A. При этом все зелёные строки станут красными, и наоборот.

### Заполнение .gitignore

Правила из `.gitignore` применяются только к новым (untracked) файлам. Если файл уже попал в staging area или в коммит, то правила на него не распространяются.

- Символ звёздочки (*) соответствует любой строке, включая пустую.

`# игнорировать все файлы, которые заканчиваются на .jpeg *.jpeg # игнорировать все файлы "tmp" во всех подпапках папки docs docs/*/tmp` 

- Вопросительный знак ? соответствует одному любому символу.

`file?.txt` 

- Квадратные скобки, как и вопросительный знак, соответствуют одному символу. При этом символ не любой, а только из списка, который указан в скобках.

`# игнорировать файлы file0.txt, file1.txt и file2.txt # при этом не игнорировать file3.txt, file4.txt, ... file[0-2].txt` 

- Косая черта, или слеш (/), указывает на каталоги.

`# игнорировать todo.txt в корне репозитория /todo.txt # для сравнения: spam.txt будет игнорироваться во всех папках spam.txt  # игнорировать папку build build/` 

- Функция парных звёздочек (**) похожа на функцию одинарной (*). Отличие в том, как они работают с вложенными папками. Двойная звёздочка может соответствовать любому количеству таких папок (в том числе нулю). Одинарная может соответствовать только одной.

`# игнорировать файлы "docs/current/tmp", "docs/old/tmp", # а также "docs/old/saved/a/b/c/d/tmp" # и даже "docs/tmp", потому что ноль вложенных папок тоже подходит docs/**/tmp # игнорировать только "docs/current/tmp" и "docs/old/tmp" # файл "docs/old/saved/a/b/c/d/tmp" не попадает в правило docs/*/tmp` 

- Любое правило в файле `.gitignore` можно инвертировать с помощью восклицательного знака (!).

`# игнорировать все JPEG-файлы *.jpeg # но только не мем с Doge !doge.jpeg` 

#### `.gitignore` и `git status`

Игнорируемые файлы не отображаются в выводе команды git status, иначе они бы засоряли вывод. Если всё же нужно отобразить все игнорируемые файлы, то это можно сделать с помощью ключа: `git status --ignored`. В таком случае в выводе git status появится раздел Ignored files

### Основы работы с ветками Git

Команда `git clone` автоматически связывает локальный и удалённый репозиторий. То есть если в GitHub-репозитории что-то поменяется (например, добавятся коммиты), вам не нужно будет заново клонировать его. Достаточно будет выполнить команду, которая обновит вашу копию.

`$ git clone <https:/...> # копирование удаленного репозитория в локальный каталог`

Fork — это GitHub-операция; напрямую с Git она не связана. «Форк» создаёт копию репозитория в аккаунте GitHub. В процессе «форка» создаётся копия всех файлов, истории коммитов и веток. Эта копия сохраняется в вашей учётной записи GitHub.

Ветка (англ. branch) — это изолированный поток разработки проекта. В таком потоке можно проверять разные идеи, тестировать новую функциональность и так далее.

Название веток: feature (англ. «особенность», «деталь») для веток, где прорабатывается новая функциональность, и bugfix (от англ. bug — «жук», «ошибка» и fix — «исправить») для веток, где ведётся работа по исправлению ошибок. После ключевого слова идёт слеш и описание проблемы или задачи.

`$ git branch # вывести ветки, которые есть в проекте.  # Звёздочкой (*) отмечено, в какой ветке вы находитесь в текущий момент. $ git branch <название_ветки> # создать ветку $ git checkout <название_ветки> # переключиться на другую ветку $ git checkout -b <название_ветки> # cоздать ветку и сразу переключиться на неё $ git branch -a # просматреть все ветки как локальные, так и удаленные`

#### Просматриваем все ветки: `git branch -a`

- Локальные ветки будут указываться как обычно при вызове git branch, например, feature/add-branch-info.
- Удалённые – с префиксом remotes/origin: remotes/origin/feature/add-branch-info.
- Отдельно будет указана основная ветка: строка с ней будет выглядеть как remotes/origin/HEAD -> origin/main. Прочитать эту строку можно как «Главной веткой является main».

`$ git diff <название_ветки1> <название_ветки2> # сравнить 2 ветки $ git diff <название_ветки> <хэш_коммита>`

#### Суффикс навигации ~

в Git есть суффикс навигации ~N, где N — это число. Он отсчитывает от заданного коммита N коммитов назад во времени. Нумерация начинается с нуля: commit~0 — это сам коммит, commit~1 — предыдущий, commit~2 — предшествующий предыдущему и так далее.

`$ git diff HEAD~ HEAD # вывести разницу между предыдущим и текущим коммитами $ git diff <название_ветки>~1 <название_ветки> # аналогично предъыдущему $ git diff <хэш_коммита>~ <хэш_коммита> # аналогично`

### Слияние веток

Добавление изменений в основную версию проекта называется **слиянием** веток.

`$ git merge <название_ветки> # слияние веток $ git branch -D <название_ветки> # удаление ветку-донора после слияния $ git branch -d <имя_ветки> # удалит ветку только если она была полностью объединена с другой — то есть если две ветки стали (или изначально были) частью одной истории`

**Fast-forward** — это режим слияния. Fast-forward (англ. «перемотка») значит, что итогом слияния будет линейная история коммитов. Такое происходит, когда истории двух веток находятся на одной прямой — то есть когда одна ветка продолжает историю, начатую другой, как в нашем примере.

Если Git не может провести слияние изменений автоматически, он сообщает о конфликте. **Конфликт** — это ситуация, в которой один или несколько человек модифицировали один и тот же файл.

### Работа с удаленным репозиторием

`$ git push -u origin <название_ветки> # запушить ветку в удаленный репозиторий и связать локальную ветку с удаленной $ git push # отправить изменения в удалённый репозиторий, в связанную ветку`

**Pull request** (англ. «запрос на изменения»; буквально: «запрос на подтягивание»). Это запрос на рассмотрение предлагаемых изменений и часть процесса ревью.  
**Code review** (англ. «рассмотрение кода») - процесс рассмотрения кода. У каждого пул-реквеста есть:

- Название — краткое описание предлагаемых изменений. Это поле заполнять необязательно, но желательно.
- Исходная ветка — та, в которой вы работали.
- Целевая ветка — основная ветка проекта, в которую вы хотите внести изменения.

У каждого пул-реквеста может быть два **исхода**:

- merge (англ. «соединить») — предлагаемые изменения приняты; код вливается в целевую ветку; пул-реквест закрывается.
- close (англ. «закрыть») — пул-реквест закрывается без слияния изменений.

`$ git pull # (от англ. pull — «вытянуть») — стянуть, или «запулить» изменения. $ git remote rm origin # эта команда удалит текущий origin`

Команда git pull позволяет подтянуть изменения из удалённого репозитория в локальный.

Перед созданием нового пул-реквеста считается хорошей практикой перейти в главную ветку, «подтянуть» в неё изменения, а затем добавить эти изменения в вашу ветку с помощью git merge main.

### Углубленная работа с ветками

Состояние **fast-forward**:

- при слиянии этих двух веток никак не возможен конфликт;
- истории этих двух веток не «разошлись»;
- одна ветка является продолжением другой.

`$ git merge <название_ветки> $ git merge --no-edit --no-ff <название_ветки>  # --no-edit отключает ввод сообщения для merge-коммита # --no-ff отключает fast-forward слияние веток $ git log --graph --oneline # с флагом --graph # Git нарисует ветки с помощью «палочек» и «звёздочек»`

Rebase (англ. «перебазирование»). Эта операция позволяет изменить точку (коммит), от которой отделилась ветка.

`$ git push --force # форсированный пуш`

### Подходы к работе с ветками (workflow «рабочий процесс» или сокращённо: flow)

- Feature branch workflow — простой и самый популярный вариант. В нём для каждого нового изменения создаётся новая ветка, которая позже вливается в main с помощью git merge.
- Git flow — более сложный вариант. Изменения (коммиты) делят на разные типы: исправление, новая функциональность и так далее. Разные типы коммитов попадают в разные ветки.
- Trunk-based — популярный в больших компаниях. Главное отличие в том, что участники проекта вливают (merge) свой код в основную ветку максимально часто. Например, каждый день.

#### Feature branch workflow

Основные правила:

- новая функциональность или исправление — новая ветка;
- когда код в feature-ветке готов, он вливается в main;
- в main всегда рабочая версия без «недоделок».