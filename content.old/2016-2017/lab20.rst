Графы Фрутландии
################

:date: 2017-03-01
:test_link: 
:lecture_link: https://youtu.be/h70kNhG3p5g
:lecture_pdf: true

.. :test_comment: Контрольная по графам №3

.. :lecture_link: 

.. default-role:: code
.. contents:: Содержание

Введение
========

Данная работа посвящена изучению возможностей библиотеки NetworkX для визуального представления графов.

Библиотека NetworkX
===================

Подключение библиотеки:

.. code-block:: python

	import networkx as nx

NetworkX предназначена для изучения структуры, динамики и функционирования сложных сетей.
Она позволяет создавать и хранить графы в стандартных и нестандартных форматах, генерировать много 
типов случайных и классических графов, анализировать их структуру, строить сетевые модели и создавать
новые алгоритмы.

Библиотека NetworkX относится к *свободному программному обеспечению*, поэтому она может быть распространена и 
изменена согласно лицензии BSD.

Обзор библиотеки NetworkX на русском языке: `https://habrahabr.ru/post/125898/`_

.. _`https://habrahabr.ru/post/125898/`: https://habrahabr.ru/post/125898/

При использовании библиотеки обращайтесь к её актуальной онлайн-документации_. Там же можно найти учебник_ и примеры_!

.. _онлайн-документации: https://networkx.github.io/documentation/latest/
.. _учебник: http://networkx.readthedocs.io/en/stable/tutorial/
.. _примеры: http://networkx.readthedocs.io/en/stable/examples/

Классы графов
-------------
NetworkX содержит четыре класса графов:

* Graph — граф без кратных рёбер (петли допустимы)
* DiGraph — ориентированный граф без кратных рёбер (петли допустимы)
* MultiGraph — граф с кратными рёбрами (в том числе с кратными петлями)
* MultiDiGraph — ориентированный граф с кратными рёбрами (в том числе с кратными петлями)

Внутреннее представление графов реализовано в виде списков смежности (словарь словарей словарей).
Однако во избежании появления несогласованности, все операции с графами должны производится
с использованием API функций библиотеки.

Вершины и рёбра
---------------

Вершиной может быть любой неизменяемый тип с вычислимой функцией `hash()`.
Например, прекрасно подойдут:

* str
* int
* float
* кортеж из строк и чисел
* frozenset (неизменяемое множество)

Каждой вершине можно добавлять и удалять свойства произвольного типа (см. раздел Attributes_).

Рёбра представляют собой связь двух вершин и чаще вершины имеют привязанные к ним данные — свойства рёбер.
Для указания веса ребра, используйте свойство **weight**.

.. _Attributes: http://networkx.readthedocs.io/en/stable/reference/classes.graph.html

Создание графа
--------------

Графы могут быть созданы тремя основными способами:

* явное добавление узлов и рёбер

.. code-block:: python

	G = nx.Graph()                                    # создаём экземпляр графа
	G.add_edge(1, 2)                                  # ребро добавляется сразу со своими вершинами
	G.add_edge(2, 3)                                  # стандартный вес ребра weight=1
	G.add_edge(3, 4, weight = 0.9)                    # можно задать weight сразу при создании ребра
	G.add_node(5)                                     # изолированный узел можно добавить отдельно
	G.add_node(6, x = 1.5, y = -5.0, data = ['any'])  # и сразу задать ему любые свойства

* генераторами графов — алгоритмами порождения стандартных сетевых топологий

.. code-block:: python

	G = nx.complete_graph(10)    # полносвязный граф с 10 вершинами
	G = nx.path_graph(10)        # 10 узлов, расположенных "в линеечку"
	G = nx.cycle_graph(10)       # 10 узлов, связанных кольцом
	G = nx.star_graph(5)         # звезда с 1 узлом в середине и 5 узлами-лучами
	G = nx.balanced_tree(2, 3)   # сбалансированное двоичное дерево высоты 3
	G = nx.empty_graph(10)       # граф с 10 вершинами без рёбер

* импорт данных графа из некоторого формата (обычно из файла)

.. code-block:: python

	d = {0: {1: {'weight': 10}, 2: {'weight': 20}},
	     1: {0: {'weight': 10}, 3: {'weight': 30}},
	     2: {0: {'weight': 20}},
	     3: {1: {'weight': 30}}}
	G = nx.Graph(d)
	dd = nx.to_dict_of_dicts(G) # d == dd

Визуализация графа
------------------

Визуализация графов — нетривиальная задача! Существует много полноценных библиотек,
предназначенных именно для этого:  Cytoscape, Gephi, Graphviz или PGF/TikZ для LaTeX.
Для их использования можно экспортировать граф из NetworkX в формат GraphML.

Однако, есть и самый простой способ визуализации, встроенный в саму библиотеку NetworkX,
при подключении библиотеки `matplotlib.pyplot`.

.. code-block:: python

	nx.draw(G)           # отобразить граф при помощи Matplotlib
	nx.draw_circular(G)  # Использовать расположение circular layout
	nx.draw_random(G)    # Использовать расположение random layout
	nx.draw_spectral(G)  # Использовать расположение spectral layout
	nx.draw_spring(G)    # Использовать расположение spring layout
	nx.draw_shell(G)     # Использовать расположение shell layout
	nx.draw_graphviz(G)  # Использовать graphviz для расположения вершин


Пример визуализации графа №1
++++++++++++++++++++++++++++

