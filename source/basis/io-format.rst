数据类型
========

GMT可以绘制笛卡尔坐标轴、地图的经纬度轴以及绝对时间轴、相对时间轴。对于
不同的坐标轴，需要的数据类型也不同。GMT所支持的数据类型主要分为四大类：

- 地理坐标
- 绝对时间坐标
- 相对时间坐标
- 一般浮点数

.. _geographic_coordinates:

地理坐标
~~~~~~~~

地理坐标（即经纬度）有两种表示方式：

#. 以浮点型的度数表示。比如 **-123.45** 代表-123.45度
#. 度分秒表示：

      [**±**]\ *ddd*\ [**:**\ *mm*\ [**:**\ *ss*\[.\ *xxx*]]][**W**\|\ **E**\|\ **S**\|\ **N**]

   其中，*ddd*\ 、*mm*\ 、*ss*\ 、*xxx* 分别表示弧度、弧分、弧秒、弧毫秒。
   **W**\ 、**E**\ 、**S**\ 、**N** 分别代表西经、东经、北纬、南纬。
   例如 **123:27W** 代表西经123度27分，\ **123:27:15.120W** 表示西经123度27分15.12秒。

.. _absolute_time_coordinates:

绝对时间坐标
~~~~~~~~~~~~

绝对时间由两部分构成，即日期和时间，表示为 *date*\ **T**\ *clock*

其中 **T** 是关键字，用于分隔日期和时间。

日期 *date* 可以是如下格式的一种：

#. *yyyy*\[**-**\ *mm*\[**-**\ *dd*]]\ ：年-月-日，例如 **2013**\ 、**2015-10**\ 、**2015-01-02**
#. *yyyy*\[**-**\ *jjj*]\ ：年-一年中的第几日，例如 **2015-040**
#. *yyyy*\[**-W**\ *ww*\[**-**\ *d*]]\ ：年-第几周-该周内第几天，例如 **2014-W01-3**\ 、\ **2014-W01**

时间 *clock* 是24小时制，其格式为 *hh*\ **:**\ [*mm*\ **:**\ [*ss*\[.\ *xxx*]]]\ ，
例如 **10:10:35.120**\ 。

使用过程中需要注意：

#. GMT的时间数据的输入/输出格式默认为 ``yyyy-mm-ddThh:mm:ss.xxx``\ 。
   若想要输入其它格式的时间数据，需要修改 :term:`FORMAT_DATE_IN`
   和 :term:`FORMAT_CLOCK_IN`\ ；
   若想要输出其它格式的时间数据，需要修改 :term:`FORMAT_DATE_OUT`
   和 :term:`FORMAT_CLOCK_OUT`
#. 若未指定 *date* 则假定 *date* 为今日
#. 若未指定 *clock* 则认为是 **00:00:00**
#. 若指定了 *clock* 则必须要加 **T**\ ，比如 **T10:20:34** 表示今天的早晨10点多
#. 所有绝对时间在程序内部都会被转换成相对于特定时刻的秒数

下面举几个绝对日期的例子：

- **2014-02-10T10:00:00.000**
- **T10:20:44.234**
- **2014-040T23:23:54.330**

.. _relative_time_coordinates:

相对时间坐标
~~~~~~~~~~~~

相对时间坐标即某个时间相对于参考时刻的秒数、小时数、天数或年数。
因而在使用相对时间时，首先要给定两个参数：参考时刻以及相对时间所使用的单位。

GMT参数 :term:`TIME_EPOCH` 用于指定参考时刻，
:term:`TIME_UNIT` 用于指定相对时间的单位。
也可以用参数 :term:`TIME_SYSTEM` 同时指定这两个参数。
默认的参考时刻为1970年1月1日午夜，默认的相对时间单位为秒。

在指定了参考时刻后，相对时间就跟一般的浮点数没什么区别了。那如何区分一般的
浮点数与相对时间呢？有两种方式：

#. 在数据后加上小写的 **t**\ ，比如 **30t** 表示相对于 :term:`TIME_EPOCH`
   间隔了 30 个 :term:`TIME_UNIT` 时间单位的时刻
#. 在命令行中使用 **-ft** 选项表明当前数据是相对时间，此时不需要在数字后加 **t**

.. _float_coordinates:

一般坐标值
~~~~~~~~~~

在绘制常规的笛卡尔坐标轴时，即输入数据不是地理坐标、绝对时间或相对时间时，
输入数据可以直接用浮点数表示，而不去在意其物理含义及单位。比如，5牛顿的力，
5千克的质量，在 GMT 看来都只是浮点数 **5**\ 。

这些浮点数坐标可以用两种方式表示：

#. 一般表示： [**±**]\ *xxx.xxx*\ ，比如 **123.45**
#. 指数表示： [**±**]\ *xxx.xx*\[**E**\|\ **e**\|\ **D**\|\ **d**\[**±**]\ *xx*]\ 。
   比如 **1.23E10**
