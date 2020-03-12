---
title: 클라이언트 및 서버 평가 - EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 8b6697cc-7067-4dc2-8007-85d80503d123
uid: core/querying/client-eval
ms.openlocfilehash: e01bd146c4dfe7a8d36b641cb52ae366fddd8239
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413788"
---
# <a name="client-vs-server-evaluation"></a>클라이언트 및 서버 평가

일반적으로 Entity Framework Core는 서버에서 쿼리를 가능한 한 많이 평가하려고 시도합니다. EF Core는 쿼리 부분들을 클라이언트 쪽에서 평가할 수 있는 매개 변수로 변환합니다. 쿼리의 나머지 부분(과 생성된 매개 변수)은 서버에서 평가할 동등한 데이터베이스 쿼리를 확인할 수 있도록 데이터베이스 공급자에게 제공됩니다. EF Core는 최상위 프로젝션(`Select()`에 대한 마지막 호출)에서 부분적인 클라이언트 평가를 지원합니다. 쿼리의 최상위 프로젝션이 서버로 변환될 수 없는 경우, EF Core는 서버에서 필요한 데이터를 가져와서 쿼리의 나머지 부분을 클라이언트에서 평가합니다. EF Core가 최상위 프로젝션이 아닌 다른 모든 곳에서 서버로 변환할 수 없는 식을 발견하면 런타임 예외를 throw합니다. EF Core가 서버로 변환될 수 없는 항목을 판단하는 방식은 [쿼리가 작동하는 방법](xref:core/querying/how-query-works)을 참조하세요.

> [!NOTE]
> 버전 3.0 전까지는 Entity Framework Core가 쿼리의 모든 곳에서 클라이언트 평가를 지원했습니다. 자세한 내용은 [이전 버전 섹션](#previous-versions)을 참조하세요.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying)을 볼 수 있습니다.

## <a name="client-evaluation-in-the-top-level-projection"></a>최상위 프로젝션에서의 클라이언트 평가

다음 예제에서는 도우미 메서드가 SQL Server 데이터베이스에 반환되는 블로그의 URL을 표준화하는 데 사용됩니다. SQL Server 공급자는 이 메서드가 구현되는 방법에 대한 인사이트가 없기 때문에 이 메서드를 SQL로 변환할 수 없습니다. 쿼리의 다른 모든 측면은 데이터베이스에서 평가되지만 이 메서드를 통해 반환된 `URL`을 전달하는 작업은 클라이언트에서 수행됩니다.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientProjection)]

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientMethod)]

## <a name="unsupported-client-evaluation"></a>지원되지 않는 클라이언트 평가

클라이언트 평가는 유용하긴 하나 때로 성능 저하를 유발할 수 있습니다. 도우미 메서드가 where 필터에서 사용되는 다음 쿼리를 살펴보겠습니다. 필터를 데이터베이스에 적용할 수 없으므로 클라이언트에 필터를 적용하려면 모든 데이터를 메모리로 가져와야 합니다. 필터와 서버에 있는 데이터의 양에 따라 클라이언트 평가는 성능 저하를 유발할 수 있습니다. 따라서 Entity Framework Core는 이러한 클라이언트 평가를 차단하고 런타임 예외를 throw합니다.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ClientWhere)]

## <a name="explicit-client-evaluation"></a>명시적 클라이언트 평가

다음과 같은 특정 경우에는 클라이언트 평가를 명시적으로 강제해야 할 수 있습니다.

- 데이터의 양이 적어서 클라이언트에서 평가하는 것이 대단한 성능 저하를 유발하지 않는 경우.
- 사용되는 LINQ 연산자의 서버 쪽 변환이 없는 경우.

위와 같은 경우에는 `AsEnumerable` 또는 `ToList`(비동기의 경우 `AsAsyncEnumerable` 또는 `ToListAsync`)와 같은 메서드를 호출하여 명시적으로 클라이언트 평가에 옵트인할 수 있습니다. `AsEnumerable`을 사용하면 결과를 스트리밍하게 되는 반면 `ToList`를 사용하면 목록을 생성하여 버퍼를 유발하므로 이로 인해 추가 메모리가 사용됩니다. 단, 여러 번 열거할 경우에는 결과를 목록에 저장하는 것이 더 낫습니다. 데이터베이스에 대한 쿼리가 하나밖에 없기 때문입니다. 구체적인 용도에 따라 어느 메서드가 더 유용한지 판단해야 합니다.

