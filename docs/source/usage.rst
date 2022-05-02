Lab1: Dependency Analysis and Dependency Graphs
=====

Introduction
=====

- 根据软件应用程序的源代码，找出模块/类/函数之间的依赖关系。
- 分析EnglishPal新架构的依赖关系（BeginOfSpring）。
- 分析EnglishPal旧架构（ColdDew）的依赖关系。
- 比较上述两种架构。哪一个看起来更好，BeginOfSpring或CodeDew？
- 分析教科书第4章中订单行分配架构的依赖关系。

Materials and Methods
=====

-----
工具
-----

- **Snakefood**：可从 Python 代码生成依赖项，从依赖项列表筛选、聚类和生成图形的工具
- **Graphviz**：Graphviz是开源的图形可视化软件。图形可视化是一种将结构信息表示为抽象图形和网络图的方式。Graphviz 使用一种叫 DOT 的语言来表示图形。
- **Mermaid**：Mermaid是一个基于 Javascript 的图表工具。它使用类似 markdown 风格的文本，来简化和加速生成图表、流程图等。

-----
方法
-----


分析模块间的依赖关系
-----------

**安装snakefood3**

.. code-block:: bash

  pip install snakefood3

**安装Graphviz**

.. code-block:: bash

  pip install graphviz

**使用**

1.运行测试，生成dot代码

.. code-block:: bash

  python -m snakefood3 PROJECT_PATH PYTHON_PACKAGE_NAME --group examples/group

2.生成dot文件

.. code-block:: bash

  python -m snakefood3 ~/code/bgmi/ bgmi -g examples/group > examples/bgmi.dot

3.使用Graphviz将dot文件转化为png文件

.. code-block:: bash

  dot -T png examples/bgmi.dot -o examples/bgmi.png # install graphviz

分析类/函数之间的依赖关系
-------------

对代码的进行分析后使用Mermaid绘制依赖关系图。

`Mermaid <https://mermaid-js.github.io/mermaid-live-editor/edit>`_


Results
=======

---------------
EnglishPal（BeginOfSpring）
---------------

模块间的依赖关系
--------

.. code-block:: dot

  strict digraph "dependencies" {
  graph [
      rankdir="LR",
      overlap="scale",
      ratio="fill",
      fontsize="16",
      dpi="150",
      clusterrank="local"
    ]

   node [
      fontsize=14
      shape=ellipse
      fontname=Consolas
   ];
  "app.Login" -> "app.account_service"
  "app.difficulty" -> "app.Article"
  "app.pickle_idea" -> "app.Article"
  "app.pickle_idea2" -> "app.Article"
  "app.wordfreqCMD" -> "app.Article"
  "app.WordFreq" -> "app.Article"
  "app.UseSqlite" -> "app.Article"
  "app.wordfreqCMD" -> "app.difficulty"
  "app.UseSqlite" -> "app.Login"
  "app.user_service" -> "app.main"
  "app.Login" -> "app.main"
  "app.account_service" -> "app.main"
  "app.Yaml" -> "app.main"
  "app.Article" -> "app.main"
  "app.pickle_idea" -> "app.user_service"
  "app.pickle_idea2" -> "app.user_service"
  "app.wordfreqCMD" -> "app.user_service"
  "app.WordFreq" -> "app.user_service"
  "app.Article" -> "app.user_service"
  "app.wordfreqCMD" -> "app.WordFreq"
  "app.pickle_idea" -> "app.wordfreqCMD"
  }

`模块依赖关系图 <https://imgtu.com/i/OFpGtg>`_

类/函数之间的依赖关系
--------

