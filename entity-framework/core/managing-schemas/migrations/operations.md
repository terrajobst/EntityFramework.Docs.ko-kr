---
title: 사용자 지정 마이그레이션 작업-EF 코어
author: bricelam
ms.author: bricelam
ms.date: 11/7/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 84d80175e719c950844b13688e1a4992614f25d8
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/14/2018
---
<a name="custom-migrations-operations"></a>사용자 지정 마이그레이션 작업
============================
MigrationBuilder API를 사용 하면 마이그레이션하는 동안 다양 한 종류의 작업을 수행할 수 있습니다 하지만 비록 쉽습니다. 그러나 API 사용자 작업을 정의할 수 있도록 확장 가능한 이기도 합니다. API를 확장 하는 방법은 두 가지가:를 사용 하는 `Sql()` 메서드, 또는 사용자 지정을 정의 하 여 `MigrationOperation` 개체입니다.

을 설명 하기 위해 살펴보겠습니다 각 방법을 사용 하는 데이터베이스 사용자를 만드는 작업을 구현 합니다. 우리의 마이그레이션에 다음 코드를 작성할 수 있도록 합니다.

``` csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

<a name="using-migrationbuildersql"></a>MigrationBuilder.Sql()를 사용 하 여
----------------------------
호출 하는 확장 메서드를 정의 하는 사용자 지정 작업을 구현 하는 가장 쉬운 방법은 것 `MigrationBuilder.Sql()`합니다.
적절 한 Transact SQL을 생성 하는 예제는 다음과 같습니다.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
    => migrationBuilder.Sql($"CREATE USER {name} WITH PASSWORD '{password}';");
```

마이그레이션을 여러 데이터베이스 공급자를 지원 해야 하는 경우 사용할 수 있습니다는 `MigrationBuilder.ActiveProvider` 속성입니다. Microsoft SQL Server와 PostgreSQL 모두 지 원하는 예제는 다음과 같습니다.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    switch (migrationBuilder.ActiveProvider)
    {
        case "Npgsql.EntityFrameworkCore.PostgreSQL":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD '{password}';");

        case "Microsoft.EntityFrameworkCore.SqlServer":
            return migrationBuilder
                .Sql($"CREATE USER {name} WITH PASSWORD = '{password}';");
    }
}
```

이 방법은 작동 모든 공급자를 알고 있는 경우 사용자 지정 작업을 적용할 위치 합니다.

<a name="using-a-migrationoperation"></a>MigrationOperation를 사용 하 여
---------------------------
SQL에서 사용자 지정 작업을 분리할를 정의할 수 있습니다 직접 `MigrationOperation` 나타냅니다. 그러면 생성 하려면 적절 한 SQL을 결정할 수 있도록 작업을 공급자에 전달 됩니다.

``` csharp
class CreateUserOperation : MigrationOperation
{
    public string Name { get; set; }
    public string Password { get; set; }
}
```

이 방법에서는 확장 메서드 하기만 하려면 이러한 작업 중 하나를 추가 `MigrationBuilder.Operations`합니다.

``` csharp
static MigrationBuilder CreateUser(
    this MigrationBuilder migrationBuilder,
    string name,
    string password)
{
    migrationBuilder.Operations.Add(
        new CreateUserOperation
        {
            Name = name,
            Password = password
        });

    return migrationBuilder;
}
```

이 접근 방식에서는 각 공급자에이 작업에 대 한 SQL을 생성 하는 방법에 알아야 자신의 `IMigrationsSqlGenerator` 서비스입니다. 새 작업을 처리 하는 SQL Server의 생성기를 재정의 하는 예제는 다음과 같습니다.

``` csharp
class MyMigrationsSqlGenerator : SqlServerMigrationsSqlGenerator
{
    public MyMigrationsSqlGenerator(
        MigrationsSqlGeneratorDependencies dependencies,
        IMigrationsAnnotationProvider migrationsAnnotations)
        : base(dependencies, migrationsAnnotations)
    {
    }

    protected override void Generate(
        MigrationOperation operation,
        IModel model,
        MigrationCommandListBuilder builder)
    {
        if (operation is CreateUserOperation createUserOperation)
        {
            Generate(createUserOperation, builder);
        }
        else
        {
            base.Generate(operation, model, builder);
        }
    }

    private void Generate(
        CreateUserOperation operation,
        MigrationCommandListBuilder builder)
    {
        var sqlHelper = Dependencies.SqlGenerationHelper;
        var stringMapping = Dependencies.TypeMapper.GetMapping(typeof(string));

        builder
            .Append("CREATE USER ")
            .Append(sqlHelper.DelimitIdentifier(name))
            .Append(" WITH PASSWORD = ")
            .Append(stringMapping.GenerateSqlLiteral(password))
            .AppendLine(sqlHelper.StatementTerminator)
            .EndCommand();
    }
}
```

업데이트 된 기본 마이그레이션 sql 생성기 서비스를 대체 합니다.

``` csharp
protected override void OnConfiguring(DbContextOptionsBuilder options)
    => options
        .UseSqlServer(connectionString)
        .ReplaceService<IMigrationsSqlGenerator, MyMigrationsSqlGenerator>();
```
