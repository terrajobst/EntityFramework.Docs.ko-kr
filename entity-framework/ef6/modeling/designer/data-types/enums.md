---
title: 열거형 지원-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 331182c4311565c94cf072eb9b9ad372ac76180a
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283942"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="6c386-102">열거형 지원-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="6c386-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="6c386-103">**EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="6c386-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="6c386-105">이 비디오 및 단계별 연습에서는 Entity Framework 디자이너를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="6c386-106">또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="6c386-107">이 연습에서는 새 데이터베이스를 만들려면 Model First 사용 됩니다 있지만 EF 디자이너를 사용 하 여 사용할 수도 있습니다는 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 기존 데이터베이스에 매핑할 워크플로.</span><span class="sxs-lookup"><span data-stu-id="6c386-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="6c386-108">열거형 지원 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="6c386-109">테이블 반환 함수, 열거형 및 공간 데이터 형식 등 새 기능을 사용 하려면.NET Framework 4.5 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="6c386-110">Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="6c386-111">Entity Framework의 열거형에는 다음 기본 형식이 있을 수 있습니다: **바이트**, **Int16**를 **Int32**를 **Int64** , 또는 **SByte**합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="6c386-112">비디오를 시청 하세요.</span><span class="sxs-lookup"><span data-stu-id="6c386-112">Watch the Video</span></span>
<span data-ttu-id="6c386-113">이 비디오에는 Entity Framework 디자이너를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="6c386-114">또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="6c386-115">**제공한**: Julia Kornich</span><span class="sxs-lookup"><span data-stu-id="6c386-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="6c386-116">**비디오**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="6c386-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="6c386-117">필수 조건</span><span class="sxs-lookup"><span data-stu-id="6c386-117">Pre-Requisites</span></span>

<span data-ttu-id="6c386-118">Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="6c386-119">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="6c386-119">Set up the Project</span></span>

1.  <span data-ttu-id="6c386-120">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="6c386-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="6c386-121">에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**</span><span class="sxs-lookup"><span data-stu-id="6c386-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="6c386-122">왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿</span><span class="sxs-lookup"><span data-stu-id="6c386-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="6c386-123">입력 **EnumEFDesigner** 고 프로젝트의 이름으로 **확인**</span><span class="sxs-lookup"><span data-stu-id="6c386-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="6c386-124">EF 디자이너를 사용 하 여 새 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="6c386-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="6c386-125">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**</span><span class="sxs-lookup"><span data-stu-id="6c386-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="6c386-126">선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서</span><span class="sxs-lookup"><span data-stu-id="6c386-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="6c386-127">입력 **EnumTestModel.edmx** 클릭 한 다음 확인 하 고 파일 이름에 대 한 **추가**</span><span class="sxs-lookup"><span data-stu-id="6c386-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="6c386-128">엔터티 데이터 모델 마법사 페이지에서 선택 **빈 모델** Model 콘텐츠 선택 대화 상자에서</span><span class="sxs-lookup"><span data-stu-id="6c386-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="6c386-129">클릭 **마침**</span><span class="sxs-lookup"><span data-stu-id="6c386-129">Click **Finish**</span></span>

