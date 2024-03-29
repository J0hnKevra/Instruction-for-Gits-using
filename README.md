# **Инструкция для работы с Git и удаленными репозиториями**

## Что такое Git?
Git — это набор консольных утилит, которые отслеживают и фиксируют изменения в файлах (чаще всего речь идет об исходном коде программ, но вы можете использовать его для любых файлов на ваш вкус). Однако инструмент так понравился разработчикам, что в последствии, он получил широкое распространение и его стали использовать в других проектах. Изначально Git был создан Линусом Торвальдсом при разработке ядра Linux. С его помощью вы можете сравнивать, анализировать, редактировать, сливать изменения и возвращаться назад к последнему сохранению. Этот процесс называется контролем версий.

Git является распределенным, то есть не зависит от одного центрального сервера, на котором хранятся файлы. Вместо этого он работает полностью локально, сохраняя данные в директориях на жестком диске, которые называются репозиторием. Тем не менее, вы можете хранить копию репозитория онлайн, это сильно облегчает работу над одним проектом для нескольких людей. Для этого используются сайты вроде github и bitbucket.

![git](Git-logo.svg.png)

## Уствновка

Установить git на свою машину очень просто:

Linux — нужно просто открыть терминал и установить приложение при помощи пакетного менеджера вашего дистрибутива. Для Ubuntu команда будет выглядеть следующим образом: 

    sudo apt-get install git    
    
Windows — мы рекомендуем git for windows, так как он содержит и клиент с графическим интерфейсом, и эмулятор bash.
    OS X — проще всего воспользоваться homebrew. После его установки запустите в терминале:

    brew install git

Если вы новичок, клиент с графическим интерфейсом(например GitHub Desktop и Sourcetree) будет полезен, но, тем не менее, знать команды очень важно.    

![git2](git_logo_icon.png)
## Настройка

Итак, мы установили git, теперь нужно добавить немного настроек. Есть довольно много опций, с которыми можно играть, но мы настроим самые важные: наше имя пользователя и адрес электронной почты. Откройте терминал и запустите команды:

    git config --global user.name "My Name"
    git config --global user.email myEmail@example.com

Теперь каждое наше действие будет отмечено именем и почтой. Таким образом, пользователи всегда будут в курсе, кто отвечает за какие изменения — это вносит порядок.Чтобы сделать эти настройки глобальными, то есть применимыми ко всем проектам, необходимо добавить флаг –global. Если вы этого не сделаете, они будут распространяться только на текущий репозиторий.
Git хранит весь пакет конфигураций в файле .gitconfig, находящемся в вашем локальном каталоге.

Для того, чтобы посмотреть все настройки системы, используйте команду:

    it config --list

Для удобства и легкости зрительного восприятия, некоторые группы команд в Гит можно выделить цветом, для этого нужно прописать в консоли:

    git config --global color.ui true
    git config --global color.status auto
    git config --global color.branch auto

Если вы не до конца настроили систему для работы, в начале своего пути - не беда. Git всегда подскажет разработчику, если тот запутался, например:

1. Команда git --help - выводит общую документацию по git
2. Если введем git log --help - он предоставит нам документацию по какой-то определенной команде (в данном случае это - log)
3. Если вы вдруг сделали опечатку - система подскажет вам нужную команду
4. После выполнения любой команды - отчитается о том, что вы натворили
5.  Также Гит прогнозирует дальнейшие варианты развития событий и всегда направит разработчика, не знающего, куда двигаться дальше

Тут стоит отметить, что подсказывать система будет на английском, но не волнуйтесь, со временем вы изучите несложный алгоритм ее работы и будете разговаривать с ней на одном языке.

## Создание нового репозитория



Обычно вы получаете репозиторий Git одним из двух способов:

    Вы можете взять локальный каталог, который в настоящее время не находится под версионным контролем, и превратить его в репозиторий Git, либо

    Вы можете клонировать существующий репозиторий Git из любого места.

В обоих случаях вы получите готовый к работе Git репозиторий на вашем компьютере.

Создайте на рабочем столе папку под названием git_exercise. Для этого в окне терминала введите:

    $ mkdir Desktop/git_exercise/
    $ cd Desktop/git_exercise/
    $ git init

