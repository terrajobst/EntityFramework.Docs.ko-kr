---
title: 공간-EF 디자이너-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
ms.openlocfilehash: 39fe54ffe9d0ffd1753844f7bffcd1832d82eec5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994369"
---
# <a name="spatial---ef-designer"></a><span data-ttu-id="07d6b-102">공간-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="07d6b-102">Spatial - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="07d6b-103">**EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="07d6b-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="07d6b-105">비디오 및 단계별 연습에는 Entity Framework 디자이너를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-105">The video and step-by-step walkthrough shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="07d6b-106">또한 LINQ 쿼리를 사용 하 여 두 위치 사이의 거리를 찾으려고 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-106">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="07d6b-107">이 연습에서는 새 데이터베이스를 만들려면 Model First 사용 됩니다 있지만 EF 디자이너를 사용 하 여 사용할 수도 있습니다는 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 기존 데이터베이스에 매핑할 워크플로.</span><span class="sxs-lookup"><span data-stu-id="07d6b-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="07d6b-108">공간 형식 지원 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-108">Spatial type support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="07d6b-109">공간 형식, 열거형 및 테이블 반환 함수 같은 새 기능을 사용 하려면 해야 대상 지정 하는.NET Framework 4.5를 참고 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-109">Note that to use the new features like spatial type, enums, and Table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="07d6b-110">Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="07d6b-111">공간 데이터 형식을 사용 하 여 공간 지원에는 Entity Framework 공급자를 사용 해야 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-111">To use spatial data types you must also use an Entity Framework provider that has spatial support.</span></span> <span data-ttu-id="07d6b-112">참조 [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md) 자세한 내용은 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-112">See [provider support for spatial types](~/ef6/fundamentals/providers/spatial-support.md) for more information.</span></span>

<span data-ttu-id="07d6b-113">기본 공간 데이터 유형은 두 가지가: geography 및 geometry 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-113">There are two main spatial data types: geography and geometry.</span></span> <span data-ttu-id="07d6b-114">타원 데이터를 저장 하는 지리 데이터 형식 (예를 들어, GPS 위도 및 경도 좌표)입니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-114">The geography data type stores ellipsoidal data (for example, GPS latitude and longitude coordinates).</span></span> <span data-ttu-id="07d6b-115">기 하 도형 데이터 형식은 유클리드 (평면) 좌표계를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-115">The geometry data type represents Euclidean (flat) coordinate system.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="07d6b-116">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="07d6b-116">Watch the video</span></span>
<span data-ttu-id="07d6b-117">이 비디오에는 Entity Framework 디자이너를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-117">This video shows how to map spatial types with the Entity Framework Designer.</span></span> <span data-ttu-id="07d6b-118">또한 LINQ 쿼리를 사용 하 여 두 위치 사이의 거리를 찾으려고 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-118">It also demonstrates how to use a LINQ query to find a distance between two locations.</span></span>

<span data-ttu-id="07d6b-119">**제공한**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="07d6b-119">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="07d6b-120">**비디오**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span><span class="sxs-lookup"><span data-stu-id="07d6b-120">**Video**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="07d6b-121">필수 조건</span><span class="sxs-lookup"><span data-stu-id="07d6b-121">Pre-Requisites</span></span>

<span data-ttu-id="07d6b-122">Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-122">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="07d6b-123">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="07d6b-123">Set up the Project</span></span>

