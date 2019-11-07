---
title: 관계-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e59ce9e19c12aa5564bc8467dcfcb3be8ee8996
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655666"
---
# <a name="relationships"></a>관계

관계는 두 엔터티가 서로 관련 되는 방식을 정의 합니다. 관계형 데이터베이스에서이는 foreign key 제약 조건으로 표시 됩니다.

> [!NOTE]  
> 이 문서의 샘플 대부분은 일 대 다 관계를 사용 하 여 개념을 보여 줍니다. 일 대 일 및 다 대 다 관계의 예는이 문서의 끝에 있는 [다른 관계 패턴](#other-relationship-patterns) 섹션을 참조 하십시오.

## <a name="definition-of-terms"></a>용어 정의

관계를 설명 하는 데 사용 되는 용어는 여러 가지가 있습니다.

* **종속 엔터티:** 외래 키 속성을 포함 하는 엔터티입니다. 경우에 따라 관계의 ' 자식 '이 라고도 합니다.

* **주 엔터티:** 기본/대체 키 속성을 포함 하는 엔터티입니다. 경우에 따라 관계의 ' 부모 ' 라고도 합니다.

* **외래 키:** 엔터티와 관련 된 주 키 속성의 값을 저장 하는 데 사용 되는 종속 엔터티의 속성입니다.

* **주 키:** 주 엔터티를 고유 하 게 식별 하는 속성입니다. 기본 키 또는 대체 키 일 수 있습니다.

* **탐색 속성:** 관련 엔터티에 대 한 참조를 포함 하는 보안 주체 및/또는 종속 엔터티에 대해 정의 된 속성입니다.

  * **컬렉션 탐색 속성:** 여러 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **참조 탐색 속성:** 단일 관련 엔터티에 대 한 참조를 포함 하는 탐색 속성입니다.

  * **역방향 탐색 속성:** 특정 탐색 속성을 설명할 때이 용어는 관계의 다른 쪽 end에 있는 탐색 속성을 나타냅니다.

다음 코드 목록에서는 `Blog`와 간의 일 대 다 관계를 보여 줍니다 `Post`

* `Post` 종속 엔터티입니다.

* `Blog`는 주 엔터티입니다.

* 외래 키 `Post.BlogId`

* `Blog.BlogId`는 주 키 (이 경우에는 대체 키가 아닌 기본 키)입니다.

* `Post.Blog`은 참조 탐색 속성입니다.

* `Blog.Posts` 컬렉션 탐색 속성입니다.

* `Post.Blog`은 `Blog.Posts`의 역 탐색 속성 이며 그 반대의 경우도 마찬가지입니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a>규칙

규칙에 따라 형식에서 탐색 속성이 검색 되 면 관계가 생성 됩니다. 속성은 가리키는 형식이 현재 데이터베이스 공급자에 의해 스칼라 형식으로 매핑될 수 없는 경우 탐색 속성으로 간주 됩니다.

> [!NOTE]  
> 규칙에 따라 검색 되는 관계는 항상 주 엔터티의 기본 키를 대상으로 합니다. 대체 키를 대상으로 하려면 흐름 API를 사용 하 여 추가 구성을 수행 해야 합니다.

### <a name="fully-defined-relationships"></a>완전히 정의 된 관계

관계의 가장 일반적인 패턴은 관계의 양쪽 end에 정의 된 탐색 속성 및 종속 엔터티 클래스에 정의 된 외래 키 속성을 포함 하는 것입니다.

* 두 형식 사이에 탐색 속성 쌍이 있으면 동일한 관계의 반전 탐색 속성으로 구성 됩니다.

* 종속 엔터티에 `<primary key property name>`, `<navigation property name><primary key property name>`또는 `<principal entity name><primary key property name>` 라는 속성이 포함 되어 있으면 외래 키로 구성 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> 두 형식 간에 여러 탐색 속성을 정의 하는 경우 (즉, 서로를 가리키는 여러 개의 고유 탐색 쌍) 규칙에 따라 관계가 생성 되지 않으므로 사용자가이를 수동으로 구성 하 여 탐색 속성이 페어링 됩니다.

### <a name="no-foreign-key-property"></a>외래 키 속성이 없습니다.

종속 엔터티 클래스에 외래 키 속성을 정의 하는 것이 좋지만 필수는 아닙니다. 외래 키 속성이 없는 경우 `<navigation property name><principal key property name>` 이름으로 섀도 외래 키 속성이 도입 됩니다 (자세한 내용은 [섀도 속성](shadow-properties.md) 참조).

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a>단일 탐색 속성

탐색 속성을 하나만 포함 하 고 (역 탐색은 없고 외래 키 속성이 없는 경우) 규칙으로 정의 된 관계를 가질 수 있습니다. 단일 탐색 속성과 외래 키 속성을 사용할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a>하위 삭제

규칙에 따라 cascade delete는 필수 관계의 경우 *cascade* 로 설정 되 고 선택적 관계의 경우 *clientsetnull* 로 설정 됩니다. *Cascade* 는 종속 엔터티만 삭제 됨을 의미 합니다. *Clientsetnull* 은 메모리에 로드 되지 않은 종속 엔터티가 변경 되지 않은 상태로 유지 되며 수동으로 삭제 하거나 유효한 주 엔터티를 가리키도록 업데이트 해야 함을 의미 합니다. 메모리에 로드 된 엔터티의 경우 EF Core는 외래 키 속성을 null로 설정 하려고 시도 합니다.

필수 및 선택적 관계 간의 차이점은 [필수 및 선택적 관계](#required-and-optional-relationships) 섹션을 참조 하세요.

규칙에 사용 되는 다른 삭제 동작 및 기본값에 대 한 자세한 내용은 [Cascade delete](../saving/cascade-delete.md) 를 참조 하세요.

## <a name="data-annotations"></a>데이터 주석

관계, `[ForeignKey]` 및 `[InverseProperty]`를 구성 하는 데 사용할 수 있는 두 가지 데이터 주석이 있습니다. 이러한 기능은 `System.ComponentModel.DataAnnotations.Schema` 네임 스페이스에서 사용할 수 있습니다.

### <a name="foreignkey"></a>[ForeignKey]

데이터 주석을 사용 하 여 지정 된 관계에 대 한 외래 키 속성으로 사용할 속성을 구성할 수 있습니다. 이는 일반적으로 외래 키 속성이 규칙에 의해 검색 되지 않는 경우에 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> `[ForeignKey]` 주석은 관계의 탐색 속성 중 하나에 배치할 수 있습니다. 종속 엔터티 클래스의 탐색 속성으로 이동할 필요가 없습니다.

### <a name="inverseproperty"></a>[InverseProperty]

데이터 주석을 사용 하 여 종속 및 주요 엔터티의 탐색 속성이 페어링 되는 방법을 구성할 수 있습니다. 이는 일반적으로 두 엔터티 형식 간에 둘 이상의 탐색 속성 쌍이 있는 경우에 수행 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a>흐름 API

흐름 API에서 관계를 구성 하려면 먼저 관계를 구성 하는 탐색 속성을 식별 합니다. `HasOne` 또는 `HasMany`는 구성을 시작 하는 엔터티 형식에 대 한 탐색 속성을 식별 합니다. 그런 다음 `WithOne` 또는 `WithMany`에 대 한 호출을 연결 하 여 역 탐색을 식별 합니다. `HasOne`/`WithOne` 참조 탐색 속성에 사용 되 고 `HasMany`/`WithMany` 컬렉션 탐색 속성에 사용 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a>단일 탐색 속성

탐색 속성이 하나만 있는 경우에는 `WithOne` 및 `WithMany`에 대 한 매개 변수가 없는 오버 로드가 있습니다. 이는 개념적으로 관계의 다른 쪽 끝에 참조 또는 컬렉션이 있지만 엔터티 클래스에 포함 된 탐색 속성이 없다는 것을 나타냅니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a>외래 키

흐름 API를 사용 하 여 지정 된 관계에 대 한 외래 키 속성으로 사용할 속성을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

다음 코드 목록에서는 복합 외래 키를 구성 하는 방법을 보여 줍니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

`HasForeignKey(...)`의 문자열 오버 로드를 사용 하 여 그림자 속성을 외래 키로 구성할 수 있습니다. 자세한 내용은 [섀도 속성](shadow-properties.md) 을 참조 하세요. 섀도 속성을 외래 키로 사용 하기 전에이를 명시적으로 모델에 추가 하는 것이 좋습니다 (아래 참조).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a>탐색 속성 없음

탐색 속성을 반드시 제공할 필요는 없습니다. 관계의 한쪽에 외래 키를 제공 하기만 하면 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a>주 키

외래 키가 기본 키 이외의 속성을 참조 하도록 하려는 경우 흐름 API를 사용 하 여 관계에 대 한 principal key 속성을 구성할 수 있습니다. 주 키로 구성 하는 속성은 대체 키로 자동으로 설정 됩니다 (자세한 내용은 [대체 키](alternate-keys.md) 참조).

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

다음 코드 목록에서는 복합 주체 키를 구성 하는 방법을 보여 줍니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> 보안 주체 키 속성을 지정 하는 순서는 외래 키에 대해 지정 된 순서와 일치 해야 합니다.

### <a name="required-and-optional-relationships"></a>필수 및 선택적 관계

흐름 API를 사용 하 여 관계가 필수 또는 선택 사항 인지 여부를 구성할 수 있습니다. 이는 궁극적으로 외래 키 속성이 필수 또는 선택 사항 인지 여부를 제어 합니다. 이는 섀도 상태 외래 키를 사용 하는 경우에 가장 유용 합니다. 엔터티 클래스에 외래 키 속성이 있는 경우 외래 키 속성이 필수 인지 선택 사항에 따라 관계를 결정 합니다. 자세한 내용은 [필수 및 선택적 속성](required-optional.md) 을 참조 하세요.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

### <a name="cascade-delete"></a>하위 삭제

흐름 API를 사용 하 여 지정 된 관계에 대 한 cascade delete 동작을 명시적으로 구성할 수 있습니다.

각 옵션에 대 한 자세한 내용은 데이터 저장 섹션의 [계단식 삭제](../saving/cascade-delete.md) 를 참조 하세요.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a>기타 관계 패턴

### <a name="one-to-one"></a>일대일

일대일 관계에는 양쪽 모두에 대 한 참조 탐색 속성이 있습니다. 이는 일 대 다 관계와 동일한 규칙을 따르고 외래 키 속성에 고유 인덱스가 도입 되어 각 보안 주체와 하나의 종속만 관련 되도록 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> EF는 외래 키 속성을 검색 하는 기능에 따라 엔터티 중 하나를 종속 항목으로 선택 합니다. 잘못 된 엔터티가 종속 항목으로 선택 된 경우 흐름 API를 사용 하 여이를 수정할 수 있습니다.

흐름 API를 사용 하 여 관계를 구성 하는 경우 `HasOne` 및 `WithOne` 메서드를 사용 합니다.

외래 키를 구성 하는 경우 종속 엔터티 형식-아래 목록에서 `HasForeignKey`에 제공 된 제네릭 매개 변수를 지정 해야 합니다. 일 대 다 관계에서 참조 탐색을 사용 하는 엔터티가 종속 항목 이며 컬렉션이 포함 된 엔터티가 주 서버 인지를 명확 하 게 알 수 있습니다. 그러나이는 일 대 일 관계가 아니므로 명시적으로 정의 해야 합니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a>다 대 다

조인 테이블을 나타내기 위해 엔터티 클래스가 없는 다 대 다 관계는 아직 지원 되지 않습니다. 그러나 조인 테이블에 대해 엔터티 클래스를 포함 하 고 두 개의 개별 일대다 관계를 매핑하여 다대다 관계를 나타낼 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
