# Вариант 20
### Условие задачи
Разработать программу вычисления числа π с точностью не хуже
0,05% посредством произведения элементов ряда Виета.

## Про оформление
Моя работа выполнена на 9 баллов, в своём отчёте я пройдусь по каждому пункту критериев, начиная от 4 баллов до 9 баллов.
- в папке asm лежат прокоммментированные файлы на ассемблере скомпилированные с флагами gcc 
- в папке asm_no_opt лежат файлы на ассемблере без оптимизации
- в папке asm_regs лежат  прокоммментированные файлы на ассемблере, где используются регистры процессора вместо переменных на стеке
- в папке size_opt лежат файлы на ассемблере, оптимизированные по размеру
- в папке speed_opt лежат файлы на ассемблере, оптимизированные по скорости
- в папке ci лежат файлы с кодом на си
- в папке tests лежат тесты

# Ну что ж, начнём
## Критерии на 4 балла:
#### :white_check_mark: *Приведено решение задачи на C*
- его можно найти в приложенных файлах (папка ci)
#### :white_check_mark: *В полученную ассемблерную программу, откомпилированную без оптимизирующих и отладочных опций, добавлены комментарии, поясняющие эквивалентное представление переменных в программе на C. То есть, для всех ссылок на память, включая и относительные адреса и регистры, указать имя переменной на языке C исходной программы.*
#### :white_check_mark: *Из ассемблерной программы убраны лишние макросы за счет использования соответствующих аргументов командной строки и/или за счет ручного редактирования исходного текста ассемблерной программы.*
- всё прокомментировано
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
- такие функции есть. Напрмер, в write_to_file передаётся имя файла для записи, посчитанный результат и кол-во знаков после запятой
#### :white_check_mark: *Использовать локальные переменные.*
- таких очень много, особенно в файле algorithm_pi
#### :white_check_mark: *В ассемблерную программу при вызове функции добавить комментарии, описывающие передачу фактических параметров и перенос возвращаемого результата.*
- при вызове функций в коде ассемблера указано, в каких регистрах лежат передаваемые занчения
- также даны комментарии, в каком регистре лежит какое возвращаемое значение
#### :white_check_mark: *В функциях для формальных параметров добавить комментарии, описывающие связь между параметрами языка Си и регистрами (стеком)*

## Критерии на 6 баллов:
#### :white_check_mark: *Рефакторинг программы на ассемблере за счет оптимизации использования регистров процессора. Или написание ассемблерной программы с нуля, используя собственное распределение регистров.*
- в функции main 1) переменные типа double number и del перемещены в xmm4 (из rbp[-24]) и xmm5 (из rbp[-32]) соотв. argc перемещена в r12d (из rbp[-68])
- в функции algorithm_pi переменные типа double sqr_two и denominator перемещены в xmm3 (из rbp[-16]) и xmm2 (из rbp[-48]) соотв.
#### :white_check_mark: *Добавление комментариев в разработанную программу, поясняющих эквивалентное использование регистров вместо переменных исходной программы на C.*
- комментирование для данного пункта хранится в папке asm_regs
#### :white_check_mark: *Представление результатов тестовых прогонов для разработанной программы. Оценка корректности ее выполнения на основе сравнения тестовых прогонов результатами тестирования программы, разработанной на языке C.*
#### :white_check_mark: *Сопоставление размеров программы на ассемблере, полученной после компиляции с языка C с модифицированной программой, использующей регистры.*
- размер исполняемого файла программы, не изпользующей регистров - 16,6 кБ (16 576 байт), с регистрами - 16,6 кБ (16 576 байт)

## Критерии на 7 баллов:
#### :white_check_mark: *Исходные данные и формируемые результаты должны вводиться и выводиться через файлы. Имена файлов задаются с использованием аргументов командной строки. Ввод данных в программу с клавиатуры и вывод их на дисплей не нужен. За исключением сообщений об ошибках.*
- есть чтение из файла и запись в файл, имена файлов передаются через аргументы командной строки
#### :white_check_mark: *Командная строка проверяется на корректность числа аргументов в программе должна присутствовать проверка на корректное открытие файлов. При наличии ошибок должны выводиться соответствующие сообщения.*
- при неверном кол-ве аргументов выводится строка "Incorrect input"
- при проблеме с работой файлов - "Problems with file"
#### :white_check_mark: *Реализация программы на ассемблере в виде двух или более единиц компиляции (программу на языке C разделять допускается, но не обязательно).*
- имеется три единицы компиляции

