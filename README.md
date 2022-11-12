# Вариант 11
### Условие задачи
Разработать программу вычисления отдельно количества гласных и согласных букв в ASCII-строке.
## Про оформление
Моя работа выполнена на 9 баллов, в своём отчёте я пройдусь по каждому пункту критериев, начиная от 4 баллов до 9 баллов.

# Ну что ж, начнём
## Критерии на 4 балла:
#### :white_check_mark: *Приведено решение задачи на C*
- его можно найти в приложенных файлах (папка ci)
#### :white_check_mark: *В полученную ассемблерную программу, откомпилированную без оптимизирующих и отладочных опций, добавлены комментарии, поясняющие эквивалентное представление переменных в программе на C.*
- код на ассемблере и его комментирование можно найти в файлах с расширением .s
#### :white_check_mark: *Из ассемблерной программы убраны лишние макросы за счет использования соответствующих аргументов командной строки и/или за счет ручного редактирования исходного текста ассемблерной программы.*
- Программа откомпилирована при помощи флагов gcc

    gcc -masm=intel \
    -fno-asynchronous-unwind-tables \
    -fno-jump-tables \
    -fno-stack-protector \
    -fno-exceptions \
    ./code.c \
    -S -o ./code.s
#### :white_check_mark: *Модифицированная ассемблерная программа отдельно откомпилирована и скомпонована без использования опций отладки.*
- имеются исполняемые файлы
#### :white_check_mark: *Представлено полное тестовое покрытие, дающее одинаковый результат на обоих программах. Приведены результаты тестовых прогонов для обоих программ, демонстрирующие эквивалентность функционирования.*
- про тесты будет сказано ниже (они есть)

## Критерии на 5 баллов:
#### :white_check_mark: *В реализованной программе необходимо использовать функции с передачей данных через параметры.*
- такие функции есть. Например, передача имён файлов для ввода и вывода данных в work_with_file.s
#### :white_check_mark: *Использовать локальные переменные.*
- используются, например, count_vowels и count_consonants(кол-во гласных и согласных) в work_with_file.s и generate_choice.s
#### :white_check_mark: *В ассемблерную программу при вызове функции добавить комментарии, описывающие передачу фактических параметров и перенос возвращаемого результата.*
- при вызове функций в коде ассемблера указано, в каких регистрах лежат передаваемые занчения
- также даны комментарии, в каком регистре лежит какое возвращаемое значение
#### :white_check_mark: *В функциях для формальных параметров добавить комментарии, описывающие связь между параметрами языка Си и регистрами (стеком)*

## Критерии на 6 баллов:
#### :white_check_mark: *Рефакторинг программы на ассемблере за счет оптимизации использования регистров процессора. Или написание ассемблерной программы с нуля, используя собственное распределение регистров.*
- в main.s rbp[-4](argc) -> r12d (4 байта)
- в work_with_file.s -> rbp[-40](размер строки в файле) -> r12 (8 байт), rbp[-4](кол-во гласных) -> r13d, rbp[-8](кол-во согласн.) -> r14d
- в generate_choice.s rbp[-20](длина строки) -> r12d, rbp[-4](кол-во гласных) -> r13d, rbp[-8](кол-во согласн.) -> r14d
#### :white_check_mark: *Добавление комментариев в разработанную программу, поясняющих эквивалентное использование регистров вместо переменных исходной программы на C.*
- комментирование для данного пункта хранится в папке asm_regs
#### :white_check_mark: *Представление результатов тестовых прогонов для разработанной программы. Оценка корректности ее выполнения на основе сравнения тестовых прогонов результатами тестирования программы, разработанной на языке C.*
#### :white_check_mark: *Сопоставление размеров программы на ассемблере, полученной после компиляции с языка C с модифицированной программой, использующей регистры.*
- потом вернусь

## Критерии на 7 баллов:
#### :white_check_mark: *Реализация программы на ассемблере в виде двух или более единиц компиляции (программу на языке C разделять допускается, но не обязательно).*
- имеется три единицы компиляции: main.s, work_with_file.s и generate_choice.s
#### :white_check_mark: *Использование файлов с исходными данными и файлов для вывода результатов. Имена файлов задаются с использованием аргументов командной строки. Командная строка проверяется на корректность числа аргументов и корректное открытие файлов.*
- через командную строку можно передать названия файлов для ввода и вывода данных
- есть проверка на кол-во аргументов командной строки, если их кол-во = 0, то выводится строка с ошибкой "Incorrect input" и программа завершается
- в методах work_with_file.s есть проверка на корректность открытия файла, если ошибка, то выводится строка "Problems with file" и программа завершается

## Критерии на 8 баллов:
#### :white_check_mark: *Использование в разрабатываемых программах генератора случайных наборов данных, расширяющих возможности тестирования*
- В программе есть возможность сгенерирвовать случайную строку длиной от 1 до 100 символов
#### :white_check_mark: *Изменение формата командной строки с учетом выбора ввода из файлов или с использованием генератора.*
- при запуске пользователь может ввести выбрать, как он хочет тестировать данные: читать из файлов или же сгенерировать строку
- если после ./test.exe(пример запуска) введена одна любая строка (без пробелов), то это будет вызван метод генерации случайной строки
- если после ./test.exe(пример запуска) введены две строки, то вызовется метод чтения и записи в файл
#### :white_check_mark: *Пункт про замеры времени*
- независимо от выбора ввода данных (файл или генерация) будет проведён замер времени работы алгоритма
- пользовтелю в конце работы программы выводится строка "Algorithm operation time: время работы в наносекундах"
#### :white_check_mark: *Представить полученные временные данные в отчете для разных вариантов тестовых прогонов (наряду с другими данными, полученные при выполнении ранее описанных требований).*
- Время работы алгоритмов будет приведено ниже

## Критерии на 9 баллов:
#### :white_check_mark: *Используя опции оптимизации по скорости, сформировать из программы на на C исходный код ассемблере. Провести сравнительный анализ с ассемблерной программой без оптимизации по размеру ассемблерного кода, размеру исполняемого файла и производительности. Сопоставить эти программы с собственной программой, разработанной на ассемблере, в которой вместо переменных максимально возможно используются регистры.*