1.  <span data-ttu-id="07d6b-124">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="07d6b-124">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="07d6b-125">에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**</span><span class="sxs-lookup"><span data-stu-id="07d6b-125">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="07d6b-126">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿</span><span class="sxs-lookup"><span data-stu-id="07d6b-126">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="07d6b-127">입력 **SpatialEFDesigner** 고 프로젝트의 이름으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="07d6b-127">Enter **SpatialEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="07d6b-128">EF 디자이너를 사용 하 여 새 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="07d6b-128">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="07d6b-129">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**</span><span class="sxs-lookup"><span data-stu-id="07d6b-129">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="07d6b-130">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서</span><span class="sxs-lookup"><span data-stu-id="07d6b-130">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="07d6b-131">입력 **UniversityModel.edmx** 클릭 한 다음 확인 하 고 파일 이름에 대 한 **추가**</span><span class="sxs-lookup"><span data-stu-id="07d6b-131">Enter **UniversityModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="07d6b-132">엔터티 데이터 모델 마법사 페이지에서 선택 **빈 모델** Model 콘텐츠 선택 대화 상자에서</span><span class="sxs-lookup"><span data-stu-id="07d6b-132">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="07d6b-133">클릭 **마침**</span><span class="sxs-lookup"><span data-stu-id="07d6b-133">Click **Finish**</span></span>

<span data-ttu-id="07d6b-134">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-134">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="07d6b-135">마법사에서는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-135">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="07d6b-136">개념적 모델, 저장소 모델 및 이들 간의 매핑을 정의 하는 EnumTestModel.edmx 파일이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-136">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="07d6b-137">생성 된 메타 데이터 파일을 어셈블리에 포함 되므로 출력 어셈블리에 포함 하려면.edmx 파일의 메타 데이터 아티팩트 처리 속성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-137">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="07d6b-138">다음 어셈블리에 대 한 참조 추가: EntityFramework 고 System.ComponentModel.DataAnnotations, System.Data.Entity 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-138">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="07d6b-139">UniversityModel.tt 파일과 UniversityModel.Context.tt 만들고.edmx 파일에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-139">Creates UniversityModel.tt and UniversityModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="07d6b-140">T4 템플릿 파일에 이러한.edmx 모델의 엔터티에 매핑되는 POCO 형식과 파생 된 DbContext 형식을 정의 하는 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-140">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="07d6b-141">새 엔터티 형식을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-141">Add a New Entity Type</span></span>

1.  <span data-ttu-id="07d6b-142">디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 **추가-&gt; 엔터티**, 새 Entity 대화 상자 표시</span><span class="sxs-lookup"><span data-stu-id="07d6b-142">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="07d6b-143">지정할 **University** 유형에 대 한 이름을 지정 하 고 지정 **UniversityID** 형식으로 유지 키 속성 이름에 대 한 **Int32**</span><span class="sxs-lookup"><span data-stu-id="07d6b-143">Specify **University** for the type name and specify **UniversityID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="07d6b-144">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-144">Click **OK**</span></span>
4.  <span data-ttu-id="07d6b-145">엔터티를 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 스칼라 속성**</span><span class="sxs-lookup"><span data-stu-id="07d6b-145">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="07d6b-146">새 속성 이름 바꾸기 **이름**</span><span class="sxs-lookup"><span data-stu-id="07d6b-146">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="07d6b-147">다른 스칼라 속성을 추가 하 고 이름을 **위치** 속성 창을 열고 새 속성의 형식을 변경 **Geography**</span><span class="sxs-lookup"><span data-stu-id="07d6b-147">Add another scalar property and rename it to **Location** Open the Properties window and change the type of the new property to **Geography**</span></span>
7.  <span data-ttu-id="07d6b-148">모델을 저장 하 고 프로젝트를 빌드하십시오</span><span class="sxs-lookup"><span data-stu-id="07d6b-148">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="07d6b-149">를 빌드할 때 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-149">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="07d6b-150">오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 했습니다 때문에 이러한 경고를 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-150">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="07d6b-151">모델에서 데이터베이스 생성</span><span class="sxs-lookup"><span data-stu-id="07d6b-151">Generate Database from Model</span></span>

