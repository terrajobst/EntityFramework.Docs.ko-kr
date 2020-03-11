---
title: 공간 형식에 대 한 공급자 지원-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413896"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="74f56-102">공간 형식에 대 한 공급자 지원</span><span class="sxs-lookup"><span data-stu-id="74f56-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="74f56-103">Entity Framework DbGeography 또는 DbGeometry 클래스를 통한 공간 데이터 작업을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="74f56-104">이러한 클래스는 Entity Framework 공급자가 제공 하는 데이터베이스 관련 기능을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="74f56-105">모든 공급자가 공간 데이터를 지 원하는 것은 아니고 공간 형식 어셈블리의 설치와 같은 추가 필수 구성 요소가 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="74f56-106">공간 형식에 대 한 공급자 지원에 대 한 자세한 내용은 아래에 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="74f56-107">응용 프로그램에서 공간 형식을 사용 하는 방법에 대 한 추가 정보는 Code First, Database First 또는 Model First에 대 한 두 가지 연습에서 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="74f56-108">Code First의 공간 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="74f56-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="74f56-109">EF 디자이너의 공간 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="74f56-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="74f56-110">공간 형식을 지 원하는 EF 릴리스</span><span class="sxs-lookup"><span data-stu-id="74f56-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="74f56-111">공간 형식에 대 한 지원은 EF5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="74f56-112">그러나 EF5 공간 형식은 응용 프로그램이 .NET 4.5에서 대상으로 지정 되 고 실행 되는 경우에만 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="74f56-113">EF6 공간 형식부터 .NET 4 및 .NET 4.5를 대상으로 하는 응용 프로그램에 대해 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="74f56-114">공간 형식을 지 원하는 EF 공급자</span><span class="sxs-lookup"><span data-stu-id="74f56-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="74f56-115">EF5</span><span class="sxs-lookup"><span data-stu-id="74f56-115">EF5</span></span>  

<span data-ttu-id="74f56-116">EF5에 대 한 Entity Framework 공급자는 공간 형식을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="74f56-117">Microsoft SQL Server 공급자</span><span class="sxs-lookup"><span data-stu-id="74f56-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="74f56-118">이 공급자는 EF5의 일부로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="74f56-119">이 공급자는 설치 해야 할 수 있는 하위 수준 라이브러리를 추가로 결정 합니다. 자세한 내용은 아래를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="74f56-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="74f56-120">Oracle 용 devart dotConnect</span><span class="sxs-lookup"><span data-stu-id="74f56-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="74f56-121">Devart의 타사 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="74f56-122">공간 유형을 지 원하는 EF5 공급자를 알고 있는 경우 연락처에 문의 하세요 .이 목록에 추가 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="74f56-123">EF6</span><span class="sxs-lookup"><span data-stu-id="74f56-123">EF6</span></span>  

<span data-ttu-id="74f56-124">EF6에 대 한 Entity Framework 공급자는 공간 형식을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="74f56-125">Microsoft SQL Server 공급자</span><span class="sxs-lookup"><span data-stu-id="74f56-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="74f56-126">이 공급자는 EF6의 일부로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="74f56-127">이 공급자는 설치 해야 할 수 있는 하위 수준 라이브러리를 추가로 결정 합니다. 자세한 내용은 아래를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="74f56-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="74f56-128">Oracle 용 devart dotConnect</span><span class="sxs-lookup"><span data-stu-id="74f56-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="74f56-129">Devart의 타사 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="74f56-130">공간 유형을 지 원하는 EF6 공급자를 알고 있는 경우 연락처에 문의 하세요 .이 목록에 추가 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="74f56-131">Microsoft SQL Server를 사용 하 여 공간 형식에 대 한 필수 조건</span><span class="sxs-lookup"><span data-stu-id="74f56-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="74f56-132">SQL Server 공간 지원은 낮은 수준의 SQL Server 특정 SqlGeography 및 Sqlgeography 형식에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="74f56-133">이러한 형식은 Microsoft SqlServer. .dll 어셈블리에서 제공 되며이 어셈블리는 EF 또는 .NET Framework의 일부로 제공 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="74f56-134">Visual Studio가 설치 되 면 버전의 SQL Server도 설치 되 고, 여기에는 Microsoft. s s d. .dll을 설치 하는 작업이 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="74f56-135">공간 형식을 사용 하려는 컴퓨터에 SQL Server이 설치 되어 있지 않거나 공간 형식이 SQL Server 설치에서 제외 된 경우에는 수동으로 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="74f56-136">형식은 Microsoft SQL Server 기능 팩의 일부인 `SQLSysClrTypes.msi`를 사용 하 여 설치할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="74f56-137">공간 형식은 버전별 SQL Server 이므로 Microsoft 다운로드 센터에서 ["SQL Server 기능 팩"을 검색](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) 하 고 사용할 SQL Server 버전에 해당 하는 옵션을 선택 하 여 다운로드 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="74f56-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
