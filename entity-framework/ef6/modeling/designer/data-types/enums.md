---
title: 열거형 지원-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182515"
---
# <a name="enum-support---ef-designer"></a><span data-ttu-id="a8a83-102">열거형 지원-EF 디자이너</span><span class="sxs-lookup"><span data-stu-id="a8a83-102">Enum Support - EF Designer</span></span>
> [!NOTE]
> <span data-ttu-id="a8a83-103">**EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-103">**EF5 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 5.</span></span> <span data-ttu-id="a8a83-104">이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="a8a83-105">이 비디오 및 단계별 연습에서는 Entity Framework Designer에서 열거형 형식을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-105">This video and step-by-step walkthrough shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="a8a83-106">또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-106">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="a8a83-107">이 연습에서는 Model First 사용 하 여 새 데이터베이스를 만들지만 EF Designer를 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 워크플로와 함께 사용 하 여 기존 데이터베이스에 매핑할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-107">This walkthrough will use Model First to create a new database, but the EF Designer can also be used with the [Database First](~/ef6/modeling/designer/workflows/database-first.md) workflow to map to an existing database.</span></span>

<span data-ttu-id="a8a83-108">열거형 지원은 Entity Framework 5에서 도입 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-108">Enum support was introduced in Entity Framework 5.</span></span> <span data-ttu-id="a8a83-109">열거형, 공간 데이터 형식 및 테이블 반환 함수와 같은 새로운 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-109">To use the new features like enums, spatial data types, and table-valued functions, you must target .NET Framework 4.5.</span></span> <span data-ttu-id="a8a83-110">Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-110">Visual Studio 2012 targets .NET 4.5 by default.</span></span>

<span data-ttu-id="a8a83-111">Entity Framework에서 열거형에는 다음과 같은 기본 형식이 있을 수 있습니다. **바이트**, **Int16**, **Int32**, **Int64** 또는 **SByte**입니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-111">In Entity Framework, an enumeration can have the following underlying types: **Byte**, **Int16**, **Int32**, **Int64** , or **SByte**.</span></span>

## <a name="watch-the-video"></a><span data-ttu-id="a8a83-112">비디오 시청</span><span class="sxs-lookup"><span data-stu-id="a8a83-112">Watch the Video</span></span>
<span data-ttu-id="a8a83-113">이 비디오는 Entity Framework Designer에서 열거형 형식을 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-113">This video shows how to use enum types with the Entity Framework Designer.</span></span> <span data-ttu-id="a8a83-114">또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-114">It also demonstrates how to use enums in a LINQ query.</span></span>

<span data-ttu-id="a8a83-115">**제공**: 줄리아 Kornich</span><span class="sxs-lookup"><span data-stu-id="a8a83-115">**Presented By**: Julia Kornich</span></span>

