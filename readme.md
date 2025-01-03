# Лабораторная работа: Использование планировщика задач cron

## Цели работы

 1. Ознакомиться с основами работы планировщика задач cron.
 2. Научиться создавать, редактировать и управлять заданиями cron.
 3. Понять типичные сценарии использования cron для автоматизации задач.

### Теоретическое введение

cron — это утилита для выполнения задач по расписанию в Unix-подобных системах. Конфигурация заданий происходит через файл crontab. Задания могут выполняться ежедневно, ежечасно, еженедельно и т.д.

Формат записи в crontab:
```cmd
* * * * * /команда/или/скрипт
# | | | | |
# | | | | +---- День недели (0 - воскресенье, 1 - понедельник, ...)
# | | | +------ Месяц (1-12) или jan,feb,mar,apr ...
# | | +-------- День месяца (1-31)
# | +---------- Час (0-23)
# +------------ Минута (0-59)
```


## Часть 1: Простое задание

### Задание: выполнение команды ```curl``` каждые 15 минут.

1. Перед установкой cron обновим локальный индекс пакетов компьютера:

   ```
   sudo apt update
   ```

2. Затем выполните установку cron с помощью следующей команды:

   ```
   sudo apt install cron
   ```

3. Убедитесь, что cron установлен и работает.
Выполните команду:

   ```
   systemctl status cron
   ```

   Если cron не запущен, включите его:

   ```
   sudo systemctl start cron
   ```

   Также нужно убедиться, что он настроен для работы в фоновом режиме: 

   ```
   sudo systemctl enable cron
   ```

   Output должен быть похож на это:
   ```
   Synchronizing state of cron.service with SysV service script with /lib/systemd/systemd-sysv-install.
   Executing: /lib/systemd/systemd-sysv-install enable cron
   ```

4. Откройте редактор crontab:

   ```
   crontab -e
   ```

   В редакторе можно добавлять или изменять задачи cron для текущего пользователя.

5. Добавьте задание для выполнения ```curl```:

   ```
   */15 * * * * curl http://www.google.com
   ```

   */15 * * * * — команда выполняется каждые 15 минут, начиная с полуночи.

   curl http://www.google.com - выполнение команды curl для google.com

   Сохраните изменения и выйдите из редактора.
   После сохранения ```cron``` начнёт выполнять задание.

6. Проверьте список заданий:

   ```
   crontab -l
   ```

   Эта команда выводит все задания, добавленные для текущего пользователя.

7. Убедитесь, что команда ```curl``` выполняется.
Подождите 15 минут или вручную выполните команду:

   ```
   curl http://www.google.com >> ~/cron_test.log 2>&1
   ```

8. Затем проверьте логи:

   ```
   cat ~/cron_test.log
   ```


## Часть 2: Задание на выполнение

### Задание: Настройте задание cron, которое отправляет статистику загрузки процессора и памяти на email каждые 5 минут.

Создайте bash-скрипт ```system_stats.sh```, в котором будет появляться статистика загрузки процессора и памяти, а так же отправка этой же статистики на почту.

Настройте cron так чтобы каждые 5 минут у Вас на почте появлялась эта статистика.


### Материалы
[Cron: что это такое и как его правильно использовать](https://timeweb.com/ru/community/articles/chto-takoe-cron)

[Cron документация с Ubuntu Wiki](https://help.ubuntu.ru/wiki/cron)

[Интересная статья про cron с хабра](https://habr.com/ru/companies/skillfactory/articles/656423/)
