# menu-telegram-bot
Contents:

RU - line 5

ENG - line 

-----------------------------------------------------------------------------------------------------------------------------------------------------------
Общие данные:

Данный бот написан для поиска блюд в базе данных, которые можно приготовить на основании заданного набора ингредиентов, создания стоп листов (красные листы), листов ожидания (жёлтые листы) и доступных к приготовлению листов (зелёные листы), а также поиска рецептов приготовления блюд. Бот поддерживает многопользовательский режим. К данному боту прилагается база данных блюд, база данных для работы в могопользовательском режиме, скрипт по переводу базы данных в документ с раширением .tex, кастомные скрипты для работы с базой данных.

!!!
Работа с ботом:
После запуска бота у пользователя появляется три команды:

1)Посчитать листы

2)Вызвать помощь

3)Выполнить поиск по базе данных

1)
При просчитывании листов, если у бота сохранились ранее введённые данные, он преложит использовать их и пропустить процецуру ввода новых. Если выбрать этот вариант, бот сразу предложит перейти к созданию зелёных, жёлтых и красных листов. В противном случае бот предложит отправит сообщение с правилами заполнения (0 -ингредиента нет; 1 - ингредиент есть) и прделожение прислать шаблон, содержащий все ингредиенты изо всех блюд. Можно как ответить на предложение бота согласием или отказом, так и проигнорировать его. Это НЕ приведёт к поломке бота.

ВАЖНО: при заполнении шаблона следует соблюдать синтаксис "Огурец - 1". В бот вшита небольшая защита от опечаток, и если вы введёте "Огурец -1", "ОГУРЕЦ - 1" или "ОГУРЕЦ -1", ничего страшного не произойдёт, однако автор настоятельно не советует вам так поступать. Также в боте присутсвует небольшая проверка на ошибки ввода данных: если вы введёте случайный набор символов или "Огурец", вы получите сообщение, что данные неверны, а если вы введёте "Огурец -", то бот сообщит, что ожидает от вас значения 0 или 1, и укажет конкретные ингредиенты, где необходимо вписать их. При этом после сообщения об ошибке не нужно выходить в меню и начинать ввод ингредиентов заново. Достаточно ещё раз отправить исправленное сообщение.

После этого у вас появится посчитать следующие листы:
1)Красный лист (стоп лист) содержит все блюда, в которых не хватает хотя бы одного ингредиента, что не даёт приготовить данное блюдо. 
2)Зелёный лист (доступный к приготовлению лист) содержит все блюда, ингредиенты для которых точно есть. 
3)Жёлтый лист (лист ожидания) содержит все остальные блюда, о которых недостаточно информации, чтобы отнести блюдо к красному или жёлтому листу.
Также бот предложит Вам вернуться в главное меню и посчиать листы заново. При выборе пересчёта листов всё описанное ранее повторится, но без возможности исполльзовать ранее введённые данные. Помимо этого в процессе ввода данных бот предложит Вам вернуться назад, если вы запустили эту функцию случайно.

2)
Команда "Помощь" вызовет краткую инструкцию по работе с ботом, после чего предложит вернуться в начало.

3)
Команда поиск предлагает нам два варианта поиска:

1)По ингредиенту

2)По названию блюда

Первый вариант осуществляется с помощью отправки сообщения с следующим синтаксисисом: "*ингредиент_1" или "*ингредиент_1, ингредиент_2". В первом случае будут выведены все блюда, содержащие ингредиент_1, а во втором все блюда, содеращие ингредиент_1 ИЛИ ингредиент_2. Соответсвенно, количество ингредиентов в данной последовательности не ограничено. В случае ввода неправильного ингредиента, бот сообщит об этом.

