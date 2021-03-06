---
title: 관계-EF Core
description: Entity Framework Core를 사용할 때 엔터티 형식 간의 관계를 구성 하는 방법
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 6d68e813cec6c989e8e4cb848f8740489645c65c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/15/2020
ms.locfileid: "79402116"
---
# <a name="relationships"></a>관계

관계는 두 엔터티가 서로 관련 되는 방식을 정의 합니다. 관계형 데이터베이스에서이는 foreign key 제약 조건으로 표시 됩니다.

> [!NOTE]  
> 이 문서의 샘플 대부분은 일 대 다 관계를 사용 하 여 개념을 보여 줍니다. 일 대 일 및 다 대 다 관계의 예는이 문서의 끝에 있는 [다른 관계 패턴](#other-relationship-patterns) 섹션을 참조 하십시오.

## <a name="definition-of-terms"></a>용어 정의

관계를 설명 하는 데 사용 되는 용어는 여러 가지가 있습니다.

* **종속 엔터티:** 외래 키 속성을 포함 하는 엔터티입니다. 경우에 따라 관계의 ' 자식 '이 라고도 합니다.

* **주 엔터티:** 기본/대체 키 속성을 포함 하는 엔터티입니다. 경우에 따라 관계의 ' 부모 ' 라고도 합니다.

* **주 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다. 기본 키 또는 대체 키 일 수 있습니다.

* **외래 키:** 관련 엔터티의 주 키 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.

* **탐색 속성:** 관련 엔터티를 참조 하는 보안 주체 및/또는 종속 엔터티에 대해 정의 된 속성입니다.

  * **컬렉션 탐색 속성:** 여러 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **참조 탐색 속성:** 단일 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **역방향 탐색 속성:** 특정 탐색 속성을 설명할 때이 용어는 관계의 다른 쪽 end에 있는 탐색 속성을 나타냅니다.
  
* **자체 참조 관계:** 종속 엔터티와 주 엔터티 형식이 동일한 관계입니다.

다음 코드에서는 `Blog`와 간의 일 대 다 관계를 보여 줍니다 `Post`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* `Post` 종속 엔터티입니다.

* `Blog`는 주 엔터티입니다.

* `Blog.BlogId`는 주 키 (이 경우에는 대체 키가 아닌 기본 키)입니다.

* 외래 키 `Post.BlogId`

* `Post.Blog`은 참조 탐색 속성입니다.

* `Blog.Posts` 컬렉션 탐색 속성입니다.

* `Post.Blog`은 `Blog.Posts`의 역 탐색 속성 이며 그 반대의 경우도 마찬가지입니다.

## <a name="conventions"></a>규칙

기본적으로 형식에서 탐색 속성이 검색 되 면 관계가 생성 됩니다. 속성은 가리키는 형식이 현재 데이터베이스 공급자에 의해 스칼라 형식으로 매핑될 수 없는 경우 탐색 속성으로 간주 됩니다.

> [!NOTE]  
> 규칙에 따라 검색 되는 관계는 항상 주 엔터티의 기본 키를 대상으로 합니다. 대체 키를 대상으로 하려면 흐름 API를 사용 하 여 추가 구성을 수행 해야 합니다.

### <a name="fully-defined-relationships"></a>완전히 정의 된 관계

관계의 가장 일반적인 패턴은 관계의 양쪽 end에 정의 된 탐색 속성 및 종속 엔터티 클래스에 정의 된 외래 키 속성을 포함 하는 것입니다.

* 두 형식 사이에 탐색 속성 쌍이 있으면 동일한 관계의 반전 탐색 속성으로 구성 됩니다.

* 종속 엔터티에 이러한 패턴 중 하 나와 일치 하는 이름의 속성이 포함 되어 있으면 외래 키로 구성 됩니다.
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

이 예제에서는 강조 표시 된 속성을 사용 하 여 관계를 구성 합니다.

> [!NOTE]
> 속성이 기본 키 이거나 주 키와 호환 되지 않는 유형인 경우 외래 키로 구성 되지 않습니다.

> [!NOTE]
> EF Core 3.0 이전에 주 키 속성과 정확히 같은 이름의 속성이 [외래 키와 일치 했습니다](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .

### <a name="no-foreign-key-property"></a>외래 키 속성이 없습니다.

종속 엔터티 클래스에 외래 키 속성을 정의 하는 것이 좋지만 필수는 아닙니다. 외래 키 속성을 찾을 수 없는 경우에는 이름 `<navigation property name><principal key property name>` 또는 종속 형식에 탐색이 없는 경우 `<principal entity name><principal key property name>` [섀도 외래 키 속성이](shadow-properties.md) 추가 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

이 예제에서는 탐색 이름이 중복 되기 때문에 그림자 외래 키가 `BlogId` 됩니다.

> [!NOTE]
> 이름이 같은 속성이 이미 있으면 섀도 속성 이름 뒤에 숫자가 붙습니다.

### <a name="single-navigation-property"></a>단일 탐색 속성

탐색 속성을 하나만 포함 하 고 (역 탐색은 없고 외래 키 속성이 없는 경우) 규칙으로 정의 된 관계를 가질 수 있습니다. 단일 탐색 속성과 외래 키 속성을 사용할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a>제한 사항

두 형식 간에 여러 탐색 속성을 정의 하는 경우 (즉, 서로를 가리키는 탐색 쌍이 둘 이상인 경우) 탐색 속성이 나타내는 관계가 모호 합니다. 모호성을 해결 하려면 수동으로 구성 해야 합니다.

### <a name="cascade-delete"></a>계단식 삭제

규칙에 따라 cascade delete는 필수 관계의 경우 *cascade* 로 설정 되 고 선택적 관계의 경우 *clientsetnull* 로 설정 됩니다. *Cascade* 는 종속 엔터티만 삭제 됨을 의미 합니다. *Clientsetnull* 은 메모리에 로드 되지 않은 종속 엔터티가 변경 되지 않은 상태로 유지 되며 수동으로 삭제 하거나 유효한 주 엔터티를 가리키도록 업데이트 해야 함을 의미 합니다. 메모리에 로드 된 엔터티의 경우 EF Core는 외래 키 속성을 null로 설정 하려고 시도 합니다.

필수 및 선택적 관계 간의 차이점은 [필수 및 선택적 관계](#required-and-optional-relationships) 섹션을 참조 하세요.

규칙에 사용 되는 다른 삭제 동작 및 기본값에 대 한 자세한 내용은 [Cascade delete](../saving/cascade-delete.md) 를 참조 하세요.

## <a name="manual-configuration"></a>수동 구성

### <a name="fluent-api"></a>[흐름 API](#tab/fluent-api)

흐름 API에서 관계를 구성 하려면 먼저 관계를 구성 하는 탐색 속성을 식별 합니다. `HasOne` 또는 `HasMany`는 구성을 시작 하는 엔터티 형식에 대 한 탐색 속성을 식별 합니다. 그런 다음 `WithOne` 또는 `WithMany`에 대 한 호출을 연결 하 여 역 탐색을 식별 합니다. `HasOne`/`WithOne` 참조 탐색 속성에 사용 되 고 `HasMany`/`WithMany` 컬렉션 탐색 속성에 사용 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotations"></a>[데이터 주석](#tab/data-annotations)

데이터 주석을 사용 하 여 종속 및 주요 엔터티의 탐색 속성이 페어링 되는 방법을 구성할 수 있습니다. 이는 일반적으로 두 엔터티 형식 간에 둘 이상의 탐색 속성 쌍이 있는 경우에 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> 종속 엔터티의 속성에 [Required]만 사용 하 여 관계의 requiredness에 영향을 줄 수 있습니다. [필수]는 일반적으로 주 엔터티 탐색에서 무시 되지만 엔터티가 종속 항목이 되도록 할 수 있습니다.

> [!NOTE]
> 데이터 주석 `[ForeignKey]` 및 `[InverseProperty]`는 `System.ComponentModel.DataAnnotations.Schema` 네임 스페이스에서 사용할 수 있습니다. `[Required]` `System.ComponentModel.DataAnnotations` 네임 스페이스에서 사용할 수 있습니다.

---

### <a name="single-navigation-property"></a>단일 탐색 속성

탐색 속성이 하나만 있는 경우에는 `WithOne` 및 `WithMany`에 대 한 매개 변수가 없는 오버 로드가 있습니다. 이는 개념적으로 관계의 다른 쪽 끝에 참조 또는 컬렉션이 있지만 엔터티 클래스에 포함 된 탐색 속성이 없다는 것을 나타냅니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

### <a name="foreign-key"></a>외래 키

#### <a name="fluent-api-simple-key"></a>[흐름 API (단순 키)](#tab/fluent-api-simple-key)

흐름 API를 사용 하 여 지정 된 관계에 대 한 외래 키 속성으로 사용할 속성을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-key"></a>[흐름 API (복합 키)](#tab/fluent-api-composite-key)

흐름 API를 사용 하 여 지정 된 관계에 대 한 복합 외래 키 속성으로 사용할 속성을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-key"></a>[데이터 주석 (단순 키)](#tab/data-annotations-simple-key)

데이터 주석을 사용 하 여 지정 된 관계에 대 한 외래 키 속성으로 사용할 속성을 구성할 수 있습니다. 이는 일반적으로 외래 키 속성이 규칙에 의해 검색 되지 않는 경우에 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> `[ForeignKey]` 주석은 관계의 탐색 속성 중 하나에 배치할 수 있습니다. 종속 엔터티 클래스의 탐색 속성으로 이동할 필요가 없습니다.

> [!NOTE]
> 탐색 속성에서 `[ForeignKey]`를 사용 하 여 지정 된 속성은 종속 형식에 존재할 필요가 없습니다. 이 경우 지정 된 이름이 섀도 외래 키를 만드는 데 사용 됩니다.

---

#### <a name="shadow-foreign-key"></a>섀도 외래 키

`HasForeignKey(...)`의 문자열 오버 로드를 사용 하 여 그림자 속성을 외래 키로 구성할 수 있습니다. 자세한 내용은 [섀도 속성](shadow-properties.md) 을 참조 하세요. 섀도 속성을 외래 키로 사용 하기 전에이를 명시적으로 모델에 추가 하는 것이 좋습니다 (아래 참조).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a>Foreign key 제약 조건 이름

규칙에 따라 관계형 데이터베이스를 대상으로 지정 하는 경우 foreign key 제약 조건의 이름은<foreign key property name> _<principal type name>_ <dependent type name>FK_ 됩니다. 복합 외래 키의 경우에는 쉼표로 구분 된 외래 키 속성 이름 목록이 <foreign key property name> 됩니다.

제약 조건 이름을 다음과 같이 구성할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a>탐색 속성 없음

탐색 속성을 반드시 제공할 필요는 없습니다. 관계의 한쪽에 외래 키를 제공 하기만 하면 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a>주 키

외래 키가 기본 키 이외의 속성을 참조 하도록 하려는 경우 흐름 API를 사용 하 여 관계에 대 한 principal key 속성을 구성할 수 있습니다. 주 키로 구성 하는 속성은 자동으로 [대체 키](alternate-keys.md)로 설정 됩니다.

#### <a name="simple-key"></a>[단순 키](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-key"></a>[복합 키](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> 보안 주체 키 속성을 지정 하는 순서는 외래 키에 대해 지정 된 순서와 일치 해야 합니다.

---

### <a name="required-and-optional-relationships"></a>필수 및 선택적 관계

흐름 API를 사용 하 여 관계가 필수 또는 선택 사항 인지 여부를 구성할 수 있습니다. 이는 궁극적으로 외래 키 속성이 필수 또는 선택 사항 인지 여부를 제어 합니다. 이는 섀도 상태 외래 키를 사용 하는 경우에 가장 유용 합니다. 엔터티 클래스에 외래 키 속성이 있는 경우 외래 키 속성이 필수 인지 선택 사항에 따라 관계를 결정 합니다. 자세한 내용은 [필수 및 선택적 속성](required-optional.md) 을 참조 하세요.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> 또한 `IsRequired(false)`를 호출 하면 외래 키 속성이 다르게 구성 된 경우를 제외 하 고이 옵션을 선택 합니다.

### <a name="cascade-delete"></a>계단식 삭제

흐름 API를 사용 하 여 지정 된 관계에 대 한 cascade delete 동작을 명시적으로 구성할 수 있습니다.

각 옵션에 대 한 자세한 내용은 [계단식 삭제](../saving/cascade-delete.md) 를 참조 하세요.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a>기타 관계 패턴

### <a name="one-to-one"></a>일 대 일

일대일 관계에는 양쪽 모두에 대 한 참조 탐색 속성이 있습니다. 이는 일 대 다 관계와 동일한 규칙을 따르고 외래 키 속성에 고유 인덱스가 도입 되어 각 보안 주체와 하나의 종속만 관련 되도록 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> EF는 외래 키 속성을 검색 하는 기능에 따라 엔터티 중 하나를 종속 항목으로 선택 합니다. 잘못 된 엔터티가 종속 항목으로 선택 된 경우 흐름 API를 사용 하 여이를 수정할 수 있습니다.

흐름 API를 사용 하 여 관계를 구성 하는 경우 `HasOne` 및 `WithOne` 메서드를 사용 합니다.

외래 키를 구성 하는 경우 종속 엔터티 형식-아래 목록에서 `HasForeignKey`에 제공 된 제네릭 매개 변수를 지정 해야 합니다. 일 대 다 관계에서 참조 탐색을 사용 하는 엔터티가 종속 항목 이며 컬렉션이 포함 된 엔터티가 주 서버 인지를 명확 하 게 알 수 있습니다. 그러나이는 일 대 일 관계가 아니므로 명시적으로 정의 해야 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>다대다

조인 테이블을 나타내기 위해 엔터티 클래스가 없는 다 대 다 관계는 아직 지원 되지 않습니다. 그러나 조인 테이블에 대해 엔터티 클래스를 포함 하 고 두 개의 개별 일대다 관계를 매핑하여 다대다 관계를 나타낼 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
