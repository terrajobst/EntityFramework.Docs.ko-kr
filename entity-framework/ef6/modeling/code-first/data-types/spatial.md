---
title: 공간-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182658"
---
# <a name="spatial---code-first"></a><span data-ttu-id="2ec33-102">공간-Code First</span><span class="sxs-lookup"><span data-stu-id="2ec33-102">Spatial - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="2ec33-103">**EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="2ec33-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="2ec33-105">비디오 및 단계별 연습에서는 Entity Framework Code First를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-105">The video and step-by-step walkthrough shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="2ec33-106">LINQ 쿼리를 사용 하 여 두 위치 간의 거리를 찾는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="2ec33-107">이 연습에서는 Code First 사용 하 여 새 데이터베이스를 만들지만 [기존 데이터베이스에 Code First를](~/ef6/modeling/code-first/workflows/existing-database.md)사용할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="2ec33-108">공간 형식 지원은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="2ec33-109">공간 형식, 열거형 및 테이블 반환 함수와 같은 새 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="2ec33-110">Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="2ec33-111">공간 데이터 형식을 사용 하려면 공간이 지원 되는 Entity Framework 공급자도 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="2ec33-112">자세한 내용은 [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="2ec33-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="2ec33-113">두 가지 주요 공간 데이터 형식인 geography 및 geometry가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="2ec33-114">Geography 데이터 형식은 타원 데이터 (예: GPS 위도 및 경도 좌표)를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="2ec33-115">Geometry 데이터 형식은 유클리드 (평면) 좌표계를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="2ec33-116">비디오 시청</span><span class="sxs-lookup"><span data-stu-id="2ec33-116">Watch the video</span></span>
<span data-ttu-id="2ec33-117">이 비디오는 Entity Framework Code First를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-117">This video shows how to map spatial types with Entity Framework Code First.</span></span> <span data-ttu-id="2ec33-118">LINQ 쿼리를 사용 하 여 두 위치 간의 거리를 찾는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="2ec33-119">제공한 **사람**: 줄리아 Kornich</span><span class="sxs-lookup"><span data-stu-id="2ec33-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="2ec33-120">**비디오**: [wmv](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="2ec33-120">**Video**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="2ec33-121">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="2ec33-121">Pre-Requisites</span></span>

<span data-ttu-id="2ec33-122">이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="2ec33-123">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="2ec33-123">Set up the Project</span></span>

1.  <span data-ttu-id="2ec33-124">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="2ec33-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="2ec33-125">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="2ec33-126">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="2ec33-127">프로젝트 이름으로 **SpatialCodeFirst** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-127">Enter **SpatialCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="2ec33-128">Code First를 사용 하 여 새 모델 정의</span><span class="sxs-lookup"><span data-stu-id="2ec33-128">Define a New Model using Code First</span></span>

<span data-ttu-id="2ec33-129">Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-129">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="2ec33-130">아래 코드는 대학 클래스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-130">The code below defines the University class.</span></span>

<span data-ttu-id="2ec33-131">대학에는 DbGeography 형식의 Location 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-131">The University has the Location property of the DbGeography type.</span></span> <span data-ttu-id="2ec33-132">DbGeography 형식을 사용 하려면 system.xml 어셈블리에 참조를 추가 하 고 문을 사용 하 여 system.string을 추가 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-132">To use the DbGeography type, you must add a reference to the System.Data.Entity assembly and also add the System.Data.Spatial using statement.</span></span>

<span data-ttu-id="2ec33-133">Program.cs 파일을 열고 다음 using 문을 파일 맨 위에 붙여넣습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-133">Open the Program.cs file and paste the following using statements at the top of the file:</span></span>

``` csharp
using System.Data.Spatial;
```

<span data-ttu-id="2ec33-134">Program.cs 파일에 다음 대학 클래스 정의를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-134">Add the following University class definition to the Program.cs file.</span></span>

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="2ec33-135">DbContext 파생 형식 정의</span><span class="sxs-lookup"><span data-stu-id="2ec33-135">Define the DbContext Derived Type</span></span>

<span data-ttu-id="2ec33-136">엔터티를 정의 하는 것 외에도 DbContext에서 파생 되 고 DbSet&lt;&gt; 속성을 노출 하는 클래스를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-136">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="2ec33-137">DbSet&lt;&gt; 속성을 사용 하면 컨텍스트에서 모델에 포함 하려는 형식을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-137">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="2ec33-138">DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-138">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="2ec33-139">DbContext 및 DbSet 형식은 EntityFramework 어셈블리에서 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-139">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="2ec33-140">EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-140">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="2ec33-141">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-141">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="2ec33-142">**NuGet 패키지 관리 ...** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-142">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="2ec33-143">NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-143">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="2ec33-144">**설치** 클릭</span><span class="sxs-lookup"><span data-stu-id="2ec33-144">Click **Install**</span></span>

<span data-ttu-id="2ec33-145">EntityFramework 어셈블리 외에도 System.componentmodel 어셈블리에 대 한 참조도 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-145">Note, that in addition to the EntityFramework  assembly, a reference to the System.ComponentModel.DataAnnotations assembly is also added.</span></span>

<span data-ttu-id="2ec33-146">Program.cs 파일의 맨 위에 다음 using 문을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-146">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="2ec33-147">Program.cs에서 컨텍스트 정의를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-147">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="2ec33-148">데이터 유지 및 검색</span><span class="sxs-lookup"><span data-stu-id="2ec33-148">Persist and Retrieve Data</span></span>

<span data-ttu-id="2ec33-149">Main 메서드가 정의 된 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-149">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="2ec33-150">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-150">Add the following code into the Main function.</span></span>

<span data-ttu-id="2ec33-151">이 코드는 두 개의 새 대학 개체를 컨텍스트에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-151">The code adds two new University objects to the context.</span></span> <span data-ttu-id="2ec33-152">공간 속성은 DbGeography. FromText 메서드를 사용 하 여 초기화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-152">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="2ec33-153">WellKnownText로 표시 된 geography point는 메서드에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-153">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="2ec33-154">그런 다음 코드에서 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-154">The code then saves the data.</span></span> <span data-ttu-id="2ec33-155">그런 다음 해당 위치가 지정 된 위치와 가장 가까운 대학 개체를 반환 하는 LINQ 쿼리가 생성 되 고 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-155">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
        {
            Name = "School of Fine Art",
            Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
        });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                        orderby u.Location.Distance(myLocation)
                        select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