Второй вариант предполагает, что Вы полностью или частично введёте название блюда. Предположим, у нас есть блюдо "блин 1" и "блин 12". Если мы введём "блин 1", бот оправит нам сообщение с пронумерованным списком "однофамильцев", и попросит нас ввести ТОЛЬКО номер конкретного блюда. Если ввести буквенный символ, бот скажет, что он просил цифру, а если ввести неверный номер, он скажет, что тут нет такого номера. При этом бот ранее предложитвернуться в главное меню и будет ждать пока вы не введёте правильный номер. После ввода нужного номера бот отправит нам тех карту на нужный блин. Если при этом вводить несуществующие блюда, бот сообщит, что такого блюда нет в базе данных.

Также в начале поиска бот опционаьно предлагает показать список заготовок.

!!!
База данных блюд:
Данная база данных содержит в себе три основные таблицы: блюда, блюда для бота, список ингредиентов.

В таблице блюда есть следующие поля:
1)Название блюда
2)Ингредиенты
3)Комментарии
4)Заготовка

Поле "Название блюда" принимает в себя строку. Так как алгоритм поиска в боте и алгоритм создание технической карты для поваров подразумевает ориентирование на "класс" блюда, то необходимо указывать перед названием данную информацию. К примеру: суп "солянка", каша овяная, блин с лососем и так далее. Соответсвующие классы указаны в коде бота в функции get_search_request_func(x), в связи с чем при изменении колличесва классов в базе данных необходимо будет изменить их и там.

Поле "Ингредиенты" принимает в себя строку. Информация записывается следующим образом: "ингредиент, количество, ингредиент, количество". Важно сохранять разделитель ", ", иначе это приведёт к поломке бота. Количесво вводится в граммах (только цифра), иногда в штуках (по контексту)

Поле "Комментарии" содержит в себе строку с пояснениями в приготовлении.

Поле "Заготовка" содержит в себе значение 0 или 1. 1 - данное блюдо является заготовкой, 0 - противоположное 1.

В таблице блюда для бота есть следующие поля:
1)Название блюда
2)Ингредиенты

Правила для этой таблицы абсолютно аналогичны предыдущей. Данная таблица была созда для облегчения написания и тестирования бота, но при необходимости её можно убрать (предварительно переписав данный элемент в коде), заменив на таблицу блюд. основное различие: в столбце ингредиенты нет граммовок, так как данная таблица использовалась для создания красных, жёлтых и зелёных листов, а не для поиска рецептов и создания технических карточек. Если возникают какие-либо проблемы с данной таблицей, запустите кастомные скрипты для базы данных, и всё будет исправлено.

В таблице список инредиентов есть следующие поля:
1)Ингредиент.

Данное поле принимает в себя название одного ингредиента (заготовки могут быть ингредиентами).


!!!
База данных пользователей:
Данная база данных включает в себя следующие поля: "user_id"	INTEGER, "state"	INTEGER, "switch"	INTEGER, "yellow_list"	TEXT, "green_list"	TEXT, "stop_list"	TEXT, "list_of_dishes"	TEXT, "parameters"	TEXT, "list_of_variants"	TEXT, "list_of_outputs"	TEXT, "iteration"	INTEGER. Все поля, кроме user_id являются глобальными переменными, так как это поле является id пользователя в телеграмме.


!!!
Скрипт перевода в .tex документ:
Скрипт на основе базы данных блюд создаёт документ с расширением .tex, что позволяет без проблем с использованием компилятора LaTeX перевести базу данных в удобные технологические карты для поваров. Все необходимые библиотеки для наиболее удачной компиляции заранее подобраны и вписаны в итоговый файл, в связи с чем достаточно лишь открыть полученный документ в LaTeX и запустить компиляцию. Документ отсортирован по категориям блюд (смотри базы данных блюд) и по алфавиту построчно.

!!!
Кастомные скрипты:
При запуске документа происходит запрос на обновление/создание дампа базы данных (вшита защита от дураков), после чего пересортировываются в алфавитном порядке блюда и заготовки, обновляются списки ингредиентов и списки блюд для бота (смотри базы данных блюд).
