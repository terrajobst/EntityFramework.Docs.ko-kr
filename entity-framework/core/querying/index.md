---
title: 데이터 쿼리 - EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993536"
---
# <a name="querying-data"></a>데이터 쿼리

Entity Framework Core는 LINQ(Language-Integrated Query)를 사용하여 데이터베이스에서 데이터를 쿼리합니다. LINQ를 사용하면 C#(원하는 .NET 언어)을 사용하여 파생된 컨텍스트 및 엔터티 클래스를 기반으로 강력한 형식의 쿼리를 작성할 수 있습니다.  LINQ 쿼리의 표현이 데이터베이스 공급자로 전달되어 데이터베이스 특정 쿼리 언어로 변환됩니다(예: 관계형 데이터베이스의 경우 SQL). 쿼리 처리 방식에 대한 자세한 내용은 [쿼리 작동 방식](overview.md)을 참조하세요.
