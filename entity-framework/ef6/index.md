---
title: 개요 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 1efadf4484a13d5df2a2f11aad3d0e8f9ceff543
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315635"
---
# <a name="entity-framework-6"></a><span data-ttu-id="ececc-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="ececc-102">Entity Framework 6</span></span>
<span data-ttu-id="ececc-103">EF6(Entity Framework 6)는 수년에 걸친 기능 개발 및 안정화 과정을 통해 테스트를 거친 .NET용 O/RM(개체 관계형 매퍼)입니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="ececc-104">O/RM으로써 EF6는 관계형 데이터베이스와 개체 중심 데이터베이스 사이의 불일치를 완화하고, 개발자가 응용 프로그램의 도메인을 나타내는 강력한 형식의 .NET 개체를 사용하여 관계형 데이터베이스에 저장된 데이터와 상호 작용할 수 있게 해주고, 일반적으로 개발자가 작성해야 하는 데이터 액세스 "내부" 코드의 많은 부분을 할 필요가 없게 만들어 줍니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="ececc-105">EF6는 다양한 인기 O/RM 기능을 구현합니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="ececc-106">EF 형식에 따라 달라지지 않는 [POCO](~/ef6/resources/glossary.md#poco) 엔터티 클래스 매핑</span><span class="sxs-lookup"><span data-stu-id="ececc-106">Mapping of [POCO](~/ef6/resources/glossary.md#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="ececc-107">자동 변경 내용 추적</span><span class="sxs-lookup"><span data-stu-id="ececc-107">Automatic change tracking</span></span>
- <span data-ttu-id="ececc-108">ID 확인 및 작업 단위</span><span class="sxs-lookup"><span data-stu-id="ececc-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="ececc-109">즉시 로드, 지연 로드 및 명시적 로드</span><span class="sxs-lookup"><span data-stu-id="ececc-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="ececc-110">LINQ(Language-Integrated Query)를 사용한 강력한 형식의 쿼리 변환</span><span class="sxs-lookup"><span data-stu-id="ececc-110">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span>
- <span data-ttu-id="ececc-111">다음 지원을 포함한 풍부한 매핑 기능:</span><span class="sxs-lookup"><span data-stu-id="ececc-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="ececc-112">일대일, 일대다 및 다대다 관계</span><span class="sxs-lookup"><span data-stu-id="ececc-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="ececc-113">상속(계층 구조별 테이블, 형식별 테이블, 구체적인 클래스별 테이블)</span><span class="sxs-lookup"><span data-stu-id="ececc-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="ececc-114">복합 형식</span><span class="sxs-lookup"><span data-stu-id="ececc-114">Complex types</span></span>
  - <span data-ttu-id="ececc-115">저장 프로시저</span><span class="sxs-lookup"><span data-stu-id="ececc-115">Stored procedures</span></span>
- <span data-ttu-id="ececc-116">엔터티 모델을 만드는 시각적 디자이너.</span><span class="sxs-lookup"><span data-stu-id="ececc-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="ececc-117">코드를 작성하여 엔터티 모델을 만드는 "Code First" 환경</span><span class="sxs-lookup"><span data-stu-id="ececc-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="ececc-118">기존 데이터베이스에서 모델을 생성한 후 직접 편집할 수도 있고, 처음부터 새로 만든 후 새 데이터베이스를 생성하는 데 사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="ececc-119">ASP.NET을 포함한 .NET Framework 응용 프로그램 모델과 통합, 데이터 바인딩을 통해 WPF 및 WinForms와 통합.</span><span class="sxs-lookup"><span data-stu-id="ececc-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="ececc-120">ADO.NET 및 SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 등에 연결할 수 있는 다양한 공급자를 기반으로 하는 데이터베이스 연결.</span><span class="sxs-lookup"><span data-stu-id="ececc-120">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="ececc-121">EF6 또는 EF Core를 사용해야 하나요?</span><span class="sxs-lookup"><span data-stu-id="ececc-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="ececc-122">EF Core는 가볍고 확장 가능한 최신 버전의 Entity Framework로, EF6와 매우 비슷한 기능을 제공합니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="ececc-123">EF Core는 완전히 다시 작성되었으며, EF6의 고급 매핑 기능 중 일부를 제공하지 않지만 EF6에 없는 여러 새 기능을 포함하고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="ececc-124">기능 집합이 요구 사항과 일치하는 경우 새 응용 프로그램에서 EF Core를 사용해 보세요.</span><span class="sxs-lookup"><span data-stu-id="ececc-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="ececc-125">[EF Core & EF6 비교](xref:efcore-and-ef6/index)에서는 이 선택에 대해 자세히 살펴봅니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="ececc-126">시작</span><span class="sxs-lookup"><span data-stu-id="ececc-126">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="ececc-127">프로젝트에 EntityFramework NuGet 패키지를 추가하거나 Visual Studio용 Entity Framework Tools를 설치합니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-127">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="ececc-128">그런 다음, EF6를 최대한 활용하는 방법을 알려주는 비디오를 시청하고, 자습서 및 고급 설명서를 읽습니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="ececc-129">이전 Entity Framework 버전</span><span class="sxs-lookup"><span data-stu-id="ececc-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="ececc-130">Entity Framework 6 최신 버전에 대한 설명서지만, 많은 부분이 이전 릴리스에도 적용됩니다.</span><span class="sxs-lookup"><span data-stu-id="ececc-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="ececc-131">EF 릴리스 및 포함된 기능의 전체 목록은 [새로운 기능](~/ef6/what-is-new/index.md) 및 [이전 릴리스](~/ef6/what-is-new/past-releases.md)를 확인하세요.</span><span class="sxs-lookup"><span data-stu-id="ececc-131">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