Командная строка должна вернуть что-то вроде:

    Initialized empty Git repository in /home/user/Desktop/git_exercise/.git/

Это значит, что наш репозиторий был успешно создан, но пока что пуст. Теперь создайте текстовый файл под названием hello.txt и сохраните его в директории git_exercise.

## **Определение состояния**

status — это еще одна важнейшая команда, которая показывает информацию о текущем состоянии репозитория: актуальна ли информация на нём, нет ли чего-то нового, что поменялось, и так далее. Запуск git status на нашем свежесозданном репозитории должен выдать:

    $ git status
    On branch master
    Initial commit
    Untracked files:
    (use "git add ..." to include in what will be committed)
    hello.txt

Сообщение говорит о том, что файл *hello.txt* неотслеживаемый. Это значит, что файл новый и система еще не знает, нужно ли следить за изменениями в файле или его можно просто игнорировать. Для того, чтобы начать отслеживать новый файл, нужно его специальным образом объявить.

## **Подготовка файлов**

В git есть концепция области подготовленных файлов. Можно представить ее как холст, на который наносят изменения, которые нужны в коммите. Сперва он пустой, но затем мы добавляем на него файлы (или части файлов, или даже одиночные строчки) командой add и, наконец, коммитим все нужное в репозиторий (создаем слепок нужного нам состояния) командой commit.
В нашем случае у нас только один файл, так что добавим его:

    $ git add hello.txt

Если нам нужно добавить все, что находится в директории, мы можем использовать

    $ git add -A

Проверим статус снова, на этот раз мы должны получить другой ответ:

    $ git status
    On branch master
    Initial commit
    Changes to be committed:
    (use "git rm --cached ..." to unstage)
    new file: hello.txt

Файл готов к коммиту. Сообщение о состоянии также говорит нам о том, какие изменения относительно файла были проведены в области подготовки — в данном случае это новый файл, но файлы могут быть модифицированы или удалены.

## **Фиксация изменений**

## Как сделать коммит

Представим, что нам нужно добавить пару новых блоков в html-разметку (index.html) и стилизовать их в файле style.css. Для сохранения изменений, их необходимо закоммитить. Но сначала, мы должны обозначить эти файлы для Гита, при помощи команды git add, добавляющей (или подготавливающей) их к коммиту. Добавлять их можно по отдельности:

    git add index.html

    git add css/style.css

или вместе - всё сразу:

    git add .

Конечно добавлять всё сразу удобнее, чем прописывать каждую позицию отдельно. Однако, тут надо быть внимательным, чтобы не добавить по ошибке ненужные элементы. Если же такое произошло изъять оттуда ошибочный файл можно при помощи команды

    git reset:

    git reset css/style.css

Теперь создадим непосредственно сам коммит

    git commit -m 'Add some code'

Флажок -m задаст commit message - комментарий разработчика. Он необходим для описания закоммиченных изменений. И здесь работает золотое правило всех комментариев в коде: «Максимально ясно, просто и содержательно обозначь написанное!»

## Как посмотреть коммиты

Для просмотра все выполненных фиксаций можно воспользоваться историей коммитов. Она содержит сведения о каждом проведенном коммите проекта. После того, как вы создали несколько коммитов или же клонировали репозиторий с уже существующей историей коммитов, вероятно вам понадобится возможность посмотреть что было сделано — историю коммитов. Одним из основных и наиболее мощных инструментов для этого является команда git log.



По умолчанию (без аргументов) git log перечисляет коммиты, сделанные в репозитории в обратном к хронологическому порядке — последние коммиты находятся вверху. Из примера можно увидеть, что данная команда перечисляет коммиты с их SHA-1 контрольными суммами, именем и электронной почтой автора, датой создания и сообщением коммита.

Команда git log имеет очень большое количество опций для поиска коммитов по разным критериям. Рассмотрим наиболее популярные из них.

