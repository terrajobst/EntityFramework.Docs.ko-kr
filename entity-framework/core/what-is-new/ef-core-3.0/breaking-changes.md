---
title: EF Core 3.0의 호환성이 손상되는 변경 - EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 534ac95cccc03e9797ba766e601e2fe86eaf8061
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319220"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a>EF Core 3.0에 포함된 호환성이 손상되는 변경(현재 미리 보기 상태)

> [!IMPORTANT]
> 이후 릴리스의 기능 집합 및 일정은 항상 변경될 수 있으며 이 페이지를 최신 상태로 유지하려고 노력함에도 불구하고 최신 계획을 항상 반영할 수 없을 수도 있습니다.

다음 API 및 동작 변경은 3.0.0으로 업그레이드할 때 EF Core 2.2.x용으로 개발된 애플리케이션을 중단시킬 가능성이 있습니다.
데이터베이스 공급자에만 영향을 줄 것으로 예상되는 변경 내용은 [공급자 변경](../../providers/provider-log.md)에 설명되어 있습니다.
3.0 미리 보기 간의 도입된 새로운 기능의 중단은 여기에 설명되어 있지 않습니다.

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a>LINQ 쿼리는 더 이상 클라이언트에서 평가되지 않습니다.

[추적 문제 #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[문제 #12795도 참조](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

3.0 이전에는 EF Core가 쿼리의 일부인 식을 SQL 또는 매개 변수로 변환할 수 없었을 때 클라이언트에서 자동으로 식을 계산했습니다.
기본적으로, 잠재적 비용이 많이 드는 식에 대한 클라이언트 평가는 경고만 트리거합니다.

**새 동작**

3.0부터 EF Core는 최상위 수준 프로젝션(쿼리의 마지막 `Select()` 호출)의 식만 클라이언트에서 평가할 수 있습니다.
쿼리의 다른 부분에서 식을 SQL 또는 매개 변수로 변환할 수 없을 때 예외가 throw됩니다.

**이유**

쿼리의 자동 클라이언트 평가는 중요한 부분을 변환할 수 없어도 많은 쿼리를 실행할 수 있게 합니다.
이 동작으로 인해 프로덕션 환경에서 분명하게 나타날 수 있는 예기치 않은 잠재적 손상을 초래할 수 있습니다.
예를 들어 변환할 수 없는 `Where()` 호출의 조건으로 인해 테이블의 모든 행이 데이터베이스 서버에서 전송되고 필터가 클라이언트에 적용될 수 있습니다.
이 경우 테이블에 개발 중인 몇 개 행만 포함하지만 애플리케이션이 수백만 개의 행이 포함될 수 있는 프로덕션으로 이동할 때 쉽게 감지되지 않을 수 있습니다.
클라이언트 평가 경고도 개발 중에 너무 쉽게 무시됩니다.

이 외에도 자동 클라이언트 평가는 특정 식에 대한 쿼리 변환 기능 개선으로 인해 릴리스 간의 의도하지 않은 호환성이 손상되는 변경이 발생할 수 있습니다.

**완화 방법**

쿼리를 완벽하게 변환할 수 없는 경우 변환할 수 있는 형식으로 쿼리를 다시 작성하거나 `AsEnumerable()`, `ToList()` 또는 유사한 형식을 사용하여 명시적으로 데이터를 클라이언트로 가져오면 LINQ to Objects를 사용하여 추가로 처리할 수 있습니다.

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a>Entity Framework Core는 더 이상 ASP.NET Core 공유 프레임워크에 포함되지 않습니다.

[추적 문제 공지 사항 #325](https://github.com/aspnet/Announcements/issues/325)

이 변경 내용은 ASP.NET Core 3.0 미리 보기 1에 도입되었습니다. 

**이전 동작**

ASP.NET Core 3.0 이전에는 `Microsoft.AspNetCore.App` 또는 `Microsoft.AspNetCore.All`에 대한 패키지 참조를 추가하면 EF Core 및 SQL Server 공급자와 같은 일부 EF Core 데이터 공급자가 포함되었습니다.

**새 동작**

3.0부터 ASP.NET Core 공유 프레임워크에는 EF Core 또는 EF Core 데이터 공급자가 포함되지 않습니다.

**이유**

이 변경 전에 EF Core를 가져오려면 애플리케이션이 ASP.NET Core 및 SQL Server를 대상으로 했는지 여부에 따라 다른 단계가 필요했습니다. 또한 ASP.NET Core를 업그레이드 하면 EF Core 및 SQL Server 업그레이드가 강제되었는데, 이는 항상 바람직하지는 않습니다.

이 변경으로 인해 EF Core를 가져오는 환경은 모든 공급자, 지원되는 .NET 구현 및 애플리케이션 유형에서 동일합니다.
또한 개발자는 이제 EF Core 및 EF Core 데이터 공급자의 업그레이드 시기를 정확히 제어할 수 있습니다.

**완화 방법**

ASP.NET Core 3.0 애플리케이션 또는 기타 지원되는 애플리케이션에서 EF Core를 사용하려면 애플리케이션에서 사용할 EF Core 데이터베이스 공급자에 대한 패키지 참조를 명시적으로 추가합니다.

## <a name="query-execution-is-logged-at-debug-level"></a>쿼리 실행이 디버그 수준에서 로깅됩니다.

[추적 문제 #14523](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 쿼리 및 기타 명령 실행이 `Info` 수준에서 로깅되었습니다.

**새 동작**

EF Core 3.0 부터 명령/SQL 실행의 로깅은 `Debug` 수준입니다.

**이유**

이 변경은 `Info` 로그 수준에서 노이즈를 줄이기 위해 수행되었습니다.

**완화 방법**

이 로깅 이벤트는 `RelationalEventId.CommandExecuting`에서 이벤트 ID 20100으로 정의됩니다.
`Info` 수준에서 다시 SQL을 로그하려면 `OnConfiguring` 또는 `AddDbContext`에서 수준을 명시적으로 구성하세요.
예:
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a>임시 키 값은 더 이상 엔터티 인스턴스에 설정되지 않습니다.

[추적 문제 #12378](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 임시 값이 데이터베이스에서 생성된 실제 값을 나중에 가질 수 있는 모든 키 속성에 할당되었습니다.
일반적으로 이러한 임시 값은 큰 음수입니다.

**새 동작**

3.0부터 EF Core는 엔터티의 추적 정보의 일부로 임시 키 값을 저장하고, 키 속성 자체는 변경하지 않고 그대로 둡니다.

**이유**

이 변경은 일부 `DbContext` 인스턴스의 의해 이전에 추적된 엔터티가 다른 `DbContext` 인스턴스로 이동할 때 임시 키 값이 틀리게 영구화되지 않도록 하기 위해 수행되었습니다. 

**완화 방법**

엔터티 간의 연결을 형성하기 위해 외래 키에 기본 키 값을 할당하는 애플리케이션은 기본 키가 저장 생성되고 `Added` 상태의 엔터티에 속하는 경우 이전 동작에 따라 달라질 수 있습니다.
이는 다음과 같은 방법으로 방지할 수 있습니다.
* 저장 생성 키를 사용하지 않습니다.
* 외래 키 값을 설정하는 대신 관계를 형성하도록 탐색 속성을 설정합니다.
* 엔터티의 추적 정보에서 실제 임시 키 값을 가져옵니다.
예를 들어 `context.Entry(blog).Property(e => e.Id).CurrentValue`는 `blog.Id` 자체가 설정되지 않았더라도 임시 값을 반환합니다.

## <a name="detectchanges-honors-store-generated-key-values"></a>DetectChanges는 저장 생성 키 값을 준수합니다.

[추적 문제 #14616](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 `DetectChanges`에서 찾은 추적되는 않은 엔터티를 `Added` 상태에서 추적하여 `SaveChanges`가 호출될 때 새로운 행으로 삽입했습니다.

**새 동작**

EF Core 3.0부터 생성된 키 값을 사용하고 일부 키 값을 설정한 경우 `Modified` 상태에서 엔터티를 추적합니다.
즉, 엔터티에 대한 행이 있는 것으로 가정하고 `SaveChanges`가 호출될 때 업데이트됩니다.
키 값이 설정되지 않았거나 엔터티 형식이 생성된 키를 사용하지 않는 경우, 새 엔터티는 이전 버전과 마찬가지로 `Added`로 추적됩니다.

**이유**

이 변경으로 인해 저장 생성 키를 사용하는 동안 연결이 분리된 엔터티 그래프를 사용하여 작업을 보다 쉽고 일관되게 할 수 있습니다.

**완화 방법**

엔터티 형식이 생성된 키를 사용하도록 구성되었지만 키 값이 새 인스턴스에 명시적으로 설정된 경우, 이 변경으로 인해 애플리케이션이 중단될 수 있습니다.
해결 방법은 생성된 값을 사용하지 않도록 키 속성을 명시적으로 구성하는 것입니다.
예를 들어 흐름 API가 있는 경우:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

데이터 주석이 있는 경우:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a>계단식 삭제는 기본적으로 즉시 발생합니다.

[추적 문제 #10114](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

3.0 이전에는 SaveChanges가 호출될 때까지 EF Core에는 연계 작업(필요한 보안 주체가 삭제되거나 필요한 보안 주체에 대한 관계가 끊어질 때 종속 엔터티 삭제)이 적용되지 않았습니다.

**새 동작**

3.0부터 EF Core는 트리거 조건이 검색되는 즉시 연계 동작을 적용합니다.
예를 들어, 보안 주체 엔터티를 삭제하기 위해 `context.Remove()`를 호출하면 추적된 모든 관련 필수 종속 항목도 즉시 `Deleted`로 설정됩니다.

**이유**

이 변경으로 인해 `SaveChanges`가 호출되기 _전에_ 삭제될 엔터티를 이해하는 것이 중요한 데이터 바인딩 및 감사 시나리오 환경이 개선되었습니다.

**완화 방법**

이전 동작은 `context.ChangedTracker`의 설정을 통해 복원할 수 있습니다.
예:

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a>쿼리 형식은 엔터티 형식과 통합됩니다.

[추적 문제 #14194](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 [쿼리 형식](xref:core/modeling/query-types)이 기본 키를 구조화된 방법으로 정의하지 않는 데이터를 쿼리하는 수단이었습니다.
즉, 쿼리 형식은 키 없이 엔터티 형식을 매핑하는데 사용(보기에서 가능할 수도 있지만 테이블에서도 가능한 경우)되는 반면, 일반 엔터티 형식은 키가 사용 가능할 때(테이블에서 가능하지만 보기에서도 가능한 경우) 사용되었습니다.

**새 동작**

이제 쿼리 형식은 기본 키가 없는 엔터티 형식이 될 뿐입니다.
키 없는 엔터티 형식은 이전 버전의 쿼리 형식과 동일한 기능이 있습니다.

**이유**

이 변경으로 인해 쿼리 형식의 목적에 대한 혼동을 줄일 수 있습니다.
특히 키 없는 엔터티 형식이며 이로 인해 본질적으로 읽기 전용이지만 엔터티 형식이 읽기 전용으로 할 필요가 있다고 해서 사용해서는 안됩니다.
마찬가지로 보기에 매핑되는 경우가 있지만 이는 보기가 키를 정의하지 않는 경우가 있기 때문입니다.

**완화 방법**

API의 다음 부분은 이제 사용되지 않습니다.
* **`ModelBuilder.Query<>()`** - 대신 엔터티 형식을 키가 없는 것으로 표시하기 위해 `ModelBuilder.Entity<>().HasNoKey()`를 호출해야 합니다.
기본 키가 필요하지만 규칙과 일치하지 않는 경우 잘못된 구성을 피하기 위해 규칙에 따라 구성되지 않습니다.
* **`DbQuery<>`** - 대신 `DbSet<>`를 사용해야 합니다.
* **`DbContext.Query<>()`** - 대신 `DbContext.Set<>()`를 사용해야 합니다.

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a>소유 형식 관계에 대한 구성 API가 변경됨

[추적 문제 #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[추적 문제 #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[추적 문제 #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 소유 관계의 구성은 `OwnsOne` 또는 `OwnsMany` 호출 직후에 수행되었습니다. 

**새 동작**

EF Core 3.0부터 이제 `WithOwner()`를 사용하여 소유자에게 탐색 속성을 구성하는 흐름 API가 있습니다.
예:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

이제 소유자와 소유 간의 관계와 관련된 구성이 다른 관계가 구성되는 것과 유사하게 `WithOwner()` 후에 연결되어야 합니다.
소유 형식 자체에 대한 구성은 `OwnsOne()/OwnsMany()` 이후에도 여전히 연결됩니다.
예:

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

또한 소유 형식 대상으로 `Entity()`, `HasOne()` 또는 `Set()`를 호출하면 예외가 throw됩니다.

**이유**

이 변경으로 인해 소유 형식 자체의 구성과 소유 형식의 _관계 대상_ 사이를 명확하게 분리하게 되었습니다.
이는 `HasForeignKey`와 같은 메서드에 대한 모호성과 혼란을 제거합니다.

**완화 방법**

위의 예와 같이 소유 형식 관계의 구성을 변경하여 새 API 화면을 사용합니다.

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a>외래 키 속성 규칙이 더 이상 보안 주체 속성과 동일한 이름을 일치시키지 않습니다.

[추적 문제 #13274](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

다음 모델을 살펴보세요.
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}

```
EF Core 3.0 이전에는 규칙에 따라 외래 키에 대해 `CustomerId` 속성이 사용되었습니다.
그러나 `Order`가 소유 형식인 경우 `CustomerId`도 기본 키가 되며 이는 일반적으로 예상되는 것은 아닙니다.

**새 동작**

3.0부터 EF Core는 보안 주체 속성과 동일한 이름을 가지는 경우 규칙에 따라 외래 키에 대한 속성을 사용하려고 하지 않습니다.
보안 주체 속성 이름과 연결된 보안 주체 유형 이름 및 보안 주체 속성 이름 패턴과 연결된 탐색 이름은 여전히 일치합니다.
예:

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

**이유**

이 변경으로 인해 소유 형식에서 기본 키 속성을 잘못 정의하지 않게 되었습니다.

**완화 방법**

속성을 외래 키로 하고, 이에 따라서 기본 키의 일부가 되는 경우 명시적으로 이와 같이 구성합니다.

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a>각 속성은 독립적인 메모리 내 정수 키 생성을 사용합니다.

[추적 문제 #6872](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

EF Core 3.0 이전에는 모든 메모리 내 정수 키 속성에 대해 하나의 공유 값 생성기가 사용되었습니다.

**새 동작**

EF Core 3.0부터 메모리 내 데이터베이스를 사용하는 경우 각 정수 키 속성은 자체 값 생성기를 가져옵니다.
또한 데이터베이스가 삭제되면 모든 테이블에 대해 키 생성이 재설정됩니다.

**이유**

이 변경으로 인해 메모리 내 키 생성을 실제 데이터베이스 키 생성에 보다 가깝게 정렬하고 메모리 내 데이터베이스를 사용할 때 테스트를 서로 격리하는 기능이 향상되었습니다.

**완화 방법**

이로 인해 특정 메모리 내 키 값을 사용하는 애플리케이션이 중단될 수 있습니다.
대신 특정 키 값을 사용하지 않거나 새 동작과 일치하도록 업데이트하지 않도록 합니다.

## <a name="backing-fields-are-used-by-default"></a>지원 필드는 기본적으로 사용됩니다.

[추적 문제 #12430](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.

**이전 동작**

3.0 이전에는 속성에 대한 지원 필드가 알려져 있더라도 EF Core는 기본적으로 속성 getter 및 setter 메서드를 사용하여 속성 값을 읽고 작성했습니다.
이에 대한 예외는 쿼리 실행으로 알려진 경우 지원 필드가 직접 설정되었습니다.

**새 동작**

EF Core 3.0부터 속성 지원 필드가 알려진 경우 항상 지원 필드를 사용하여 해당 속성을 읽고 씁니다.
이로 인해 애플리케이션이 getter 또는 setter 메서드로 코딩된 추가 동작을 사용하는 경우 애플리케이션이 중단될 수 있습니다.

**이유**

이 변경으로 인해 엔터티가 포함된 데이터베이스 작업을 수행할 때 EF Core가 기본적으로 비즈니스 논리를 잘못 트리거하는 것을 방지합니다.

**완화 방법**

modelBuilder 흐름 API에서 속성 액세스 모드의 구성을 통해 3.0 이전 버전의 동작을 복원할 수 있습니다.
예:

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a>호환이 가능한 여러 지원 필드가 발견되면 throw됩니다.

[추적 문제 #12523](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

EF Core 3.0 이전에는 여러 필드가 속성의 지원 필드를 찾기 위한 규칙과 일치하면 우선순위에 따라 하나의 필드가 선택되었습니다.
이로 인해 모호한 경우 잘못된 필드가 사용될 수 있습니다.

**새 동작**

EF Core 3.0부터 여러 필드가 동일한 속성과 일치하면 예외가 throw됩니다.

**이유**

이 변경으로 인해 하나의 필드만 정확할 때 다른 필드보다 한 필드만 자동으로 사용하는 것을 방지했습니다.

**완화 방법**

모호한 지원 필드가 있는 속성은 사용할 필드가 명시적으로 지정되어야 합니다.
예를 들어 흐름 API 사용:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a>AddDbContext/AddDbContextPool이 더 이상 AddLogging 및 AddMemoryCache를 호출하지 않음

[추적 문제 #14756](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

EF Core 3.0 이전에는 `AddDbContext` 또는 `AddDbContextPool`을 호출하면 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 및 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)에 대한 호출을 통해 D.I를 사용하여 로깅 및 메모리 캐시 서비스도 등록합니다.

**새 동작**

EF Core 3.0부터 `AddDbContext` 및 `AddDbContextPool`은 DI(종속성 주입)를 사용하여 이러한 서비스를 더 이상 등록하지 않습니다.

**이유**

EF Core 3.0에서는 이러한 서비스가 애플리케이션의 DI 컨테이너에 있을 필요가 없습니다. 그러나 `ILoggerFactory`가 애플리케이션의 DI 컨테이너에 등록된 경우 EF Core에서 여전히 DI를 사용합니다.

**완화 방법**

애플리케이션에 이러한 서비스가 필요한 경우 [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) 또는 [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache)를 사용하여 DI 컨테이너에 명시적으로 등록합니다.

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a>DbContext.Entry는 이제 로컬 DetectChanges를 수행합니다.

[추적 문제 #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 `DbContext.Entry`를 호출하면 추적된 모든 엔터티에 대해 변경 내용이 검색되었습니다.
이로 인해 `EntityEntry`에 노출된 상태가 최신 상태로 유지되었습니다.

**새 동작**

EF Core 3.0부터 `DbContext.Entry` 호출은 지정된 엔터티와 이와 관련된 추적된 모든 주체 엔터티의 변경 내용만 검색하려고 합니다.
즉, 이 메서드를 호출하여 다른 곳의 변경 내용을 검색하지 못할 수 있으므로 애플리케이션 상태에 영향을 줄 수 있습니다.

`ChangeTracker.AutoDetectChangesEnabled`를 `false`로 설정하면 이 로컬 변경 검색도 비활성화됩니다.

변경 검색(예: `ChangeTracker.Entries` 및 `SaveChanges`)을 유발하는 다른 메서드는 추적된 모든 엔터티의 전체 `DetectChanges`를 유발합니다.

**이유**

이 변경으로 인해 `context.Entry`를 사용하는 기본 성능이 향상되었습니다.

**완화 방법**

`Entry`를 호출하기 전에 명시적으로 `ChgangeTracker.DetectChanges()`를 호출하여 3.0 이전 버전의 동작을 확인합니다.

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a>문자열 및 바이트 배열 키는 기본적으로 클라이언트에서 생성되지 않습니다.

[추적 문제 #14617](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

EF Core 3.0 이전에는 null이 아닌 값을 명시적으로 설정하지 않고도 `string` 및 `byte[]` 키 속성을 사용할 수 있었습니다.
이 경우 키 값은 클라이언트에서 GUID로 생성되고 `byte[]`의 바이트로 직렬화됩니다.

**새 동작**

EF Core 3.0부터 키 값이 설정되지 않았음을 나타내는 예외가 throw됩니다.

**이유**

클라이언트에서 생성한 `string`/`byte[]` 값은 일반적으로 유용하지 않고 기본 동작으로 인해 생성된 키 값을 일반적인 방식으로 추론하기가 어려웠기 때문에 이 변경이 이루어졌습니다.

**완화 방법**

다른 null이 아닌 값이 설정되지 않은 경우 키 속성이 생성된 값을 사용해야 함을 명시적으로 지정하면 3.0 이전 버전 동작을 얻을 수 있습니다.
예를 들어 흐름 API가 있는 경우:

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

데이터 주석이 있는 경우:

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a>ILoggerFactory는 이제 범위가 지정된 서비스입니다.

[추적 문제 #14698](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 `ILoggerFactory`가 싱글톤 서비스로 등록되었습니다.

**새 동작**

EF Core 3.0부터 이제 `ILoggerFactory`는 범위가 지정된 대로 등록됩니다.

**이유**

이 변경으로 인해 `DbContext` 인스턴스와 로거를 연결하여 다른 기능을 활성화하고 내부 서비스 공급자의 급증과 같은 병리적인 동작의 일부 사례가 제거됩니다.

**완화 방법**

이 변경 내용은 EF Core 내부 서비스 공급자에 사용자 지정 서비스를 등록하고 사용하지 않는 한 애플리케이션 코드에 영향을 미치지 않아야 합니다.
이는 일반적이지 않습니다.
이러한 경우 대부분의 작업은 여전히 작동하지만 `ILoggerFactory`에 종속된 싱글톤 서비스는 `ILoggerFactory`를 얻기 위해 다른 방식으로 변경해야 합니다.

이와 같은 상황에 처한 경우 [EF Core GitHub 문제 추적기](https://github.com/aspnet/EntityFrameworkCore/issues)에 문제를 제출하여 향후 이 문제를 해결할 수 있는 방법을 더 잘 이해할 수 있도록 `ILoggerFactory`를 사용하는 방법을 알려주세요.

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a>IDbContextOptionsExtensionWithDebugInfo가 IDbContextOptionsExtension에 병합됨

[추적 문제 #13552](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

`IDbContextOptionsExtensionWithDebugInfo`는 2.x 릴리스 주기 동안 인터페이스에 대한 호환성이 손상되는 변경을 방지하기 위해 `IDbContextOptionsExtension`에서 확장된 추가 선택적 인터페이스입니다.

**새 동작**

이제 인터페이스가 `IDbContextOptionsExtension`으로 병합됩니다.

**이유**

이 변경은 인터페이스가 개념적으로 하나이기 때문에 이루어졌습니다.

**완화 방법**

새 멤버를 지원하기 위해 `IDbContextOptionsExtension` 구현을 업데이트해야 합니다.

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a>지연 로드 프록시는 더 이상 탐색 속성이 완전히 로드되었다고 가정하지 않습니다.

[추적 문제 #12780](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

EF Core 3.0 이전에는 `DbContext`가 삭제되면 해당 컨텍스트에서 가져온 엔터티의 지정된 탐색 속성이 완전히 로드되었는지 여부를 알 수 없었습니다.
대신 프록시는 참조 탐색에 null이 아닌 값이 있으면 로드되고 컬렉션 탐색이 비어 있지 않으면 로드된다고 가정합니다.
이러한 경우 지연 로드를 시도하면 아무런 작업도 수행되지 않습니다.

**새 동작**

EF Core 3.0부터 프록시는 탐색 속성이 로드되었는지 여부를 추적합니다.
즉, 컨텍스트가 삭제된 후에 로드되는 탐색 속성에 액세스하려고 하면 로드된 탐색이 비어 있거나 null인 경우에도 항상 아무런 작업도 수행되지 않습니다.
반대로, 로드되지 않은 탐색 속성에 액세스하려고 하면 탐색 속성이 비어 있지 않은 컬렉션이라도 컨텍스트가 삭제되면 예외가 throw됩니다.
이 상황이 발생하는 경우 애플리케이션 코드가 잘못된 시간에 지연 로드를 사용하려고 시도하고 있음을 의미하며, 애플리케이션이 이를 수행하지 않도록 변경해야 합니다.

**이유**

이 변경으로 인해 삭제된 `DbContext` 인스턴스에 지연 로드를 시도할 때 동작을 일관되고 정확하게 만듭니다.

**완화 방법**

애플리케이션을 업데이트하여 삭제된 컨텍스트로 지연 로드를 시도하지 않도록 하거나 예외 메시지에 설명된 대로 작업을 수행하지 않도록 구성합니다.

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a>내부 서비스 공급자의 과도한 생성은 이제 기본적으로 오류입니다.

[추적 문제 #10236](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 병리적인 수의 내부 서비스 공급자를 만드는 애플리케이션에 경고가 기록되었습니다.

**새 동작**

EF Core 3.0부터 이제 이 경고가 고려되며 오류와 예외가 throw됩니다. 

**이유**

이 변경으로 인해 병리적인 사례를 더 명시적으로 노출하여 더 나은 애플리케이션 코드를 구동하게 되었습니다.

**완화 방법**

이 오류가 발생했을 때 가장 적절한 조치는 근본 원인을 이해하고 많은 내부 서비스 공급자를 만드는 것을 중단하는 것입니다.
그러나 오류는 `DbContextOptionsBuilder`의 구성을 통해 경고(또는 무시)로 다시 변환될 수 있습니다.
예:

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a>단일 문자열로 호출되는 HasOne/HasMany의 새 동작

[추적 문제 #9171](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입될 것입니다.

**이전 동작**

EF Core 3.0 이전에는 단일 문자열을 사용하여 `HasOne` 또는 `HasMany`를 호출하는 코드가 혼란스러운 방식으로 해석되었습니다.
예:
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

코드는 비공개일 수 있는 `Entrance` 탐색 속성을 사용하여 `Samurai`를 다른 엔터티 형식과 관련시키려는 것 같습니다.

실제로 이 코드는 탐색 속성을 사용하지 않고 `Entrance`라고 하는 엔터티 형식과 관계를 생성하려고 합니다.

**새 동작**

EF Core 3.0부터 이제 위 코드는 이전에 수행했어야 하는 것처럼 보이는 방식으로 동작합니다.

**이유**

이전 동작은 특히 구성 코드를 읽고 오류를 찾을 때 매우 혼란스럽습니다.

**완화 방법**

새 동작은 형식 이름의 문자열을 사용하고 탐색 속성을 명시적으로 지정하지 않은 채 명시적으로 관계를 구성하는 애플리케이션만 중단됩니다.
이는 일반적이지 않습니다.
이전 동작은 탐색 속성 이름에 대해 `null`을 전달하여 명시적으로 얻을 수 있습니다.
예:

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a>관계형:TypeMapping 주석은 이제 TypeMapping일 뿐입니다.

[추적 문제 #9913](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

이 변경 내용은 EF Core 3.0 미리 보기 2에 도입되었습니다.

**이전 동작**

형식 매핑 주석에 대한 주석 이름은 "관계형:TypeMapping"이었습니다.

**새 동작**

형식 매핑 주석에 대한 주석 이름은 이제 "TypeMapping"입니다.

**이유**

이제 형식 매핑은 관계형 데이터베이스 공급자 이상의 용도로 사용됩니다.

**완화 방법**

이는 일반적이지 않은 주석으로 형식 매핑에 직접 액세스하는 애플리케이션만 중단합니다.
수정하기 위한 가장 적절한 작업은 직접 주석을 사용하는 대신 API 화면을 사용하여 형식 매핑에 액세스하는 것입니다.

## <a name="totable-on-a-derived-type-throws-an-exception"></a>파생된 형식의 ToTable에서 예외가 throw됩니다. 

[추적 문제 #11811](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 유효하지 않은 경우 상속 매핑 전략만 TPH이므로 파생된 형식에 대해 호출된 `ToTable()`은 무시되었습니다. 

**새 동작**

EF Core 3.0부터 시작하여 이후 릴리스에서 TPT 및 TPC 지원을 추가하기 위해 파생 형식에서 호출된 `ToTable()`에서는 나중에 예기치 않은 매핑 변화를 방지하기 위해 예외가 throw됩니다.

**이유**

현재 파생된 형식을 다른 테이블에 매핑하는 것은 올바르지 않습니다.
이 변경으로 인해 할 일이 유효하게 될 때 나중에 중단되는 것을 방지할 수 있습니다.

**완화 방법**

파생된 형식을 다른 테이블에 매핑하려는 시도를 제거합니다.

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a>ForSqlServerHasIndex가 HasIndex로 바뀝니다. 

[추적 문제 #12366](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 `ForSqlServerHasIndex().ForSqlServerInclude()`가 `INCLUDE`와 함께 사용되는 열을 구성하는 방법을 제공했습니다.

**새 동작**

EF Core 3.0부터 인덱스에 `Include`를 사용하여 이제 관계형 수준에서 지원됩니다.
`HasIndex().ForSqlServerInclude()`을 사용하십시오.

**이유**

이 변경으로 인해 `Includes`가 있는 인덱스의 API를 모든 데이터베이스 공급자에 대해 한 곳으로 통합할 수 있습니다.

**완화 방법**

위에 표시된 것처럼 새 API를 사용합니다.

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a>EF Core는 더 이상 SQLite FK 적용을 위한 pragma를 보내지 않습니다.

[추적 문제 #12151](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

이 변경 내용은 EF Core 3.0 미리 보기 3에 도입되었습니다.

**이전 동작**

EF Core 3.0 이전에는 EF Core가 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보냈습니다.

**새 동작**

EF Core 3.0부터 EF Core는 더 이상 SQLite에 대한 연결이 열릴 때 `PRAGMA foreign_keys = 1`을 보내지 않습니다.

**이유**

이 변경은 EF Core가 기본적으로 `SQLitePCLRaw.bundle_e_sqlite3`을 사용하기 때문에 이루어졌으며, 이는 FK 적용이 기본적으로 켜져 있고 연결이 열릴 때마다 명시적으로 활성화할 필요가 없음을 의미합니다.

**완화 방법**

외래 키는 기본적으로 EF Core에 사용되는 SQLitePCLRaw.bundle_e_sqlite3에서 기본적으로 활성화됩니다.
다른 경우에는 연결 문자열에 `Foreign Keys=True`를 지정하여 외래 키를 활성화할 수 있습니다.

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a>Microsoft.EntityFrameworkCore.Sqlite는 이제 SQLitePCLRaw.bundle_e_sqlite3에 종속됩니다.

**이전 동작**

EF Core 3.0 이전에는 EF Core가 `SQLitePCLRaw.bundle_green`을 사용했습니다.

**새 동작**

EF Core 3.0부터 EF Core가 `SQLitePCLRaw.bundle_e_sqlite3`을 사용합니다.

**이유**

이 변경으로 인해 iOS에서 사용되는 SQLite의 버전이 다른 플랫폼과 일치되도록 할 수 있습니다.

**완화 방법**

iOS에서 네이티브 SQLite 버전을 사용하려면 다른 `SQLitePCLRaw` 번들을 사용하도록 `Microsoft.Data.Sqlite`를 구성합니다.

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a>Char 값은 이제 SQLite에 텍스트로 저장됨

[추적 문제 #15020](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.

**이전 동작**

Char 값은 이전에 SQLite에 정수 값으로 저장되었습니다. 예를 들어, *A*의 char 값은 정수 값 65로 저장되었습니다.

**새 동작**

Char 값은 이제 텍스트로 저장됩니다.

**이유**

값을 텍스트로 저장하는 것이 더 자연스럽고 데이터베이스가 다른 기술과 더 잘 호환됩니다.

**완화 방법**

다음과 같이 SQL을 실행하여 기존 데이터베이스를 새 형식으로 마이그레이션할 수 있습니다.

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

EF Core에서는 이러한 속성에 값 변환기를 구성하여 이전 동작을 계속 사용할 수도 있습니다.

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

또한 Microsoft.Data.Sqlite는 정수 및 텍스트 열에서 문자 값을 읽을 수 있으므로 특정 시나리오에서는 아무런 조치도 필요하지 않을 수 있습니다.

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a>이제 마이그레이션 ID가 고정 문화권의 달력을 사용하여 생성됨

[추적 문제 #12978](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

이 변경 내용은 EF Core 3.0 미리 보기 4에 도입되었습니다.

**이전 동작**

마이그레이션 ID는 현재 문화권의 달력을 사용하여 의도치 않게 생성되었습니다.

**새 동작**

이제 마이그레이션 ID는 항상 고정 문화권의 달력(그레고리력)을 사용하여 생성됩니다.

**이유**

데이터베이스를 업데이트하거나 병합 충돌을 해결할 때 마이그레이션 순서가 중요합니다. 고정 달력을 사용하면 팀 멤버가 서로 다른 시스템 캘린더로 인해 발생할 수 있는 주문 문제를 방지할 수 있습니다.

**완화 방법**

이 변경은 그레고리력(예: 태국 불교식 달력)보다 큰 년도가 있는 그레고리력이 아닌 달력을 사용하는 사용자에게 영향을 줍니다. 기존 마이그레이션 ID를 업데이트하여 기존 마이그레이션 후 새 마이그레이션 순서를 지정해야 합니다.

마이그레이션 ID는 마이그레이션 디자이너 파일의 마이그레이션 속성에서 찾을 수 있습니다.

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

마이그레이션 기록 테이블도 업데이트해야 합니다.

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```
