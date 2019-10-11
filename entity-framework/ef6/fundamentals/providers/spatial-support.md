---
title: 공간 형식에 대 한 공급자 지원-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181589"
---
# <a name="provider-support-for-spatial-types"></a>공간 형식에 대 한 공급자 지원
Entity Framework DbGeography 또는 DbGeometry 클래스를 통한 공간 데이터 작업을 지원 합니다. 이러한 클래스는 Entity Framework 공급자가 제공 하는 데이터베이스 관련 기능을 사용 합니다. 모든 공급자가 공간 데이터를 지 원하는 것은 아니고 공간 형식 어셈블리의 설치와 같은 추가 필수 구성 요소가 있을 수 있습니다. 공간 형식에 대 한 공급자 지원에 대 한 자세한 내용은 아래에 나와 있습니다.  

응용 프로그램에서 공간 형식을 사용 하는 방법에 대 한 추가 정보는 Code First, Database First 또는 Model First에 대 한 두 가지 연습에서 찾을 수 있습니다.  

- [Code First의 공간 데이터 형식](~/ef6/modeling/code-first/data-types/spatial.md)  
- [EF 디자이너의 공간 데이터 형식](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a>공간 형식을 지 원하는 EF 릴리스  

공간 형식에 대 한 지원은 EF5에서 도입 되었습니다. 그러나 EF5 공간 형식은 응용 프로그램이 .NET 4.5에서 대상으로 지정 되 고 실행 되는 경우에만 지원 됩니다.  

EF6 공간 형식부터 .NET 4 및 .NET 4.5를 대상으로 하는 응용 프로그램에 대해 지원 됩니다.  

## <a name="ef-providers-that-support-spatial-types"></a>공간 형식을 지 원하는 EF 공급자  

### <a name="ef5"></a>EF5  

EF5에 대 한 Entity Framework 공급자는 공간 형식을 지원 합니다.  

- Microsoft SQL Server 공급자  
    - 이 공급자는 EF5의 일부로 제공 됩니다.  
    - 이 공급자는 설치 해야 할 수 있는 하위 수준 라이브러리를 추가로 결정 합니다. 자세한 내용은 아래를 참조 하세요.  
- [Oracle 용 devart dotConnect](https://www.devart.com/dotconnect/oracle/)  
    - Devart의 타사 공급자입니다.  

공간 유형을 지 원하는 EF5 공급자를 알고 있는 경우 연락처에 문의 하세요 .이 목록에 추가 하는 것이 좋습니다.  

### <a name="ef6"></a>EF6  

EF6에 대 한 Entity Framework 공급자는 공간 형식을 지원 합니다.  

- Microsoft SQL Server 공급자  
    - 이 공급자는 EF6의 일부로 제공 됩니다.  
    - 이 공급자는 설치 해야 할 수 있는 하위 수준 라이브러리를 추가로 결정 합니다. 자세한 내용은 아래를 참조 하세요.  
- [Oracle 용 devart dotConnect](https://www.devart.com/dotconnect/oracle/)  
    - Devart의 타사 공급자입니다.  

공간 유형을 지 원하는 EF6 공급자를 알고 있는 경우 연락처에 문의 하세요 .이 목록에 추가 하는 것이 좋습니다.  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a>Microsoft SQL Server를 사용 하 여 공간 형식에 대 한 필수 조건  

SQL Server 공간 지원은 낮은 수준의 SQL Server 특정 SqlGeography 및 Sqlgeography 형식에 따라 달라 집니다. 이러한 형식은 Microsoft SqlServer. .dll 어셈블리에서 제공 되며이 어셈블리는 EF 또는 .NET Framework의 일부로 제공 되지 않습니다.  

Visual Studio가 설치 되 면 버전의 SQL Server도 설치 되 고, 여기에는 Microsoft. s s d. .dll을 설치 하는 작업이 포함 됩니다.  

공간 형식을 사용 하려는 컴퓨터에 SQL Server이 설치 되어 있지 않거나 공간 형식이 SQL Server 설치에서 제외 된 경우에는 수동으로 설치 해야 합니다. 이 형식은 Microsoft SQL Server 기능 팩의 일부인 `SQLSysClrTypes.msi`을 사용 하 여 설치할 수 있습니다. 공간 형식은 버전별 SQL Server 이므로 Microsoft 다운로드 센터에서 ["SQL Server 기능 팩"을 검색](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) 하 고 사용할 SQL Server 버전에 해당 하는 옵션을 선택 하 여 다운로드 하는 것이 좋습니다.