Одним из самых полезных аргументов является -p или --patch, который показывает разницу (выводит патч), внесённую в каждый коммит. Так же вы можете ограничить количество записей в выводе команды; используйте параметр -2 для вывода только двух записей
 Отследить интересующие вас операции в списке изменений, можно по хэшу коммита, при помощи команды git show :

    git show hash_commit

Ну а если вдруг нам нужно переделать commit message и внести туда новый комментарий, можно написать следующую конструкцию:

    git commit --amend -m 'Новый комментарий'

В данном случае сообщение последнего коммита перезапишется. Но злоупотреблять этим не стоит, поскольку эта операция опасная и лучше ее делать до отправки коммита на сервер.

# **Работа с удаленным репозиторием**

Для того, чтобы внести вклад в какой-либо Git-проект, вам необходимо уметь работать с удалёнными репозиториями. Удалённые репозитории представляют собой версии вашего проекта, сохранённые в интернете или ещё где-то в сети. У вас может быть несколько удалённых репозиториев, каждый из которых может быть доступен для чтения или для чтения-записи. Взаимодействие с другими пользователями предполагает управление удалёнными репозиториями, а также отправку и получение данных из них. Управление репозиториями включает в себя как умение добавлять новые, так и умение удалять устаревшие репозитории, а также умение управлять различными удалёнными ветками, объявлять их отслеживаемыми или нет и так далее.

![лого гит хаба](GitHub-logo.png)

## 1. Как подключиться к удаленному репозитарию?

Для загрузки данных в удаленный репозитарию сначала нужно к нему подключиться. В нашем примере мы используем адрес *https://github.com/tutorialzine/awesome-project*, однако пользователь может создать собственный удаленный репозитарий на GitHub, BitBucket или другом подобном сервисе. Это занимает некоторое время, однако в дальнейшем полностью себя оправдывает, тем более, что подобные службы имеют пошаговые инструкции для правильно выполнения нужных действий.


Для того, чтобы связать созданный нами локальный репозитарий с удаленным, выполним такую команду:

    This is only an example. Replace the URI with your own repository address.
    $ git remote add origin https://github.com/tutorialzine/awesome-project.git


Первая строка напоминает нам, что URI репозитария, который приведен в примере, нужно изменить на свой.


Иногда бывает так, что проект имеет несколько удаленных репозитариев – в таком случае каждому из них присваивается собственное имя. Главный репозитарий принято называть origin.

## 2. Как отправить изменения в удаленный репозитарий?

Теперь, когда у нас в локальном репозитарии создан коммит и мы подключились к удаленному, можем отправить его на сервер. Мы это будем делать каждый раз, когда хотим обновить данные в удаленном репозитарии.


Отправка коммита осуществляется с помощью команды push, которая имеет два параметра - имя удаленного репозитория (в нашем случае origin) и ветку, в которую необходимо внести изменения (master — это ветка по умолчанию для всех репозиториев).

    $ git push origin master
    Counting objects: 3, done.
    Writing objects: 100% (3/3), 212 bytes | 0 bytes/s, done.
    Total 3 (delta 0), reused 0 (delta 0)
    To https://github.com/tutorialzine/awesome-project.git
    * [new branch] master -> master


Если мы все сделали правильно, то отправленный файл hello.txt на удаленном сервере мы можем увидеть с помощью браузера. Важный момент – некоторые сервисы для отправки изменений могут требовать дополнительной аутентификации.

## 3. Как клонировать удаленный репозитарий?

Если у других пользователей возникла необходимость клонировать удаленный репозитарий, они могут получить полностью работоспособную копию при помощи команды clone:

    $ git clone https://github.com/tutorialzine/awesome-project.git


GitHub автоматически создаст новый локальный репозитарий в виде удаленного на собственном сервере.

## 4. Как запросить изменения с удаленного репозитария?

В случае, если другим пользователям нет необходимости делать клон удаленного репозитария, а нужно просто получить информацию об изменениях, это можно сделать с помощью команды pull:

    $ git pull origin master
    From https://github.com/tutorialzine/awesome-project
    * branch master -> FETCH_HEAD
    Already up-to-date.


Она скачивает новые изменения. Так как мы ничего нового не вносили с тех пор, как клонировали проект, изменений, доступных к скачиванию, нет.