<span data-ttu-id="07d6b-152">이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-152">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="07d6b-153">Entity Designer 화면에서 빈 공간을 마우스 오른쪽 단추로 클릭 **모델에서 데이터베이스 생성**</span><span class="sxs-lookup"><span data-stu-id="07d6b-153">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="07d6b-154">데이터베이스 생성 마법사의 데이터 연결 대화 상자는 선택 표시를 클릭 합니다 **새 연결** 지정 단추 **(localdb)\\mssqllocaldb** 고서버이름에대한 **대학** 데이터베이스 및 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="07d6b-154">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **University** for the database and click **OK**</span></span>
3.  <span data-ttu-id="07d6b-155">새 데이터베이스를 만들 것인지 묻는 대화 상자가 팝업을 누릅니다 **예**합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-155">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="07d6b-156">클릭 **다음** 데이터베이스 만들기 마법사의 요약 및 설정 대화 상자 참고 DDL에 대 한 정의 포함 하지 않는에 표시 되는 데이터베이스를 만드는 중 생성된 된 DDL에 대 한 DDL (데이터 정의 언어)을 생성 하 고는 열거형 형식에 매핑되는 테이블</span><span class="sxs-lookup"><span data-stu-id="07d6b-156">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="07d6b-157">클릭 **완료** 마침을 클릭 하면 DDL 스크립트 실행 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-157">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="07d6b-158">다음을 수행 하는 데이터베이스 만들기 마법사: 엽니다는 **UniversityModel.edmx.sql** 추가 연결 문자열 정보를 App.config 파일에는 EDMX의 저장소 스키마 및 매핑 섹션을 생성 하는 T-SQL 편집기에서에서 파일</span><span class="sxs-lookup"><span data-stu-id="07d6b-158">The Create Database Wizard does the following: Opens the **UniversityModel.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="07d6b-159">T-SQL 편집기에서 마우스 오른쪽 단추를 클릭 하 고 선택 **Execute** 나타나면 서버 대화 상자에는 연결 2 단계의 연결 정보를 입력 하 고 클릭 **연결**</span><span class="sxs-lookup"><span data-stu-id="07d6b-159">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="07d6b-160">생성된 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**</span><span class="sxs-lookup"><span data-stu-id="07d6b-160">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="07d6b-161">유지 하 고 데이터를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-161">Persist and Retrieve Data</span></span>

<span data-ttu-id="07d6b-162">Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-162">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="07d6b-163">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-163">Add the following code into the Main function.</span></span>

<span data-ttu-id="07d6b-164">두 가지 새로운 University 개체를 컨텍스트에 추가 하는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-164">The code adds two new University objects to the context.</span></span> <span data-ttu-id="07d6b-165">공간 속성 DbGeography.FromText 메서드를 사용 하 여 초기화 됩니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-165">Spatial properties are initialized by using the DbGeography.FromText method.</span></span> <span data-ttu-id="07d6b-166">지리 지점 나타낸 WellKnownText 메서드에 전달 됩니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-166">The geography point represented as WellKnownText is passed to the method.</span></span> <span data-ttu-id="07d6b-167">다음 코드는 데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-167">The code then saves the data.</span></span> <span data-ttu-id="07d6b-168">그런 다음는 LINQ 쿼리 해당 위치가 지정된 된 위치에 가장 가까운 University 개체를 반환 하는 생성 되 고 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-168">Then, the LINQ query that that returns a University object where its location is closest to the specified location, is constructed and executed.</span></span>

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

<span data-ttu-id="07d6b-169">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-169">Compile and run the application.</span></span> <span data-ttu-id="07d6b-170">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-170">The program produces the following output:</span></span>

```
The closest University to you is: School of Fine Art.
```

<span data-ttu-id="07d6b-171">데이터베이스에서 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-171">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="07d6b-172">그런 다음 선택한 테이블에서 마우스 오른쪽 단추를 클릭 **데이터 보기**합니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-172">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="07d6b-173">요약</span><span class="sxs-lookup"><span data-stu-id="07d6b-173">Summary</span></span>

<span data-ttu-id="07d6b-174">이 연습에서는 Entity Framework 디자이너를 사용 하 여 공간 형식을 매핑하는 방법 및 코드에서 공간 형식을 사용 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="07d6b-174">In this walkthrough we looked at how to map spatial types using the Entity Framework Designer and how to use spatial types in code.</span></span> 
