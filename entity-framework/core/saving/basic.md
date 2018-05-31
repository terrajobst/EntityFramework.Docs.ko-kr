---
title: 기본 저장 - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
ms.locfileid: "31006665"
---
# <a name="basic-save"></a>기본 저장

컨텍스트 및 엔터티 클래스를 사용하여 데이터를 추가, 수정 및 제거하는 방법을 알아보세요.

> [!TIP]  
> GitHub에서 이 문서의 [샘플](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/)을 볼 수 있습니다.

## <a name="adding-data"></a>데이터 추가

*DbSet.Add* 메서드를 사용하여 엔터티 클래스의 새 인스턴스를 추가하세요. *SaveChanges*를 호출할 때 데이터가 데이터베이스에 삽입됩니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Add, Attach 및 Update 메서드는 모두 [관련 데이터](related-data.md) 섹션에 설명된 대로 해당 메서드에 전달된 엔터티의 전체 그래프에서 작동합니다. 또는 EntityEntry.State 속성을 사용하여 단일 엔터티의 상태를 설정할 수 있습니다. 예를 들어, `context.Entry(blog).State = EntityState.Modified`을 입력합니다.

## <a name="updating-data"></a>데이터 업데이트

EF는 컨텍스트에서 추적하는 기존 엔터티의 변경 내용을 자동으로 검색합니다. 여기에는 데이터베이스에서 로드/쿼리하는 엔터티 및 이전에 데이터베이스에 추가되고 저장된 엔터티가 포함됩니다.

속성에 할당된 값을 수정한 후 *SaveChanges*를 호출하면 됩니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>데이터 삭제

*DbSet.Remove* 메서드를 사용하여 엔터티 클래스의 인스턴스를 삭제하세요.

엔터티가 데이터베이스에 이미 있는 경우 *SaveChanges* 중에 삭제됩니다. 엔터티가 아직 데이터베이스에 저장되지 않은 경우(예: 추가됨으로 추적되는 경우) 컨텍스트에서 제거되고 *SaveChanges*가 호출될 때 더 이상 삽입되지 않습니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>단일 SaveChanges의 여러 작업

여러 가지 추가/업데이트/제거 작업을 *SaveChanges*에 대한 단일 호출에 결합할 수 있습니다.

> [!NOTE]  
> 대부분의 데이터베이스 공급자에서 *SaveChanges*는 트랜잭션입니다. 즉, 모든 작업이 성공 또는 실패이며 작업이 부분적으로 적용된 상태로 유지되지 않음을 의미합니다.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