<span data-ttu-id="2ec33-156">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-156">Compile and run the application.</span></span> <span data-ttu-id="2ec33-157">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-157">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a><span data-ttu-id="2ec33-158">생성 된 데이터베이스 보기</span><span class="sxs-lookup"><span data-stu-id="2ec33-158">View the Generated Database</span></span>

<span data-ttu-id="2ec33-159">응용 프로그램을 처음 실행 하면 Entity Framework 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-159">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="2ec33-160">Visual Studio 2012가 설치 되어 있으므로 데이터베이스가 LocalDB 인스턴스에 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-160">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="2ec33-161">기본적으로 Entity Framework은 파생 컨텍스트의 정규화 된 이름 (이 예제에서는 **SpatialCodeFirst UniversityContext**) 뒤에 데이터베이스 이름을 부여 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-161">By default, the Entity Framework names the database after the fully qualified name of the derived context (in this example that is **SpatialCodeFirst.UniversityContext**).</span></span> <span data-ttu-id="2ec33-162">기존 데이터베이스가 사용 되는 이후 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-162">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="2ec33-163">데이터베이스를 만든 후 모델을 변경 하는 경우 Code First 마이그레이션를 사용 하 여 데이터베이스 스키마를 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-163">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="2ec33-164">마이그레이션을 사용 하는 예제는 [새 데이터베이스에 대 한 Code First를](~/ef6/modeling/code-first/workflows/new-database.md) 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="2ec33-164">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="2ec33-165">데이터베이스 및 데이터를 보려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-165">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="2ec33-166">Visual Studio 2012 주 메뉴에서 **보기** -&gt; **SQL Server 개체 탐색기**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-166">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="2ec33-167">LocalDB가 서버 목록에 없으면 **SQL Server** 에서 마우스 오른쪽 단추를 클릭 하 고 추가를 선택 **SQL Server** 기본 **Windows 인증** 을 사용 하 여 localdb 인스턴스에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-167">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the the LocalDB instance</span></span>
3.  <span data-ttu-id="2ec33-168">LocalDB 노드 확장</span><span class="sxs-lookup"><span data-stu-id="2ec33-168">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="2ec33-169">**데이터베이스** 폴더를 펼침 하 여 새 데이터베이스를 확인 하 고 **대학** 테이블로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-169">Unfold the **Databases** folder to see the new database and browse to the **Universities** table</span></span>
5.  <span data-ttu-id="2ec33-170">데이터를 보려면 테이블을 마우스 오른쪽 단추로 클릭 하 고 **데이터 보기** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-170">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="2ec33-171">요약</span><span class="sxs-lookup"><span data-stu-id="2ec33-171">Summary</span></span>

<span data-ttu-id="2ec33-172">이 연습에서는 Entity Framework Code First에서 공간 형식을 사용 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="2ec33-172">In this walkthrough we looked at how to use spatial types with Entity Framework Code First.</span></span> 
