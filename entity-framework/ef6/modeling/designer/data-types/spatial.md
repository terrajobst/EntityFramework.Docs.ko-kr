---
title: 공간-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: a9c54fbc14dd02ce5d4d91449a0d5f9e72f7f0f7
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182509"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="b523a-102">공간-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="b523a-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="b523a-103">**EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="b523a-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b523a-105">비디오 및 단계별 연습에서는 공간 형식을 Entity Framework Designer에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="b523a-106">LINQ 쿼리를 사용 하 여 두 위치 간의 거리를 찾는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="b523a-107">이 연습에서는 Model First 사용 하 여 새 데이터베이스를 만들지만 EF Designer를 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 워크플로와 함께 사용 하 여 기존 데이터베이스에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="b523a-108">공간 형식 지원은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="b523a-109">공간 형식, 열거형 및 테이블 반환 함수와 같은 새 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="b523a-110">Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="b523a-111">공간 데이터 형식을 사용 하려면 공간이 지원 되는 Entity Framework 공급자도 사용 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="b523a-112">자세한 내용은 [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md) 을 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="b523a-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="b523a-113">두 가지 주요 공간 데이터 형식인 geography 및 geometry가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="b523a-114">Geography 데이터 형식은 타원 데이터 (예: GPS 위도 및 경도 좌표)를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="b523a-115">Geometry 데이터 형식은 유클리드 (평면) 좌표계를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="b523a-116">비디오 시청</span><span class="sxs-lookup"><span data-stu-id="b523a-116">Watch the video</span></span>
<span data-ttu-id="b523a-117">이 비디오에서는 공간 형식을 Entity Framework Designer에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="b523a-118">LINQ 쿼리를 사용 하 여 두 위치 간의 거리를 찾는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="b523a-119">제공한 **사람**: 줄리아 Kornich</span><span class="sxs-lookup"><span data-stu-id="b523a-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="b523a-120">**비디오**: [wmv](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="b523a-120">**Video**: [WMV](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="b523a-121">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="b523a-121">Pre-Requisites</span></span>

<span data-ttu-id="b523a-122">이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="b523a-123">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="b523a-123">Set up the Project</span></span>

1.  <span data-ttu-id="b523a-124">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="b523a-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="b523a-125">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="b523a-126">왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="b523a-127">프로젝트 이름으로 **SpatialEFDesigner** 를 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="b523a-128">EF Designer를 사용 하 여 새 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="b523a-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="b523a-129">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="b523a-130">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="b523a-131">파일 이름에 UniversityModel를 입력 한 다음 **추가** 를 클릭 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="b523a-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="b523a-132">엔터티 데이터 모델 마법사 페이지의 Model 콘텐츠 선택 대화 상자에서 **빈 모델** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="b523a-133">**마침** 클릭</span><span class="sxs-lookup"><span data-stu-id="b523a-133">Click **Finish**</span></span>

<span data-ttu-id="b523a-134">모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="b523a-135">마법사에서는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="b523a-136">개념적 모델, 저장소 모델 및 두 모델 간의 매핑을 정의 하는 EnumTestModel .edmx 파일을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="b523a-137">생성 된 메타 데이터 파일이 어셈블리에 포함 되도록 .edmx 파일의 메타 데이터 아티팩트 처리 속성을 출력 어셈블리에 포함 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="b523a-138">EntityFramework, System.componentmodel 및 System.object 어셈블리에 대 한 참조를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="b523a-139">UniversityModel.tt 및 UniversityModel.Context.tt 파일을 만들고 .edmx 파일 아래에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="b523a-140">이러한 T4 템플릿 파일은 .edmx 모델의 엔터티에 매핑되는 DbContext 파생 형식 및 POCO 형식을 정의 하는 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="b523a-141">새 엔터티 형식 추가</span><span class="sxs-lookup"><span data-stu-id="b523a-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="b523a-142">디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 엔터티**를 선택 합니다. 새 엔터티 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="b523a-143">유형 이름에 대해 **대학** 을 지정 하 고 키 속성 이름에 **UniversityID** 를 지정 하 고 형식을 **Int32** 로 그대로 둡니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="b523a-144">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-144">Click **OK**</span></span>
4.  <span data-ttu-id="b523a-145">엔터티를 마우스 오른쪽 단추로 클릭 하 고 **추가 새로 만들기-&gt; 스칼라 속성** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="b523a-146">새 속성의 이름을 **Name** 으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="b523a-147">다른 스칼라 속성을 추가 하 고 **위치** 에 이름 변경 속성 창를 열고 새 속성의 형식을 **Geography** 로 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="b523a-148">모델을 저장 하 고 프로젝트를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="b523a-149">빌드하면 매핑되지 않은 엔터티와 연결에 대 한 경고가 오류 목록 표시 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="b523a-150">모델에서 데이터베이스를 생성 하는 것을 선택한 후 오류가 사라집니다. 이러한 경고는 무시 해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="b523a-151">모델에서 데이터베이스 생성</span><span class="sxs-lookup"><span data-stu-id="b523a-151">Generate Database from Model</span></span>

