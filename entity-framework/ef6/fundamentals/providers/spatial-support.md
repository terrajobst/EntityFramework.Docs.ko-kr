---
title: 공간 형식-EF6에 대 한 공급자 지원
author: divega
ms.date: 2016-10-23
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 07eeecb5f5e3e3eab8548c4c7c0ed55c5ffb4f31
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998289"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="d74cf-102">공간 형식에 대 한 공급자 지원</span><span class="sxs-lookup"><span data-stu-id="d74cf-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="d74cf-103">Entity Framework DbGeography 또는 DbGeometry 클래스를 통해 공간 데이터 사용을 지원합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="d74cf-104">이러한 클래스는 Entity Framework 공급자에서 제공 하는 데이터베이스 관련 기능을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="d74cf-105">공간 데이터를 지원 하지 않는 공급자 및 않는 공간 형식 어셈블리의 설치와 같은 추가 필수 구성 요소에 있을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="d74cf-106">공간 형식에 대 한 공급자 지원에 대 한 자세한 내용은 아래에 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="d74cf-107">Code first, Database First 또는 Model First에 대 한 다른 하나는 두 가지 연습에서 응용 프로그램에서 공간 형식을 사용 하는 방법에 대 한 추가 정보를 찾을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="d74cf-108">먼저 코드에서 공간 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="d74cf-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="d74cf-109">EF 디자이너에서 공간 데이터 형식</span><span class="sxs-lookup"><span data-stu-id="d74cf-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="d74cf-110">공간 형식을 지 원하는 EF 릴리스</span><span class="sxs-lookup"><span data-stu-id="d74cf-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="d74cf-111">공간 형식에 대 한 지원은 EF5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="d74cf-112">그러나 EF5 공간 형식만 지원 됩니다 응용 프로그램을 대상으로.NET 4.5에서 실행 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="d74cf-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="d74cf-113">.NET 4 및.NET 4.5를 대상으로 하는 응용 프로그램에 대 한 지 공간 형식을 EF6부터 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="d74cf-114">공간 형식을 지 원하는 EF 공급자</span><span class="sxs-lookup"><span data-stu-id="d74cf-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="d74cf-115">EF5</span><span class="sxs-lookup"><span data-stu-id="d74cf-115">EF5</span></span>  

<span data-ttu-id="d74cf-116">인식 하는 것의 공간 형식을 지 원하는 EF5에 대 한 Entity Framework 공급자:</span><span class="sxs-lookup"><span data-stu-id="d74cf-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="d74cf-117">Microsoft SQL Server 공급자</span><span class="sxs-lookup"><span data-stu-id="d74cf-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="d74cf-118">이 공급자는 EF5의 일부로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="d74cf-119">이 공급자를 설치 해야 할 수는 몇 가지 추가 하위 수준 라이브러리에 의존-아래 세부 정보를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="d74cf-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="d74cf-120">Devart dotConnect for Oracle</span><span class="sxs-lookup"><span data-stu-id="d74cf-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="d74cf-121">Devart의 타사 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="d74cf-122">알고 있는 경우 지 공간 형식 다음 하세요 EF5 공급자에 게 문의 하 고는 것은이 목록에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="d74cf-123">EF6</span><span class="sxs-lookup"><span data-stu-id="d74cf-123">EF6</span></span>  

<span data-ttu-id="d74cf-124">인식 하는 것의 공간 형식을 지 원하는 EF6에 대 한 Entity Framework 공급자:</span><span class="sxs-lookup"><span data-stu-id="d74cf-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="d74cf-125">Microsoft SQL Server 공급자</span><span class="sxs-lookup"><span data-stu-id="d74cf-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="d74cf-126">이 공급자는 EF6의 일부로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="d74cf-127">이 공급자를 설치 해야 할 수는 몇 가지 추가 하위 수준 라이브러리에 의존-아래 세부 정보를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="d74cf-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="d74cf-128">Devart dotConnect for Oracle</span><span class="sxs-lookup"><span data-stu-id="d74cf-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="d74cf-129">Devart의 타사 공급자입니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="d74cf-130">알고 있는 경우 지 공간 형식 다음 하세요 EF6 공급자에 게 문의 하 고는 것은이 목록에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="d74cf-131">Microsoft SQL server 공간 형식에 대 한 필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="d74cf-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="d74cf-132">SQL Server 공간 지원 SqlGeometry 및 SqlGeography 하위 수준, SQL Server 관련 형식에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="d74cf-133">이러한 형식은 Microsoft.SqlServer.Types.dll 어셈블리가 거주 하 고 EF의 일부로 또는.NET Framework의 일부로이 어셈블리를 제공 하지 않는.</span><span class="sxs-lookup"><span data-stu-id="d74cf-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="d74cf-134">Visual Studio를 설치 하면 SQL Server의 버전을 자주도 설치 됩니다 및 여기 Microsoft.SqlServer.Types.dll 설치에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="d74cf-135">공간 형식을 사용 하려는 컴퓨터에 설치 되어 있지 않으면 SQL Server 또는 SQL Server 설치의 공간 형식 제외 된 경우 수동으로 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="d74cf-136">형식을 사용 하 여 설치할 수 있습니다 `SQLSysClrTypes.msi`, Microsoft SQL Server 기능 팩의 일부인 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="d74cf-137">공간 형식 되므로 SQL Server 버전 별로 것이 좋습니다 ["SQL Server 기능 팩"에 대 한 검색](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) Microsoft 다운로드 센터에서 선택 하 고 해당 하는 옵션을 사용 하 여 SQL Server의 버전을 다운로드 합니다.</span><span class="sxs-lookup"><span data-stu-id="d74cf-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/en-us/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