.. code-block:: python

	import matplotlib.pyplot as plt
	import networkx as nx

	G=nx.path_graph(8)
	nx.draw(G)
	plt.savefig("simple_path.png") # сохранить как png файл
	plt.show() # вывести на экран

Пример визуализации графа №2
++++++++++++++++++++++++++++

Пример добавления этикеток на вершины и подкрашивания рёбер:

.. code-block:: python

	"""
	Отрисовка графа через matplotlib, с разными цветами.

	"""
	__author__ = """Aric Hagberg (hagberg@lanl.gov)"""

	import matplotlib.pyplot as plt
	import networkx as nx

	G=nx.cubical_graph()
	pos=nx.spring_layout(G) # позиции всех вершин

	# вершины
	nx.draw_networkx_nodes(G, pos,
		               nodelist=[0,1,2,3], # список вершин
		               node_color='r',     # красный цвет
		               node_size=500,      # размер
		           alpha=0.8)              # прозрачность
	nx.draw_networkx_nodes(G, pos,
		               nodelist=[4,5,6,7],
		               node_color='b',
		               node_size=500,
		           alpha=0.8)

	# рёбра
	nx.draw_networkx_edges(G, pos, width=1.0, alpha=0.5) # все рёбра
	nx.draw_networkx_edges(G, pos,
		               edgelist=[(0,1),(1,2),(2,3),(3,0)],
		               width=8, alpha=0.5, edge_color='r')   # красные рёбра
	nx.draw_networkx_edges(G, pos,
		               edgelist=[(4,5),(5,6),(6,7),(7,4)],
		               width=8, alpha=0.5, edge_color='b')   # синие рёбра

	# добавим математические названия вершин
	labels={}
	labels[0]=r'$a$'
	labels[1]=r'$b$'
	labels[2]=r'$c$'
	labels[3]=r'$d$'
	labels[4]=r'$\alpha$'
	labels[5]=r'$\beta$'
	labels[6]=r'$\gamma$'
	labels[7]=r'$\delta$'
	nx.draw_networkx_labels(G, pos, labels, font_size=16)

	plt.axis('off')
	plt.savefig("labels_and_colors.png") # сохранить как png картинку
	plt.show() # вывести на экран

Пример визуализации графа №3
++++++++++++++++++++++++++++

Ещё один пример добавления этикеток на вершины и подкрашивания рёбер:

.. code-block:: python

	"""
	Пример использования Graph как взешенного.
	"""
	__author__ = """Aric Hagberg (hagberg@lanl.gov)"""
	
    import matplotlib.pyplot as plt
	import networkx as nx

	G = nx.Graph()
	
	#   добавляем рёбра и вершины

	G.add_edge('a', 'b', weight=0.6)
	G.add_edge('a', 'c', weight=0.2)
	G.add_edge('c', 'd', weight=0.1)
	G.add_edge('c', 'e', weight=0.7)
	G.add_edge('c', 'f', weight=0.9)
	G.add_edge('a', 'd', weight=0.3)

	elarge = [(u,v) for (u,v,d) in G.edges(data=True) if d['weight'] >0.5]  # "тяжёлые"
	esmall = [(u,v) for (u,v,d) in G.edges(data=True) if d['weight'] <=0.5] # "лёгкие"

	pos = nx.spring_layout(G) # позиции всех вершин

	# вершины
	nx.draw_networkx_nodes(G, pos, node_size=700)

	# рёбра
	nx.draw_networkx_edges(G, pos, edgelist=elarge,
	                width=6)                                   # "тяжёлые"
	nx.draw_networkx_edges(G, pos, edgelist=esmall,
	       width=6, alpha=0.5, edge_color='b', style='dashed') # "лёгкие"

	# метки
	nx.draw_networkx_labels(G,pos,font_size=20,font_family='sans-serif')

	plt.axis('off')
	plt.savefig("weighted_graph.png") # сохранить как png картинку
	plt.show() # вывести на экран


Задача
===================

1. Считать и отобразить граф городов;
2. Построить и отобразить остовное дерево методом обхода в ширину (DFS);
3. Построить и отобразить остовное дерево методом обхода в глубину (BFS);
4. Выделить и отобразить компоненты связности, проверить связность графа;
5. Найти длины кратчайших путей от заданной вершины ко всем остальным (алгоритм Дейкстры);
6. Отобразить кратчайший путь между двумя вершинами (восстановление пути);

**Дополнительно:**

1. Проверить эйлеровость графа и отобразить эйлеров цикл
2. Найти и отобразить гамильтонов цикл в графе или вывести сообщение, что граф не гамильтонов

Пример файла с входными данными для задач данной работы
-------------------------------------------------------

.. code-block:: text

	Апельсиновый Мандариновый 100
	Мандариновый Ананасовый 200
	Мандариновый Папайя 300
	Мандариновый Кивиновый 400
	Кивиновый Ананасовый 500
	Яблочный Грушевый 100
	Яблочный Вишнёвый 200
	Вишнёвый Сливовый 300
	Грушевый Сливовый 400
	Вишнёвый Черешневый 500
	Кивиновый Фейхоа 600
	Сливовый Алычовый 600
	Алычовый Терновый 700
	Мандариновый Персиковый 1000
	Персиковый Абрикосовый 300
	Абрикосовый Сливовый 400
	Абрикосовый Алычовый 200
	Земляничный Клубничный 100
	Клубничный Брусничный 200
