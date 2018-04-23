---
title: 기본 저장-EF 코어
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a>기본 저장

추가, 수정 및 사용자 컨텍스트 및 엔터티 클래스를 사용 하 여 데이터를 제거 하는 방법에 알아봅니다.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)을 볼 수 있습니다.

## <a name="adding-data"></a>데이터 추가

사용 하 여는 *DbSet.Add* 메서드를 엔터티 클래스의 새 인스턴스를 추가 합니다. 호출 하는 경우 데이터베이스에 삽입 될 데이터 *SaveChanges*합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> 에 설명 된 대로에 전달 되는 엔터티의 전체 그래프에 사용 하는 추가, 연결, 및 Update 메서드는 [관련 데이터](related-data.md) 섹션. 또는 단일 엔터티의 상태를 설정 하는 EntityEntry.State 속성을 사용할 수 있습니다. 예를 들어, `context.Entry(blog).State = EntityState.Modified`을 입력합니다.

## <a name="updating-data"></a>데이터 업데이트

EF는 변경 내용을 컨텍스트에 의해 추적 되는 기존 엔터티를 자동으로 검색 합니다. 여기에 부하/쿼리할 데이터베이스에서 엔터티 및 엔터티 이전에 추가 하 여 데이터베이스에 저장 됩니다.

속성에 할당 된 값을 수정 하 고 호출 *SaveChanges*합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>데이터 삭제

사용 하 여는 *DbSet.Remove* 메서드를 엔터티 클래스의 인스턴스를 삭제 합니다.

삭제 하는 동안 됩니다 엔터티가 데이터베이스에 이미 있으면 *SaveChanges*합니다. 엔터티 (예: 것은 추적 추가) 데이터베이스에 아직 저장 되지 않은 경우 컨텍스트를 제거 하 고 더 이상 됩니다 때 삽입 *SaveChanges* 호출 됩니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>단일 SaveChanges에 여러 작업

단일 호출으로 여러 개의 추가/업데이트/제거 작업을 결합할 수 *SaveChanges*합니다.

> [!NOTE]  
> 대부분의 데이터베이스 공급자에 대 한 *SaveChanges* 가 트랜잭션 큐입니다. 즉, 모든 작업이 성공 하거나 실패 하 고 작업 적용 되지 것입니다 왼쪽 부분적으로.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
