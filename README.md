## Домашнее задание 3 (дедлайн 28.09 14:40)

1. (3 балла) Проверьте следующие языки на регулярность, за каждое 1 балл. Либо докажите их нерегулярность, либо постройте для них автомат или регулярку.

    * ![equation](https://latex.codecogs.com/gif.latex?%5C%7Buabv%20%7C%20u%20%5Cin%20%5C%7Ba%2C%20b%5C%7D*%2C%20v%20%5Cin%20%5C%7Ba%2C%20b%5C%7D*%2C%20%7Cu%7C%20%3D%20%7Cv%7C%2C%20u%20%5Cneq%20v%5ER%5C%7D)

    * ![equation](https://latex.codecogs.com/gif.latex?%5C%7Ba%5Ekc%5Eme%5En%20%7C%20k%20%5Cge%200%2C%20n%20%5Cge%200%2C%20m%20%3D%20k%20&plus;%20n%20&plus;%201%5C%7D)

    * ![equation](https://latex.codecogs.com/gif.latex?%5C%7Ba%5En%20%7C%20%5Cexists%20p%20%5Cge%20n%3A%20p%7E%5Ctexttt%7Bprime%7D%7E%5Ctexttt%7Band%7D%7Ep%20&plus;%202%7E%5Ctexttt%7Bprime%7D%5D%5C%7D)

3. (5 баллов) Реализуйте на любом языке программирования (кроме Хаскелля) парсинг с помощью производных. Сами регулярки парсить из строки не нужно, просто объявите в коде несколько (не меньше 5) регулярок и тестите с их помощью. Программе на вход подаётся строка, на выход true/false &mdash; подходит ли эта строка под захардкоженную регулярку.

    За саму реализацию того же, что было на паре (и прикреплено в файле) даётся максимум 2 балла. Тесты обязательны, без них решение вообще не будет оцениваться. Дальше своё решение нужно дополнять:

    * (+1 балл) Оптимизируйте алгоритм:

        * `nullable` мы на доске определяли как функцию из регулярки в регулярку. Это удобно в теории, чтобы написать правило для `derivative a (Concat s t)`, но на практике лучше, чтобы `nullable` возвращал true/false, принимает ли данная регулярка пустую строку.

        * Основная проблема в производительности, как было сказано на паре &mdash; разрастание регулярок. Предлагается каждый раз, когда мы строим регулярку, проверять, можем ли мы обойтись более простой регуляркой. Для этого воспользуемся следующими равенствами:

            ```
            ØR = Ø
            RØ = Ø
            εR = R
            Rε = R
            Ø|R = R
            R|Ø = R
            R|ε = R if nullable(R)
            ε|R = R if nullable(R)
            R|Q = R if R = Q
            Ø* = Ø
            ε* = ε
            R** = R*
            ```


        * Если вы придумаете свою оптимизацию, опишете её в отчёте и докажете, что стало работать быстрее, за это возможны допбаллы.

    * (+1 балл) Замерьте на тестах время работы до оптимизаций и после. Сделайте выводы, стало ли лучше. Включите всё это в отчёт.

    * (+1 балл) Найдите несколько входных данных (регулярка и строка) адекватного размера, на котором даже после оптимизаций ваше решение работает больше 2 секунд. Приведите эти данные в отчёте. Исследуйте причины медленной работы (на конкретных тестах), изложите эти причины в отчёте.


## Решения теории

### Задача 1

Язык является нерегулярным. Покажем это, используя лемму о накачке. Давайте рассмотрим такое слово: сперва у нас будет `n` букв
 `a`, потом будет `ab`, а потом будет `n` букв `b`. Хорошо, что же в этом слове такого замечательного. Ну заметим, что 
в таком случае `y` из формулировки отрицания леммы о накачке будет состоять только из `a`-шек. Хорошо, давайте тогда заметим, что при удалении
у нас однозначно определяется место, где заканчивается `u` и начинается `v`. Ну изначально у нас там было поровну символов.
А теперь мы удалили из `u` ненулевое число символов. Получается, что так мы вышли за пределы нашего языка.

### Задача 2

Положим `n = k`. Тогда заметим, что количество `a` у нас будет `n`, количество `c` будет `2n + 1`, а `e` будет `n` штук. 
Заметим, что у нас получается, что если мы опять рассмотрим `y`, т.ч. `|y| <= n`, то там вновь окажутся одни `a`. 
Ну отлично, получается, что если мы их все уберём, то победим. Потому что теперь не выполняется условие на суммарное 
количество `c` и `a, b`. То есть, такое слово не будет лежать в языке.

### Задача 3

Насколько мне известно, пока нет никакого доказательства того, что таких простых, которые описаны в задании, бесконечно много.
Однако и обратного тоже нет. То есть, их может быть и конечное число (хотя и звучит довольно сомнительно).

Хорошо, ну раз у нас нет про это каких-то чётких сведений, то рассмотрим 2 случая. С одной стороны: пусть таких простых конечное число.
Ну в таком случае мы понимаем, что язык точно будет регулярным, просто потому что под заданное условие подойдёт лишь конечное число слов.
Мы уже не раз обсуждали, что конечный язык всегда регулярный.  

С другой стороны, пусть таких чисел бесконечно много. Тогда нам подойдёт регулярка вида `a+`. 

То есть, в обоих случая получается, что язык регулярный.

## Отчёт

Для начала хочется привести времена работы на некоторых тестах движка без оптимизаций и с ними.

Время работы на тесте (с оптимизациями): `0.000184233` секунд

Время работы на тесте (без оптимизаций): `0.852099` секунд

При чём, на строке длины 17 моя первая версия даже не завершалась и операционная система сама аварийно завершала программу.

Теперь же, после всех этих оптимизаций мы можем обработать просто нереально быстро вот такую строку:

```aaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb```

Которая пыталась быть заматченой с такой регуляркой: `(a|b)*`.

Ну, подводя итог, просто невозможно сделать никакого иного вывода кроме как о том, что после оптимизаций всё стало 
работать просто в тысячи раз быстрее. Более того, в данном случае, кажется, что мы получили асимптотически более хорошее 
решение (если не пытаться строить специальные контртесты).

Пока что не очень получается придумать тест, который мог бы завалить моё решение `:(((`. Даже попробовал погонять то, 
что другие ребята придумали, но, к сожалению, (или к счастью) у меня отрабатывает быстрее в несколько тысяч раз `:(((`.

Даже на строке длины 1700 алгоритм отрабатывает молниеносно `0.0177686` сек. При том, что там ещё зашита не самая простая регулярка.

Даже на строке длины 17000... Он всё ещё даже не близок к ТЛю. `0.112739` сек.

Кстати, я зачем-то захардкодил просто огромные строчки в файл с тестами. Очень надеюсь, что на гитхабе это отобразится нормально.