# Aruco detector
## Описание
Aruco detector - один из компонентов системы футбола луноходов. 
Выполняет следующие задачи:
- Поиск и распознавание на поле мяча и aruco меток
- Пересчёт в мировые координаты

## Установка
Для начала требуется установить основные компненты
### Windows
1. Установите [Python]  

### Linux (Ubuntu)
1. Установите Python: `sudo apt-get install python3`
2. Установите pip3: `sudo apt-get install python3-pip`

Установка пакетов производится в командной строке. 
Процесс не зависит от ОС.
```sh
pip3 install opencv-contrib-python==4.5.4.60
pip3 install numpy
pip3 install paho.mqtt
```

## Конфигурация
Перед запуском требуется сконфигурировать наш компнент для каждой отдельный камеры.
Для этого в файле `arucoConfig.py` необходимо задать следующие параметры:
1. `camId` Уникальный идентификатор камеры. Рекомендуется задавать порядковым номером камеры.
2. `hostName` Адрес mqtt брокера.
3. `mqtt_login` и `mqtt_pwd` Пароль и логин для подключения к mqtt брокеру.

В файле `pitchCalibration.py` необходимо задать физические координаты точек. Это делается в списке с названием `physCoords`

Пример заполнения:
```
	[10, 10], 		#0
	[185, 10],		#1
	[185, 283],		#2
	[10, 283],		#3
	[185, 146.5],	#4
	[10, 146.5],    #5
	[57, 62],     #6
	[138, 62],		#7
	[138, 231],		#8
	[57, 231]		#9
```
В [x, y] задаются координаты по осям x и y. Они измеряются рулеткой по полю. Для корректной работы необходимо сделать границу в 10 см, т.е. к каждой координате прибавить по 10см, а крайняя точка с координатам [0,0] станет с координатами [10, 10].
Под `#0` указан номер метки, для который вводятся координаты, где `0` - номер.
Точки 6 - 9 заполняются координатами [0,0].
## Работа компонента

### Подготовка к запуску
При каждом включении программы производится калибровка камеры. 
Для этого необходимо по углам и по центру поля разместить специальные [метки] с определённым идентификаторами, как показано на [схеме].


### Запуск скрипта
- Перейдите в директорию с файлами компонента.
- Выполните команду: `python3 camMain.py`
- Начнётся калибровка камеры, в появившемся окне будет показываться изображение с камеры и распознанными метками. Следует убедиться, что все необходимые метки попадают в угол обзора: каждая камера должна видеть ровно 4 метки, задающие границы. 
- После калибровки начнётся выполнение основного цикла программы, в этот момент следует убрать угловые метки, чтобы они не мешали работе. 

## Тестер камер
В репозитории содержится скрипт для проверки скорости и совместимости камеры. 

### Запуск скрипта
- Перейдите в директорию с файлами компонента.
- Выполните команду: `python3 camTester.py`
- Откроектся окно с изображением с камеры и начнётся подсчёт количества кадров.  

Во время работы скрипт выводит сообщения о количестве кадров за текущую секунду.  
По результатам работы скрипт выведет сообщение `Total FPS:XX`, где XX - итоговое среднее значение кадров в секунду.
Значения не должны отличатся от установленных в `arucoConfig.py` больше, чем на 5 кадров.

## Лицензия

GNU General Public License v3.0

[//]: # 
   [Python]: <https://www.python.org/downloads/>
   [схеме]: <pitchScheme.png>
   [метки]: <https://disk.yandex.ru/d/M4ZuyR_FUNPEDg>