.. code-block:: mermaid

  classDiagram
    account_service..>Login
    Article..>WordFreq
    Article..>wordfreqCMD
    Article..>UseSqlite
    Article..>pickle_idea
    Article..>difficulty
    difficulty..>wordfreqCMD
    Login..>UseSqlite
    main ..> Article
    main ..> Yaml
    main ..> user_service
    main ..> account_service
    user_service..>Article
    user_service..>WordFreq
    user_service..>wordfreqCMD
    user_service..>pickle_idea
    user_service..>pickle_idea2
    WordFreq ..> wordfreqCMD
    wordfreqCMD..> pickle_idea
    Sqlite3Template <|-- InsertQuery
    Sqlite3Template <|-- RecordQuery

    class account_service{
    +signup()
    +login()
    +logout()
    +reset()

   }
    class Article{
    +total_number_of_essays()
    +get_article_title(s)
    +get_article_body(s)
    +get_today_article(user_word_list, articleID)
    +load_freq_history(path)
    +within_range(x, y, r)
    +get_question_part(s)
    +get_answer_part(s)
   }
    class difficulty{
    +load_record(pickle_fname)
    +difficulty_level_from_frequency(word, d)
    +get_difficulty_level(d1, d2)
    +revert_dict(d)
    +user_difficulty_level(d_user, d)
    +text_difficulty_level(s, d)
   }
    class Login{
    +verify_user(username, password)
    +add_user(username, password)
    +check_username_availability(username)
    +change_password(username, old_password, new_password)
    +get_expiry_date(username)
    +md5(s)
   }
    class pickle_idea{
    +lst2dict(lst, d)
    +dict2lst(d)
    +merge_frequency(lst1, lst2)
    +load_record(pickle_fname)
    +save_frequency_to_pickle(d, pickle_fname)
    +unfamiliar(path,word)
    +familiar(path,word)
   }
    class pickle_idea2{
    +lst2dict(lst, d)
    +deleteRecord(path,word)
    +dict2lst(d)
    +merge_frequency(lst1, lst2)
    +load_record(pickle_fname)
    +save_frequency_to_pickle(d, pickle_fname)
   }
    class user_service{
    +user_reset(username)
    +unfamiliar(username, word)
    +familiar(username, word)
    +deleteword(username, word)
    +userpage(username)
    +user_mark_word(username)
    +get_time()
    +get_flashed_messages_if_any()
   }
    class Sqlite3Template{
    +__init__(self, db_fname)
    +connect(self, db_fname)
    +instructions(self, query_statement)
    +operate(self)
    +format_results(self)
    +do(self)
    +instructions_with_parameters(self, query_statement, parameters)
    +do_with_parameters(self)
    +operate_with_parameters(self)
   }
    class InsertQuery{
    +instructions(self, query)
   }
    class RecordQuery{
    +instructions(self, query)
    +format_results(self)
    +get_results(self)
   }
    class WordFreq{
    +__init__(self, s)
    +get_freq(self)
   }
    class wordfreqCMD{
    +freq(fruit)
    +youdao_link(s)
    +file2str(fname)
    +remove_punctuation(s)
    +sort_in_descending_order(lst)
    +sort_in_ascending_order(lst)
    +make_html_page(lst, fname)
   }
    class main{
    +get_random_image(path)
    +get_random_ads()
    +appears_in_test(word,d)
    +mark_word()
    +mainpage()
   }

---------------
EnglishPal（ColdDew） 
---------------

模块间的依赖关系
-------

.. code-block:: dot

  # This file was generated by snakefood3.

   strict digraph "dependencies" {
    graph [
            rankdir="LR",
            overlap="scale",
            ratio="fill",
            fontsize="16",
            dpi="150",
            clusterrank="local"
        ]

       node [
            fontsize=14
            shape=ellipse
            fontname=Consolas
       ];
    "app.wordfreqCMD" -> "app.difficulty"
    "app.wordfreqCMD" -> "app.main"
    "app.UseSqlite" -> "app.main"
    "app.WordFreq" -> "app.main"
    "app.pickle_idea" -> "app.main"
    "app.pickle_idea2" -> "app.main"
    "app.difficulty" -> "app.main"
    "app.wordfreqCMD" -> "app.WordFreq"
    "app.pickle_idea" -> "app.wordfreqCMD"

   }

`模块间依赖关系图 <https://imgtu.com/i/OF9n5F>`_

类/函数之间的依赖关系
-------------

