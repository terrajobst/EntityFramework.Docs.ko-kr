---
title: WinForms-EF6 사용 하 여 데이터 바인딩
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 48e6d997875a25a5954484f854953df69a267d05
ms.sourcegitcommit: 8d04a2ad98036f32ca70c77ce3040c5edb1cdf82
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44384854"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="c07b3-102">WinForms 사용 하 여 데이터 바인딩</span><span class="sxs-lookup"><span data-stu-id="c07b3-102">Databinding with WinForms</span></span>
<span data-ttu-id="c07b3-103">이 단계별 연습에는 POCO 형식 "마스터-세부 정보" 형태로 Window Forms (WinForms) 컨트롤에 바인딩하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="c07b3-104">Entity Framework를 사용 하 여 데이터베이스에서 데이터를 사용 하 여 개체를 채우기, 변경 내용 추적 및 데이터베이스에 데이터를 유지 하는 응용 프로그램입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="c07b3-105">모델에 일 대 다 관계에 참여 하는 두 가지 형식을 정의 합니다: 범주 (주\\마스터) 및 제품 (종속\\세부 정보).</span><span class="sxs-lookup"><span data-stu-id="c07b3-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="c07b3-106">그런 다음 Visual Studio 도구는 WinForms 컨트롤 모델에서 정의 된 형식에 바인딩할 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="c07b3-107">WinForms 데이터 바인딩 프레임 워크에 관련 된 개체 사이 탐색할 수 있습니다: 세부 정보 뷰에서 해당 자식 데이터를 사용 하 여 업데이트를 사용 하면 마스터 뷰에 행을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="c07b3-108">스크린샷 및이 연습의 코드 샘플은 Visual Studio 2013에서 가져옵니다 하지만 Visual Studio 2012 또는 Visual Studio 2010을 사용 하 여이 연습을 완료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="c07b3-109">필수 조건</span><span class="sxs-lookup"><span data-stu-id="c07b3-109">Pre-Requisites</span></span>

