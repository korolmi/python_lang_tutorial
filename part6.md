
== Структуры данных ==

Эта глава описывает подробнее некоторые вещи, которые вы уже изучили, а также раскрывает некоторые новые темы.

=== Подробнее о списках ===

У типа данных список также имеются не описанные ранее методы. Ниже приведены все методы объекта типа список:

* <code>list.append(x)</code>: Добавить элемент к концу списка; эквивалент <code>a[len(a):] = [x]</code>

* <code>list.extend(iterable)</code>: Расширить список за счёт добавления всех элементов переданного iterable; эквивалентно <code>a[len(a):] = iterable</code>.

* <code>list.insert(i, x)</code>: Вставить элемент в указанную позицию. Первый аргумент — это индекс того элемента, перед которым требуется выполнить операцию вставки, поэтому вызов <code>a.insert(0, x)</code> вставляет элемент в начало списка, а <code>a.insert(len(a), x)</code> эквивалентно <code>a.append(x)</code>.

* <code>list.remove(x)</code>: Удалить первый найденный элемент из списка, значение которого — <code>x</code>. Если элемент не найден, генерируется исключение ValueError.

* <code>list.pop([i])</code>: Удалить элемент, находящийся на указанной позиции в списке, и вернуть его. Если индекс не указан, <code>a.pop()</code> удаляет и возвращает последний элемент списка. (Квадратные скобки вокруг <code>i</code> в сигнатуре метода означают, что параметр необязателен, а не необходимость набора квадратных скобок в этой позиции. Вы часто будете встречать такую нотацию в [[Справочник по библиотеке Python 3.1|Справочнике по библиотеке]].)

* <code>list.clear()</code>: Удалить все элементы из списка. Эквивалент <code>del a[:]</code>.

* <code>list.index(x[, start[, end] ])</code>: Вернуть индекс первого найденного в списке элемента, значение которого равно <code>x</code>. Если элемент не найден, генерируется исключение ValueError.

Опцональные аргументы <code>start</code> и <code>end</code> интерпретируются также, как и в нотации срезов (slices) и используются для ограничения поиска в определенном подмножестве списка. Результирующий индексс вычисляется относительно начала полной последовательности, а не аргумента <code>start</code>.

* <code>list.count(x)</code>: Вернуть значение количество вхождений <code>x</code> в список.

* <code>list.sort(*, key=None, reverse=False)</code>:Сортировать элементы списка, на месте (аргументы могут быть использованы для настройки сортировки, см. детали в описании <code>sorted()</code>).

* <code>list.reverse()</code>: Обратить порядок элементов списка, на месте.

* <code>list.copy()</code>: Вернуть shallow копию списка. Эквивалент <code>a[:]</code>.

Пример, использующий большинство методов списка:

<syntaxhighlight lang="python">>>> fruits = ['orange', 'apple', 'pear', 'banana', 'kiwi', 'apple', 'banana']
>>> fruits.count('apple')
2
>>> fruits.count('tangerine')
0
>>> fruits.index('banana')
3
>>> fruits.index('banana', 4)  # Find next banana starting a position 4
6
>>> fruits.reverse()
>>> fruits
['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange']
>>> fruits.append('grape')
>>> fruits
['banana', 'apple', 'kiwi', 'banana', 'pear', 'apple', 'orange', 'grape']
>>> fruits.sort()
>>> fruits
['apple', 'apple', 'banana', 'banana', 'grape', 'kiwi', 'orange', 'pear']
>>> fruits.pop()
'pear'</syntaxhighlight>

Вы могли заметить, что такие методы как <code>insert</code>,<code>remove</code> или <code>sort</code> только изменяют список и не возвращают видимого значения - они возвращают значение None. Это - принцип, применимый для всех изменяемых структур данных в Python.

Есть еще одно наблюдение - не всякие данные могут быть отсортированы/не все данные можно сравнить. Например, список <code>[None,'hello',10]</code> нельзя упорядочить, потому что нельзя сравнить целые со строками, <code>None</code> с другими типами вообще не сравнивается. Также для некоторых типов нет предопределенного отношения порядка. Например, это относится к комплексным числам - <code>3+4j < 5+7j</code> - неправильное сравнение.

==== Использование списка в качестве стека ====

