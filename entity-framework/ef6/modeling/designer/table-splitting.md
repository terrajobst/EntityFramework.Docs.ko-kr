---
title: 디자이너 테이블 분할-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 452f17c3-9f26-4de4-9894-8bc036e23b0f
ms.openlocfilehash: f5e7532e6c0b473d8ce77cbd11e3e673b0af6cbe
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921786"
---
# <a name="designer-table-splitting"></a><span data-ttu-id="0062e-102">디자이너 테이블 분할</span><span class="sxs-lookup"><span data-stu-id="0062e-102">Designer Table Splitting</span></span>
<span data-ttu-id="0062e-103">이 연습에서는 Entity Framework Designer (EF Designer)를 사용 하 여 모델을 수정 하 여 여러 엔터티 형식을 단일 테이블에 매핑하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-103">This walkthrough shows how to map multiple entity types to a single table by modifying a model with the Entity Framework Designer (EF Designer).</span></span>

<span data-ttu-id="0062e-104">테이블 분할을 사용 하는 한 가지 이유는 지연 로드를 사용 하 여 개체를 로드할 때 일부 속성의 로드를 지연 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-104">One reason you may want to use table splitting is delaying the loading of some properties when using lazy loading to load your objects.</span></span><span data-ttu-id="0062e-105"> 매우 많은 양의 데이터를 포함 하는 속성을 별도의 엔터티로 분리 하 고 필요한 경우에만 로드할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-105"> You can separate the properties that might contain very large amount of data into a separate entity and only load it when required.</span></span>

<span data-ttu-id="0062e-106">다음 그림에서는 EF Designer를 사용할 때 사용 되는 기본 창을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-106">The following image shows the main windows that are used when working with the EF Designer.</span></span>

![EF 디자이너](~/ef6/media/efdesigner.png)

## <a name="prerequisites"></a><span data-ttu-id="0062e-108">전제 조건</span><span class="sxs-lookup"><span data-stu-id="0062e-108">Prerequisites</span></span>

<span data-ttu-id="0062e-109">이 연습을 완료하려면 다음 사항이 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-109">To complete this walkthrough, you will need:</span></span>

- <span data-ttu-id="0062e-110">최신 버전의 Visual Studio입니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-110">A recent version of Visual Studio.</span></span>
- <span data-ttu-id="0062e-111">[School 예제 데이터베이스](~/ef6/resources/school-database.md).</span><span class="sxs-lookup"><span data-stu-id="0062e-111">The [School sample database](~/ef6/resources/school-database.md).</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="0062e-112">프로젝트 설정</span><span class="sxs-lookup"><span data-stu-id="0062e-112">Set up the Project</span></span>

<span data-ttu-id="0062e-113">이 연습에서는 Visual Studio 2012을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-113">This walkthrough is using Visual Studio 2012.</span></span>

-   <span data-ttu-id="0062e-114">Visual Studio 2012을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-114">Open Visual Studio 2012.</span></span>
-   <span data-ttu-id="0062e-115">**파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트**를 클릭합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-115">On the **File** menu, point to **New**, and then click **Project**.</span></span>
-   <span data-ttu-id="0062e-116">왼쪽 창에서 Visual C\#를 클릭 한 다음 콘솔 응용 프로그램 템플릿을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-116">In the left pane, click Visual C\#, and then select the Console Application template.</span></span>
-   <span data-ttu-id="0062e-117">프로젝트 이름으로 **표** 를 입력 하 고 **확인**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-117">Enter **TableSplittingSample** as the name of the project and click **OK**.</span></span>

## <a name="create-a-model-based-on-the-school-database"></a><span data-ttu-id="0062e-118">School 데이터베이스를 기반으로 모델 만들기</span><span class="sxs-lookup"><span data-stu-id="0062e-118">Create a Model based on the School Database</span></span>

-   <span data-ttu-id="0062e-119">솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-119">Right-click the project name in Solution Explorer, point to **Add**, and then click **New Item**.</span></span>
-   <span data-ttu-id="0062e-120">왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-120">Select **Data** from the left menu and then select **ADO.NET Entity Data Model** in the Templates pane.</span></span>
-   <span data-ttu-id="0062e-121">파일 이름에 대해 파일 이름에 대해 파일 이름으로 **.edmx** 를 입력 한 다음 **추가**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-121">Enter **TableSplittingModel.edmx** for the file name, and then click **Add**.</span></span>
-   <span data-ttu-id="0062e-122">Model 콘텐츠 선택 대화 상자에서 **데이터베이스에서 생성**을 선택 하 고 다음을 클릭 **합니다.**</span><span class="sxs-lookup"><span data-stu-id="0062e-122">In the Choose Model Contents dialog box, select **Generate from database**, and then click **Next.**</span></span>
-   <span data-ttu-id="0062e-123">새 연결을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-123">Click New Connection.</span></span> <span data-ttu-id="0062e-124">연결 속성 대화 상자에서 서버 이름 (예: **(localdb)\\mssqllocaldb**)을 입력 하 고, 인증 방법을 선택 하 고, 데이터베이스 이름으로 **School** 을 입력 한 다음, **확인**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-124">In the Connection Properties dialog box, enter the server name (for example, **(localdb)\\mssqllocaldb**), select the authentication method, type **School** for the database name, and then click **OK**.</span></span>
    <span data-ttu-id="0062e-125">데이터 연결 선택 대화 상자가 데이터베이스 연결 설정으로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-125">The Choose Your Data Connection dialog box is updated with your database connection setting.</span></span>
