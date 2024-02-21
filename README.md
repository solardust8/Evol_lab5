﻿# <a name="xb74fee5d1158fd9a3f3a54e8571f3bb0214b050"></a>**Лабораторная работа №5.**
# **Проектирование эволюционного алгоритма для задачи расстановки ферзей.**

<a name="цель-работы"></a>**Выполнил Домницкий Е.А. М4130**

**Цель работы**

Освоение всего цикла разработки эволюционных алгоритмов, начиная с анализа проблемы и проектирования, заканчивая настройкой параметров и анализом эффективности.

<a name="оборудование-и-программное-обеспечение"></a>**Оборудование и программное обеспечение**

- Использована Java JDK 1.8
- [Watchmaker framework версии 0.7.1](https://github.com/dwdyer/watchmaker) (требует JDK 1.8 и выше)
## <a name="задача"></a>**Задача**
Разработать генетический алгоритм, решающий задачу расстановки N ферзей на доске NxN таким образом, чтобы они не били друг друга (не могли побить по горзизонтали, вертикали и по диагоналям).
## <a name="представление-решений-queenssolution"></a>**Представление решений** 
Для реализации решения был написан **QueensSolution**, он имеет поле **rowIndexes** - целочисленный массив, хранящий гены. 

В задаче сказано, что ферзи (королевы) не должны бить друг друга по горизонтали, вертикали и по диагоналям – это накладывает ограничение на область решений, которые можно считать валидными.

Отчасти эти ограничения учитываются в представлении генов. Гены представлены в виде одномерного массива длиной N, содержащего перестановку чисел от 0 до N-1. Элемент i данного массива содержит номер строки королевы, стоящей на i-ом столбце. Такое представление гарантирует, что в любом решении ни один ферзь не будет стоять с другим на одной строке или столбце.

**QueensSolution** имеет вспомогательные методы для реализации генетических операторов. Генетические опреаторы не взаимодействуют с полем **rowIndexes** напрямую: только с помощью этих методов. Здесь все аналогично лабораторной работе 4.
## <a name="фитнес-функция-queensfitnessfunction"></a>**Фитнес-функция** 
Фитнес функция подсчитывает число ферзей, находящихся под ударом. Благодаря представлению решений нам достаточно проверять, находятся ли ферзи на одной диагонали<a name="терминация"></a>.
## <a name="x90700534f0189a53fccb436b0a0651cf2694d00"></a>**Мутации** 
Реализованы 4 мутации: вставкой, перемешиванием, инверсией и перестановкой. Все эти мутации имеют один параметр (передается в конструктор) - вероятность для каждого элемента популяции, что к нему будет применена данная мутация.
## <a name="кроссовер-queenscrossover"></a>**Кроссовер** 
Реализует упорядоченный кроссовер.
## <a name="результаты-экспериментов"></a>**Выполнение программы с разными параметрами**
В таблице представлены результаты работы алгоритма при разных N. Результаты усреднены по 10 запускам.

Все мутации было решено сделать равновероятными (с вероятностью 0.5). Вероятность кроссовера составляет 1. Данные параметры были выбраны с целью имитации природы, потому что в реальности характер и количество мутаций непредсказуемы, а гены всегда «смешиваются». В паре экспериментов менялся размер популяции.

|N|Размер популяции|Итераций до решения|Минимум итераций до решения|Предыдущий минимум, число итераций до схода|
| :- | :- | :- | :- | :- |
|4|10|1|1|-|
|8|100|7\.7|2|2, 1.3|
|16|2000|510\.9|7|2, 29.3|
|16|20000|67\.3|4|1, 1.6|
|16|200000|7\.2|2|2, 10|
|16|1000000|4\.5|2|2, 2.4|
|32|2000000|695|-|1, 317|
## <a name="ответы-на-вопросы"></a>**Ответы на вопросы**
1. Является ли задача оптимизационной или ограниченной?

Данную задачу можно назвать оптимизационной, потому что мы ищем решение, соответствующее оптимальному значению фитнес-функции. Оптимум фитнес-функции известен заранее. Функцию качества можем вычислить всегда, а за счет представления решений было сужено пространство поиска.

2. Как растет сложность задачи при увеличении размерности?

С увеличением размерности задачи возрастает ее сложность. Это связано с тем, что количество возможных решений при данной размерности равно факториалу числа N. Последовательность чисел, представляющих количество возможных расстановок ферзей, растет намного медленнее, чем последовательность факториалов. Поэтому не только пространство поиска увеличивается факториально, но и вероятность случайного выбора правильного решения снижается факториально.

