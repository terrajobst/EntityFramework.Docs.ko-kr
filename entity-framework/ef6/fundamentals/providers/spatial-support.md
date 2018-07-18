---
title: 공간 형식-EF6에 대 한 공급자 지원
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
caps.latest.revision: 3
ms.openlocfilehash: 76020e2a3127b1026a5cb8f032686cc8ce9c0c5f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121893"
---
# <a name="provider-support-for-spatial-types"></a>공간 형식에 대 한 공급자 지원
Entity Framework DbGeography 또는 DbGeometry 클래스를 통해 공간 데이터 사용을 지원합니다. 이러한 클래스는 Entity Framework 공급자에서 제공 하는 데이터베이스 관련 기능을 사용 합니다. 공간 데이터를 지원 하지 않는 공급자 및 않는 공간 형식 어셈블리의 설치와 같은 추가 필수 구성 요소에 있을 수 있습니다. 공간 형식에 대 한 공급자 지원에 대 한 자세한 내용은 아래에 제공 됩니다.  

Code first, Database First 또는 Model First에 대 한 다른 하나는 두 가지 연습에서 응용 프로그램에서 공간 형식을 사용 하는 방법에 대 한 추가 정보를 찾을 수 있습니다.  

- [먼저 코드에서 공간 데이터 형식](~/ef6/modeling/code-first/data-types/spatial.md)  
- [EF 디자이너에서 공간 데이터 형식](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>공간 형식을 지 원하는 EF 릴리스  

공간 형식에 대 한 지원은 EF5에서 도입 되었습니다. 그러나 EF5 공간 형식만 지원 됩니다 응용 프로그램을 대상으로.NET 4.5에서 실행 하는 경우.  

.NET 4 및.NET 4.5를 대상으로 하는 응용 프로그램에 대 한 지 공간 형식을 EF6부터 시작 합니다.  

## <a name="ef-providers-that-support-spatial-types"></a>공간 형식을 지 원하는 EF 공급자  

### <a name="ef5"></a>EF5  

인식 하는 것의 공간 형식을 지 원하는 EF5에 대 한 Entity Framework 공급자:  

- Microsoft SQL Server 공급자  
    - 이 공급자는 EF5의 일부로 제공 됩니다.  
    - 이 공급자를 설치 해야 할 수는 몇 가지 추가 하위 수준 라이브러리에 의존-아래 세부 정보를 참조 하세요.  
- [Devart dotConnect for Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Devart의 타사 공급자입니다.  

알고 있는 경우 지 공간 형식 다음 하세요 EF5 공급자에 게 문의 하 고는 것은이 목록에 추가 합니다.  

### <a name="ef6"></a>EF6  

인식 하는 것의 공간 형식을 지 원하는 EF6에 대 한 Entity Framework 공급자:  

- Microsoft SQL Server 공급자  
    - 이 공급자는 EF6의 일부로 제공 됩니다.  
    - 이 공급자를 설치 해야 할 수는 몇 가지 추가 하위 수준 라이브러리에 의존-아래 세부 정보를 참조 하세요.  
- [Devart dotConnect for Oracle](http://www.devart.com/dotconnect/oracle/)  
    - Devart의 타사 공급자입니다.  

알고 있는 경우 지 공간 형식 다음 하세요 EF6 공급자에 게 문의 하 고는 것은이 목록에 추가 합니다.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Microsoft SQL server 공간 형식에 대 한 필수 구성 요소  

SQL Server 공간 지원 SqlGeometry 및 SqlGeography 하위 수준, SQL Server 관련 형식에 따라 달라 집니다. 이러한 형식은 Microsoft.SqlServer.Types.dll 어셈블리가 거주 하 고 EF의 일부로 또는.NET Framework의 일부로이 어셈블리를 제공 하지 않는.  

Visual Studio를 설치 하면 SQL Server의 버전을 자주도 설치 됩니다 및 여기 Microsoft.SqlServer.Types.dll 설치에 포함 됩니다.  

공간 형식을 사용 하려는 컴퓨터에 설치 되어 있지 않으면 SQL Server 또는 SQL Server 설치의 공간 형식 제외 된 경우 수동으로 설치 해야 합니다. 형식을 사용 하 여 설치할 수 있습니다 `SQLSysClrTypes.msi`, Microsoft SQL Server 기능 팩의 일부인 합니다. 공간 형식 되므로 SQL Server 버전 별로 것이 좋습니다 ["SQL Server 기능 팩"에 대 한 검색](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) Microsoft 다운로드 센터에서 선택 하 고 해당 하는 옵션을 사용 하 여 SQL Server의 버전을 다운로드 합니다.