-   <span data-ttu-id="0062e-126">데이터베이스 개체 선택 대화 상자에서 **테이블** 노드를 펼침 **Person** 테이블을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-126">In the Choose Your Database Objects dialog box, unfold the **Tables** node and check the **Person** table.</span></span> <span data-ttu-id="0062e-127">그러면 지정 된 테이블이 **School** 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-127">This will add the specified table to the **School** model.</span></span>
-   <span data-ttu-id="0062e-128"> **마침**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-128">Click **Finish**.</span></span>

<span data-ttu-id="0062e-129">모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-129">The Entity Designer, which provides a design surface for editing your model, is displayed.</span></span> <span data-ttu-id="0062e-130"> **데이터베이스 개체** 선택 대화 상자에서 선택한 모든 개체가 모델에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-130">All the objects that you selected in the **Choose Your Database Objects** dialog box are added to the model.</span></span>

## <a name="map-two-entities-to-a-single-table"></a><span data-ttu-id="0062e-131">단일 테이블에 두 엔터티 매핑</span><span class="sxs-lookup"><span data-stu-id="0062e-131">Map Two Entities to a Single Table</span></span>

<span data-ttu-id="0062e-132">이 섹션에서는 **Person** 엔터티를 두 엔터티로 분할 한 다음 단일 테이블에 매핑합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-132">In this section you will split the **Person** entity into two entities and then map them to a single table.</span></span>

> [!NOTE]
> <span data-ttu-id="0062e-133">**Person** 엔터티는 많은 양의 데이터를 포함할 수 있는 속성을 포함 하지 않습니다. 이는 예제로만 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-133">The **Person** entity does not contain any properties that may contain large amount of data; it is just used as an example.</span></span>

-   <span data-ttu-id="0062e-134">디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **새로 추가**를 가리킨 다음 **엔터티**를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-134">Right-click an empty area of the design surface, point to **Add New**, and click **Entity**.</span></span>
    <span data-ttu-id="0062e-135"> **새 엔터티** 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-135">The **New Entity** dialog box appears.</span></span>
-   <span data-ttu-id="0062e-136"> **엔터티 이름에 대해** PersonID **를 입력 하**고 **키 속성** 이름에 대해을 입력 합니다.  </span><span class="sxs-lookup"><span data-stu-id="0062e-136">Type **HireInfo** for the **Entity name** and **PersonID** for the **Key Property** name.</span></span>
-   <span data-ttu-id="0062e-137"> **확인을**클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-137">Click **OK**.</span></span>
-   <span data-ttu-id="0062e-138">새 엔터티 형식이 만들어져 디자인 화면에 표시됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-138">A new entity type is created and displayed on the design surface.</span></span>
-   <span data-ttu-id="0062e-139">\ \*\*\**  \ \*\*\** Person엔터티 형식의 HireDate 속성을 선택 하 고 **ctrl + X** 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-139">Select the **HireDate** property of the **Person** entity type and press **Ctrl+X** keys.</span></span>
-   <span data-ttu-id="0062e-140">아웃 **정보** 엔터티를 선택 하 고 **ctrl + V** 키를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-140">Select the **HireInfo** entity and press **Ctrl+V** keys.</span></span>
-   <span data-ttu-id="0062e-141">**Person** 과 **정보**간의 연결을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-141">Create an association between **Person** and **HireInfo**.</span></span> <span data-ttu-id="0062e-142">이렇게 하려면 디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **새로 추가**를 가리킨 다음 **연결**을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-142">To do this, right-click an empty area of the design surface, point to **Add New**, and click **Association**.</span></span>
-   <span data-ttu-id="0062e-143">\ \*\*\** 연결 추가 대화 상자가 나타납니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-143">The **Add Association** dialog box appears.</span></span> <span data-ttu-id="0062e-144">**PersonHireInfo** 이름은 기본적으로 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-144">The **PersonHireInfo** name is given by default.</span></span>
-   <span data-ttu-id="0062e-145">관계의 양쪽 끝에 복합성 **1 (1)** 을 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-145">Specify multiplicity **1(One)** on both ends of the relationship.</span></span>
-   <span data-ttu-id="0062e-146">**확인**을 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-146">Press **OK**.</span></span>

