---
title: 열거형 지원-Code First – EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 986991c2dd9470867aaf5424ecb176d7db172426
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489507"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="099bf-102">열거형 지원-Code First</span><span class="sxs-lookup"><span data-stu-id="099bf-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="099bf-103">**EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="099bf-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="099bf-105">이 비디오 및 단계별 연습에서는 Entity Framework Code First를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="099bf-106">또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="099bf-107">새 데이터베이스를 만들려면이 연습에서는 Code First 사용 하지만 사용할 수도 있습니다 [Code First는 기존 데이터베이스에 매핑할](~/ef6/modeling/code-first/workflows/existing-database.md)합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="099bf-108">열거형 지원 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="099bf-109">테이블 반환 함수, 열거형 및 공간 데이터 형식 등 새 기능을 사용 하려면.NET Framework 4.5 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="099bf-110">Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="099bf-111">Entity Framework의 열거형에는 다음 기본 형식이 있을 수 있습니다: **바이트**, **Int16**를 **Int32**를 **Int64** , 또는 **SByte**합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="099bf-112">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="099bf-112">Watch the video</span></span>
<span data-ttu-id="099bf-113">이 비디오에는 Entity Framework Code First를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="099bf-114">또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="099bf-115">**제공한**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="099bf-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="099bf-116">**비디오**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="099bf-116">**Video**: [WMV](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="099bf-117">필수 조건</span><span class="sxs-lookup"><span data-stu-id="099bf-117">Pre-Requisites</span></span>

<span data-ttu-id="099bf-118">Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="099bf-119">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="099bf-119">Set up the Project</span></span>

1.  <span data-ttu-id="099bf-120">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="099bf-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="099bf-121">에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**</span><span class="sxs-lookup"><span data-stu-id="099bf-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="099bf-122">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿</span><span class="sxs-lookup"><span data-stu-id="099bf-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="099bf-123">입력 **EnumCodeFirst** 고 프로젝트의 이름으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="099bf-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="099bf-124">Code First를 사용 하 여 새 모델을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-124">Define a New Model using Code First</span></span>

<span data-ttu-id="099bf-125">Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="099bf-126">아래 코드는 부서 클래스를 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-126">The code below defines the Department class.</span></span>

<span data-ttu-id="099bf-127">코드는 또한 DepartmentNames 열거형을 정의합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="099bf-128">기본적으로 열거형에는 **int** 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="099bf-129">부서 클래스의 이름 속성 DepartmentNames 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="099bf-130">Program.cs 파일을 열고 다음 클래스 정의 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-130">Open the Program.cs file and paste the following class definitions.</span></span>

``` csharp
public enum DepartmentNames
{
    English,
    Math,
    Economics
}     

public partial class Department
{
    public int DepartmentID { get; set; }
    public DepartmentNames Name { get; set; }
    public decimal Budget { get; set; }
}
```
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="099bf-131">정의 DbContext 파생 형식</span><span class="sxs-lookup"><span data-stu-id="099bf-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="099bf-132">엔터티를 정의 하는 것 외에도 DbContext에서 파생 되며 DbSet을 노출 하는 클래스를 정의 해야&lt;TEntity&gt; 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="099bf-133">DbSet&lt;TEntity&gt; 모델에 포함 하려는 유형을 알고 있어야 하는 컨텍스트 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="099bf-134">런타임에 데이터베이스에서 데이터를 사용 하 여 개체를 채우는 포함 하는 엔터티 개체를 관리 하는 파생 된 DbContext 형식의 인스턴스, 데이터베이스, 추적 및 저장 데이터를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="099bf-135">DbContext 형식과 DbSet EntityFramework 어셈블리에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="099bf-136">EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="099bf-137">솔루션 탐색기에서 프로젝트 이름을 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="099bf-138">선택 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="099bf-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="099bf-139">NuGet 패키지 관리 대화 상자에서 선택 합니다 **온라인** 탭을 선택 합니다 **EntityFramework** 패키지.</span><span class="sxs-lookup"><span data-stu-id="099bf-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="099bf-140">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="099bf-140">Click **Install**</span></span>

<span data-ttu-id="099bf-141">Note는 EntityFramework 어셈블리 외에도 System.Data.Entity 및 System.ComponentModel.DataAnnotations 어셈블리에 대 한 참조로도 함께 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="099bf-142">추가 다음 Program.cs 파일의 맨 위에 있는 문을 사용 하 여:</span><span class="sxs-lookup"><span data-stu-id="099bf-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="099bf-143">Program.cs의 상황에 맞는 정의 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="099bf-144">유지 하 고 데이터를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="099bf-145">Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="099bf-146">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-146">Add the following code into the Main function.</span></span> <span data-ttu-id="099bf-147">새 부서 개체 컨텍스트를 추가 하는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="099bf-148">데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-148">It then saves the data.</span></span> <span data-ttu-id="099bf-149">코드는 또한 여기서 name은 DepartmentNames.English 부서를 반환 하는 LINQ 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestContext())
{
    context.Departments.Add(new Department { Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="099bf-150">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-150">Compile and run the application.</span></span> <span data-ttu-id="099bf-151">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="099bf-152">생성된 된 데이터베이스 뷰</span><span class="sxs-lookup"><span data-stu-id="099bf-152">View the Generated Database</span></span>

<span data-ttu-id="099bf-153">처음으로 응용 프로그램을 실행 하면 Entity Framework를 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="099bf-154">Visual Studio 2012를 설치 했으므로 LocalDB 인스턴스에서 데이터베이스가 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="099bf-155">기본적으로 Entity Framework에 이름을 파생 컨텍스트의 정규화 된 이름 뒤에 오는 하는 데이터베이스 (이 예에 대 한 **EnumCodeFirst.EnumTestContext**).</span><span class="sxs-lookup"><span data-stu-id="099bf-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="099bf-156">사용할 기존 데이터베이스를 이후 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="099bf-157">참고로 하는 경우 변경 내용을 모델 데이터베이스가 만들어진 후에 사용 하는 Code First 마이그레이션을 데이터베이스 스키마를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="099bf-158">참조 [Code First 새 데이터베이스로](~/ef6/modeling/code-first/workflows/new-database.md) 마이그레이션을 사용 하 여의 예입니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="099bf-159">데이터베이스 및 데이터를 보려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="099bf-160">Visual Studio 2012 주 메뉴에서 선택 **뷰**  - &gt; **SQL Server 개체 탐색기**합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="099bf-161">LocalDB 서버 목록에 없는 경우에서 마우스 오른쪽 단추를 클릭 **SQL Server** 선택한 **SQL Server 추가** 기본값을 사용 하 여 **Windows 인증** 에 연결 하는 LocalDB 인스턴스</span><span class="sxs-lookup"><span data-stu-id="099bf-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="099bf-162">LocalDB 노드를 확장 합니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="099bf-163">펼치려면 합니다 **데이터베이스** 폴더를 찾아 새 데이터베이스가 표시를 합니다 **부서** Code First 만들어지지는지 않습니다 열거형 형식으로 매핑하는 테이블을 보면 테이블</span><span class="sxs-lookup"><span data-stu-id="099bf-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="099bf-164">데이터를 보고, 테이블을 마우스 오른쪽 단추로 클릭 합니다. 선택 **데이터 보기**</span><span class="sxs-lookup"><span data-stu-id="099bf-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="099bf-165">요약</span><span class="sxs-lookup"><span data-stu-id="099bf-165">Summary</span></span>

<span data-ttu-id="099bf-166">이 연습에서는 Entity Framework Code First를 사용 하 여 열거형을 사용 하는 방법에 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="099bf-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
