---
title: EF Core 1.1의 새로운 기능 - EF Core
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656007"
---
# <a name="new-features-in-ef-core-11"></a>EF Core 1.1의 새로운 기능

## <a name="modeling"></a>모델링

### <a name="field-mapping"></a>필드 매핑

속성에 대한 지원 필드를 구성할 수 있습니다. 이는 속성보다는 Get/Set 메서드가 있는 데이터 또는 읽기 전용 속성에 유용할 수 있습니다.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>SQL Server의 메모리 최적화 테이블에 매핑

엔터티가 매핑된 테이블이 메모리 최적화되도록 지정할 수 있습니다. EF Core를 사용하여 모델을 기반으로 데이터베이스를 만들고 유지 관리할 경우(마이그레이션 또는 `Database.EnsureCreated()` 사용) 이러한 엔터티에 대한 메모리 최적화 테이블이 만들어집니다.

## <a name="change-tracking"></a>Change tracking

### <a name="additional-change-tracking-apis-from-ef6"></a>EF6의 추가 변경 내용 추적 API

예: `Reload`, `GetModifiedProperties`, `GetDatabaseValues` 등

## <a name="query"></a>쿼리

### <a name="explicit-loading"></a>명시적 로드

이전에 데이터베이스에서 로드된 엔터티에서 탐색 속성 채우기를 트리거할 수 있습니다.

### <a name="dbsetfind"></a>DbSet.Find

기본 키 값을 기반으로 엔터티를 쉽게 페치하는 방법을 제공합니다.

## <a name="other"></a>기타

### <a name="connection-resiliency"></a>연결 복원력

실패한 데이터베이스 명령을 자동으로 다시 시도합니다. 이 기능은 일시적인 실패가 일반적인 SQL Azure에 연결할 때 특히 유용합니다.

### <a name="simplified-service-replacement"></a>간단한 서비스 바꾸기

EF가 사용하는 내부 서비스를 더욱 쉽게 바꿀 수 있습니다.