Методы списков позволяют легко использовать список в качестве стека, где последний добавленный элемент становится первым полученным («первый вошёл — последний вышел»). Чтобы положить элемент на вершину стека, используйте метод <code>append()</code>. Для получения элемента с вершины стека — метод <code>pop()</code> без указания явного индекса. Например:

<syntaxhighlight lang="python">>>> stack = [3, 4, 5]
>>> stack.append(6)
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop()
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]</syntaxhighlight>

==== Использование списка в качестве очереди ====

Вы можете без труда использовать список также и в качестве очереди, где первый добавленный элемент оказывается первым полученным («первый вошёл — первый вышел»); стоит заметить, что списки не эффективны для этой цели. Добавление в конец списка и выталкивание последнего элемента списка происходит быстро, вставка в начало или выталкивание из начала список - это медленно (потому что все последующие элементы смещаются на один).

Чтобы добавить элемент в конец очереди, используйте метод <code>append()</code>, а чтобы получить элемент из начала очереди — метод <code>pop()</code> с нулём в качестве индекса. Например:

<syntaxhighlight lang="python">>>> queue = ["Eric", "John", "Michael"]
>>> queue.append("Terry")           # Прибыл Terry
>>> queue.append("Graham")          # Прибыл Graham
>>> queue.pop(0)
'Eric'
>>> queue.pop(0)
'John'
>>> queue
['Michael', 'Terry', 'Graham']</syntaxhighlight>

==== Сборка списков (List Comprehensions) ====

Использование метода списковой сборки — легкий способ создать список. В большинстве случаев он применяется для создания списков, в которых каждый элемент является результатом некой операции, произведённой над каждым членом последовательности или перечисления (iterable), или для создания выборок из тех элементов, которые удовлетворяют определённому условию.

Например, представим, что мы хотим получить список квадратов:

<syntaxhighlight lang="python">>>> squares = []
>>> for x in range(10):
...     squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]</syntaxhighlight>

Обратите внимание, что это создает (или перезаписывает) переменную по имени <code>x</code>, которая будет существовать после завершения цикла. Мы можем вычислить список квадратов без побочных эффектов следующим образом:

<syntaxhighlight lang="python">>>> squares = list(map(lambda x: x**2, range(10)))</syntaxhighlight>

или такой эквивалент:

<syntaxhighlight lang="python">>>> squares = [x**2 for x in range(10)]</syntaxhighlight>

Последний способ короче и читабельнее.

Любая списковая сборка состоит из выражения, за которым следует оператор <code>for</code>, а затем ноль или более операторов <code>for</code> или <code>if</code>. Результатом станет список, получившийся через вычисление выражения в контексте следующих за ним операторов <code>for</code> и/или <code>if</code>. Например, эта сборка объединяет элементы двух списокв в случае, если они не равны:

<syntaxhighlight lang="python">>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]</syntaxhighlight>

это эквивалент следующего:

<syntaxhighlight lang="python">>>> combs = []
>>> for x in [1,2,3]:
...     for y in [3,1,4]:
...         if x != y:
...             combs.append((x, y))
...
>>> combs
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]</syntaxhighlight>

Обратите внимание на то, что порядок операторов <code>for</code> и <code>if</code> одинаков в обоих фрагментах кода.

Если в результате вычисления выражения строится кортеж (как, например, <code>(x, y)</code> в предыдущем пример), его нужно явно обернуть в скобки.

В следующем примере на основе списка чисел создаётся список, где каждое число удвоено:

<syntaxhighlight lang="python">>>> vec = [-4, -2, 0, 2, 4]
>>> [2*x for x in vec]
[-8, -4, 0, 4, 8]</syntaxhighlight>

Отфильтруем список, исключив отрицательные элементы

<syntaxhighlight lang="python">>>> [x for x in vec if x >= 0]
[0, 2, 4]</syntaxhighlight>

Применим функцию ко всем элементам:

<syntaxhighlight lang="python">>>> [abs(x) for x in vec]
[4, 2, 0, 2, 4]</syntaxhighlight>

Здесь мы вызываем метод по очереди с каждым элементом последовательности:

<syntaxhighlight lang="python">>>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
>>> [weapon.strip() for weapon in freshfruit]
['banana', 'loganberry', 'passion fruit']</syntaxhighlight>

Создадим набор пар вида (число, его квадрат):

