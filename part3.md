
== Использование интерпретатора Python ==

=== Запуск интерпретатора ===

Интерпретатор Python после установки располагается, обычно, по пути <tt>/usr/local/bin/python3.1</tt> — на тех компьютерах, где этот путь доступен. Добавление каталога <tt>/usr/local/bin</tt> к пути поиска Unix-шелла (переменная <tt>PATH</tt>) позволит запустить интерпретатор набором команды

<code>python3.1</code>

прямо из шелла<ref>В случае операционных систем UNIX, интерпретатор версии 3.1 по умолчанию устанавливается с исполняемым файлом, имеющим имя, отличное от <tt>python</tt> — дабы не иметь конфликтов с (возможно) установленным исполняемым файлом версии 2.6</ref>. Поскольку выбор каталога, в котором будет обитать интерпретатор, осуществляется при его установке, то возможны и другие варианты — посоветуйтесь с вашим Python-гуру или системным администратором. (Например, путь <tt>/usr/local/python</tt> тоже популярен в качестве альтернативного расположения.)

На машинах с ОС Windows, инсталляция Python обычно осуществляется в каталог <tt>C:\Python31</tt>, но и он может быть изменён во время установки. Чтобы добавить этот каталог к вашему пути поиска, вы можете набрать в окне DOS следующую команду, в ответ на приглашение:

<code>set path=%path%;C:\python31</code>

При наборе символа конца файла (<tt>Ctrl-D</tt> в Unix, <tt>Ctrl-Z</tt> в Windows) в ответ на основное приглашение интерпретатора, последний будет вынужден закончить работу с нулевым статусом выхода. Если это не сработает — вы можете выйти из интерпретатора путём ввода следующих команд:

<syntaxhighlight lang="python">import sys; sys.exit()</syntaxhighlight>

