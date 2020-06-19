# Задание №1




##  За что отвечает файл [link](https://github.com/misterch0c/shadowbroker/blob/master/windows/Resources/TeDi/PyScripts/sigs.py)? Зачем он нужен и как может использоваться? Какой группой он мог быть использован? На основании открытых источников составьте список (с ссылками) соотношения сигнатур и известных АПТ-групп.




##  Умение анализировать код. Нужно найти ключ и описать, как это было сделано?

Я был немного удивлен, когда увидел обфусцированный питоновский скрипт. Но восстановить это бедстие было совсем несложно. Я решил пойти по-порядку
```python
  if __name__ == '__main__':
    O000O000O0OO00000O00()  
```
O000O000O0OO00000O00 - Это имя функции, с помощью Ctr + F ищу тукую же последовательность и переименовываю ее в function_one
```python
  if __name__ == '__main__':
    function_one()
```
Перехожу к function_one, чтобы привести ее к читабельному виду
```python
  def function_one():
    OO0OOOO0OO0OO0OO000O = input('> ')              # Мой ввод сохраняется в некоторую переменную
    if O0O0O0O0OO0O000OOOO0(OO0OOOO0OO0OO0OO000O):  # Вызов функции с переменной, содержащей мой ввод
        print('Ok')
    else:
        print('No')
```
Немного магии и получается следующий читаемый вид
```python
  def function_one():
    my_input = input('> ')
    if function_two(my_input):
        print('Ok')
    else:
        print('No')
```
Перехожу к function_two, действия аналогичны предыдущим
```python
  def function_two(my_input):
    O0O000OOO0OOOOO0OO0O = OO0OOO0OOOOO0000OOO0(my_input)                     # Вызов функции и сохранение возвращаемого значения в переменную
    O00OOO0O0OO0OOOOO000(O0O000OOO0OOOOO0OO0O)    # Вызов функции
    OOO0O0000O0O0O00OO0O(O0O000OOO0OOOOO0OO0O)    # Вызов функции
    OO0O0OO0O00O0000OO0O(O0O000OOO0OOOOO0OO0O)    # Вызов функции
    print(O0O000OOO0OOOOO0OO0O)                   # Вывод переменной
    return O0O00O0OO00OO0O00OO0(O0O000OOO0OOOOO0OO0O, O0O0O00OO0OO0OOOOOO0)   # Вызов функции с двумя параметрами и возврат возвращаемого значения функции
```
Читаемый вариант выгядит так:
```python
  def function_two(my_input):
    result = function_three(my_input)
    function_four(result)
    function_five(result)
    function_six(result)
    print(result)
    return function_seven(result, two_array)
```
####  Листинг кода
```python
two_array = [
    [2106, -7516, 764, -4620, -8448, 7794, -8047, 5554],
    [3617, 5156, -700, -9005, -8527, -1123, -3721, 9422],
    [-82, -8959, -7452, 4810, 3331, 5148, -2218, -6985],
    [-7087, 3842, -9310, -3049, -8457, 5378, 6376, 2405],
    [-5082, -5196, 4239, -1167, -7170, -9237, -7335, 7224],
    [631, -6429, 736, 1769, 3891, 6355, -1082, -2624],
    [326, 9142, 894, 5149, -3627, 8222, 7043, 9555],
    [-4432, -118, 3357, -1005, -6215, -4896, -686, -6897]]

def function_three(my_input):
    x = 1
    while x * x < len(my_input):
        x += 1
    result = [[0 for i in range(x)] for j in range(x)]  # Создание двумерного массива x * x
    for i, j in enumerate(my_input):                 
        result[i // x][i % x] = ord(j)                  # Заполнение двумерного массива аски кодом символов из введенной строки 
    for i in range(len(my_input), x*x):
        result[i // x][i % x] = -i                      # Заполнение остатка отрицательным счетчиком 
    return result


def function_four(result):
    for i in result:                                    # Перемещение сосдених индексов, начания с первых отрицательных чисел
        for j in range(0, len(i), 2):
            i[j], i[j+1] = i[j+1], i[j]
    for j in range(len(result[0])):                     # Перемещение измененных элементов к первоначальному состоянию
        for z in range(0, len(result), 2):
            result[j][z], result[j][z + 1] = result[j][z+1], result[j][z]


def function_five(result):
    for i in range(len(result)):
        for j in range(len(result[0])):
            result[i][j] += result[0][j]


def function_six(result):
    import random
    random.seed(753)
    for i in result:
        for j in range(len(i)):
            i[j] += random.randrange(-10000, 10000)     # Прибавление псевдослучайного числа, причем последовательность всегда одинаковая


def function_seven(result, two_array):
    if len(result) != len(two_array) or len(result[0]) != len(two_array[0]):  # Сравнение длин двумерных массивов и длин одномерных массивов под индексом 0
        return False
    return all(all(i == j for i, j in zip(x, y)) for x, y in zip(result, two_array))  # Поэлементное сравнение двумерных массивов


def function_two(my_input):
    result = function_three(my_input)   # Создание и инициализация двумерного массива
    function_four(result)               # Функция не изменяет двумерный массив
    function_five(result)               # Поэлементное сложение всех одномерных массивов с одномерным масивов под индексом 0
    function_six(result)                # Прибавление к каждому элементу псевдослучайной последовательности с одинаковым зерном
    print(result)                       # Вывод двумерного массива
    return function_seven(result, two_array)  # Сравнение result и two_array. Если они равны, возвращение True, иначе False


def function_one():
    my_input = input('> ')        # Считывается мой ввод
    if function_two(my_input):
        print('Ok')
    else:
        print('No')


if __name__ == '__main__':
    function_one()
```






##  В какой атаке засветился IP 159.226.234.29?

Где же засветился этот IP адрес? Первое что я сделал - это посмотрел информацию об этом IP адресе с помощью сервиса [2ip](https://2ip.ru/whois/).

![](info_ip.png)

Хм, Китай, да к тому же провайдер - Китайская Научно-Тухническая сеть.
> Мой интерес подогревается.

Затем я пошел в Гугл, и он меня не разочаровал:

  [11+ суперкомпьютеров было взломано криптомайнерами](https://securityboulevard.com/2020/05/11-plus-supercomputers-hacked-with-cryptominers-by-china/)
  
  [Вредоносные крипто-майнеры ставят под угрозу академические центры обработки данных](https://cloudsek.com/threatintelligence/malicious-crypto-miners-compromise-academic-data-centers/)
  
  [Просмотр недавних атак на суперкомпьютерные системы с точки зрения анализа угроз](https://www.freebuf.com/vuls/237119.html)
  
Отсюда делаю вывод, что данный IP - сервер Китайской Научно-Технической сети, занимающейся исследованиями COVID-19.

Этот IP был жертвой в недавнем инциденте, связанном со взломом суперкопьютеров. Источники утверждают, что злоумышленники подключались к серверу с помощью SSH, используя скомпрометированные данные. Злоумышленники установили вредоносное ПО, для майнинга криптовалюты Monero, и использовали различные методы, чтобы скрыть активность, включая вредоносный модуль ядра Linux. Было замечено, что основная вредоносная активность происходит в ночное время, чтобы исбежать обнаружения. Также был оставлен бэкдор в качестве SUID программы.

В данном инциденте пострадало больше одиннадцати суперкомпьютеров, в разных странах.

Сейчас скомпрометированные суперкомпьтеры отключены от внешней сети, и будут отключены до тех пор, пока безопасность не будет восстановлена.
