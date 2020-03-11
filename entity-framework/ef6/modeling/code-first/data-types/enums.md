---
title: 열거형 지원-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 77a42501-27c9-4f4b-96df-26c128021467
ms.openlocfilehash: 1cecbf7065367deb3d202977fe39187bd907d824
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415792"
---
# <a name="enum-support---code-first"></a><span data-ttu-id="e5ca6-102">열거형 지원-Code First</span><span class="sxs-lookup"><span data-stu-id="e5ca6-102">Enum Support - Code First</span></span>
> [!NOTE]
> <span data-ttu-id="e5ca6-103">**EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="e5ca6-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="e5ca6-105">이 비디오 및 단계별 연습에서는 Entity Framework Code First에서 열거형 형식을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-105">This video and step-by-step walkthrough shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="e5ca6-106">또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="e5ca6-107">이 연습에서는 Code First 사용 하 여 새 데이터베이스를 만들지만 Code First를 사용 하 여 [기존 데이터베이스에 매핑할](~/ef6/modeling/code-first/workflows/existing-database.md)수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-107">This walkthrough will use Code First to create a new database, but you can also use [Code First to map to an existing database](~/ef6/modeling/code-first/workflows/existing-database.md).</span></span>

<span data-ttu-id="e5ca6-108">열거형 지원은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="e5ca6-109">열거형, 공간 데이터 형식 및 테이블 반환 함수와 같은 새로운 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="e5ca6-110">Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="e5ca6-111">Entity Framework에서 열거형은 **Byte**, **Int16**, **Int32**, **Int64** 또는 **SByte**의 기본 형식을 가질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="e5ca6-112">비디오 보기</span><span class="sxs-lookup"><span data-stu-id="e5ca6-112">Watch the video</span></span>
<span data-ttu-id="e5ca6-113">이 비디오는 Entity Framework Code First에서 열거형 형식을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-113">This video shows how to use enum types with Entity Framework Code First.</span></span> <span data-ttu-id="e5ca6-114">또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="e5ca6-115">제공한 **사람**: 줄리아 Kornich</span><span class="sxs-lookup"><span data-stu-id="e5ca6-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="e5ca6-116">**비디오**: [wmv](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span><span class="sxs-lookup"><span data-stu-id="e5ca6-116">**Video**: [WMV](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.wmv) | [MP4](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-mp4video-enumwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/A/5/8/A583DEE8-FD5C-47EE-A4E1-966DDF39D1DA/HDI-ITPro-MSDN-winvideo-enumwithcodefirst.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="e5ca6-117">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="e5ca6-117">Pre-Requisites</span></span>

<span data-ttu-id="e5ca6-118">이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

 

## <a name="set-up-the-project"></a><span data-ttu-id="e5ca6-119">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="e5ca6-119">Set up the Project</span></span>

1.  <span data-ttu-id="e5ca6-120">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="e5ca6-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="e5ca6-121">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="e5ca6-122">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="e5ca6-123">**Enumcodefirst** 를 프로젝트 이름으로 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-123">Enter **EnumCodeFirst** as the name of the project and click **OK**</span></span>

## <a name="define-a-new-model-using-code-first"></a><span data-ttu-id="e5ca6-124">Code First를 사용 하 여 새 모델 정의</span><span class="sxs-lookup"><span data-stu-id="e5ca6-124">Define a New Model using Code First</span></span>

<span data-ttu-id="e5ca6-125">Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-125">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="e5ca6-126">아래 코드는 부서 클래스를 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-126">The code below defines the Department class.</span></span>

<span data-ttu-id="e5ca6-127">또한이 코드는 DepartmentNames 열거형을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-127">The code also defines the DepartmentNames enumeration.</span></span> <span data-ttu-id="e5ca6-128">기본적으로 열거형은 **int** 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-128">By default, the enumeration is of **int** type.</span></span> <span data-ttu-id="e5ca6-129">학과 클래스의 Name 속성은 DepartmentNames 유형입니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-129">The Name property on the Department class is of the DepartmentNames type.</span></span>

<span data-ttu-id="e5ca6-130">Program.cs 파일을 열고 다음 클래스 정의를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-130">Open the Program.cs file and paste the following class definitions.</span></span>

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
 

## <a name="define-the-dbcontext-derived-type"></a><span data-ttu-id="e5ca6-131">DbContext 파생 형식 정의</span><span class="sxs-lookup"><span data-stu-id="e5ca6-131">Define the DbContext Derived Type</span></span>

<span data-ttu-id="e5ca6-132">엔터티를 정의 하는 것 외에도 DbContext에서 파생 되 고 DbSet&lt;&gt; 속성을 노출 하는 클래스를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-132">In addition to defining entities, you need to define a class that derives from DbContext and exposes DbSet&lt;TEntity&gt; properties.</span></span> <span data-ttu-id="e5ca6-133">DbSet&lt;&gt; 속성을 사용 하면 컨텍스트에서 모델에 포함 하려는 형식을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-133">The DbSet&lt;TEntity&gt; properties let the context know which types you want to include in the model.</span></span>

<span data-ttu-id="e5ca6-134">DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-134">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

<span data-ttu-id="e5ca6-135">DbContext 및 DbSet 형식은 EntityFramework 어셈블리에서 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-135">The DbContext and DbSet types are defined in the EntityFramework assembly.</span></span> <span data-ttu-id="e5ca6-136">EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-136">We will add a reference to this DLL by using the EntityFramework NuGet package.</span></span>

1.  <span data-ttu-id="e5ca6-137">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-137">In Solution Explorer, right-click on the project name.</span></span>
2.  <span data-ttu-id="e5ca6-138">**NuGet 패키지 관리 ...** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-138">Select **Manage NuGet Packages…**</span></span>
3.  <span data-ttu-id="e5ca6-139">NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-139">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package.</span></span>
4.  <span data-ttu-id="e5ca6-140">**설치**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-140">Click **Install**</span></span>

<span data-ttu-id="e5ca6-141">EntityFramework 어셈블리 외에도 System.componentmodel 및 system.xml 어셈블리에 대 한 참조도 추가 됩니다 (참고).</span><span class="sxs-lookup"><span data-stu-id="e5ca6-141">Note, that in addition to the EntityFramework  assembly, references to System.ComponentModel.DataAnnotations and System.Data.Entity assemblies are added as well.</span></span>

<span data-ttu-id="e5ca6-142">Program.cs 파일의 맨 위에 다음 using 문을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-142">At the top of the Program.cs file, add the following using statement:</span></span>

``` csharp
using System.Data.Entity;
```

<span data-ttu-id="e5ca6-143">Program.cs에서 컨텍스트 정의를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-143">In the Program.cs add the context definition.</span></span> 

``` csharp
public partial class EnumTestContext : DbContext
{
    public DbSet<Department> Departments { get; set; }
}
```
 

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="e5ca6-144">데이터 유지 및 검색</span><span class="sxs-lookup"><span data-stu-id="e5ca6-144">Persist and Retrieve Data</span></span>

<span data-ttu-id="e5ca6-145">Main 메서드가 정의 된 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-145">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="e5ca6-146">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-146">Add the following code into the Main function.</span></span> <span data-ttu-id="e5ca6-147">이 코드는 컨텍스트에 새 부서 개체를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-147">The code adds a new Department object to the context.</span></span> <span data-ttu-id="e5ca6-148">그런 다음 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-148">It then saves the data.</span></span> <span data-ttu-id="e5ca6-149">또한이 코드는 이름이 DepartmentNames 인 부서를 반환 하는 LINQ 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-149">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="e5ca6-150">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-150">Compile and run the application.</span></span> <span data-ttu-id="e5ca6-151">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-151">The program produces the following output:</span></span>

``` csharp
DepartmentID: 1 Name: English
```
 

## <a name="view-the-generated-database"></a><span data-ttu-id="e5ca6-152">생성 된 데이터베이스 보기</span><span class="sxs-lookup"><span data-stu-id="e5ca6-152">View the Generated Database</span></span>

<span data-ttu-id="e5ca6-153">응용 프로그램을 처음 실행 하면 Entity Framework 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-153">When you run the application the first time, the Entity Framework creates a database for you.</span></span> <span data-ttu-id="e5ca6-154">Visual Studio 2012가 설치 되어 있으므로 데이터베이스가 LocalDB 인스턴스에 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-154">Because we have Visual Studio 2012 installed, the database will be created on the LocalDB instance.</span></span> <span data-ttu-id="e5ca6-155">기본적으로 Entity Framework은 파생 컨텍스트의 정규화 된 이름 (이 예에서는 **Enumcodefirst. EnumTestContext**) 뒤에 데이터베이스 이름을 부여 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-155">By default, the Entity Framework names the database after the fully qualified name of the derived context (for this example that is **EnumCodeFirst.EnumTestContext**).</span></span> <span data-ttu-id="e5ca6-156">기존 데이터베이스가 사용 되는 이후 시간입니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-156">The subsequent times the existing database will be used.</span></span>  

<span data-ttu-id="e5ca6-157">데이터베이스를 만든 후 모델을 변경 하는 경우 Code First 마이그레이션를 사용 하 여 데이터베이스 스키마를 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-157">Note, that if you make any changes to your model after the database has been created, you should use Code First Migrations to update the database schema.</span></span> <span data-ttu-id="e5ca6-158">마이그레이션을 사용 하는 예제는 [새 데이터베이스에 대 한 Code First를](~/ef6/modeling/code-first/workflows/new-database.md) 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-158">See [Code First to a New Database](~/ef6/modeling/code-first/workflows/new-database.md) for an example of using Migrations.</span></span>

<span data-ttu-id="e5ca6-159">데이터베이스 및 데이터를 보려면 다음을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-159">To view the database and data, do the following:</span></span>

1.  <span data-ttu-id="e5ca6-160">Visual Studio 2012 주 메뉴에서 **보기** -&gt; **SQL Server 개체 탐색기**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-160">In the Visual Studio 2012 main menu, select **View** -&gt; **SQL Server Object Explorer**.</span></span>
2.  <span data-ttu-id="e5ca6-161">LocalDB가 서버 목록에 없으면 **SQL Server** 에서 마우스 오른쪽 단추를 클릭 하 고 추가를 선택 **SQL Server** 기본 **Windows 인증** 을 사용 하 여 localdb 인스턴스에 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-161">If LocalDB is not in the list of servers, click the right mouse button on **SQL Server** and select **Add SQL Server** Use the default **Windows Authentication** to connect to the LocalDB instance</span></span>
3.  <span data-ttu-id="e5ca6-162">LocalDB 노드 확장</span><span class="sxs-lookup"><span data-stu-id="e5ca6-162">Expand the LocalDB node</span></span>
4.  <span data-ttu-id="e5ca6-163">**데이터베이스** 폴더를 펼침 새 데이터베이스를 확인 하 고 **부서** 테이블을 탐색 합니다. Code First이는 열거형 형식에 매핑되는 테이블을 만들지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-163">Unfold the **Databases** folder to see the new database and browse to the **Department** table Note, that Code First does not create a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="e5ca6-164">데이터를 보려면 테이블을 마우스 오른쪽 단추로 클릭 하 고 **데이터 보기** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-164">To view data, right-click on the table and select **View Data**</span></span>

## <a name="summary"></a><span data-ttu-id="e5ca6-165">요약</span><span class="sxs-lookup"><span data-stu-id="e5ca6-165">Summary</span></span>

<span data-ttu-id="e5ca6-166">이 연습에서는 Entity Framework Code First에서 열거형 형식을 사용 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="e5ca6-166">In this walkthrough we looked at how to use enum types with Entity Framework Code First.</span></span> 