Особенности редактирования строк в интерпретаторе не оказываются обычно чересчур сложными. Те, кто установил интерпретатор на машину Unix, потенциально имеют поддержку библиотеки <tt>GNU readline</tt>, обеспечивающей усовершенствованное интерактивное редактирование и сохранение истории. Самый быстрый, наверное, способ проверить, поддерживается ли расширенное редактирование командной строки, заключается в нажатии <tt>Ctrl-P</tt> в ответ на первое полученное приглашение Python. Если вы услышите сигнал — значит вам доступно редактирование командной строки — тогда обратитесь к [[Учебник Python 3.1 — Приложения#Интерактивное редактирование входных данных и подстановка истории|Приложению об Интерактивном редактировании входных данных]] за описанием клавиш. Если на ваш взгляд ничего не произошло или отобразился символ <tt>^P</tt> — редактирование командной строки недоступно — удалять символы из текущей строки возможно будет лишь использованием клавиши <tt>Backspace</tt>.

Интерпретатор ведёт себя сходно шеллу Unix: если он вызван, когда ''стандартный ввод'' привязан к устройству <tt>tty</tt> — он считывает и выполняет команды в режиме диалога; будучи вызванным с именем файла в качестве параметра или с файлом, назначенным на ''стандартный ввод'' — он читает и выполняет сценарий из этого файла.

Другой способ запустить интерпретатор — директива <code>'''python -c''' ''команда'' <tt>[arg]</tt> ...</code> — при её использовании поочередно выполняются инструкции(-ция) из ''команды'' (как при использовании опции '''-c''' Unix-шелла). В связи с тем, что инструкции Python часто содержат пробелы или другие специальные для шелла символы, рекомендуется заключать ''команды'' полностью в одинарные кавычки.

Некоторые модули Python оказываются полезными при использовании их в качестве сценариев. Они могут быть запущены в этом виде командой <code>'''python -m''' ''модуль'' <tt>[arg]</tt> ...</code> — таким образом исполняется исходный файл модуля (как произошло бы, если бы вы ввели его полное имя в командной строке).

Обратите внимание на различие между командами <tt>python file</tt> и <tt>python <file</tt>. В последнем случае запросы на ввод из программы, такие как вызов <code>sys.stdin.read()</code>, удовлетворяются из самого файла. Поскольку файл уже был прочитан до конца ещё до начала выполнения программы — символ конца файла будет обнаружен программой незамедлительно. В большинстве случаев (это, чаще всего, и есть те ситуации, которых вы ожидали) вызовы удовлетворяются независимо от того, файл или устройство привязаны к стандартному вводу интерпретатора Python.

При использовании файла сценария иногда полезно иметь возможность запустить сценарий и затем войти в интерактивный режим. Это может быть сделано через указание параметра '''-i''' перед именем сценария. (Этот способ не сработает, если сценарий считывается со стандартного ввода — по той самой причине, которая описана в предыдущем абзаце).

==== Передача параметров ====

В случае, если интерпретатору известны имя сценария и дополнительные параметры, с которыми он вызван, все они передаются сценарию в переменной <code>sys.argv</code>, представляющей собой список (<code>list</code>) строк. Длина (<code>length</code>) списка — минимум, единица; если не переданы ни имя сценария, ни аргументы — то <code>sys.argv[0]</code> содержит пустую строку. Когда в качестве имени сценария передан <code>'-'</code> (означает ''стандартный ввод''), <code>sys.argv[0]</code> устанавливается в <code>'-'</code>. Если используется директива '''-c''' ''команда'' — <code>sys.argv[0]</code> содержит <code>'-c'</code>. В случае, если используется директива '''-m''' ''модуль'' — то <code>sys.argv[0]</code> устанавливается равным полному имени модуля по расположению. Опции, обнаруженные после сочетаний '''-c''' ''команда'' или '''-m''' ''модуль'' не обрабатываются интерпретатором Python, но остаются в переменной <code>sys.argv</code>, дабы обеспечить возможность отслеживания в самой команде или в модуле.

==== Интерактивный режим ====

Если команды считываются с <tt>tty</tt> — говорят, что интерпретатор находится в ''интерактивном режиме'' (режиме диалога). В этом режиме он приглашает к вводу следующей команды, отобразив ''основное приглашение''<ref>''(Прим. перев.)'' <tt>primary prompt</tt> — ''зд.'', основное приглашение: приглашение к вводу команды или нескольких команд (сценария);</ref> (обычно это три знака «больше-чем» — <tt>&gt;&gt;&gt;</tt>); в то же время, для ''продолжающих строк''<ref>''(Прим. перев.)'' <tt>continuation lines</tt> — ''зд.'', продолжающие строки (строки продолжения): строки, продолжающие текст команды (оператора, выражения), или раскрывающие внутреннее устройство внешнего оператора (команды, выражения), в последнем случае определяются дополнительным отступом от края — углублением в структуру;</ref> выводится ''вспомогательное приглашение''<ref>''(Прим. перев.)'' <tt>secondary prompt</tt> — ''зд.'', вспомогательное приглашение: приглашение к продолжению ввода команды или набора команд (сценария) с использованием ''продолжающих строк'';</ref> (по умолчанию — три точки — <tt>...</tt>). Перед выводом первого приглашения интерпретатор отображает приветственное сообщение, содержащее номер его версии и пометку о правах копирования:

<syntaxhighlight lang="python">$ python3.1
Python 3.1a1 (py3k, Sep 12 2007, 12:21:02)
[GCC 3.4.6 20060404 (Red Hat 3.4.6-8)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>></syntaxhighlight>

''Продолжающие строки'' используются в случаях, когда необходимо ввести многострочную конструкцию. Взгляните, например, на следующий оператор <code>if</code><ref>''(Прим. перев.)'' Здесь и далее тексты некоторых исходных кодов также будут переведены в примечаниях (вместе с ключевыми словами, поэтому их нельзя будет запустить в интерпретаторе), чтобы показать их смысл или задумку, которые не всегда очевидны без перевода (если он вам нужен :)):<br /><br />
<code style="background-color: inherit !important;"><span style="color: #aaa">>>></span> мир_плоский = 1<br/>
<span style="color: #aaa">>>></span> '''если''' мир_плоский:<br/>
<span style="color: #aaa">...</span>&nbsp;&nbsp;&nbsp;&nbsp;'''вывести'''("Будьте осторожны — не упадите!")<br/>
<span style="color: #aaa">...</span> <br/>
<span style="color: #030">Будьте осторожны — не упадите!</span></code><br /></ref>:

<syntaxhighlight lang="python">>>> the_world_is_flat = 1
>>> if the_world_is_flat:
...     print("Be careful not to fall off!")
...
Be careful not to fall off!</syntaxhighlight>

=== Интерпретатор и его окружение ===

==== Обработка ошибок ====

В случае появления ошибки интерпретатор выводит сообщение об ошибке, завершая его стеком вызовов. В интерактивном режиме он снова возвращается в состояние приглашения для ввода команд; если ввод происходит из файла — интерпретатор выходит с ненулевым статусом, сразу после распечатки стека вызовов. (Исключения, обрабатываемые в блоке <code>except</code> оператора <code>try</code> в этом контексте не считаются ошибками.) Некоторые ошибки исключительно фатальны и вызывают собой принудительное завершение работы с ненулевым статусом — это применимо к внутренним противоречиям языка и к некоторым случаям нехватки памяти. Все сообщения об ошибках выводятся в ''стандартный поток ошибок'' (<tt>error stream</tt>). Обычный вывод исполняемых команд направляется в ''стандартный вывод''.

Нажатие клавиш прерывания процесса (обычно <tt>Ctrl-C</tt> или <tt>DEL</tt>), в ответ на приглашение в основном или вспомогательном режиме, отменяет ввод и возвращает вас к основному приглашению.<ref>Известная проблема в пакете <tt>GNU Readline</tt> может этому помешать.</ref> Символ прерывания, набранный во время выполнения какой-либо команды порождает исключение <code>KeyboardInterrupt</code>, которое, в свою очередь, может быть перехвачено оператором <code>try</code>.

==== Исполняемые сценарии на Python ====

На Unix-системах семейства BSD сценарии на Python могут быть сделаны исполняемыми, также как и шелл-сценарии, путём добавления следующей строки

<code>#! /usr/bin/env python3.1</code>

(предполагается, что интерпретатор может быть найден по одному из путей, указанных в пользовательской переменной <tt>PATH</tt>) в начало сценария и установки файла в исполняемый режим. Символы <tt>#!</tt> должны быть первыми символами в файле. На некоторых платформах строка должна оканчиваться символом конца строки в стиле Unix (<tt>'\n'</tt>), а не в стиле Windows (<tt>'\r\n'</tt>). Заметьте, что символ решётки <tt>'#'</tt> используется в Python для указания начала комментария.

Исполняемый режим (или разрешение на исполнение) может быть установлен сценарию использованием команды '''chmod''':

<code>$ chmod +x myscript.py</code>

У систем с операционной системой Windows нет такого понятия, как исполняемый режим. Установщик Python автоматически связывает файлы <tt>.py</tt> с файлом <tt>python.exe</tt>, таким образом двойной клик на файле Python запустит его в виде сценария. Расширение может быть и <tt>.pyw</tt> в случае, если окно консоли (которое, обычно, отображается) при запуске сценария подавляется.

==== Кодировка исходных файлов ====

По умолчанию, исходники Python считаются созданными в кодировке <tt>UTF-8</tt>. В этой кодировке в строковых литералах, идентификаторах и комментариях могут быть использованы символы большинства языков мира — учитывая то, что стандартная библиотека Python использует символы <tt>ASCII</tt> для именования идентификаторов — и этому соглашению должен следовать любой переносимый код. Для корректного отображения всех этих символов, ваш редактор должен опознавать файл как закодированный в <tt>UTF-8</tt> и должен использовать шрифт, который содержит все символы, используемые в файле.

Также, есть возможность указать другую кодировку для исходных файлов. Для этого нужно добавить специальный комментарий следом за строкой <tt>#!</tt>, дабы описать кодировку исходного файла:

<code># -*- coding: encoding -*-</code>

Если используется это описание — всё, что находится в этом файле будет опознаваться как имеющее соответствующую кодировку ''<tt>encoding</tt>'', а не <tt>UTF-8</tt>. Список возможных кодировок представлен в [[Справочник по библиотеке Python 3.1|Справочнике по библиотеке]] — в разделе, описывающем модуль [[Справочник по библиотеке Python 3.1#codecs|codecs]].

Например, если выбранный вами редактор не поддерживает файлы, закодированные <tt>UTF-8</tt> и требует применения какой-либо другой кодировки, допустим <tt>Windows-1252</tt>, вы можете написать:

<code># -*- coding: cp-1252 -*-</code>

И, с этого момента, использовать в исходных файлах только символы из таблицы символов <tt>Windows-1252</tt>. Устанавливающий (отличную от установленной по умолчанию) кодировку специальный комментарий должен являться первой или второй строкой файла.

==== Интерактивный файл запуска ====

Если вы используете Python интерактивно — часто бывает удобным выполнить некоторые стандартные команды перед запуском интерпретатора. Вы можете сделать это, установив переменную окружения с именем <tt>PYTHONSTARTUP</tt> равной имени файла, содержащего ваши команды запуска. Способ схож с использованием файла <tt>.profile</tt> в Unix-шелле.

Этот файл читается только в интерактивных сессиях, но не в случае считывания команд из сценария, и не в случае, если <tt>/dev/tty</tt> назначен как независимый источник команд (который в других случаях ведет себя сходно интерактивным сессиям). Файл исполняется в том же пространстве имён, что и исполняемые команды — поэтому объекты и импортированные модули, которые он определяет могут свободно использоваться в интерактивной сессии. Также в этом файле вы можете изменить приглашения: <code>sys.ps1</code> и <code>sys.ps2</code>.

Если вы хотите прочитать дополнительный файл запуска из текущего каталога — вы можете использовать код вроде:

<syntaxhighlight lang="python">if os.path.isfile('.pythonrc.py'): exec(open('.pythonrc.py').read())</syntaxhighlight>

Если вы хотите использовать файл запуска в сценарии — вам нужно будет указать это явно:

<syntaxhighlight lang="python">import os
filename = os.environ.get('PYTHONSTARTUP')
if filename and os.path.isfile(filename):
    exec(open(filename).read())</syntaxhighlight>