<span data-ttu-id="a8a83-116">**비디오**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv)@NO__T-[1MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span><span class="sxs-lookup"><span data-stu-id="a8a83-116">**Video**: [WMV](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="a8a83-117">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="a8a83-117">Pre-Requisites</span></span>

<span data-ttu-id="a8a83-118">이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-118">You will need to have Visual Studio 2012, Ultimate, Premium, Professional, or Web Express edition installed to complete this walkthrough.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="a8a83-119">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="a8a83-119">Set up the Project</span></span>

1.  <span data-ttu-id="a8a83-120">Visual Studio 2012 열기</span><span class="sxs-lookup"><span data-stu-id="a8a83-120">Open Visual Studio 2012</span></span>
2.  <span data-ttu-id="a8a83-121">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-121">On the **File** menu, point to **New**, and then click **Project**</span></span>
3.  <span data-ttu-id="a8a83-122">왼쪽 창에서 **Visual C @ no__t-1**을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-122">In the left pane, click **Visual C\#**, and then select the **Console** template</span></span>
4.  <span data-ttu-id="a8a83-123">프로젝트 이름으로 **Enumefdesigner** 를 입력 하 고 **확인** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-123">Enter **EnumEFDesigner** as the name of the project and click **OK**</span></span>

## <a name="create-a-new-model-using-the-ef-designer"></a><span data-ttu-id="a8a83-124">EF Designer를 사용 하 여 새 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="a8a83-124">Create a New Model using the EF Designer</span></span>

1.  <span data-ttu-id="a8a83-125">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-125">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**</span></span>
2.  <span data-ttu-id="a8a83-126">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-126">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane</span></span>
3.  <span data-ttu-id="a8a83-127">파일 이름으로 **Enumtestmodel .edmx** 를 입력 한 다음 **추가** 를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-127">Enter **EnumTestModel.edmx** for the file name, and then click **Add**</span></span>
4.  <span data-ttu-id="a8a83-128">엔터티 데이터 모델 마법사 페이지의 Model 콘텐츠 선택 대화 상자에서 **빈 모델** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-128">On the Entity Data Model Wizard page, select **Empty Model** in the Choose Model Contents dialog box</span></span>
5.  <span data-ttu-id="a8a83-129">**마침** 클릭</span><span class="sxs-lookup"><span data-stu-id="a8a83-129">Click **Finish**</span></span>

<span data-ttu-id="a8a83-130">모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-130">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span>

<span data-ttu-id="a8a83-131">마법사에서는 다음 작업을 수행합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-131">The wizard performs the following actions:</span></span>

-   <span data-ttu-id="a8a83-132">개념적 모델, 저장소 모델 및 두 모델 간의 매핑을 정의 하는 EnumTestModel .edmx 파일을 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-132">Generates the EnumTestModel.edmx file that defines the conceptual model, the storage model, and the mapping between them.</span></span> <span data-ttu-id="a8a83-133">생성 된 메타 데이터 파일이 어셈블리에 포함 되도록 .edmx 파일의 메타 데이터 아티팩트 처리 속성을 출력 어셈블리에 포함 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-133">Sets the Metadata Artifact Processing property of the .edmx file to Embed in Output Assembly so the generated metadata files get embedded into the assembly.</span></span>
-   <span data-ttu-id="a8a83-134">다음 어셈블리에 대 한 참조를 추가 합니다. EntityFramework, System.componentmodel 및 System.object를 모두.</span><span class="sxs-lookup"><span data-stu-id="a8a83-134">Adds a reference to the following assemblies: EntityFramework, System.ComponentModel.DataAnnotations, and System.Data.Entity.</span></span>
-   <span data-ttu-id="a8a83-135">EnumTestModel.tt 및 EnumTestModel.Context.tt 파일을 만들고 .edmx 파일 아래에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-135">Creates EnumTestModel.tt and EnumTestModel.Context.tt files and adds them under the .edmx file.</span></span> <span data-ttu-id="a8a83-136">이러한 T4 템플릿 파일은 .edmx 모델의 엔터티에 매핑되는 DbContext 파생 형식 및 POCO 형식을 정의 하는 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-136">These T4 template files generate the code that defines the DbContext derived type and POCO types that map to the entities in the .edmx model.</span></span>

## <a name="add-a-new-entity-type"></a><span data-ttu-id="a8a83-137">새 엔터티 형식 추가</span><span class="sxs-lookup"><span data-stu-id="a8a83-137">Add a New Entity Type</span></span>

1.  <span data-ttu-id="a8a83-138">디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 엔터티**를 선택 합니다. 새 엔터티 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-138">Right-click an empty area of the design surface, select **Add -&gt; Entity**, the New Entity dialog box appears</span></span>
2.  <span data-ttu-id="a8a83-139">유형 이름에 대해 **학과** 를 지정 하 고 키 속성 이름에 **DepartmentID** 를 지정 하 고 형식을 **Int32** 로 그대로 둡니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-139">Specify **Department** for the type name and specify **DepartmentID** for the key property name, leave the type as **Int32**</span></span>
3.  <span data-ttu-id="a8a83-140">**확인** 을 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-140">Click **OK**</span></span>
4.  <span data-ttu-id="a8a83-141">엔터티를 마우스 오른쪽 단추로 클릭 하 고 **추가 새로 만들기-&gt; 스칼라 속성** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-141">Right-click the entity and select **Add New -&gt; Scalar Property**</span></span>
5.  <span data-ttu-id="a8a83-142">새 속성의 이름을 **Name** 으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-142">Rename the new property to **Name**</span></span>
6.  <span data-ttu-id="a8a83-143">새 속성의 형식을 **int32** (기본적으로 새 속성은 문자열 형식)로 변경 하 여 형식을 변경 하 고 속성 창를 연 다음 형식 속성을 **int32** 로 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-143">Change the type of the new property to **Int32** (by default, the new property is of String type) To change the type, open the Properties window and change the Type property to **Int32**</span></span>
7.  <span data-ttu-id="a8a83-144">다른 스칼라 속성을 추가 하 고 **예산**으로 이름을 변경 하 고, 형식을 **10 진수로** 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-144">Add another scalar property and rename it to **Budget**, change the type to **Decimal**</span></span>

## <a name="add-an-enum-type"></a><span data-ttu-id="a8a83-145">열거형 형식 추가</span><span class="sxs-lookup"><span data-stu-id="a8a83-145">Add an Enum Type</span></span>

1.  <span data-ttu-id="a8a83-146">Entity Framework Designer에서 Name 속성을 마우스 오른쪽 단추로 클릭 하 고 **열거형으로 변환** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-146">In the Entity Framework Designer, right-click the Name property, select **Convert to enum**</span></span>

    ![열거형으로 변환](~/ef6/media/converttoenum.png)

2.  <span data-ttu-id="a8a83-148">**열거 추가** 대화 상자에서 Enum 형식 이름에 **DepartmentNames** 를 입력 하 고 기본 형식을 **Int32**로 변경한 후 다음 멤버를 형식에 추가 합니다. 영어, 수학 및 경제</span><span class="sxs-lookup"><span data-stu-id="a8a83-148">In the **Add Enum** dialog box type **DepartmentNames** for the Enum Type Name, change the Underlying Type to **Int32**, and then add the following members to the type: English, Math, and Economics</span></span>

    ![열거형 형식 추가](~/ef6/media/addenumtype.png)

3.  <span data-ttu-id="a8a83-150">**확인을** 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-150">Press **OK**</span></span>
4.  <span data-ttu-id="a8a83-151">모델을 저장 하 고 프로젝트를 빌드합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-151">Save the model and build the project</span></span>
    > [!NOTE]
    > <span data-ttu-id="a8a83-152">빌드하면 매핑되지 않은 엔터티와 연결에 대 한 경고가 오류 목록 표시 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-152">When you build, warnings about unmapped entities and associations may appear in the Error List.</span></span> <span data-ttu-id="a8a83-153">모델에서 데이터베이스를 생성 하는 것을 선택한 후 오류가 사라집니다. 이러한 경고는 무시 해도 됩니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-153">You can ignore these warnings because after we choose to generate the database from the model, the errors will go away.</span></span>

<span data-ttu-id="a8a83-154">속성 창를 살펴보면 이름 속성의 형식이 **DepartmentNames** 로 변경 되 고 새로 추가 된 열거형 형식이 형식 목록에 추가 된 것을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-154">If you look at the Properties window, you will notice that the type of the Name property was changed to **DepartmentNames** and the newly added enum type was added to the list of types.</span></span>

<span data-ttu-id="a8a83-155">모델 브라우저 창으로 전환 하면 형식이 열거형 형식 노드에도 추가 된 것을 확인할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-155">If you switch to the Model Browser window, you will see that the type was also added to the Enum Types node.</span></span>

![모델 브라우저](~/ef6/media/modelbrowser.png)

>[!NOTE]
> <span data-ttu-id="a8a83-157">마우스 오른쪽 단추를 클릭 하 고 **열거형 형식 추가**를 선택 하 여이 창에서 새 열거형 형식을 추가할 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-157">You can also add new enum types from this window by clicking the right mouse button and selecting **Add Enum Type**.</span></span> <span data-ttu-id="a8a83-158">형식이 생성 되 면 형식 목록에 표시 되 고 속성에 연결할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-158">Once the type is created it will appear in the list of types and you would be able to associate with a property</span></span>

## <a name="generate-database-from-model"></a><span data-ttu-id="a8a83-159">모델에서 데이터베이스 생성</span><span class="sxs-lookup"><span data-stu-id="a8a83-159">Generate Database from Model</span></span>

<span data-ttu-id="a8a83-160">이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-160">Now we can generate a database that is based on the model.</span></span>

1.  <span data-ttu-id="a8a83-161">Entity Designer 화면의 빈 공간을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-161">Right-click an empty space on the Entity Designer surface and select **Generate Database from Model**</span></span>
2.  <span data-ttu-id="a8a83-162">데이터베이스 생성 마법사의 데이터 연결 선택 대화 상자가 표시 되 면 **새 연결** 단추를 클릭 하 여 **localdb (2mssqllocaldb @no__t)** 를 클릭 하 고 데이터베이스에 대 한 **Enumtest** 를 지정 하 고 **확인** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-162">The Choose Your Data Connection Dialog Box of the Generate Database Wizard is displayed Click the **New Connection** button Specify **(localdb)\\mssqllocaldb** for the server name and **EnumTest** for the database and click **OK**</span></span>
3.  <span data-ttu-id="a8a83-163">새 데이터베이스를 만들지 여부를 묻는 대화 상자가 표시 되 면 **예**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-163">A dialog asking if you want to create a new database will pop up, click **Yes**.</span></span>
4.  <span data-ttu-id="a8a83-164">**다음** 을 클릭 하 고 데이터베이스 만들기 마법사에서 데이터베이스를 만들기 위한 ddl (데이터 정의 언어)을 생성 합니다. 생성 된 Ddl은 요약 및 설정 대화 상자에 표시 되며, ddl에는에 매핑되는 테이블에 대 한 정의가 포함 되지 않습니다. 열거형 형식</span><span class="sxs-lookup"><span data-stu-id="a8a83-164">Click **Next** and the Create Database Wizard generates data definition language (DDL) for creating a database The generated DDL is displayed in the Summary and Settings Dialog Box Note, that the DDL does not contain a definition for a table that maps to the enumeration type</span></span>
5.  <span data-ttu-id="a8a83-165">마침 **을 클릭 하면 DDL** 스크립트가 실행 되지 않습니다 .를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-165">Click **Finish** Clicking Finish does not execute the DDL script.</span></span>
6.  <span data-ttu-id="a8a83-166">데이터베이스 만들기 마법사는 다음을 수행 합니다. T-sql 편집기에서 **enumtest** 를 엽니다. .edmx 파일의 저장소 스키마 및 매핑 섹션에서 app.config 파일에 연결 문자열 정보를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-166">The Create Database Wizard does the following: Opens the **EnumTest.edmx.sql** in T-SQL Editor Generates the store schema and mapping sections of the EDMX file Adds connection string information to the App.config file</span></span>
7.  <span data-ttu-id="a8a83-167">T-sql 편집기에서 마우스 오른쪽 단추를 클릭 하 고 서버에 연결 대화 상자가 나타납니다 .를 선택 하 고 2 단계의 연결 정보를 입력 한 다음 **연결** 을 **클릭 합니다.**</span><span class="sxs-lookup"><span data-stu-id="a8a83-167">Click the right mouse button in T-SQL Editor and select **Execute** The Connect to Server dialog appears, enter the connection information from step 2 and click **Connect**</span></span>
8.  <span data-ttu-id="a8a83-168">생성 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 고침** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-168">To view the generated schema, right-click on the database name in SQL Server Object Explorer and select **Refresh**</span></span>

## <a name="persist-and-retrieve-data"></a><span data-ttu-id="a8a83-169">데이터 유지 및 검색</span><span class="sxs-lookup"><span data-stu-id="a8a83-169">Persist and Retrieve Data</span></span>

<span data-ttu-id="a8a83-170">Main 메서드가 정의 된 Program.cs 파일을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-170">Open the Program.cs file where the Main method is defined.</span></span> <span data-ttu-id="a8a83-171">Main 함수에 다음 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-171">Add the following code into the Main function.</span></span> <span data-ttu-id="a8a83-172">이 코드는 컨텍스트에 새 부서 개체를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-172">The code adds a new Department object to the context.</span></span> <span data-ttu-id="a8a83-173">그런 다음 데이터를 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-173">It then saves the data.</span></span> <span data-ttu-id="a8a83-174">또한이 코드는 이름이 DepartmentNames 인 부서를 반환 하는 LINQ 쿼리를 실행 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-174">The code also executes a LINQ query that returns a Department where the name is DepartmentNames.English.</span></span>

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

<span data-ttu-id="a8a83-175">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-175">Compile and run the application.</span></span> <span data-ttu-id="a8a83-176">프로그램에서는 다음이 출력됩니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-176">The program produces the following output:</span></span>

```console
DepartmentID: 1 Name: English
```

<span data-ttu-id="a8a83-177">데이터베이스의 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 고침**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-177">To view data in the database, right-click on the database name in SQL Server Object Explorer and select **Refresh**.</span></span> <span data-ttu-id="a8a83-178">그런 다음 테이블에서 마우스 오른쪽 단추를 클릭 하 고 **데이터 보기**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-178">Then, click the right mouse button on the table and select **View Data**.</span></span>

## <a name="summary"></a><span data-ttu-id="a8a83-179">요약</span><span class="sxs-lookup"><span data-stu-id="a8a83-179">Summary</span></span>

<span data-ttu-id="a8a83-180">이 연습에서는 Entity Framework Designer를 사용 하 여 열거형 형식을 매핑하는 방법 및 코드에서 열거형을 사용 하는 방법을 살펴보았습니다.</span><span class="sxs-lookup"><span data-stu-id="a8a83-180">In this walkthrough we looked at how to map enum types using the Entity Framework Designer and how to use enums in code.</span></span> 