<syntaxhighlight lang="python">>>> [(x, x**2) for x in range(6)]
[(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]</syntaxhighlight>

Кортежи должны явно заключаться в скобки:

<syntaxhighlight lang="python">>>> [x, x**2 for x in vec]   # ошибка - для кортежей необходимы скобки</syntaxhighlight>

Вытянем вложенный список в плоскую структуру с использованием двух <code>for</code>:

<syntaxhighlight lang="python">>>> vec = [[1,2,3], [4,5,6], [7,8,9]]
>>> [num for elem in vec for num in elem]
[1, 2, 3, 4, 5, 6, 7, 8, 9]</syntaxhighlight>

Списковые сборки могут применяться в сложных выражениях и вложенных функциях:

<syntaxhighlight lang="python">>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']</syntaxhighlight>

==== Вложенные списковые сборки ====

Начальным выражением в списковой сборке может быть произвольное выражение, включающее другую списковую сборку.

Представьте нижеследующий пример матрицы 3x3 в виде списка, содержащего три других списка, по одному в ряд:

<syntaxhighlight lang="python">>>> matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]</syntaxhighlight>

Чтобы поменять строки и столбцы местами, можно использовать такую списковую сборку:

<syntaxhighlight lang="python">>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]</syntaxhighlight>

Как мы видели в предыдущей секции вложенные сборки вычисляются в контексте того оператора <code>for</code>, который следует за ней, поэтому этот пример эквивалентен следующему:

<syntaxhighlight lang="python">>>> transposed = []
>>> for i in range(4):
...     transposed.append([row[i] for row in matrix])
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]</syntaxhighlight>

который, в свою очередь, то же самое, что и:

<syntaxhighlight lang="python">>>> transposed = []
>>> for i in range(4):
...     # the following 3 lines implement the nested listcomp
...     transposed_row = []
...     for row in matrix:
...         transposed_row.append(row[i])
...     transposed.append(transposed_row)
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]</syntaxhighlight>

В реальных случаях лучше использовать встроенные функции вместо сложных выражений. В нашем случае поможет функция <code>zip()</code>:

