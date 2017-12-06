---
title: "전역 쿼리 필터-EF 코어"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a>전역 쿼리 필터

전역 쿼리 필터는 LINQ 쿼리 조건자 (linq 일반적으로 전달 되는 부울 식 *여기서* 쿼리 연산자) 메타 데이터 모델의 엔터티 형식에 적용 (대개 *OnModelCreating*). 이러한 필터는 엔터티 형식 또는 직접 포함을 사용 하 여 같은 간접적으로 참조 탐색 속성 참조를 포함 하 여 해당 엔터티 형식과 관련 된 모든 LINQ 쿼리를 자동으로 적용 됩니다. 이 기능의 일부 일반적인 응용 프로그램은:

* **일시 삭제** -는 엔터티 형식 정의 *IsDeleted* 속성입니다.
* **다중 테 넌 트** -는 엔터티 형식 정의 *TenantId* 속성입니다.

## <a name="example"></a>예제

다음 예제에서는 간단한 블로깅 모델의 소프트 삭제 및 다중 테 넌 트 쿼리 동작을 구현 하 전역 쿼리 필터를 사용 하는 방법을 보여 줍니다.

> [!TIP]
> 이 문서를 볼 수 있습니다 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) GitHub에서 합니다.

첫째, 엔터티를 정의 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

_의 선언을 확인_tenantId_ 필드에 _블로그_ 엔터티. 블로그 인스턴스마다 특정 테 넌 트와 연결할 사용 됩니다. 또한 정의 _IsDeleted_ 속성에는 _Post_ 엔터티 형식입니다. 이 옵션은 사용 여부를 추적 하려면이 옵션을 _Post_ 인스턴스가 "일시 삭제 된" 되었습니다. 즉 인스턴스는 기본 데이터를 제거 하는 물리적으로 삭제 된 withouth로 표시 됩니다.

그런 다음에 있는 쿼리 필터를 구성 _OnModelCreating_ 를 사용 하는 ```HasQueryFilter``` API입니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

조건자 식에 전달 되는 _HasQueryFilter_ 호출 이러한 형식에 대 한 LINQ 쿼리를 자동으로 적용 이제 됩니다.

> [!TIP]
> DbContext 인스턴스 수준 필드를 사용 하 여: ```_tenantId``` 현재 테 넌 트를 설정 하는 데 사용 합니다. 모델 수준 필터에서 올바른 컨텍스트 인스턴스 값이 사용 됩니다. 즉 쿼리를 실행 하는 인스턴스.

## <a name="disabling-filters"></a>필터를 사용 하지 않도록 설정

필터를 사용 하 여 개별 LINQ 쿼리에 대 한 비활성화 될 수 있습니다는 ```IgnoreQueryFilters()``` 연산자입니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a>제한 사항

전역 쿼리 필터는 다음과 같은 제한 사항이 있습니다.

* 필터는 탐색 속성에 대 한 참조를 포함할 수 없습니다.
* 엔터티 유형의 상속 계층 구조의 루트에 대 한 필터를 정의할 수만 있습니다.
