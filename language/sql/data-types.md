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

## 附录：参考资料

- [SQL For Dummies, 9th Edition](https://www.dummies.com/store/product/SQL-For-Dummies-9th-Edition.productCd-1119527074.html#)