<syntaxhighlight lang="python">>>> list(zip(*matrix))
[(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]</syntaxhighlight>

В разделе [[Учебник Python 3.1#Распаковка списков параметров|Распаковка списков параметров]] описано предназначение звёздочки в этих строках.

=== Оператор <code>del</code> ===

Существует способ удалить элемент, указывая его индекс, а не его значение: оператор <code>del</code>. В отличие от метода <code>pop()</code>, он не возвращает значения. Оператор <code>del</code> может также использоваться для удаления срезов из списка или полной очистки списка (что мы делали ранее через присваивание пустого списка срезу). Например:

<syntaxhighlight lang="python">>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]</syntaxhighlight>

<code>del</code> может быть также использован для удаления переменных полностью:

<syntaxhighlight lang="python">>>> del a</syntaxhighlight>

Ссылка на имя <code>a</code> в дальнейшем вызовет ошибку (по крайней мере до тех пор, пока с ним не будет связано другое значение). Позже мы с вами узнаем другие способы использования <code>del</code>.

=== Кортежи и последовательности ===

Мы видели, что списки и строки поддерживают много привычных свойств, таких как индексирование и операция получения срезов. Существует два подвида типов данных ''последовательность'' (<tt>sequence</tt>) (см. [[Справочник по библиотеке Python 3.1#Последовательности|Справочник по библиотеке — Последовательности]]), и поскольку Python — развивающийся язык, со временем могут быть добавлены другие последовательные типы данных. Итак, существует также и другой, достойный рассмотрения, стандартный последовательный тип данных: ''кортеж'' (<tt>tuple</tt>).

Кортеж состоит из некоторого числа значений разделённых запятыми, например:

<syntaxhighlight lang="python">>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')
>>> # Кортежи могут быть вложенными:
... u = t, (1, 2, 3, 4, 5)
>>> u
((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))</syntaxhighlight>

Кортежи являются неизменяемыми:

<syntaxhighlight lang="python">>>> t[0] = 88888</syntaxhighlight>

но могут содержать изменяемые объекты:

<syntaxhighlight lang="python">>>> v = ([1, 2, 3], [3, 2, 1])
>>> v
([1, 2, 3], [3, 2, 1])</syntaxhighlight>

Как видите, кортежи на выводе всегда заключены в скобки, таким образом вложенные кортежи интерпретируются корректно; они могут быть введены и с обрамляющими скобками и без, тем не менее в любом случае скобки чаще всего необходимы (если кортеж — часть более крупного выражения). Невозможно присвоить значение отдельным элементам кортежа, при этом можно создавать кортежи, содержащие изменяюемые объекты, такие как списки.

Хотя кортежи кажутся похожими на списки, они часто используются в других ситуациях и для других целей. Кортежи являются неизменяемыми и обечто содержат разнородные последовательности элементов, для доступа к которым используется операция распаковки (будет описана ниже в этой главе) или индексация (или даже доступ по атрибуту в случае именованных кортежей). Списки являются изменяемыми, их элементы обычно однородны и для доступа к ним используется итерирование по списку.

Определённая проблема состоит в конструировании кортежей, состоящих из нуля или одного элемента: в синтаксисе языка есть дополнительные хитрости, позволяющие достигнуть этого. Пустые кортежи формируются за счёт пустой пары скобок; кортеж с одним элементом конструируется за счёт запятой, следующей за первым и единственным значением (его не обязательно заключать в скобки). Необычно, но эффективно. Например:

<syntaxhighlight lang="python">>>> empty = ()
>>> singleton = 'hello',    # <-- обратите внимание на замыкающую запятую
>>> len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)</syntaxhighlight>

Выражение <code>t = 12345, 54321, 'hello!'</code> является примером ''упаковки кортежей'' (<tt>tuple packing</tt>): значения <code>12345</code>, <code>54321</code> и <code>'hello!'</code> упаковываются в кортеж вместе. Обратная операция также возможна:

<code>x, y, z = t</code>

Такое действие называется, довольно удачно, ''распаковкой последовательности'' (<tt>sequence unpacking</tt>). Для распаковки на левой стороне требуется список переменных с количеством элементов равным длине последовательности. Обратите внимание, что множественное присваивание на самом деле является лишь комбинацией упаковки кортежа и распаковки последовательности.

=== Множества ===

Python имеет также тип данных ''множество'' (<tt>set</tt>). Множество — это неупорядоченная коллекция без дублирующихся элементов. Основные способы использования — проверка на вхождение и устранение дублирующихся элементов. Объекты этого типа поддерживают обычные математические операции над множествами, такие как объединение, пересечение, разность и симметрическая разность.

Для создания множеств могут быть использованы фигурные скобки или функция <tt>set()</tt>. ''Заметьте:'' Для создания пустого множества нужно использовать <code>set()</code>, а не <code>{}</code>: в последнем случае создаётся пустой ''словарь'' (<tt>dictionary</tt>) — тип данных, который мы обсудим в следующем разделе.

Продемонстрируем работу с множествами на небольшом примере:

<syntaxhighlight lang="python">>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket) # покажем, что дубликаты были удалены
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # быстрая проверка на вхождение
True
>>> 'crabgrass' in basket
False</syntaxhighlight>
<syntaxhighlight lang="python">>>> # Демонстрация операций со множествами на примере букв из двух слов
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # уникальные буквы в a
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # буквы в a но не в b
{'r', 'd', 'b'}
>>> a | b                              # все буквы, которые встречаются в a или в b
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # буквы, которые есть и в a и в b
{'a', 'c'}
>>> a ^ b                              # буквы в a или в b, но не в обоих
{'r', 'd', 'b', 'm', 'z', 'l'}</syntaxhighlight>

Как и у списков, у множеств существует синтаксис сборок:

<syntaxhighlight lang="python">>>> a = {x for x in 'abracadabra' if x not in 'abc'}
>>> a
{'r', 'd'}</syntaxhighlight>

=== Словари ===

Другой полезный встроенный в Python тип данных — это ''словарь'' (<tt>dictionary</tt>) (см. [[Справочник по библиотеке Python 3.1#Связывающие типы|Справочник по библиотеке — Связывающие типы]]). Словари иногда встречаются в других языках в виде «ассоциативных записей» или «ассоциативных массивов». В отличие от последовательностей, которые индексируются по диапазону чисел, словари индексируются по ''ключам'' (<tt>keys</tt>), которые, в свою очередь, могут быть любого неизменяемого типа; строки и числа всегда могут быть ключами. Кортежи могут быть ключами только если они составлены из строк, чисел или кортежей; если кортеж содержит какой-либо изменяемый объект, явно или неявно, то он не может быть использован в качестве ключа. Вы не можете использовать списки в роли ключей, поскольку списки могут быть изменены на месте присваиванием по индексу, присваиванием по срезу или такими методами как <code>append()</code> и <code>extend()</code>.

Лучше всего воспринимать словарь как неупорядоченный набор пар ''ключ: значение'' с требованием, чтобы ключи были уникальны (в пределах одного словаря). Пара скобок создает пустой словарь: <code>{}</code>. Указывая разделённый запятыми список пар ''ключ: значение'' внутри скобок, вы задаёте содержимое словаря; в таком же формате производится вывод словарей.

Главные операции над словарём — это сохранение значения с каким-либо ключом и извлечение значения по указанному ключу. Также возможно удалить пару ''ключ: значение'' используя оператор <code>del</code>. Если вы сохраняете значение используя ключ, который уже встречается в словаре — старое значение, ассоциированное с этим ключом, стирается. Извлечение значения по несуществующему ключу вызывает ошибку.

Выполнение конструкции <code>list(d)</code> с объектом словаря возвращает список всех ключей, использующихся в словаре, в порядки их вставки (если вы хотите отсортировать его, к списку ключей можно применить функцию <code>sorted()</code>). Чтобы проверить, содержит ли словарь определённый ключ, используйте ключевое слово <code>in</code>.

Вот небольшой пример использования словарей:

<syntaxhighlight lang="python">>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'sape': 4139, 'jack': 4098, 'guido': 4127}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['irv'] = 4127
>>> tel
{'jack': 4098, 'guido': 4127, 'irv': 4127}
>>> list(tel)
['jack', 'guido', 'irv']
>>> sorted(tel)
['guido', 'irv', 'jack']
>>> 'guido' in tel
True
>>> 'jack' not in tel
False</syntaxhighlight>

Конструктор <code>dict()</code> строит словарь непосредственно на основе пар ключей и значений:

<syntaxhighlight lang="python">>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'guido': 4127, 'jack': 4098}</syntaxhighlight>

В дополнение ко всему этому, для создания словарей из произвольных выражений для ключей и значений, могут быть использованы словарные сборки:

<syntaxhighlight lang="python">>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}</syntaxhighlight>

Если ключи являются простыми строками, иногда легче описать пары используя именованные параметры:

<syntaxhighlight lang="python">>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'guido': 4127, 'jack': 4098}</syntaxhighlight>

=== Организация циклов ===

При организации перебора элементов из словаря ключ и соответствующее ему значение могут быть получены одновременно посредством метода <code>items()</code>.

<syntaxhighlight lang="python">>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():
...     print(k, v)
...
gallahad the pure
robin the brave</syntaxhighlight>

С помощью функции <code>enumerate()</code> можно получить одновременно индекс и значение элемента перебираемой в цикле последовательности:

<syntaxhighlight lang="python">>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print(i, v)
...
0 tic
1 tac
2 toe</syntaxhighlight>

Для того, чтобы организовать цикл параллельно для двух или более последовательностей, элементы можно предварительно сгруппировать функцией <code>zip()</code>.

<syntaxhighlight lang="python">>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(q, a))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.</syntaxhighlight>

Изменить порядок следования последовательности на обратный поможет функция <code>reversed()</code>.

<syntaxhighlight lang="python">>>> for i in reversed(range(1, 10, 2)):
...     print(i)
...
9
7
5
3
1</syntaxhighlight>

Для организации цикла в порядке сортировки можно применить функцию <code>sorted()</code>, которая возвращает отсортированный список, оставляя исходный без изменений.

<syntaxhighlight lang="python">>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for i in sorted(basket):
...     print(i)
...
apple
apple
banana
orange
orange
pear</syntaxhighlight>

Применение <code>set()</code> к последовательности избавляет от дублирующих элементов. Использование <code>sordet()</code> в сочетании с <code>set()</code> является идиоматическим способом прохождения по уникальным элементам последовательности в отсортированном порядке.

<syntaxhighlight lang="python">>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for f in sorted(set(basket)):
...     print(f)
...
apple
banana
orange
pear</syntaxhighlight>

Иногда есть соблазн изменить список в процессе прохождения по нему; часто проще и безопаснее создать вместо этого новый список.

<syntaxhighlight lang="python">>>> import math
>>> raw_data = [56.2, float('NaN'), 51.7, 55.3, 52.5, float('NaN'), 47.8]
>>> filtered_data = []
>>> for value in raw_data:
...     if not math.isnan(value):
...         filtered_data.append(value)
...
>>> filtered_data
[56.2, 51.7, 55.3, 52.5, 47.8]</syntaxhighlight> 
=== Подробнее об условиях ===

Условия в операторах <code>if</code> и <code>while</code> могут содержать любые операции, а не только операции сравнения.

Операции сравнения <code>in</code> и <code>not in</code> проверяют, встречается значение в последовательности или нет. Операции <code>is</code> и <code>is not</code> проверяют, не являются ли два объекта на самом деле одним и тем же (это имеет смысл лишь для изменяемых объектов, таких как списки). Все операции сравнения имеют один и тот же приоритет, меньший чем у любых операций над числами.

Сравнения можно объединять в цепочки. Например, <code>a < b == c</code> проверяет, меньше ли <code>a</code> чем <code>b</code> и, сверх того, равны ли <code>b</code> и <code>c</code>.

Сравнения могут быть скомбинированы с использованием булевых операций <code>and</code> и <code>or</code>, а результат сравнения (или любого другого булева выражения) можно отрицать используя <code>not</code>. Эти операции имеют меньший приоритет, чем у операций сравнения; среди них у <code>not</code> высший приоритет, а у <code>or</code> — низший, поэтому <code>A and not B or C</code> эквивалентно <code>(A and (not B)) or C</code>. Как всегда, явно заданные скобки помогут выразить желаемый порядок выполнения операций.

Булевы операции <code>and</code> и <code>or</code> — это так называемые ''коротящие операции'' (<tt>short-circuit operators</tt>): их операнды вычисляются слева направо и вычисление заканчивается как только результат становится определён (очевиден). Например, если <code>A</code> и <code>C</code> истинны, а <code>B</code> — ложно, в условии <code>A and B and C</code> выражение <code>C</code> не вычисляется. Когда ''коротящая операция'' используется не в контексте логической операции, она возвращает последний элемент, который был вычислен.

Можно присвоить результат сравнения, или другого булева выражения, переменной. Например,

<syntaxhighlight lang="python">>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 or string2 or string3
>>> non_null
'Trondheim'</syntaxhighlight>

Заметьте, что в Python (в отличие от C) для присваивания внутри выражений необходимо использовать оператор <code>:=</code>. Это позволяет избежать ряда проблем, обычного для программ на C: указания оператора присваивания (<code>=</code>) в выражении, вместо предполагавшегося сравнения (<code>==</code>).

=== Сравнение последовательностей и других типов ===

Объекты последовательностей можно сравнивать с другими объектами с тем же типом последовательности. Сравнение использует лексикографический порядок: сравниваются первые два элемента, и если они различны — результат сравнения определён; если они равны, сравниваются следующие два элемента и так далее до тех пор, пока одна из последовательностей не будет исчерпана. Если сравниваемые два элемента сами являются последовательностями одного типа, лексикографическое сравнение происходит в них рекурсивно. Если все элементы обеих последовательностей оказались равны, последовательности считаются равными. Если одна последовательность оказывается стартовой последовательностью другой, более короткая последовательность считается меньшей. Лексикографическое упорядочивание строк использует порядок в таблице Unicode для индивидуальных символов. Несколько примеров сравнений между однотипными последовательностями:

<code>
(1, 2, 3)              < (1, 2, 4)
[1, 2, 3]              < [1, 2, 4]
'ABC' < 'C' < 'Pascal' < 'Python'
(1, 2, 3, 4)           < (1, 2, 4)
(1, 2)                 < (1, 2, -1)
(1, 2, 3)             == (1.0, 2.0, 3.0)
(1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)
</code>

Обратите внимание, что сравнение объектов различных типов операциями <code><</code> или <code>></code> позволено, если объекты имеют соответствующие методы сравнения. Например, смешанные числовые типы сравниваются в соответствии с их численными значениями, так что <code>0</code> равен <code>0.0</code> и т. д. В противном случае интерпретатор вместо произвольного порядка возбудит исключение <code>TypeError</code>.

