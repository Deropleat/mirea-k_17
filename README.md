# К_17 Моделирование работы 3D принтера
Описание системы:

Дана система, состоящая из следующих элементов:
+ 3D принтер;
+ панель управления 3D принтером;
+ множество катушек с филаментом (расходного материала, используемого для печати на 3D-принтере);
+ компьютер;
+ множество изделий для печати;
+ экрана отображения информации о функционировании системы.

3D принтер имеет следующие параметры:
+ скорость печати, измеряемая в метрах в такт (метр/такт);
+ скорость нагрева/охлаждения (℃/такт);
+ температура нагрева сопла экструдера (печатающей головки 3D принтера) в градусах Цельсия (℃);
+ катушки филамента.

Катушка филамента имеет следующие параметры:
+ температура печати пластика в градусах Цельсия (℃);
+ длина нитки филамента (объём катушки) в метрах;
+ идентификатор (одно слово).

Изделие имеет следующие параметры:
+ наименование;
+ идентификатор катушки (одно слово);
+ объём изделия (длина нити филамента, необходимая для печати изделия) в метрах.

3D принтер обслуживает ПК.

У 3D принтера множество состояний:
+ выключен = 0;
+ ожидание команды = 1;
+ печать изделия = 2;
+ нагрев/охлаждение сопла = 3;
+ ожидание катушки филамента = 4;
+ настройка 3D принтера = 5.

Система функционирует по тактам. Такты нумеруются с 1.

## Модель системы.
Правила функционирования системы:

1. В сети один персональный компьютер.
2. Количество катушек филамента задаётся параметром n.
3. Объём и температура нагрева для катушки филамента задаётся параметрами k и t соответственно.
4. Скорость печати для принтера – количество метров филамента в такт задаётся параметром v.
5. Скорость охлаждения и нагрева принтера, а также его начальная температура задаются параметрами r и j соответственно.
6. ПК организует очередь изделий для печати. Изделие характеризуется именем, названием филамента и объёмом (количеством метров филамента, необходимого для печати изделия).
7. После поступления изделия в очередь для распечатки на ПК, посылается сигнал 3D принтеру. Постановка изделия на ПК происходит вначале такта. (То есть, принтер полностью отрабатывает последующие действия.) Принтер организует очередь запросов. По мере готовности (температуры принтера) делает запрос к ПК, копирует модель изделия (получает от ПК) и организует печать. После передачи на 3D принтер изделие из очереди ПК удаляется.
8. Копирование нового изделия происходит с началом нового такта. То есть, если в этот такт до печаталось изделие, то копирование нового произойдёт в следующий такт.
9. Копирование изделия на печать занимает весь такт. (Действия после копирования не отрабатываются).
10. Перед началом печати 3D принтер нагревается/охлаждается до температуры, необходимой, чтобы данный филамент начал плавиться. Нагрев/охлаждение происходит со скоростью r. В данный момент 3D принтер переводится в состояния нагрева/охлаждения. Скорость r – максимальная для 1 такта, однако не обязательно достигать скорости r для нагрева до определённой температуры (пример currentTemp = 110, r = 100, то для нагрева до 120 всё равно необходим 1 такт ).
11. Если в катушке закончился филамент, то принтер переходит в состоянии ожидания загрузки катушки. Катушка загружается в течение трёх тактов в независимости от объёма (на конец 3 такта считается восполненной). Первый такт восполнения катушки проходит в момент её окончания после печати. Катушка не восполняется, если в этом нет необходимости. (То есть, в такт, когда 3D принтер напечатал некоторый объём изделия и закончилась катушка, происходит её первый такт восполнения, за ним следует ещё два, а потом она начинает работать).
12. Если очередь запросов  3D принтера пуста, то он переходит в состоянии ожидания.
13. Перед началом очередного такта может быть подана команда. Команда отрабатывает до отработки действий такта.

Необходимо моделировать работу данной системы.

## Команды системы:
1. Команда постановки изделия в очередь для печати на ПК.
2. Команда отображения состояния ПК.
3. Команда отображения состояния системы.
4. Пустая команда (строка ничего не содержит). Элементы системы выполняют действия согласно такту.
5. Команда завершения работы системы.

## Построить программу-систему, которая использует объекты:
1. Объект «система».
2. Панель управления 3D принтером - объект для чтения исходных данных и команд. Считывает данные для первоначальной подготовки и настройки системы. Считывает команды. Объект моделирует нажатия на кнопки 3D принтера или действия пользователя за ПК. После чтения очередной порции данных для настройки или данных команды, объект выдает сигнал с текстом полученных данных. Все данные настройки и данные команд по структуре корректны. Каждая строка команд соответствует одному такту (отрабатывается в начале такта). Если строка пустая, то система отрабатывает один такт (все элементы системы отрабатывают положенные действия или находятся в состоянии ожидания).
3. Объект ПК. Содержит очередь изделий для печати. Данному объекту по иерархии подчинены объекты изделий, ожидающих печать. Сигналами общается с 3D принтером.
4. Объект изделие. Содержит наименование, объём (длина нити филамента, необходимая для печати изделия) в метрах, идентификатор катушки.
5. Объект катушка филамента. Содержит идентификатор (одно слово), объём (длина нити филамента в катушке) в метрах, температура печати.
6. Объект 3D принтера, моделирует работу контролера управления 3D принтером. Анализирует состояние, управляет отработкой действий, предусмотренных в рамках одного такта. Объект выдает соответствующие сигналы для других основных элементов системы.
7. Объект для вывода информации. Текст для вывода объект получает по сигналу от других объектов системы. Каждое присланное сообщение выводиться с новой строки.

## Архитектура иерархи объектов:

Объект системы моделирования работы 3д принтера.
1. Объект ввода (панель управления).
2. Объект вывода (информационное окно).
3. Объект 3D принтера
    1. Объект катушка филамента 1.
    2. ….
    3. Объект катушка филамента n.
    4. Объект изделие, которое печатается.
5. Объект ПК
    1. Объект изделие 1 в очереди на печать.
    2. Объект изделие 2 в очереди на печать.
    3. ….
    4. Объект изделие m в очереди на печать.

## Сконструировать программу-систему, реализующую следующий алгоритм:
1. Вызов от объекта «система» метода build_tree_objects(), построения иерархии объектов.
     1. Построение исходного дерева иерархии объектов. После построения все объекты перевести в состояние готовности.
     2. Цикл для обработки вводимых данных.
         1. Выдача сигнала объекту чтения для ввода очередной строки данных (количество катушек филамента, начальная температура сопла, скорость нагрева/охлаждения сопла, скорость печати).
         2. Отработка операции чтения очередной строки входных данных.
         3. Исходя из значения количества катушек филамента, создание соответствующего количества объектов катушек филамента с параметрами (идентификатор, температура печати, объём). Установка связей сигналов и обработчиков с новым объектом.
    3.Установка связей сигналов и обработчиков между объектами.
2. Вызов от объекта «система» метода exec_app().
    1. Цикл для обработки вводимых команд.
        1. Определение номера очередного такта.
        2. Выдача сигнала объекту ввода для чтения очередной команды.
        3. Отработка команды.
        4. Отработка действий согласно такту.
    2. После ввода команды «Turn off the system» немедленно завершить работу.

Все приведенные сигналы и соответствующие обработчики должны быть реализованы. Запрос от объекта означает выдачу сигнала. Все сообщения на консоль выводятся с новой строки.

В набор поддерживаемых команд добавить команду «SHOWTREE» и по этой команде вывести дерево иерархии объектов системы с отметкой о готовности и завершить работу системы (программы). Реализовать два отладочных теста такой командой. Первый после завершения построения дерева иерархии объектов. Второй перед завершением работы системы. Во втором тесте обязательно отработать не менее одной команды запроса печати изделия.

При решении задачи необходимо руководствоваться методическим пособием и приложением к методическому пособию.

## Входные данные
Первая строка, количество катушек филамента n, скорость 3D принтера v,  скорость нагрева/охлаждения r, начальная температура сопла 3D принтера j:
> «целое число»_«целое число»_«целое число»_«целое число»

Далее n строк вводятся параметры катушки филамента (идентификатор, температура печати, длина нитки филамента):

>«одно слово»_«целое число»_«целое число»

Далее строка:
> End of settings

Далее построчно вводятся команды. Они могут следовать в произвольном порядке.

Команда постановки изделия в очередь для печати на ПК.
> PC_«объём изделия»_«идентификатор катушки»_«название изделия»

По этой команде, создается объект изделие и на дереве иерархии объектов подчиняется объекту ПК. Изделие ставится в очередь для печати на ПК и сигнал запроса на печать посылается 3D принтеру. Если 3D принтер не печатает изделие в текущий момент, то объект переназначается на 3D принтер.

Гарантируется, что катушка по параметру идентификатора существует в системе.

Команда отображения состояния ПК:
> PC condition

Команда отображения состояния катушки филамента:
> Filament coil condition «идентификатор»

Команда отображения состояния системы:
> System status

Пустая команда (строка ничего не содержит).

Элементы системы выполняют действия согласно такту.

Команда завершения работы системы.
> Turn off the system

## Пример ввода:
```
3 5 100 0
abs 200 50
pla 160 100
petg 190 5
End of settings
System status
PC 5 pla Product 1
PC 10 abs Product 2
PC condition
Filament coil condition pla
PC 10 petg Product 3
System status
PC condition
  
  
  
  
  
Filament coil condition petg
PC condition
System status
Turn off the system
```

## Выходные данные
После завершения ввода исходных данных выводиться текст:
> Ready to work

После команды ввода параметров катушки филамента, если существует объект с таким названием во всём дереве.
> Failed to create filament coil

После команды отображения состояния ПК вывести.

Если у ПК есть очередь изделий на печать, не считая текущего:
> PC condition turned on «наименование изделия 1»; «наименование изделия  2»; . . .

Наименования документов упорядочены согласно очереди на печать на ПК.  

Если у пк нет изделий на печать:
> PC condition turned off

После команды создания изделия.

Если в дереве есть объект с именем изделия:
> Failed to create product

После команды отображения состояния катушки филамента:
> Filament coil condition: «идентификатор» «оставшееся количество филамента»

После команды отображения состояния системы:
> 3D printer tact: «номер такта»; temp: «температура сопла»; status: «статус принтера»; print product: «количество оставшегося объёма печатаемого изделия»; queue products: «количество изделий в очереди на печать»;  PC: («название изделия 1»:«объём изделия») («название изделия 2»:«объём изделия») («название изделия 3»:«объём изделия»)  . . .

После команды завершения работы системы вывести:
> Turn off the system

## Пример вывода:
```
Ready to work
3D printer tact: 1; temp: 0; status: 1; print product: 0; queue products: 0; PC:
PC condition turned on Product 1; Product 2;
Filament coil condition: pla 100
3D printer tact: 7; temp: 200; status: 3; print product: 0; queue products: 2; PC: Product 2:10 Product 3:10
PC condition turned on Product 3;
Filament coil condition: petg 5
PC condition turned off
3D printer tact: 17; temp: 190; status: 1; print product: 0; queue products: 0; PC:
Turn off the system
```
---
### Автор - https://vk.com/prosto_garlk