<span data-ttu-id="6c386-130">모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="6c386-131">마법사에서는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="6c386-132">개념적 모델, 저장소 모델 및 이들 간의 매핑을 정의 하는 EnumTestModel.edmx 파일이 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="6c386-133">생성 된 메타 데이터 파일을 어셈블리에 포함 되므로 출력 어셈블리에 포함 하려면.edmx 파일의 메타 데이터 아티팩트 처리 속성을 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="6c386-134">다음 어셈블리에 대 한 참조 추가: EntityFramework 고 System.ComponentModel.DataAnnotations, System.Data.Entity 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="6c386-135">EnumTestModel.tt 파일과 EnumTestModel.Context.tt 만들고.edmx 파일에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="6c386-136">T4 템플릿 파일에 이러한.edmx 모델의 엔터티에 매핑되는 POCO 형식과 파생 된 DbContext 형식을 정의 하는 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="6c386-137">새 엔터티 형식을 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="6c386-138">디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 **추가-&gt; 엔터티**, 새 Entity 대화 상자 표시</span><span class="sxs-lookup"><span data-stu-id="6c386-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="6c386-139">지정할 **부서** 유형에 대 한 이름을 지정 하 고 지정 **DepartmentID** 형식으로 유지 키 속성 이름에 대 한 **Int32**</span><span class="sxs-lookup"><span data-stu-id="6c386-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="6c386-140">**확인**을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-140">Click **OK**</span></span>
4.  <span data-ttu-id="6c386-141">엔터티를 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 스칼라 속성**</span><span class="sxs-lookup"><span data-stu-id="6c386-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="6c386-142">새 속성 이름 바꾸기 **이름**</span><span class="sxs-lookup"><span data-stu-id="6c386-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="6c386-143">새 속성의 형식을 변경 **Int32** (기본적으로 새 속성은 문자열 형식의) 유형을 변경 하려면 속성 창을 열고 유형 속성을 변경 **Int32**</span><span class="sxs-lookup"><span data-stu-id="6c386-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="6c386-144">다른 스칼라 속성을 추가 하 고 이름을 **예산**, 유형을 변경 하 여 **10 진수**</span><span class="sxs-lookup"><span data-stu-id="6c386-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="6c386-145">열거형 형식 추가</span><span class="sxs-lookup"><span data-stu-id="6c386-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="6c386-146">Entity Framework 디자이너에서 Name 속성을 단추로 **열거형으로 변환**</span><span class="sxs-lookup"><span data-stu-id="6c386-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![열거형으로 변환](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="6c386-148">에 **추가 열거형** 대화 상자 유형을 **DepartmentNames** 열거형 형식 이름에 대 한 기본 유형을 변경 하 여 **Int32**, 다음 형식으로 다음 멤버를 추가 하 고: 영어 수치 연산 및 경제성</span><span class="sxs-lookup"><span data-stu-id="6c386-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![열거형 형식 추가](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="6c386-150">키를 눌러 **확인**</span><span class="sxs-lookup"><span data-stu-id="6c386-150">Press **OK**</span></span>
4.  <span data-ttu-id="6c386-151">모델을 저장 하 고 프로젝트를 빌드하십시오</span><span class="sxs-lookup"><span data-stu-id="6c386-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="6c386-152">를 빌드할 때 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="6c386-153">오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 했습니다 때문에 이러한 경고를 무시할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="6c386-154">속성 창을 보면 Name 속성의 형식에 변경 되었는지 알 수 있습니다 **DepartmentNames** 새로 추가 된 열거형 형식의 목록에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="6c386-155">모델 브라우저 창으로 전환 하면 형식에서 열거형 형식 노드를 추가한 표시 메시지가 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![모델 브라우저](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="6c386-157">또한 새 열거형이이 창에서 마우스 오른쪽 단추를 클릭 하 고 선택 하 여 추가할 수 있습니다 **열거형 형식 추가**합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="6c386-158">형식이 생성 되 면 형식 목록에 표시 됩니다 및 속성을 사용 하 여 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="6c386-159">모델에서 데이터베이스 생성</span><span class="sxs-lookup"><span data-stu-id="6c386-159">Generate Database from Model</span></span>

<span data-ttu-id="6c386-160">이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="6c386-161">Entity Designer 화면에서 빈 공간을 마우스 오른쪽 단추로 클릭 **모델에서 데이터베이스 생성**</span><span class="sxs-lookup"><span data-stu-id="6c386-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="6c386-162">데이터베이스 생성 마법사의 데이터 연결 대화 상자는 선택 표시를 클릭 합니다 **새 연결** 지정 단추 **(localdb)\\mssqllocaldb** 고서버이름에대한 **EnumTest** 데이터베이스 및 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="6c386-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="6c386-163">새 데이터베이스를 만들 것인지 묻는 대화 상자가 팝업을 누릅니다 **예**합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="6c386-164">클릭 **다음** 데이터베이스 만들기 마법사의 요약 및 설정 대화 상자 참고 DDL에 대 한 정의 포함 하지 않는에 표시 되는 데이터베이스를 만드는 중 생성된 된 DDL에 대 한 DDL (데이터 정의 언어)을 생성 하 고는 열거형 형식에 매핑되는 테이블</span><span class="sxs-lookup"><span data-stu-id="6c386-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="6c386-165">클릭 **완료** 마침을 클릭 하면 DDL 스크립트 실행 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="6c386-166">다음을 수행 하는 데이터베이스 만들기 마법사: 엽니다는 **EnumTest.edmx.sql** 추가 연결 문자열 정보를 App.config 파일에는 EDMX의 저장소 스키마 및 매핑 섹션을 생성 하는 T-SQL 편집기에서에서 파일</span><span class="sxs-lookup"><span data-stu-id="6c386-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="6c386-167">T-SQL 편집기에서 마우스 오른쪽 단추를 클릭 하 고 선택 **Execute** 나타나면 서버 대화 상자에는 연결 2 단계의 연결 정보를 입력 하 고 클릭 **연결**</span><span class="sxs-lookup"><span data-stu-id="6c386-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="6c386-168">생성된 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**</span><span class="sxs-lookup"><span data-stu-id="6c386-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="6c386-169">유지 하 고 데이터를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="6c386-170">Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="6c386-171">다음 코드를 Main 함수에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-171">Add the following code into the Main function.</span></span> <span data-ttu-id="6c386-172">새 부서 개체 컨텍스트를 추가 하는 코드입니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="6c386-173">데이터를 저장합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-173">It then saves the data.</span></span> <span data-ttu-id="6c386-174">코드는 또한 여기서 name은 DepartmentNames.English 부서를 반환 하는 LINQ 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

<span data-ttu-id="6c386-175">응용 프로그램을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-175">Compile and run the application.</span></span> <span data-ttu-id="6c386-176">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-176">The program produces the following output:</span></span>

```
DepartmentID: 1 Name: English
```

<span data-ttu-id="6c386-177">데이터베이스에서 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="6c386-178">그런 다음 선택한 테이블에서 마우스 오른쪽 단추를 클릭 **데이터 보기**합니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="6c386-179">요약</span><span class="sxs-lookup"><span data-stu-id="6c386-179">Summary</span></span>

<span data-ttu-id="6c386-180">이 연습에서는 Entity Framework 디자이너를 사용 하 여 열거형 형식을 매핑하는 방법 및 코드의 열거형을 사용 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="6c386-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
