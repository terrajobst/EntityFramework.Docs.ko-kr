---
title: "쿼리 유형-EF 코어"
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: dfd08cd1c30debddc79740bbf05c39c22e973855
ms.sourcegitcommit: 01b5cf3b7c983bcced91e7cc4c78391ced2d2caa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/17/2018
---
# <a name="query-types"></a>쿼리 유형
> [!NOTE]
> 이 기능은 EF 코어 2.1의 새로운 기능

쿼리 유형은 EF 핵심 모델에 추가할 수 있는 읽기 전용 쿼리 결과 형식입니다. 쿼리 유형 (예: 익명 형식), 임시 쿼리를 활성화 하지만 지정 된 매핑 구성 수 있으므로 유연 합니다.

이러한은 있는 엔터티 형식에 개념적으로 유사 합니다.

- 모델에 추가 되는 C# POCO 형식이 ```OnModelCreating``` 를 사용 하 여는 ```ModelBuilder.Query``` 메서드를 또는 DbContext "설정" 속성을 통해 (쿼리는 이러한 속성 형식으로 입력 된 ```DbQuery<T>``` 대신 ```DbSet<T>```).
- 대부분의 일반 엔터티 형식으로 동일한 매핑 기능 지원 합니다. 예: 상속 매핑, 탐색 (limitiations 아래 참조) 및 관계형 저장소를 통해 대상 데이터베이스 스키마 개체를 구성 하는 기능에서 ```ToTable```, ```HasColumn``` fluent api 메서드 (또는 데이터 주석).

쿼리 유형으로는 엔터티에서 다른 것을 형식:

- 키를 정의할 수는 필요 하지 않습니다.
- 변경 추적 장치에 의해 추적 되지 됩니다.
- 규칙으로 검색 되지 않습니다.
- 특히 탐색 매핑 기능이-의 일부 지원, 관계의 주 끝으로 작동 하지 않을 수 있습니다.
- 에 매핑할 수 있습니다는 _쿼리 정의_ -A 정의 쿼리는 쿼리 유형에 대 한 데이터 소스 역할을 하는 보조 쿼리입니다.

쿼리 형식에 대 한 주요 사용 시나리오는 다음과 같습니다.

- 데이터베이스 뷰 매핑입니다.
- 기본 키가 정의 되지 않은 테이블을 매핑하고 있습니다.
- 에 대 한 반환 형식으로 처리 하는 임시 ```FromSql()``` 쿼리 합니다.
- 쿼리는 모델에 정의 된 매핑.

> [!TIP]
> 사용 하 여 이루어집니다 데이터베이스 뷰를 쿼리 형식을 매핑하는 ```ToTable``` fluent API입니다.

## <a name="example"></a>예제

다음 예제에서는 쿼리 유형을 사용 하 여 데이터베이스 뷰를 쿼리 하는 방법을 보여 줍니다.

> [!TIP]
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes)을 볼 수 있습니다.

첫째, 간단한 블로그 및 Post 모델 정의:

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

다음으로 연결 된 각 블로그 게시물의 수를 쿼리할 수 있도록 간단한 데이터베이스 뷰를 정의 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

다음으로, 데이터베이스 보기에서 결과 보관할 클래스를 정의 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

다음으로에서는 구성에서 쿼리 유형을 _OnModelCreating_ 를 사용 하 여는 ```modelBuilder.Query<T>``` API입니다.
쿼리 형식에 대 한 매핑을 구성 하려면 표준 fluent 구성 Api를 사용 합니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

마지막으로, 표준 방식으로 데이터베이스 뷰를 쿼리할 수 했습니다.

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> Note도 컨텍스트 수준 쿼리 속성 (DbQuery)이이 형식에 대 한 쿼리의 루트 역할을 정의 합니다.
