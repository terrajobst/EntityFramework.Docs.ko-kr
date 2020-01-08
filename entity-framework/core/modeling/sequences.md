---
title: 시퀀스-EF Core
author: roji
ms.date: 12/18/2019
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/sequences
ms.openlocfilehash: 6343e58dd79837cc69308a070c9814030505d7dd
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502440"
---
# <a name="sequences"></a>시퀀스

> [!NOTE]  
> 시퀀스는 일반적으로 관계형 데이터베이스 에서만 지원 되는 기능입니다. Cosmos와 같은 비관계형 데이터베이스를 사용 하는 경우 고유한 값을 생성 하는 방법에 대 한 자세한 내용은 데이터베이스 설명서를 참조 하십시오.

시퀀스는 데이터베이스에서 고유한 순차 숫자 값을 생성 합니다. 시퀀스는 특정 테이블과 연결 되지 않으며, 여러 테이블을 설정 하 여 동일한 시퀀스에서 값을 그릴 수 있습니다.

## <a name="basic-usage"></a>기본 사용

모델에서 시퀀스를 설정 하 고이를 사용 하 여 속성에 대 한 값을 생성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

시퀀스에서 값을 생성 하는 데 사용 되는 특정 SQL은 데이터베이스 전용입니다. 위의 예는 SQL Server에서 작동 하지만 다른 데이터베이스에서는 실패 합니다. 자세한 내용은 특정 데이터베이스의 설명서를 참조 하세요.

## <a name="configuring-sequence-settings"></a>시퀀스 설정 구성

또한 해당 스키마, 시작 값, 증가값 등 시퀀스의 추가 측면을 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
