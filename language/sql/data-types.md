# SQL 数据类型

SQL 规范认可六种预定义的常规类型：

- Numeric
- String
- Boolean
- Datetime
- Interval
- XML

## 1 精确数字

精确数字 (_exact numeric_) 数据类型允许您精确地表达一个数字的值。

### 1.1 INTEGER

`INTEGER` 类型没有小数位，并且精度取决于具体的 SQL 实现。作为数据库开发者，您不能指定它的精度。

### 1.2 SMALLINT

`SMALLINT` 也是整形数据类型，但是 `SMALLINT` 的精度在具体的实现中不能超过同一实现中的 `INTEGER` 精度。

### 1.3 BIGINT

`BIGINT` 数据类型的精度在具体的实现中不能少于同一个实现中的 `INTEGER` 精度。

### 1.4 NUMERIC

`NUMERIC` 数据除了拥有整数部分外，还有一个小数部分。

- `NUMERIC`：默认精度与小数位
- `NUMERIC(p)`：指定的精度 `p` 以及默认的小数位
- `NUMERIC(p, s)`：指定的精度 `p` 以及默认的小数位 `s`

其中 `p` (_precision_) 表示精度占位符，`s` (_scale_) 表示小数位占位符。

### 1.5 DECIMAL

`DECIMAL` 数据类型与 `NUMERIC` 类似。这种数据类型也拥有小数部分，您也可以指定它的精度与小数位。不同的是，您的实现可能会指定一个大于您指定的精度。

比如，`NUMERIC(5, 2)` 不可能包含一个绝对值大于 `999.99` 的值，但是 `DECIMAL(5, 2)` 却可以。如果您的 SQL 实现允许更大的值，那么 DBMS 就不会拒绝这个值。

### 1.6 DECFLOAT

`DECFLOAT` 数据类型是 SQL: 2016 中新增的，不同于 `REAL` 与 `DOUBLE PRECISION` 数据类型的近似值，`DECFLOAT` 结合了 `DECIMAL` 数据类型的精确度以及 `FLOAT` 数据类型的性能优势。这对于那些近似值不可接受的商业应用来说很重要。

## 2 近似数字

### 2.1 REAL

`REAL` 是单精度浮点数字数据类型，精度取决于 SQL 的实现。通常来说，您使用的硬件决定了它的精度。比如，一个 64 位的机器会比 32 位的机器提供给多的精度。

### 2.2 DOUBLE PRECISION

`DOUBLE PRECISION` 是双精度浮点数字数据类型，精度同样取决于实现。不可思议的是，`DOUBLE` 的含义也是取决于实现。

### 2.3 FLOAT

`FLOAT` 数据类型是比较实用的，它可以指定精度。

> **提示**
>
> 尽量使用 `FLOAT` 而非 `REAL` 或 `DOUBLE PRECISION` 数据类型，这会使得数据库迁移更加方便。这是因为 `FLOAT` 数据类型能够让您指定精度，而让硬件帮您决定使用单精度或是双精度计算（记住，`REAL` 和 `DOUBLE PRECISION` 数字与硬件有关）。

如果您不确定使用精确数据类型（`NUMERIC` 和 `DECIMAL`）还是近似数据类型（`FLOAT` 和 `REAL`），那么就使用 `NUMERIC` 类型。精确数据类型占用更少的系统资源，同时能够提供精确的结果。如果您的数据可能值的范围足够大以至于需要您使用近似数据类型，而这个其实是可以提前预知的。

## 3 字符字符串

### 3.1 `CHARACTER`

`CHARACTER` 或 `CHAR` 数据类型可以指定字符的个数，语法 `CHAR(x)`，`x` 代表字符的个数。如果一个列的数据类型为 `CHAR(16)`，那么您能输入进这一列的数据最多容纳 16 个字符。

如果没有指定参数（也就是 `x` 占位符的值），那么 SQL 会假定长度为 1 个字符。如果您输入进 `CHARACTER` 字段中的数据长度小于指定的长度，那么 SQL 会用空白符来填充余下的字符。

### 3.2 `CHARACTER VARYING`

如果条目中某一列的数据长度可变并且您不希望 SQL 给空余的长度填充空白符，那么 `CHARACTER VARYING` 数据类型就派上用场了。这种数据类型可以让您存储精确的字符个数。语法 `CHARACTER VARYING(x)` 或 `VARCHAR(x)`，其中 `x` 为字符的最大数量。

### 3.3 `CHARACTER LARGE OBJECT`

`CHARACTER LARGE OBJECT` (`CLOB`) 数据类型在 SQL:1999 中引入。它用于存储海量的字符串，而这个是 `CHARACTER` 所不能胜任的。`CLOB` 和原始的字符串很类似，但是它有一些使用约束。

在 `CLOB` 数据类型上不能使用 `PRIMARY KEY`，`FOREIGN KEY`，`UNIQUE`，除了等于或不等于，它们不能用于其它形式的比较。由于它们很大尺寸的特性，应用通常不在数据库之间传输 `CLOB`。相反，使用一个特殊的客户端 `CLOB locator` 数据类型来处理 `CLOB` 数据。它是一个参数，而它的值用于标识一个很大的字符串对象。