<span data-ttu-id="c07b3-110">이 연습을 완료 하려면 Visual Studio 2012 또는 Visual Studio 2010 설치, Visual Studio 2013이 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="c07b3-111">Visual Studio 2010을 사용 하는 경우 NuGet을 설치 하려면 수도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="c07b3-112">자세한 내용은 [NuGet 설치](http://docs.nuget.org/docs/start-here/installing-nuget)합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="c07b3-113">응용 프로그램 만들기</span><span class="sxs-lookup"><span data-stu-id="c07b3-113">Create the Application</span></span>

-   <span data-ttu-id="c07b3-114">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-114">Open Visual Studio</span></span>
-   <span data-ttu-id="c07b3-115">**파일만&gt; 새로운 기능-&gt; 프로젝트...**</span><span class="sxs-lookup"><span data-stu-id="c07b3-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="c07b3-116">선택 **Windows** 왼쪽된 창에서와 **Windows FormsApplication** 오른쪽 창에서</span><span class="sxs-lookup"><span data-stu-id="c07b3-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="c07b3-117">입력 **WinFormswithEFSample** 이름으로</span><span class="sxs-lookup"><span data-stu-id="c07b3-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="c07b3-118">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="c07b3-119">Entity Framework NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="c07b3-120">솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **WinFormswithEFSample** 프로젝트</span><span class="sxs-lookup"><span data-stu-id="c07b3-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="c07b3-121">선택 **NuGet 패키지 관리...**</span><span class="sxs-lookup"><span data-stu-id="c07b3-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="c07b3-122">NuGet 패키지 관리 대화 상자에서 선택 합니다 **Online** 탭을 선택 합니다 **EntityFramework** 패키지</span><span class="sxs-lookup"><span data-stu-id="c07b3-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="c07b3-123">클릭 **설치**</span><span class="sxs-lookup"><span data-stu-id="c07b3-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="c07b3-124">EntityFramework 어셈블리 외에도 System.ComponentModel.DataAnnotations에 대 한 참조도 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="c07b3-125">프로젝트 System.Data.Entity에 대 한 참조가 있으면 다음 제거 됩니다 EntityFramework 패키지를 설치한 경우.</span><span class="sxs-lookup"><span data-stu-id="c07b3-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="c07b3-126">System.Data.Entity 어셈블리는 Entity Framework 6 응용 프로그램에 더 이상 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="c07b3-127">컬렉션에 대 한 IListSource 구현</span><span class="sxs-lookup"><span data-stu-id="c07b3-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="c07b3-128">컬렉션 속성에는 Windows Forms를 사용 하는 경우 정렬 된 양방향 데이터 바인딩을 사용할 수 있도록 IListSource 인터페이스를 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="c07b3-129">이렇게 하려면 IListSource 기능을 추가 하는 ObservableCollection 확장 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="c07b3-130">추가 된 **ObservableListSource** 프로젝트에 클래스:</span><span class="sxs-lookup"><span data-stu-id="c07b3-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="c07b3-131">프로젝트 이름을 마우스 오른쪽 단추로 클릭</span><span class="sxs-lookup"><span data-stu-id="c07b3-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="c07b3-132">선택 **추가-&gt; 새 항목**</span><span class="sxs-lookup"><span data-stu-id="c07b3-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="c07b3-133">선택 **클래스** enter **ObservableListSource** 클래스 이름</span><span class="sxs-lookup"><span data-stu-id="c07b3-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="c07b3-134">다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="c07b3-135">*이 클래스를 사용 하면 양방향 데이터 바인딩 뿐만 아니라 정렬 합니다. 클래스는 ObservableCollection에서 파생 됩니다&lt;T&gt; IListSource의 명시적 구현을 추가 합니다. IListSource GetList() 메서드의 ObservableCollection와 동기화 상태로 유지 되는 IBindingList 구현을 반환 하도록 구현 됩니다. IBindingList 구현 ToBindingList에서 생성 된 정렬을 지원 합니다. ToBindingList 확장 메서드는 EntityFramework 어셈블리에 정의 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="c07b3-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extention method is defined in the EntityFramework assembly.*</span></span>

``` csharp
    using System.Collections;
    using System.Collections.Generic;
    using System.Collections.ObjectModel;
    using System.ComponentModel;
    using System.Diagnostics.CodeAnalysis;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public class ObservableListSource<T> : ObservableCollection<T>, IListSource
            where T : class
        {
            private IBindingList _bindingList;

            bool IListSource.ContainsListCollection { get { return false; } }

            IList IListSource.GetList()
            {
                return _bindingList ?? (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a><span data-ttu-id="c07b3-136">모델 정의</span><span class="sxs-lookup"><span data-stu-id="c07b3-136">Define a Model</span></span>

<span data-ttu-id="c07b3-137">이 연습에서는 있습니다 Code First 또는 EF 디자이너를 사용 하 여 모델을 구현 하도록 선택 했습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="c07b3-138">다음 두 섹션 중 하나를 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="c07b3-139">옵션 1: Code First를 사용 하 여 모델 정의</span><span class="sxs-lookup"><span data-stu-id="c07b3-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="c07b3-140">이 섹션에는 모델과 Code First를 사용 하 여 연결된 된 데이터베이스를 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="c07b3-141">다음 섹션을 건너뜁니다 (**옵션 2: Database First를 사용 하 여 모델 정의)** 사용 하려는 Database First 순서를 반대로 바꿀 경우 EF 디자이너를 사용 하 여 데이터베이스에서 모델을 설계 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="c07b3-142">Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="c07b3-143">새 **제품** 프로젝트에 클래스</span><span class="sxs-lookup"><span data-stu-id="c07b3-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="c07b3-144">다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-144">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }
```

-   <span data-ttu-id="c07b3-145">추가 된 **범주** 프로젝트에 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="c07b3-146">다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-146">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace WinFormswithEFSample
    {
        public class Category
        {
            private readonly ObservableListSource<Product> _products =
                    new ObservableListSource<Product>();

            public int CategoryId { get; set; }
            public string Name { get; set; }
            public virtual ObservableListSource<Product> Products { get { return _products; } }
        }
    }
```

<span data-ttu-id="c07b3-147">파생 된 클래스를 정의 해야 엔터티를 정의 하는 것 외에도 **DbContext** 노출 **DbSet&lt;TEntity&gt;**  속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="c07b3-148">합니다 **DbSet** 모델에 포함 하려는 유형을 알고 있어야 하는 컨텍스트 속성을 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="c07b3-149">합니다 **DbContext** 하 고 **DbSet** 형식은 EntityFramework 어셈블리에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="c07b3-150">런타임에 데이터베이스에서 데이터를 사용 하 여 개체를 채우는 포함 하는 엔터티 개체를 관리 하는 파생 된 DbContext 형식의 인스턴스, 데이터베이스, 추적 및 저장 데이터를 변경 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="c07b3-151">새 **ProductContext** 프로젝트에 클래스입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="c07b3-152">다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-152">Replace the code generated by default with the following code:</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;
    using System.Text;

    namespace WinFormswithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

<span data-ttu-id="c07b3-153">프로젝트를 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="c07b3-154">Database First를 사용 하 여 모델을 정의 하는 옵션 2:</span><span class="sxs-lookup"><span data-stu-id="c07b3-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="c07b3-155">이 섹션에서는 Database First 리버스 엔지니어링을 사용 하는 방법을 EF 디자이너를 사용 하 여 데이터베이스에서 모델을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="c07b3-156">이전 섹션을 완료 하는 경우 (**옵션 1: Code First를 사용 하 여 모델 정의)**, 그런 다음이 섹션을 건너뛰고 바로 이동 합니다 **지연 로드** 섹션입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="c07b3-157">기존 데이터베이스를 만들려면</span><span class="sxs-lookup"><span data-stu-id="c07b3-157">Create an Existing Database</span></span>

<span data-ttu-id="c07b3-158">일반적으로 이미 만들어집니다, 기존 데이터베이스를 대상으로 할 때 있지만이 연습에 액세스 하려면 데이터베이스를 만들려고 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="c07b3-159">Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="c07b3-160">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="c07b3-161">Visual Studio 2012를 사용 하는 경우를 만들 수는 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="c07b3-162">데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="c07b3-163">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="c07b3-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="c07b3-164">마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**</span><span class="sxs-lookup"><span data-stu-id="c07b3-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="c07b3-165">Microsoft SQL Server 데이터 원본으로 선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않은 경우</span><span class="sxs-lookup"><span data-stu-id="c07b3-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![데이터 소스 변경](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="c07b3-167">LocalDB 또는 어느에 따라 설치한 SQL Express에 연결 하 고 입력 **제품** 데이터베이스 이름으로</span><span class="sxs-lookup"><span data-stu-id="c07b3-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![연결 LocalDB를 추가 합니다.](~/ef6/media/addconnectionlocaldb.png)

    ![연결 Express 추가](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="c07b3-170">선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**</span><span class="sxs-lookup"><span data-stu-id="c07b3-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![데이터베이스 만들기](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="c07b3-172">새 데이터베이스 이제 서버 탐색기에서 마우스 나타나고 선택 **새 쿼리**</span><span class="sxs-lookup"><span data-stu-id="c07b3-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="c07b3-173">새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**</span><span class="sxs-lookup"><span data-stu-id="c07b3-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

``` SQL
    CREATE TABLE [dbo].[Categories] (
        [CategoryId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        CONSTRAINT [PK_dbo.Categories] PRIMARY KEY ([CategoryId])
    )

    CREATE TABLE [dbo].[Products] (
        [ProductId] [int] NOT NULL IDENTITY,
        [Name] [nvarchar](max),
        [CategoryId] [int] NOT NULL,
        CONSTRAINT [PK_dbo.Products] PRIMARY KEY ([ProductId])
    )

    CREATE INDEX [IX_CategoryId] ON [dbo].[Products]([CategoryId])

    ALTER TABLE [dbo].[Products] ADD CONSTRAINT [FK_dbo.Products_dbo.Categories_CategoryId] FOREIGN KEY ([CategoryId]) REFERENCES [dbo].[Categories] ([CategoryId]) ON DELETE CASCADE
```

#### <a name="reverse-engineer-model"></a><span data-ttu-id="c07b3-174">모델 리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="c07b3-174">Reverse Engineer Model</span></span>

<span data-ttu-id="c07b3-175">모델을 만들려면 Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="c07b3-176">**프로젝트-&gt; 새 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="c07b3-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="c07b3-177">선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**</span><span class="sxs-lookup"><span data-stu-id="c07b3-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="c07b3-178">입력 **ProductModel** 이름과 클릭 **확인**</span><span class="sxs-lookup"><span data-stu-id="c07b3-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="c07b3-179">그러면는 **엔터티 데이터 모델 마법사**</span><span class="sxs-lookup"><span data-stu-id="c07b3-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="c07b3-180">선택 **데이터베이스에서 생성** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="c07b3-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="c07b3-182">첫 번째 섹션에서 만든 데이터베이스에 연결을 선택, 입력 **ProductContext** 연결 문자열 및 클릭의 이름으로 **다음**</span><span class="sxs-lookup"><span data-stu-id="c07b3-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![연결 선택](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="c07b3-184">모든 테이블을 가져오고 '마침' 클릭 '테이블' 옆의 확인란을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![개체 선택](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="c07b3-186">리버스 엔지니어링 프로세스가 완료 되 면 새 모델 프로젝트에 추가 되 고 Entity Framework 디자이너에서 확인할 수 있게 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="c07b3-187">또한 App.config 파일을 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="c07b3-188">Visual Studio 2010의 추가 단계</span><span class="sxs-lookup"><span data-stu-id="c07b3-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="c07b3-189">Visual Studio 2010에서 작업 하는 경우 EF6 코드 생성을 사용 하 여 EF 디자이너를 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="c07b3-190">EF 디자이너에서 모델의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **코드 생성 항목 추가...**</span><span class="sxs-lookup"><span data-stu-id="c07b3-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="c07b3-191">선택 **온라인 템플릿을** 검색에 대 한 확인 하 고 왼쪽된 메뉴에서 **DbContext**</span><span class="sxs-lookup"><span data-stu-id="c07b3-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="c07b3-192">선택 합니다 **EF 6.x C에 대 한 DbContext 생성기\#를** 입력 **ProductsModel** 이름으로 추가 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="c07b3-193">데이터 바인딩에 대 한 코드 생성 업데이트</span><span class="sxs-lookup"><span data-stu-id="c07b3-193">Updating code generation for data binding</span></span>

<span data-ttu-id="c07b3-194">EF T4 템플릿을 사용 하 여 모델에서 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="c07b3-195">Visual Studio를 함께 사용 하거나 Visual Studio 갤러리에서 다운로드 한 템플릿에 범용 사용이 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="c07b3-196">즉, 이러한 템플릿에서 생성 한 엔터티 간단한 ICollection&lt;T&gt; 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="c07b3-197">그러나 데이터 바인딩을 수행할 때 것 컬렉션 속성이 IListSource 구현 하는 것이 바람직합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="c07b3-198">이 때문에 위의 ObservableListSource 클래스를 만들었습니다 및 있도록 템플릿을 수정 하려고 이제이 클래스를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="c07b3-199">엽니다는 **솔루션 탐색기** 찾고 **ProductModel.edmx** 파일</span><span class="sxs-lookup"><span data-stu-id="c07b3-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="c07b3-200">찾을 합니다 **ProductModel.tt** ProductModel.edmx 파일 아래에 중첩 될는 파일</span><span class="sxs-lookup"><span data-stu-id="c07b3-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![제품 모델 템플릿](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="c07b3-202">Visual Studio 편집기에서 열려는 ProductModel.tt 파일을 두 번 클릭</span><span class="sxs-lookup"><span data-stu-id="c07b3-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="c07b3-203">찾기 및 바꾸기 2 개 "**ICollection**"with"**ObservableListSource**"입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="c07b3-204">이들은 296 및 484 약 줄 위치입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="c07b3-205">찾기 및 바꾸기 첫 번째로 나타나는 "**HashSet**"with"**ObservableListSource**"입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="c07b3-206">이 항목은 약 50 줄에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="c07b3-207">**그렇지 않은** HashSet 코드 나중에 두 번째 항목으로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="c07b3-208">ProductModel.tt 파일을 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="c07b3-209">이 인해 다시 생성 하는 엔터티에 대 한 코드를 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="c07b3-210">코드를 자동으로 다시 생성 하지 않습니다, 경우 ProductModel.tt 마우스 오른쪽 단추로 클릭 하 고 "사용자 지정 도구 실행"을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="c07b3-211">이제 (함은 ProductModel.tt 아래에 중첩) Category.cs 파일을 열 경우 제품 컬렉션 형식에 표시 되어야 **ObservableListSource&lt;제품&gt;** 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="c07b3-212">프로젝트를 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="c07b3-213">지연 로드</span><span class="sxs-lookup"><span data-stu-id="c07b3-213">Lazy Loading</span></span>

<span data-ttu-id="c07b3-214">**제품** 속성에는 **범주** 클래스 및 **범주** 속성에는 **제품** 클래스는 탐색 속성.</span><span class="sxs-lookup"><span data-stu-id="c07b3-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="c07b3-215">Entity Framework, 탐색 속성에는 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="c07b3-216">EF는 관련된 엔터티 로드 데이터베이스에서 자동으로 처음 탐색 속성에 액세스 하는 옵션을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="c07b3-217">이 유형의 로드 (지연 로딩 이라고 함)는 각 탐색 속성에 액세스 하는 처음으로 별도 쿼리가 실행 됩니다 데이터베이스에 대 한 내용이 없는 경우 컨텍스트에서 알아야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="c07b3-218">POCO 엔터티 형식을 사용 하는 경우 EF는 런타임에 파생된 프록시 형식의 인스턴스를 만들고 다음 로드 후크를 추가 하려면 클래스에서 가상 속성을 재정의 하 여 지연 로딩을 달성 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="c07b3-219">관련된 개체의 지연 로드를 가져오려면 선언 해야 탐색으로 속성 getter **공용** 하 고 **가상** (**Overridable** Visual basic에서), 있습니다 클래스가 아니어야 하 고 **봉인** (**NotOverridable** Visual basic에서).</span><span class="sxs-lookup"><span data-stu-id="c07b3-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="c07b3-220">데이터베이스를 사용 하는 경우 첫 번째 탐색 속성이 자동으로 지연 로드를 사용 하도록 설정 하려면 가상 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="c07b3-221">같은 이유로 가상 탐색 속성을 확인 하기로 코드 첫 번째 섹션에서</span><span class="sxs-lookup"><span data-stu-id="c07b3-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="c07b3-222">컨트롤에 개체 바인딩</span><span class="sxs-lookup"><span data-stu-id="c07b3-222">Bind Object to Controls</span></span>

<span data-ttu-id="c07b3-223">이 WinForms 응용 프로그램에 대 한 데이터 원본으로 모델에 정의 된 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="c07b3-224">주 메뉴에서 선택 **프로젝트-&gt; 새 데이터 소스 추가...**</span><span class="sxs-lookup"><span data-stu-id="c07b3-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="c07b3-225">(Visual Studio 2010에서 선택 해야 **데이터-&gt; 새 데이터 소스 추가...** )</span><span class="sxs-lookup"><span data-stu-id="c07b3-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="c07b3-226">데이터 원본 유형 창에서 선택 **개체** 를 클릭 하 고 **다음**</span><span class="sxs-lookup"><span data-stu-id="c07b3-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="c07b3-227">데이터 개체 대화 상자 선택에서 펼침 합니다 **WinFormswithEFSample** 두 시간과 선택 **범주** 있습니다 이므로 제품 데이터 원본을 선택 하지 않아도 제품의 진행 과정을 살펴보겠습니다 범주 데이터 원본에 대 한 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![데이터 원본](~/ef6/media/datasource.png)

-   <span data-ttu-id="c07b3-229">클릭 **완료.** 
     *데이터 소스 창에서 표시 되지 않으면, 선택 \* \* \* 보기-&gt; 다른 Windows-&gt; 데이터 원본*\*</span><span class="sxs-lookup"><span data-stu-id="c07b3-229">Click **Finish.**
*If the Data Sources window is not showing up, select\*\*\*View -&gt; Other Windows-&gt; Data Sources*\*</span></span>
-   <span data-ttu-id="c07b3-230">고정 아이콘을 하므로 데이터 소스 창에서는 자동 숨기기 단추를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-230">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="c07b3-231">창의 창이 이미 열려 있는 경우 새로 고침 단추를 적중 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-231">You may need to hit the refresh button if the window was already visible.</span></span>

    ![데이터 원본 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="c07b3-233">솔루션 탐색기에서 두 번 클릭 합니다 **Form1.cs** 파일을 기본 폼 디자이너에서 엽니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-233">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="c07b3-234">선택 된 **범주** 데이터 원본 및 폼에 놓습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-234">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="c07b3-235">기본적으로 새 DataGridView (**categoryDataGridView**) 탐색 도구 모음 컨트롤 디자이너에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-235">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="c07b3-236">이러한 컨트롤은 BindingSource에 바인딩됩니다 (**categoryBindingSource**) 및 바인딩 Navigator (**categoryBindingNavigator**)도 생성 된 구성 요소입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-236">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="c07b3-237">열을 편집 합니다 **categoryDataGridView**합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-237">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="c07b3-238">설정 해야 합니다 **CategoryId** 읽기 전용으로 열입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-238">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="c07b3-239">에 대 한 값을 **CategoryId** 속성 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-239">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="c07b3-240">DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 열 편집을 선택 하는 중...</span><span class="sxs-lookup"><span data-stu-id="c07b3-240">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="c07b3-241">CategoryId 열을 선택 하 고 ReadOnly를 True로 설정</span><span class="sxs-lookup"><span data-stu-id="c07b3-241">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="c07b3-242">확인</span><span class="sxs-lookup"><span data-stu-id="c07b3-242">Press OK</span></span>
-   <span data-ttu-id="c07b3-243">범주 데이터 원본에서 제품을 선택 하 고 폼에 놓습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-243">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="c07b3-244">ProductBindingSource 고 productDataGridView 폼에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-244">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="c07b3-245">productDataGridView에서 열을 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-245">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="c07b3-246">CategoryId 및 범주 열을 숨기고 ProductId를 읽기 전용으로 설정 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-246">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="c07b3-247">ProductId 속성의 값은 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-247">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="c07b3-248">DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 선택 **열 편집...** .</span><span class="sxs-lookup"><span data-stu-id="c07b3-248">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="c07b3-249">선택 된 **ProductId** 열 집합과 **ReadOnly** 에 **True**합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-249">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="c07b3-250">선택 합니다 **CategoryId** 열 및 키를 눌러 합니다 **제거** 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-250">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="c07b3-251">동일한 작업을 수행 합니다 **범주** 열입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-251">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="c07b3-252">키를 눌러 **확인**합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-252">Press **OK**.</span></span>

    <span data-ttu-id="c07b3-253">지금 DataGridView 컨트롤이 BindingSource 구성 요소 디자이너를 사용 하 여 연결 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-253">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="c07b3-254">다음 섹션에서는 categoryBindingSource.DataSource DbContext에서 현재 추적 되는 엔터티의 컬렉션을 설정 하려면 코드를 코드 숨김에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-254">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="c07b3-255">때 끌어 놓은 제품 범주에는 WinForms에서 수행한 할까요 productsBindingSource.DataSource 속성 제품 categoryBindingSource 및 productsBindingSource.DataMember 속성을 설정 하는 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-255">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="c07b3-256">이 바인딩 때문에 현재 선택 된 범주에 속하는 제품만 productDataGridView에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-256">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="c07b3-257">사용 하도록 설정 합니다 **저장** 마우스 오른쪽 단추를 클릭 하 고 선택 하 여 탐색 도구 모음 단추 **Enabled**.</span><span class="sxs-lookup"><span data-stu-id="c07b3-257">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![1 폼 디자이너](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="c07b3-259">저장에 대 한 이벤트 처리기를 추가 단추를 두 번 클릭 하면 단추입니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-259">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="c07b3-260">이벤트 처리기를 추가 되 고 폼의 코드 숨김 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-260">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="c07b3-261">에 대 한 코드를 **categoryBindingNavigatorSaveItem\_클릭** 이벤트 처리기는 다음 섹션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-261">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="c07b3-262">데이터 상호 작용을 처리 하는 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-262">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="c07b3-263">이제 데이터 액세스를 수행 하는 ProductContext를 사용 하도록 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-263">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="c07b3-264">아래와 같이 기본 폼 창에 대 한 코드를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-264">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="c07b3-265">코드는 ProductContext의 장기 실행 인스턴스를 선언합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-265">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="c07b3-266">ProductContext 개체를 쿼리하고 데이터를 데이터베이스에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-266">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="c07b3-267">ProductContext 인스턴스에서 dispose () 메서드 재정의 OnClosing 메서드에서 호출 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-267">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="c07b3-268">코드 주석을 코드 수행 작업에 대 한 세부 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-268">The code comments provide details about what the code does.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.ComponentModel;
    using System.Data;
    using System.Drawing;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using System.Windows.Forms;
    using System.Data.Entity;

    namespace WinFormswithEFSample
    {
        public partial class Form1 : Form
        {
            ProductContext _context;
            public Form1()
            {
                InitializeComponent();
            }

            protected override void OnLoad(EventArgs e)
            {
                base.OnLoad(e);
                _context = new ProductContext();

                // Call the Load method to get the data for the given DbSet
                // from the database.
                // The data is materialized as entities. The entities are managed by
                // the DbContext instance.
                _context.Categories.Load();

                // Bind the categoryBindingSource.DataSource to
                // all the Unchanged, Modified and Added Category objects that
                // are currently tracked by the DbContext.
                // Note that we need to call ToBindingList() on the
                // ObservableCollection<TEntity> returned by
                // the DbSet.Local property to get the BindingList<T>
                // in order to facilitate two-way binding in WinForms.
                this.categoryBindingSource.DataSource =
                    _context.Categories.Local.ToBindingList();
            }

            private void categoryBindingNavigatorSaveItem_Click(object sender, EventArgs e)
            {
                this.Validate();

                // Currently, the Entity Framework doesn’t mark the entities
                // that are removed from a navigation property (in our example the Products)
                // as deleted in the context.
                // The following code uses LINQ to Objects against the Local collection
                // to find all products and marks any that do not have
                // a Category reference as deleted.
                // The ToList call is required because otherwise
                // the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can do LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                // Save the changes to the database.
                this._context.SaveChanges();

                // Refresh the controls to show the values         
                // that were generated by the database.
                this.categoryDataGridView.Refresh();
                this.productsDataGridView.Refresh();
            }

            protected override void OnClosing(CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }
    }
```

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="c07b3-269">Windows Forms 응용 프로그램 테스트</span><span class="sxs-lookup"><span data-stu-id="c07b3-269">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="c07b3-270">컴파일 및 실행 하는 응용 프로그램 기능을 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-270">Compile and run the application and you can test out the functionality.</span></span>

    ![1을 구성 하기 전에 저장](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="c07b3-272">저장 한 후 저장소 생성 키를 화면에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-272">After saving the store generated keys are shown on the screen.</span></span>

    ![1 후 저장을 형성 합니다.](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="c07b3-274">Code First에서 사용 되는 경우 표시 됩니다는 **WinFormswithEFSample.ProductContext** 데이터베이스 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="c07b3-274">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![Server 개체 탐색기](~/ef6/media/serverobjexplorer.png)