[!code-csharp[Main](../../../samples/core/Querying/ClientEval/Sample.cs#ExplicitClientEval)]

## <a name="potential-memory-leak-in-client-evaluation"></a>클라이언트 평가의 잠재적인 메모리 누수

쿼리 변환과 컴파일에는 비용이 많이 들기 때문에 EF Core는 컴파일된 쿼리 계획을 캐시에 저장합니다. 캐시된 대리자는 최상위 프로젝션의 클라이언트 평가를 수행할 때 클라이언트 코드를 사용할 수 있습니다. EF Core는 트리에서 클라이언트 평가된 부분에 대한 매개 변수를 생성한 다음 매개 변수 값을 대체하여 쿼리 계획을 재사용합니다. 그러나 식 트리의 특정 상수는 매개 변수로 변환할 수 없습니다. 캐시된 대리자가 이러한 상수를 포함하는 경우, 해당 개체는 여전히 참조되고 있으므로 가비지 수집될 수 없습니다. 이러한 개체가 DbContext 또는 기타 서비스를 포함할 경우, 이로 인해 시간이 흐름에 따라 앱의 메모리 사용량이 늘어날 수 있습니다. 이 동작은 일반적으로 메모리 누수의 신호가 됩니다. EF Core는 현재 데이터베이스 공급자를 사용하여 매핑할 수 없는 유형의 상수를 발견할 때마다 예외를 throw합니다. 일반적인 원인과 해결 방법은 다음과 같습니다.

- **인스턴스 메서드 사용**: 클라이언트 프로젝션에서 인스턴스 메서드를 사용하는 경우, 식 트리는 인스턴스의 상수를 포함합니다. 메서드가 인스턴스의 데이터를 사용하지 않는다면 메서드를 정적으로 만드는 방법을 고려하세요. 메서드 본문에 인스턴스 데이터가 필요하다면 해당 데이터를 메서드에 인수로 전달하세요.
- **메서드에 상수 인수 전달**: 이 경우는 일반적으로 클라이언트 메서드에 대한 인수에서 `this`를 사용할 때 발생합니다. 인수를 데이터베이스 공급자에 의해 매핑될 수 있는 여러 개의 스칼라 인수로 분할하는 방안을 고려하세요.
- **다른 상수**: 그 밖의 다른 경우에 상수가 발견될 경우, 해당 상수가 처리에 필요한지 여부를 평가할 수 있습니다. 상수를 가지고 있어야 하거나 위 경우의 해결 방법을 사용할 수 없다면 값을 저장할 지역 변수를 만든 다음 쿼리에서 이 지역 변수를 사용하세요. EF Core가 지역 변수를 매개 변수로 변환합니다.

## <a name="previous-versions"></a>이전 버전

다음 섹션은 3.0 이전의 EF Core 버전에 적용됩니다.

이전의 EF Core 버전에서는 최상위 프로젝션만이 아닌 쿼리의 모든 부분에서 클라이언트 평가를 지원합니다. 따라서 [지원되지 않는 클라이언트 평가](#unsupported-client-evaluation) 섹션에 있는 것과 비슷한 쿼리가 올바르게 작동했던 것입니다. 이 동작은 발견되지 않은 성능 문제를 야기할 수 있기 때문에 EF Core는 클라이언트 평가 경고를 기록했습니다. 로그 출력을 보는 방법은 [로깅](xref:core/miscellaneous/logging)을 참조하세요.

선택적으로, EF Core에서 클라이언트 평가를 수행할 때(프로젝션에서의 수행은 제외) 기본 동작을 예외 throw하거나 아무것도 하지 않음으로 변경할 수 있었습니다. 예외 throw 동작을 선택하면 3.0에서의 동작과 비슷해집니다. 동작을 변경하려면 컨텍스트의 옵션을 설정할 때 `DbContext.OnConfiguring`에서(ASP.NET Core를 사용하는 경우에는 `Startup.cs`에서) 경고를 구성해야 합니다.

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFQuerying;Trusted_Connection=True;")
        .ConfigureWarnings(warnings => warnings.Throw(RelationalEventId.QueryClientEvaluationWarning));
}
```