### 3.4 `NATIONAL ...`

不同的语言之间可能使用不一样的字符集。比如，德语中某些特殊的字符在英语的字符集中就不存在。而也有某些语言，俄语就和英语的字符集就完全不同。如果您将英语作为了系统默认的字符集，那么您就可以使用 `NATIONAL CHARACTER`，`NATIONAL CHARACTER VARYING`，`NATIONAL CHARACTER LARGE OBJECT` 数据类型来存储不一样的字符集数据。

```sql
CREATE TABLE XLTAE (
    LANGUAGE_1 CHARACTER(40)
    , LANGUAGE_2 CHARACTER VARYING(40) CHARACTER SET GREEK
    , LANGUAGE_3 NATIONAL CHARACTER(40)
    , LANGUAGE_4 CHARACTER(40) CHARACTER SET KANJI
)
```

上面的示例中，`LANGUAGE_1` 使用的是默认的字符集，`LANGUAGE_3` 使用的是国际字符集，`LANGUAGE_2` 使用的是指定的 Greek 字符集，而 `LANGUAGE_4` 使用的指定的 Kanji 字符集。

## 4 二进制字符串

### 4.1 `BINARY`

### 4.2 `BINARY VARYING`

### 4.3 `BINARY LARGE OBJECT`

## 5 布尔

`BOOLEAN` 数据类型包含两种不同的值 `True` 和 `False`。

## 6 日期/时间

SQL 标准定义了 5 种数据类型来处理日期与时间，它们被称为 _datetime_ 数据类型。因为这些数据类型一定程度上存在重合，所以某些实现不一定提供所有的这些类型。

### 6.1 `DATE`

`DATE` 类型存储日期的年，月，日。年份是 4 个数字长度，月份和天都是 2 个数字长度。`DATE` 能够表示 _0001_ 到 _9999_ 年份的任意日期。`DATE` 的长度为 10 位，比如 `2019-08-14`。

### 6.2 `TIME WITHOUT TIME ZONE`

`TIME WITHOUT TIME ZONE` 存储时间的时，分，秒。时和分各由 2 个数字组成，秒可能仅有 2 个数字，也有可能扩展包含可选的小数部分。比如：_09:32:58.436_。

小数部分的精度取决于具体的实现，但是至少有 6 个数字的长度。没有小数位的时候 `TIME WITHOUT TIME ZONE` 占用 8 位，反之 9 位。`TIME WITHOUT TIME ZONE` 数据类型也可以使用 `TIME` 表示，默认没有小数位数字，而 `TIME WITHOUT TIME ZONE(p)` 中的 `p` 表示小数位数字个数。上面示例中的数据就可以使用 `TIME WITHOUT TIME ZONE(3)` 表示。

### 6.3 `TIMESTAMP WITHOUT TIME ZONE`

`TIMESTAMP WITHOUT TIME ZONE` 数据包含了日期和时间信息。`TIMESTAMP WITHOUT TIME ZONE` 数据各部分的长度与约束与 `DATE` 和 `TIME WITHOUT TIME ZONE` 等同，但有一个不同的地方除外：时间组件的小数部分默认长度为 6 个数字。

如果这个值没有小数部分数字，那么 `TIMESTAMP WITHOUT TIME ZONE` 占用 19 位长度——10 个日期位，1 个空格作为分隔符，以及 8 个时间位。如果有小数部分（默认 6 个小数数字），那么加上小数部分数字就为 20 位长度。`TIMESTAMP WITHOUT TIME ZONE` 数据类型可以使用 `TIMESTAMP WITHOUT TIME ZONE` 或 `TIMESTAMP WITHOUT TIME ZONE(p)` 指定，`p` 为小数部分的数字个数，`p` 的值不能为负数，同时具体的实现决定了它的最大值。

### 6.4 `TIME WITH TIME ZONE`

`TIME WITH TIME ZONE` 数据类型与 `TIME WITHOUT TIME ZONE` 数据类型一样，但是这个类型会在最后添加 UTC (Coordinated Universal  Time) 协调世界时偏移信息（UTC 是 GMT 的后继者）。偏移值范围为 `–12:59` 到 `+13:00`。这个额外的信息占用了 6 个数字位——一个作为分隔符的连字符，一个加或减号，然后是小时偏移（两个数字），分钟偏移（两个数字），小时与分钟之间使用分号隔开。没有小数位的 `TIME WITH TIME ZONE` 值（默认）是 14 位长。如果你制定了小数部分，那么加上小数部分数字位 15 位。

### 6.5 `TIMESTAMP WITH TIME ZONE`

`TIMESTAMP WITH TIME ZONE` 与 `TIMESTAMP WITHOUT TIME ZONE` 也类似，除了额外的 UTC 偏移。额外的信息占用了 6 个数字位（参见 `TIME WITH TIME ZONE`）。没有小数位占用 25 位，反之，占用 26 位（小数位默认为 6 个数字）。

## 附录：参考资料

- [SQL For Dummies, 9th Edition](https://www.dummies.com/store/product/SQL-For-Dummies-9th-Edition.productCd-1119527074.html#)
