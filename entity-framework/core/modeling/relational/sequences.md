---
title: 시퀀스-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656109"
---
# <a name="sequences"></a>시퀀스

> [!NOTE]  
> 이 섹션의 구성은 일반적인 관계형 데이터베이스에 적용됩니다. 여기서 나오는 확장 메서드는 관계형 데이터베이스 공급자를 설치할 때 사용 가능할 것입니다(공유 *Microsoft.EntityFrameworkCore.Relational* 패키지 때문).

시퀀스는 데이터베이스에 순차 숫자 값을 생성 합니다. 시퀀스는 특정 테이블과 연결 되지 않습니다.

## <a name="conventions"></a>규칙

규칙에 따라 시퀀스는 모델에 추가 되지 않습니다.

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 시퀀스를 구성할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 모델에서 시퀀스를 만들 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

또한 해당 스키마, 시작 값, 증가값 등 시퀀스의 추가 측면을 구성할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

시퀀스가 도입 되 면 모델에서 속성 값을 생성 하는 데 사용할 수 있습니다. 예를 들어 [기본값](default-values.md) 을 사용 하 여 시퀀스에서 다음 값을 삽입할 수 있습니다.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
