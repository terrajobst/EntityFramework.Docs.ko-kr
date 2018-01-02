---
title: "데이터베이스 공급자 - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
ms.technology: entity-framework-core
uid: core/providers/index
ms.openlocfilehash: 19c275b7e89c62e79c8bded977e39b2cfb2b439a
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/27/2017
---
# <a name="database-providers"></a>데이터베이스 공급자

Entity Framework Core에서는 공급자 모델을 사용하여 EF가 다양한 데이터베이스에 액세스하도록 허용할 수 있습니다. 개념은 상당 부분 대부분의 데이터베이스에서 공통이며 기본 EF Core 구성 요소에 포함됩니다. 이러한 개념에는 LINQ에서의 쿼리 표현, 트랜잭션, 데이터베이스에서 로드된 후 개체에 대한 변경 내용 추적 등이 포함됩니다. 일부 개념은 특정 공급자에게만 적용됩니다. 예를 들어 SQL Server 공급자에서는 메모리 최적화 테이블을 작성할 수 있습니다(SQL Server 특정 기능). 다른 개념은 공급자 클래스에 특정합니다. 예를 들어 관계형 데이터베이스에 대한 EF Core 공급자는 공통 `Microsoft.EntityFrameworkCore.Relational` 라이브러리 위에 작성되며 테이블 및 열 매핑, 외부 키 제약 조건 등을 구성하기 위한 API를 제공합니다.

EF Core 공급자는 다양한 원본을 바탕으로 작성됩니다. 일부 공급자는 Entity Framework Core 프로젝트의 일부로 유지되지 않습니다. 타사 공급자를 고려할 때는 품질, 라이선싱, 지원 등을 평가하여 요구 사항에 부합하는지 확인합니다.