---
title: 대체 키-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655781"
---
# <a name="alternate-keys"></a>대체 키

대체 키는 기본 키 외에도 각 엔터티 인스턴스에 대 한 대체 고유 식별자 역할을 합니다. 대체 키를 관계의 대상으로 사용할 수 있습니다. 관계형 데이터베이스를 사용 하는 경우이는 대체 키 열에 대 한 고유 인덱스/제약 조건 개념 및 열을 참조 하는 하나 이상의 foreign key 제약 조건에 매핑됩니다.

> [!TIP]  
> 열의 고유성을 강제 적용 하려는 경우에는 대체 키가 아닌 고유 인덱스가 필요 합니다. [인덱스](indexes.md)를 참조 하십시오. EF에서 대체 키는 외래 키의 대상으로 사용할 수 있기 때문에 고유 인덱스 보다 더 많은 기능을 제공 합니다.

일반적으로 필요에 따라 대체 키가 소개 되며 수동으로 구성할 필요가 없습니다. 자세한 내용은 [규칙](#conventions) 을 참조 하세요.

## <a name="conventions"></a>규칙

규칙에 따라 기본 키가 아닌 속성을 관계의 대상으로 식별 하는 경우에 대체 키가 도입 됩니다.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a>데이터 주석

데이터 주석을 사용 하 여 대체 키를 구성할 수 없습니다.

## <a name="fluent-api"></a>흐름 API

흐름 API를 사용 하 여 단일 속성을 대체 키로 구성할 수 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

흐름 API를 사용 하 여 여러 속성을 대체 키 (복합 대체 키 라고 함)로 구성할 수도 있습니다.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