## Критерии на 8 баллов:
#### :white_check_mark: *Использование в разрабатываемых программах генератора случайных наборов данных, расширяющих возможности тестирования*
- при выборе генерации - пользователю случайно сгенерируется кол-во знаков после запятой в посчитанном числе пи
- также независимо от выбора (читать из файла или генерировать) - всегда генерируется точность вычислений числа пи (т.е насколько посчитаннное число отличается от реального числа пи)
#### :white_check_mark: *Изменение формата командной строки с учетом выбора ввода из файлов или с использованием генератора.*
- чтобы использовать генерацию данных - нужно после названия запускаемого файла написать "g" и имя файла, в который будет выведен результат
- если же пользователь хочет считать данные из файла, то после названия запускаемого файла нужно написать "file", откуда читаем, куда записываем ответ
#### :white_check_mark: *Включение в программы функций, обеспечивающих замеры времени для проведения сравнения на производительность. Необходимо добавить замеры во времени, которые не учитывают время ввода и вывода данных.*
- есть такое
- для каждого запуска в консоль выводится время работы алгоритма
#### :white_check_mark: *Представить полученные временные данные в отчете для разных вариантов тестовых прогонов (наряду с другими данными, полученные при выполнении ранее описанных требований).*
- Время работы алгоритмов будет приведено ниже (на скриншотах будет видна строка "Algorithm operation time: время на наносекундах")

## Критерии на 9 баллов:
#### :white_check_mark: *Используя опции оптимизации по скорости, сформировать из программы на на C исходный код ассемблере. Провести сравнительный анализ с ассемблерной программой без оптимизации по размеру ассемблерного кода, размеру исполняемого файла и производительности. Сопоставить эти программы с собственной программой, разработанной на ассемблере, в которой вместо переменных максимально возможно используются регистры.*
- для получения файлов, оптимизированных по скорости я использовала флаг -O3
- для получения файлов, оптимизированных по размеру я использовала флаг -Os

- Файлы из папки asm_no_opt (без оптимизаций):
  - main.s - 230 строк, files.s - 63 строки, algorithm_pi.s - 147 строк
  - размер исполняемого файла - 16,6 кБ (16 624 байта)
  - время работы - 767.5 в среднем
- Файлы из папки speed_opt (оптимизированные по скорости)
  - main.s - 202 строки, files.s - 59 строк, algorithm_pi.s - 143 строки
  - размер исполняемого файла - 16,7 кБ (16 656 байт)
  - время работы - 359.75 в среднем
- Файлы из папки size_opt (опитизированные по размеру)
  - main.s - 188 строк, files.s - 56 строк, algorithm_pi.s - 99 строк
  - размер исполняемого файла - 16,7 кБ (16 656 байт)
  - 654 в среднем
- Файлы из папки asm_regs (моя программа с использованием регистров процессора)
  - main.s - 214 строк, files.s - 47 строк, algorithm_pi.s - 124 строки
  - размер исполняемого файла - 16,6 кБ (16 576 байт)
  - 720.5 в среднем
  
  Итоги:
  1) Сравниваю speed_opt, asm_no_opt, asm_regs
    - в asm_regs меньше всего строк кода
    - у asm_regs самый маленький размер исполняемого файла
    - speed_opt самая быстрая
  2) Сравниваю size_opt, asm_no_opt, asm_regs
    - в size_opt меньше всего строк кода
    - у size_opt самый маленький размер исполняемого файла
    - size_opt самая быстрая
    
    
### Теперь тестовые прогоны
Их я буду делать с программой, где используются регистры процессора и кодом на си

В моей проорамме нужно вывести число пи. Если бы я оставила одну точность, то результат всегда бы одинаковым получался, а это не интересно, поэтому точность генерируется разная, в силу этого для каждого запуска свой ответ.

Для начала я протестирую программу на си (для файла и для рандномной генерации), затем на ассемблере. 

Время работы алгоритма, кол-ва знаков после запятой, точность вычислений и результат - всё будет видно в консоле (результат также пишется в файл всегда)
## Для си (генерация кол-ва чисел после запятой):
![image](https://user-images.githubusercontent.com/115434090/203661755-da8188de-55ec-46a5-a651-747abb94236b.png)
## Для си (чтение из файла, каждый раз буду менять значения для кол-ва чисел после запятой ):
![image](https://user-images.githubusercontent.com/115434090/203663054-0fc8a1e6-6ce6-40f5-a865-5acccc663ad6.png)
## Для ассемблера (генерация кол-ва чисел после запятой)
![image](https://user-images.githubusercontent.com/115434090/203662468-9e74f524-dac0-4654-9bbb-87575bd1ed9e.png)
## Для ассемблера (чтение из файла, каждый раз буду менять значения для кол-ва чисел после запятой )
![image](https://user-images.githubusercontent.com/115434090/203662821-9547b5a3-f19d-49fd-8260-44a138c0ed5c.png)


