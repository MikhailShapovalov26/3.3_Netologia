"3.3. Операционные системы, лекция 1"
===============================
1. 
       execve("/bin/bash", ["/bin/bash", "-c", "cd /tmp"], 0x7ffe2d9bde30 /* 52 vars */) = 0
2. 
 /etc/ld.so.cache
 В Системном вызове openat() 
 openat() позволяет реализовать отдельный «текущий рабочий каталог» для каждой нити посредством файлового дескриптора, сопровождаемого приложением. Эта возможность также может быть получена с использованием /proc/self/fd/dirfd, но менее эффективно
 3. 
 Листинг

        ping 8.8.8.8 >log.log &
        [1] 7311
Далее смотрит процессы

        Ps
        PID TTY          TIME CMD
        7035 pts/2    00:00:00 bash
        7311 pts/2    00:00:00 ping
        7315 pts/2    00:00:00 ps
Смотрим, что процесс идёт. 
Смотрим наличие файла в папке

        ls log.log 
        log.log
Далее удаляем лог файл

        rm -rf log.log 
проверяем

        ls log.log 
        ls: невозможно получить доступ к 'log.log': Нет такого файла или каталога
Проверяем процесс

        ps
        PID TTY          TIME CMD
        7035 pts/2    00:00:00 bash
        7311 pts/2    00:00:00 ping
        7326 pts/2    00:00:00 ps
Далее пришлось перейти по root!
Так как знаем номер процесса, то мы знаем каталог в каком смотреть

        ls -l /proc/7311/fd | grep "log.log"
Результат подсказал, какой дальше путь в котором находится наш закэшированый файл

        l-wx------ 1 root root 64 янв 19 21:46 1 -> /home/mikhail/log.log (deleted)

Далее удаляем 

        truncate -s 0 /proc/7311/fd/1 
        #Смотрим процессы на пинг и убиваем. 
        ps aux  | grep ping
        mikhail     7311  0.0  0.0   7200   800 pts/2    S    21:43   0:00 ping 8.8.8.8
        root        7533  0.0  0.0   6204   648 pts/2    S+   21:56   0:00 grep ping
4. 
Нашёл в интернете скрипт питона для zombi процесса. 

        mikhail     8532  0.0  0.0      0     0 pts/3    Z+   22:11   0:00 [ls] <defunct>
Судя по 0 во всех столбцах, ресурсы он не расходует.
5. 

    /usr/sbin/opensnoop-bpfcc
    972    teamviewerd        14   0 /dev/fb0
    972    teamviewerd        14   0 /sys/devices/virtual/tty/tty0/active
    972    teamviewerd        14   0 /sys/devices/virtual/tty/tty0/active
    972    teamviewerd        14   0 /dev/tty7
    4288   code               59   0 /proc/8393/cmdline
    972    teamviewerd        14   0 /sys/devices/virtual/tty/tty0/active
    4288   code               59   0 /proc/8393/cmdline
6. 
        uname() returns system information in the structure pointed to by buf.

Возращает системную информацию в структуре, на которую указывает буфер

        Part of the utsname information is also accessible via /proc/sys/kernel/{ostype, hostname, osrelease, version, domainname}.
7. 
&& это аналог и это аналог логической операции. к примеру если Операция 1 вернула 1(true) и операция 2 вернула 1, то вся связка пройдет.  

Операция 1 вернёт false (0) и вторая операция вернёт 2, то команда вернёт ответ что ожидается унитарный оператор  

Если оператор 1=1 и оператор 2 =0 то выдаст результат 1 команды, а на вторую выдаст ошибку.   

; служит для перебора команд, результат 1 команды никак не влияет на выод команды полностью.   

set -e- не имеет смысла, так как при ошибке , выполнение команд прекратиться.

8. 
Для того, чтоб знать о ошибке во время выполнения конвейера можно выставить опцию pipefail, которая указывает оболочке, что код завершения конвейера будет идентичен первому ненулевому коду завершения одной из команд данного конвейра.  
 Конвейеры чаще всего используются в shell-скриптах для связи нескольких команд путем перенаправления вывода одной команды (stdout) на вход (stdin) последующей, используя символ конвеера ‘|’.  

9. 
S* -процессы ожидающие завершения
I* -фоновые (бездействующие) процессоры ядра
Доп символы это дополнительные характеристики для поиска, фильтрации, приоритет, непрерывный сон.  