# Домашнее задание к занятию 3 «Резервное копирование»

 ---

### Задание 1

- Составьте команду rsync, которая позволяет создавать зеркальную копию домашней директории пользователя в директорию `/tmp/backup`
- Необходимо исключить из синхронизации все директории, начинающиеся с точки (скрытые)
- Необходимо сделать так, чтобы rsync подсчитывал хэш-суммы для всех файлов, даже если их время модификации и размер идентичны в источнике и приемнике.
- На проверку направить скриншот с командой и результатом ее выполнения


### Решение 1

#### Команды резервного копирования с проверкой хеш суммы


	'rsync -avhc --progress --exclude=".*/" ~andy/* /tmp/backup/'
	
	'-a - копировать всё, включая атрибуты'
	'-h - читаемо для людей'
	'-v - выдавать больше информации о копировании'
	'-с - проверять check sum, копировать файлы с изменившейся контрольной суммой' 


  *Скриншоты задания №1*

*Завершение копирования*

![Commit Task1](https://github.com/AndrewZnamenskiy/Rsync_HW/blob/main/img/task1p1.png)

*Результат резервного копирования*

![Commit Task1](https://github.com/AndrewZnamenskiy/Rsync_HW/blob/main/img/task1p2.png)


 ---

### Задание 2

- Написать скрипт и настроить задачу на регулярное резервное копирование домашней директории пользователя с помощью rsync и cron.
- Резервная копия должна быть полностью зеркальной
- Резервная копия должна создаваться раз в день, в системном логе должна появляться запись об успешном или неуспешном выполнении операции
- Резервная копия размещается локально, в директории `/tmp/backup`
- На проверку направить файл crontab и скриншот с результатом работы утилиты.


### Решение 2

#### Команды для добавления резервного копирования в планировщик cron

	
	'crontab -e'

	'30 22 * * * rsync -av --delete --log-file=/home/andy/rsync_log.log ~andy/ /tmp/backup/'



  *Скриншоты задания №2*

*Результ копирования в целевую папку*

![Commit Task2](https://github.com/AndrewZnamenskiy/Rsync_HW/blob/main/img/task2p1.png)


*Отредактированный планировщик*

![Commit Task2](https://github.com/AndrewZnamenskiy/Rsync_HW/blob/main/img/task2p2.png)


*Файл конфигурации Task2: [Rsync_Task2](task2-cfg/crontab)*


 ---


### Задание 3*
- Настройте ограничение на используемую пропускную способность rsync до 1 Мбит/c
- Проверьте настройку, синхронизируя большой файл между двумя серверами
- На проверку направьте команду и результат ее выполнения в виде скриншота

### Решение 3

#### Для исполнения команды без запроса пароля требуется добавить публичный ключ на удалённый сервер

*Генерируем публичный ключ и копируем его на удалённый сервер: *

	'ssh-keygen -t rsa -b 2048'

	'ssh-copy-id -i /home/andy/.ssh/id_rsa.pub andy@192.168.101.56'

			'или'

	'ssh-copy-id andy@192.168.101.56'

*Запускаем команду rsync с ключём ограничения скорости 1Мбит/c:*

	'rsync --bwlimit=1MiB --progress -avhe ssh /home/andy/sorochan.avi andy@192.168.101.56:/home/andy/'


  *Скриншоты задания №3*

  *Процесс резервного коприрования на удалённый сервер*

![Commit Task3](https://github.com/AndrewZnamenskiy/Rsync_HW/blob/main/img/task3p1.png)

  *Результат копирования на удалённый сервер*

![Commit Task3](https://github.com/AndrewZnamenskiy/Rsync_HW/blob/main/img/task3p2.png)


-----
