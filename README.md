# Process mining процесса обработки и доставки данных онлайн-магазина
### _Кейс онлайн-этапа чемпионата SPARTA от Сбер_
### __третье место__

__Команда: No more decimal__

* Андрей Супрун - __тимлид__
* Чэмэлиинэ А***********
* Мария Г********
* Александра К*******
* Олег К******

__Описание кейса:__
* магазин обрабатывает заказы с 8:00 до 23:59
* работает до последнего клиента
* входящий поток от 100 до 400 заказов ежедневно
* анализируемый период с начала октября по декабрь 2022 года
* владелец магазина ожидает, что заказ передается курьеру в течение 30 минут после сборки 
* по словам владельца,  в октябре и декабре были зафиксированы сбои в работе магазина, при первой итерации анализа не удалось определить характер проблем

__Цель проекта:__
Помочь владельцу интернет магазина улучшить бизнес-процесс, сократить издержки на доставку, оптимизировать работу и численность персонала

__Задачи проекта:__
* выявить классические процессные неэффективности
* рассчитать хронометраж процесса
* проверить SLA передачи заказов курьерам
* предоставить отчет в формате .pptx

### Итоги

В рамках исследования мы провели работу по изучению процессов обработки и доставки заказов интернет-магазина.

__На этапе обзора и предобработки данных:__
- получили логи процессов обработки и доставки заказов (таблица из 8 столбцов и 179 тыс 30 строк)
- обработали логи с помощью библиотеки PM4PY, добавили новые столбцы (общая длительность процессов, количество этапов, время от завершения сборки до передачи товара курьеру, длительность каждого этапа)
- преобразовали типы данных в корректные форматы (datetime, минуты вместо timedelta)
- создали две дополнительные таблицы (только успешные и только неуспешные процессы)

__На этапе исследовательского анализа:__
- построили общую модель прохождения процессов в виде DFG
- выделили выбросы в данных и устранили их (3% от датасета)
- проследили хронометраж, разделив процессы на три группы - 25-30, 60-70 и 90-100 минут
- выделили "идеальный" срок протекания процесса - 9 неповторяющихся этапов за 60-70 минут
- проверили выполнение SLA: требование по "работе до последнего клиента" соблюдается, а требование по передаче заказа курьеру в течение 40 минут соблюдается в 94% случаев

__Выделение процессных неэффективностей:__
- выделяется большая группа "зацикленностей": возврат на этап сборки заказа, "пинг-понг" между сборкой и поступлением заказа сборщику, зацикленности "в себя" на этапах "Звонок клиенту", "Оплата" и "Упаковка товара"
- две операции, а именно "Сборка заказа" и "Доставка заказа", классифицируются как длительные типа "нестандартные / ручные операции"
- выделяются также такие неэффективности, как "ошибки системы" (этап "Отмена заказа" при наличии этапа "Заказ доставлен") и "нерегулярные операции" ("Звонок клиенту")

__Оценка потенциального эффекта:__
- суммарный финансовый эффект от неуспешных экземпляров потенциально составляет 2 556 596 р. за три месяца
- больше половины указанной суммы приходится на отмену заказов во время этапа доставки
- оставшаяся часть равномерно распределяется между этапами проверки, оплаты и звонков клиентам
- из остальных неэффективностей наиболее крупными и потенциально опасными оказываются неэффективности зацикливания: “Возврат” и “Пинг-понг”, сосредоточенные вокруг этапа “Сборка заказа” - суммарно потенциальный финансовый эффект составляет 653 431 р.
- эффект от остальных неэффективностей за три месяца работы на порядок меньше (десятки тысяч)

__Основные рекомендации:__
- для этапа "Оплата" - проверка качества, надежности и отказоустойчивости используемой платежной системы
- для этапа "Звонок клиенту" - регламентация работы, совершенствование системы обновления количества товаров в наличии на сайте магазина
- для этапов "Сборка заказа" и "Доставка заказа" - регламентация и автоматизация этапа
- для этапа "Упаковка товара" - усиление системы контроля качества
- в целом, в первую очередь необходимо сосредоточить внимание на совершенствовании трех этапов работы:
1. этап “Сборка заказа” - требует регламентации деятельности и обучения работников, чтобы сократить количество зацикливаний и повторений процедур, сокращение длительности операции (стремиться к 10 минутам)
2. этап “Доставка заказа” требует большего контроля за деятельностью курьеров, своевременностью доставки, т.к. является потенциально самым финансово опасным
3. этап “Звонок клиенту” - это этап, который необходимо минимизировать за счет совершенствования автоматизации процессов сбора информации о клиенте, о заказе и о наличии товаров


### Используемый стек инструментов

- Python
- Pandas
- PM4PY
- Matplotlib
- Jupyter Notebook