.. code-block:: mermaid

  classDiagram

    difficulty ..> wordfreqCMD
    main ..> wordfreqCMD
    main ..> WordFreq
    main ..> InsertQuery
    main ..> RecordQuery
    main ..> pickle_idea
    main ..> pickle_idea2 
    main ..> difficulty 
    Sqlite3Template <|-- InsertQuery
    Sqlite3Template <|-- RecordQuery
    WordFreq ..> wordfreqCMD
    wordfreqCMD..> pickle_idea
    
    class difficulty{
      +load_record(pickle_fname)
      +difficulty_level_from_frequency(word, d)
      +get_difficulty_level(d1, d2)
      +revert_dict(d)
      +user_difficulty_level(d_user, d)
      +text_difficulty_level(s, d)
    }
    class main{
      +get_random_image(path)
      +get_random_ads()
      +total_number_of_essays()
      +load_freq_history(path)
      +verify_user(username, password)
      +add_user(username, password)
      +check_username_availability(username)
      +get_expiry_date(username)
      +within_range(x, y, r)
      +get_article_title(s)
      +get_article_body(s)
      +get_today_article(user_word_list, articleID)
      +appears_in_test(word, d)
      +get_time()
      +get_question_part(s)
      +get_answer_part(s)
      +get_flashed_messages_if_any()
      +user_reset(username)
      +mark_word()
      +mainpage()
      +user_mark_word(username)
      +unfamiliar(username,word)
      +familiar(username,word)
      +deleteword(username,word)
      +userpage(username)
      +signup()
      +login()
      +logout()
    }
    class pickle_idea{
      +lst2dict(lst, d)
      +dict2lst(d)
      +merge_frequency(lst1, lst2)
      +load_record(pickle_fname)
      +save_frequency_to_pickle(d, pickle_fname)
      +unfamiliar(path,word)
      +familiar(path,word)
    }
    class pickle_idea2{
      +lst2dict(lst, d)
      +deleteRecord(path,word)
      +dict2lst(d)
      +merge_frequency(lst1, lst2)
      +load_record(pickle_fname)
      +save_frequency_to_pickle(d, pickle_fname)
    }
    class Sqlite3Template{
      +__init__(self, db_fname)
      +connect(self, db_fname)
      +instructions(self, query_statement)
      +operate(self)
      +format_results(self)
      +do(self)
      +instructions_with_parameters(self, query_statement, parameters)
      +do_with_parameters(self)
      +operate_with_parameters(self)
    }
    class InsertQuery{
      +instructions(self, query)
    }
    class RecordQuery{
      +instructions(self, query)
      +format_results(self)
      +get_results(self)
    }
    class WordFreq{
      +__init__(self, s)
      +get_freq(self)
    }
    class wordfreqCMD{
      +freq(fruit)
      +youdao_link(s)
      +file2str(fname)
      +remove_punctuation(s)
      +sort_in_descending_order(lst)
      +sort_in_ascending_order(lst)
      +make_html_page(lst, fname)
    }


-----------------------
The order line allocation’s architecture in Chapter 4
-----------------------

模块间的依赖关系
------------

.. code-block:: dot

  # This file was generated by snakefood3.

   strict digraph "dependencies" {
    graph [
            rankdir="LR",
            overlap="scale",
            ratio="fill",
            fontsize="16",
            dpi="150",
            clusterrank="local"
        ]
    

       node [
            fontsize=14
            shape=ellipse
            fontname=Consolas
       ];
    "allocation.config" -> "allocation.adapters.notifications"
    "allocation.domain.model" -> "allocation.adapters.orm"
    "allocation.domain.events" -> "allocation.adapters.redis_eventpublisher"
    "allocation.config" -> "allocation.adapters.redis_eventpublisher"
    "allocation.adapters.orm" -> "allocation.adapters.repository"
    "allocation.domain.model" -> "allocation.adapters.repository"
    "allocation.domain.events" -> "allocation.domain.model"
    "allocation.domain.commands" -> "allocation.domain.model"
    "allocation.bootstrap" -> "allocation.entrypoints.flask_app"
    "allocation.service_layer.handlers" -> "allocation.entrypoints.flask_app"
    "allocation.domain.commands" -> "allocation.entrypoints.flask_app"
    "allocation.views" -> "allocation.entrypoints.flask_app"
    "allocation.bootstrap" -> "allocation.entrypoints.redis_eventconsumer"
    "allocation.domain.commands" -> "allocation.entrypoints.redis_eventconsumer"
    "allocation.config" -> "allocation.entrypoints.redis_eventconsumer"
    "allocation.adapters.notifications" -> "allocation.service_layer.handlers"
    "allocation.service_layer.unit_of_work" -> "allocation.service_layer.handlers"
    "allocation.domain.commands" -> "allocation.service_layer.handlers"
    "allocation.domain.model" -> "allocation.service_layer.handlers"
    "allocation.domain.events" -> "allocation.service_layer.handlers"
    "allocation.domain.events" -> "allocation.service_layer.messagebus"
    "allocation.service_layer.unit_of_work" -> "allocation.service_layer.messagebus"
    "allocation.domain.commands" -> "allocation.service_layer.messagebus"
    "allocation.adapters.repository" -> "allocation.service_layer.unit_of_work"
    "allocation.config" -> "allocation.service_layer.unit_of_work"
    "allocation.adapters.notifications" -> "allocation.bootstrap"
    "allocation.service_layer.unit_of_work" -> "allocation.bootstrap"
    "allocation.service_layer.handlers" -> "allocation.bootstrap"
    "allocation.adapters.orm" -> "allocation.bootstrap"
    "allocation.service_layer.messagebus" -> "allocation.bootstrap"
    "allocation.adapters.redis_eventpublisher" -> "allocation.bootstrap"
    "allocation.service_layer.unit_of_work" -> "allocation.views"

   }

