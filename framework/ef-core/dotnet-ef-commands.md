# `dotnet ef` 命令

工具安装

```console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

安装确认：

```console
dotnet ef
```

## database

### `dotnet ef database drop`

删除数据库。

### `dotnet ef database update`

使用最新或指定的迁移更新数据库。

```console
dotnet ef database update
dotnet ef database update InitialCreate
dotnet ef database update 20180904195021_InitialCreate
```

## dbcontext

### `dotnet ef dbcontext info`

获取 `DbContext` 类型信息。

### `dotnet ef dbcontext list`

列出可用的 `DbContext` 类型。

## migrations

### `dotnet ef migrations add`

添加一个新的迁移。

### `dotnet ef migrations list`

列出可用的迁移。

### `dotnet ef migrations remove`

移除最新的迁移。

## 附录：参考资料

- [Entity Framework Core tools reference - .NET CLI](https://docs.microsoft.com/en-us/ef/core/miscellaneous/cli/dotnet)