<span data-ttu-id="b523a-152">이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="b523a-153">Entity Designer 화면의 빈 공간을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="b523a-154">데이터베이스 생성 마법사의 데이터 연결 선택 대화 상자가 표시 됩니다. 데이터베이스에 대 한 서버 이름 및 **대학** 에 대해 **새 연결** 단추 지정 **(Localdb)\\Mssqllocaldb** 를 클릭 하 고 **확인** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="b523a-155">새 데이터베이스를 만들지 여부를 묻는 대화 상자가 표시 되 면 **예**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="b523a-156">**다음** 을 클릭 하 고 데이터베이스 만들기 마법사에서 데이터베이스를 만들기 위한 ddl (데이터 정의 언어)을 생성 합니다. 생성 된 Ddl은 요약 및 설정 대화 상자에 표시 되며, 해당 ddl에는 열거형 형식에 매핑되는 테이블에 대 한 정의가 포함 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="b523a-157">마침 **을 클릭 하면 DDL** 스크립트가 실행 되지 않습니다 .를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="b523a-158">데이터베이스 만들기 마법사는 다음을 수행 합니다. T-sql 편집기에서 **UniversityModel** 를 열면 .edmx 파일의 저장소 스키마 및 매핑 섹션이 생성 되어 app.config 파일에 연결 문자열 정보가 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="b523a-159">T-sql 편집기에서 마우스 오른쪽 단추를 클릭 하 고 서버에 연결 대화 상자가 나타납니다 .를 선택 하 고 2 단계의 연결 정보를 입력 한 다음 **연결** 을 **클릭 합니다.**</span><span class="sxs-lookup"><span data-stu-id="b523a-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="b523a-160">생성 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 고침** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="b523a-161">데이터 유지 및 검색</span><span class="sxs-lookup"><span data-stu-id="b523a-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="b523a-162">Main 메서드가 정의 된 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="b523a-163">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="b523a-164">이 코드는 두 개의 새 대학 개체를 컨텍스트에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="b523a-165">공간 속성은 DbGeography. FromText 메서드를 사용 하 여 초기화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="b523a-166">WellKnownText로 표시 된 geography point는 메서드에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="b523a-167">그런 다음 코드에서 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-167">The code then saves the data.</span></span> <span data-ttu-id="b523a-168">그런 다음 해당 위치가 지정 된 위치와 가장 가까운 대학 개체를 반환 하는 LINQ 쿼리가 생성 되 고 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
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

<span data-ttu-id="b523a-169">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-169">Compile and run the application.</span></span> <span data-ttu-id="b523a-170">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-170">The program produces the following output:</span></span>

```console
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="b523a-171">데이터베이스의 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 고침**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="b523a-172">그런 다음 테이블에서 마우스 오른쪽 단추를 클릭 하 고 **데이터 보기**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="b523a-173">요약</span><span class="sxs-lookup"><span data-stu-id="b523a-173">Summary</span></span>

<span data-ttu-id="b523a-174">이 연습에서는 Entity Framework Designer를 사용 하 여 공간 형식을 매핑하는 방법 및 코드에서 공간 형식을 사용 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="b523a-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