`模块间依赖关系图 <https://imgtu.com/i/OFCCdK>`_

类/函数之间的依赖关系
-----------

.. code-block:: mermaid

  classDiagram
    OrderLine <|-- orm
    Batch <|-- orm
    AbstractRepository o-- SqlAlchemyRepository
    Batch <|-- AbstractRepository
    OrderLine <|-- Batch
    OrderLine <|-- services
    AbstractRepository <|-- services
    model <|-- services
    OrderLine <|-- model
    Batch <|-- model
    OutOfStock <|-- model
    SqlAlchemyRepository <|-- flask_app
    OrderLine <|-- flask_app
    OutOfStock <|-- flask_app
    InvalidSku <|-- flask_app
    config <|-- flask_app
    orm <|-- flask_app
    services <|-- flask_app
    class orm{
      +start_mappers()
    }
    class AbstractRepository{
      +add(self, batch: model.Batch)
      +get(self, reference)
    }
    class SqlAlchemyRepository{
      +session
      +add(self, batch)
      +get(self, reference)
      +list(self)
    }
    class OrderLine{
        +str:orderid
        +str:sku
        +int:qty
    }
    class Batch{
        +str:reference
        +str:sku
        -int:purchased_quantity
        +Optional[date]:eta
        -allocations:Set[OrderLine]
        __repr__(self)
        __eq__(self, other)
        __hash__(self)
        __gt__(self, other)
        +allocate(self, line: OrderLine)
        +deallocate_one(self)
        +allocated_quantity(self)
        +available_quantity(self)
        +can_allocate(self, line: OrderLine)
    }
    class flask_app{
        +allocate_endpoint()
    }
    class OutOfStock{

    }
    class model{
        +allocate(line: OrderLine, batches: List[Batch])
    }
    class InvalidSku{
    
    }
    class services{
        +is_valid_sku(sku, batches)
        +allocate(line: OrderLine, repo: AbstractRepository, session)
    }
    class config{
        +get_postgres_uri()
        +get_api_url()
    }

Discussions
============

Table 1: Comparing five aspects between the two versions of EnglishPal, ColdDew and BeginningOfSpring.

+---------------------------------------------------------+---------+-----------------------+
|                                                         | ColdDew | **BeginningOfSpring** |
+=========================================================+=========+=======================+
|    Lines of code in main.py (excluding blank lines)     |   431   |          56           |
+---------------------------------------------------------+---------+-----------------------+
|        Number of HTML files in folder templates         |    3    |          10           |
+---------------------------------------------------------+---------+-----------------------+
|         Has a service layer? Answer Yes or No.          |   Yes   |          Yes          |
+---------------------------------------------------------+---------+-----------------------+
| Front-end and back-end coupling. Answer Strong or Weak. | Strong  |         Weak          |
+---------------------------------------------------------+---------+-----------------------+
|           Number of module-level dependencies           |    9    |          21           |
+---------------------------------------------------------+---------+-----------------------+

From a scale 1 (worst) to scale 5 (best), how would you evaluate the architectural health of each version of EnglishPal?Which version of EnglishPal is easier to understand and maintain? Explain in no more than 3 sentences.

   ColdDew版本等级为2，原因为系统能正常运行，但前后端的耦合较强；一些文件代码较为冗长，阅读代码时不易理解；若修改部分代码，可能会导致多处地方均需修改。
   BeginningOfSpring版本等级为4，原因为前后端分离，耦合性减弱，便于对代码进行修改；每个文件代码量减少，阅读代码时较容易理解。
   BeginningOfSpring版本更易理解与维护，每个文件代码量较少，单一职责，逻辑清晰，便于阅读理解；前后端分离，耦合性减弱，便于修改代码。

References
===========

`snakefood · PyPI <https://pypi.org/project/snakefood/>`_

`Mermaid <https://mermaid-js.github.io/mermaid-live-editor/edit>`_

`Graphviz 安装并使用 (Python) - 乌漆WhiteMoon - 博客园 (cnblogs.com) <https://www.cnblogs.com/linfangnan/p/13210536.html>`_

成员信息
=======

[刘奕秀]-201931990209-1978933929@qq.com (TECH LEAD)

[李敏]--201931990403-2609891867@qq.com

[吴佩媛]-201931990410-29723741292@qq.com