<span data-ttu-id="0062e-147">다음 단계를 수행 하려면 **매핑 정보** 창이 필요 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-147">The next step requires the **Mapping Details** window.</span></span> <span data-ttu-id="0062e-148">이 창이 표시 되지 않으면 디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **매핑 정보**를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-148">If you cannot see this window, right-click the design surface and select **Mapping Details**.</span></span>

-   <span data-ttu-id="0062e-149">To **reinfo** 엔터티 유형을 선택 하 고 **매핑 정보**  **&lt;창에서테이블&gt;또는 뷰 추가를**클릭합니다 .</span><span class="sxs-lookup"><span data-stu-id="0062e-149">Select the **HireInfo** entity type and click **&lt;Add a Table or View&gt;** in the **Mapping Details** window.</span></span>
-   <span data-ttu-id="0062e-150">**&lt;테이블 또는 뷰&gt;** 필드추가드롭다운목록에서Person을선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-150">Select **Person** from the **&lt;Add a Table or View&gt;** field drop-down list.</span></span> <span data-ttu-id="0062e-151">목록에는 선택한 엔터티를 매핑할 수 있는 테이블 또는 뷰가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-151">The list contains tables or views to which the selected entity can be mapped.</span></span>
    <span data-ttu-id="0062e-152">적절 한 속성은 기본적으로 매핑되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-152">The appropriate properties should be mapped by default.</span></span>

    ![매핑](~/ef6/media/mapping.png)

-   <span data-ttu-id="0062e-154">디자인 화면에서 **PersonHireInfo** 연결을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-154">Select the **PersonHireInfo** association on the design surface.</span></span>
-   <span data-ttu-id="0062e-155">디자인 화면에서 연결을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-155">Right-click the association on the design surface and select **Properties**.</span></span>
-   <span data-ttu-id="0062e-156">**속성** 창에서 **참조 제약 조건** 속성을 선택 하 고 줄임표 단추를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-156">In the **Properties** window, select the **Referential Constraints** property and click the ellipses button.</span></span>
-   <span data-ttu-id="0062e-157">**주** 드롭다운 목록에서 **Person** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-157">Select **Person** from the **Principal** drop-down list.</span></span>
-   <span data-ttu-id="0062e-158">**확인**을 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-158">Press **OK**.</span></span>

 

## <a name="use-the-model"></a><span data-ttu-id="0062e-159">모델 사용</span><span class="sxs-lookup"><span data-stu-id="0062e-159">Use the Model</span></span>

-   <span data-ttu-id="0062e-160">Main 메서드에 다음 코드를 붙여 넣습니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-160">Paste the following code in the Main method.</span></span>

``` csharp
    using (var context = new SchoolEntities())
    {
        Person person = new Person()
        {
            FirstName = "Kimberly",
            LastName = "Morgan",
            Discriminator = "Instructor",
        };

        person.HireInfo = new HireInfo()
        {
            HireDate = DateTime.Now
        };

        // Add the new person to the context.
        context.People.Add(person);

        // Insert a row into the Person table.  
        context.SaveChanges();

        // Execute a query against the Person table.
        // The query returns columns that map to the Person entity.
        var existingPerson = context.People.FirstOrDefault();

        // Execute a query against the Person table.
        // The query returns columns that map to the Instructor entity.
        var hireInfo = existingPerson.HireInfo;

        Console.WriteLine("{0} was hired on {1}",
            existingPerson.LastName, hireInfo.HireDate);
    }
```
-   <span data-ttu-id="0062e-161">애플리케이션을 컴파일하고 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-161">Compile and run the application.</span></span>

<span data-ttu-id="0062e-162">다음 T-sql 문은이 응용 프로그램을 실행 한 결과로 **School** 데이터베이스에 대해 실행 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-162">The following T-SQL statements were executed against the **School** database as a result of running this application.</span></span> 

-   <span data-ttu-id="0062e-163">다음 **INSERT** 는 컨텍스트 실행의 결과로 실행 되었습니다. SaveChanges () 및 **Person** **및 개체의 데이터를 결합** 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-163">The following **INSERT** was executed as a result of executing context.SaveChanges() and combines data from the **Person** and **HireInfo** entities</span></span>

    ![Insert](~/ef6/media/insert.png)

-   <span data-ttu-id="0062e-165">다음 **SELECT** 는 컨텍스트 실행의 결과로 실행 되었습니다. FirstOrDefault ()를 선택 하 고 **Person** 으로 매핑된 열만 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-165">The following **SELECT** was executed as a result of executing context.People.FirstOrDefault() and selects just the columns mapped to **Person**</span></span>

    ![1 선택](~/ef6/media/select1.png)

-   <span data-ttu-id="0062e-167">다음 **SELECT** 는 탐색 속성 existingperson에 액세스 한 결과로 실행 되었습니다. 강사는 **속성에 매핑된** 열만 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="0062e-167">The following **SELECT** was executed as a result of accessing the navigation property existingPerson.Instructor and selects just the columns mapped to **HireInfo**</span></span>

    ![2 선택](~/ef6/media/select2.png)
