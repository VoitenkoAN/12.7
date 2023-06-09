# Домашнее задание к занятию 12.7. «Репликация и масштабирование. Часть 2» - `Мальцев Виктор`

---

Задание 1

Опишите основные преимущества использования масштабирования методами:

    активный master-сервер и пассивный репликационный slave-сервер;
    master-сервер и несколько slave-серверов;
    активный сервер со специальным механизмом репликации — distributed replicated block device (DRBD);
    SAN-кластер.

Дайте ответ в свободной форме.

Ответ:

Активный master-сервер и пассивный репликационный slave-сервер:
Основные преимущества данного подхода заключаются в повышении надежности и обеспечении отказоустойчивости. 
В случае сбоя активного master-сервера, пассивный slave-сервер может быстро взять на себя его функции, минимизируя время простоя. 
Этот метод также предоставляет возможность создания резервных копий данных с минимальным влиянием на производительность основного сервера.

Master-сервер и несколько slave-серверов:
Подход с использованием нескольких slave-серверов позволяет распределить нагрузку между ними, 
увеличивая общую пропускную способность и отказоустойчивость системы. 
Это также облегчает горизонтальное масштабирование, так как можно добавлять дополнительные slave-серверы в случае роста нагрузки.

Активный сервер со специальным механизмом репликации — distributed replicated block device (DRBD):
DRBD обеспечивает синхронизацию данных на блочном уровне между двумя серверами в режиме реального времени. 
Это позволяет обеспечить высокую доступность и отказоустойчивость системы, а также возможность быстрого восстановления после сбоев. 
DRBD также поддерживает автоматическое переключение между серверами в случае сбоя одного из них.

SAN-кластер (Storage Area Network):
SAN-кластер представляет собой высокопроизводительную сеть хранения данных, которая подключает серверы к централизованным хранилищам. 
Основные преимущества использования SAN-кластера включают централизованное управление данными, гибкость, масштабируемость и высокую доступность. 
SAN-кластеры также обеспечивают высокую скорость передачи данных между серверами и хранилищами, 
что способствует повышению производительности и эффективности системы.


---

Задание 2

Разработайте план для выполнения горизонтального и вертикального шаринга базы данных. База данных состоит из трёх таблиц:

    пользователи,
    книги,
    магазины (столбцы произвольно).

Опишите принципы построения системы и их разграничение или разбивку между базами данных.

Пришлите блоксхему, где и что будет располагаться. Опишите, в каких режимах будут работать сервера.

Ответ:

Для выполнения горизонтального и вертикального шаринга базы данных, предлагается следующий план:

    Вертикальный шаринг:
    Вертикальный шаринг предполагает разделение базы данных на несколько независимых частей по столбцам. 
    Это позволяет оптимизировать производительность, так как каждая часть может обрабатываться отдельным сервером. 
    В данном случае:
        Сервер 1: Таблица пользователи (ID, имя, фамилия, адрес, электронная почта);
        Сервер 2: Таблица книги (ID, название, автор, жанр, год издания);
        Сервер 3: Таблица магазины (ID, название, адрес, контактная информация).

    Горизонтальный шаринг:
    Горизонтальный шаринг предполагает разделение таблицы на несколько частей по строкам. 
    Это может быть выполнено на основе определенных критериев. 
    В данном случае, предполагается разделение каждой таблицы на две части:
        Сервер 4: Таблица пользователи (половина записей);
        Сервер 5: Таблица пользователи (вторая половина записей);
        Сервер 6: Таблица книги (половина записей);
        Сервер 7: Таблица книги (вторая половина записей);
        Сервер 8: Таблица магазины (половина записей);
        Сервер 9: Таблица магазины (вторая половина записей).


![alt text](https://github.com/vmmaltsev/screenshot2/blob/main/Screenshot_72.png)


Сервера будут работать в следующих режимах:

    Сервера 1, 2, и 3 (вертикальный шаринг) будут функционировать в качестве основных серверов для своих соответствующих таблиц. 
    Они будут обрабатывать запросы, связанные с чтением и записью данных в свои таблицы.

    Сервера 4-9 (горизонтальный шаринг) будут работать в качестве дополнительных серверов для разделенных таблиц. 
    Они будут обрабатывать запросы, связанные с чтением данных, а также синхронизировать данные с основными серверами (вертикальный шаринг).

Для обеспечения надежности и отказоустойчивости системы, рекомендуется настроить репликацию данных между серверами вертикального и горизонтального шаринга. 
Это может быть выполнено с помощью технологии master-slave репликации или других подходов.

Таким образом, предложенная схема позволяет оптимизировать производительность и обеспечить масштабируемость базы данных. 
Горизонтальное шаринг распределяет нагрузку между серверами, а вертикальное шаринг позволяет серверам фокусироваться на обработке определенных столбцов таблиц, 
что может существенно повысить эффективность работы системы.



