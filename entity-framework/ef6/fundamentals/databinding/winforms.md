---
title: WinForms를 사용 하 여 데이터 바인딩-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 3c7c58f5ded29c136bbdca1d81c64b07c53ce583
ms.sourcegitcommit: 7391cc31193c1216ec9ed485709042ad0c2106cf
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985479"
---
# <a name="databinding-with-winforms"></a><span data-ttu-id="af2a3-102">WinForms를 사용 하 여 데이터 바인딩</span><span class="sxs-lookup"><span data-stu-id="af2a3-102">Databinding with WinForms</span></span>
<span data-ttu-id="af2a3-103">이 단계별 연습에서는 POCO 형식을 "마스터-세부 정보" 폼의 WinForms (Window Forms) 컨트롤에 바인딩하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-103">This step-by-step walkthrough shows how to bind POCO types to Window Forms (WinForms) controls in a “master-detail" form.</span></span> <span data-ttu-id="af2a3-104">응용 프로그램은 Entity Framework를 사용 하 여 데이터베이스의 데이터로 개체를 채우고, 변경 내용을 추적 하 고, 데이터를 데이터베이스에 보관 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-104">The application uses Entity Framework to populate objects with data from the database, track changes, and persist data to the database.</span></span>

<span data-ttu-id="af2a3-105">이 모델은 일 대 다 관계에 참여 하는 두 가지 형식을 정의 합니다. 범주 (주\\마스터) 및 제품 (종속\\정보)입니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-105">The model defines two types that participate in one-to-many relationship: Category (principal\\master) and Product (dependent\\detail).</span></span> <span data-ttu-id="af2a3-106">그런 다음 Visual Studio 도구를 사용 하 여 모델에 정의 된 형식을 WinForms 컨트롤에 바인딩합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-106">Then, the Visual Studio tools are used to bind the types defined in the model to the WinForms controls.</span></span> <span data-ttu-id="af2a3-107">WinForms 데이터 바인딩 프레임 워크를 사용 하면 관련 개체 간을 탐색할 수 있습니다. 즉, 마스터 뷰에서 행을 선택 하면 세부 정보 보기가 해당 자식 데이터로 업데이트 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-107">The WinForms data-binding framework enables navigation between related objects: selecting rows in the master view causes the detail view to update with the corresponding child data.</span></span>

<span data-ttu-id="af2a3-108">이 연습의 스크린 샷 및 코드 목록은 Visual Studio 2013에서 가져온 것 이지만 Visual Studio 2012 또는 Visual Studio 2010를 사용 하 여이 연습을 완료할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-108">The screen shots and code listings in this walkthrough are taken from Visual Studio 2013 but you can complete this walkthrough with Visual Studio 2012 or Visual Studio 2010.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="af2a3-109">필수 구성 요소</span><span class="sxs-lookup"><span data-stu-id="af2a3-109">Pre-Requisites</span></span>

<span data-ttu-id="af2a3-110">이 연습을 완료 하려면 Visual Studio 2013, Visual Studio 2012 또는 Visual Studio 2010이 설치 되어 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-110">You need to have Visual Studio 2013, Visual Studio 2012 or Visual Studio 2010 installed to complete this walkthrough.</span></span>

<span data-ttu-id="af2a3-111">Visual Studio 2010을 사용 하는 경우 NuGet도 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-111">If you are using Visual Studio 2010, you also have to install NuGet.</span></span> <span data-ttu-id="af2a3-112">자세한 내용은 [NuGet 설치](http://docs.nuget.org/docs/start-here/installing-nuget)를 참조 하세요.</span><span class="sxs-lookup"><span data-stu-id="af2a3-112">For more information, see [Installing NuGet](http://docs.nuget.org/docs/start-here/installing-nuget).</span></span>

## <a name="create-the-application"></a><span data-ttu-id="af2a3-113">애플리케이션 만들기</span><span class="sxs-lookup"><span data-stu-id="af2a3-113">Create the Application</span></span>

-   <span data-ttu-id="af2a3-114">Visual Studio를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-114">Open Visual Studio</span></span>
-   <span data-ttu-id="af2a3-115">**파일-&gt; 새로 만들기&gt; -프로젝트 ....**</span><span class="sxs-lookup"><span data-stu-id="af2a3-115">**File -&gt; New -&gt; Project….**</span></span>
-   <span data-ttu-id="af2a3-116">왼쪽 창에서 **windows** 를 선택 하 고 오른쪽 창에서 **windows 양식 응용 프로그램** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-116">Select **Windows** in the left pane and **Windows FormsApplication** in the right pane</span></span>
-   <span data-ttu-id="af2a3-117">**Win양식 Withefsample** 을 이름으로 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-117">Enter **WinFormswithEFSample** as the name</span></span>
-   <span data-ttu-id="af2a3-118">**확인**을 선택합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-118">Select **OK**</span></span>

## <a name="install-the-entity-framework-nuget-package"></a><span data-ttu-id="af2a3-119">Entity Framework NuGet 패키지를 설치 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-119">Install the Entity Framework NuGet package</span></span>

-   <span data-ttu-id="af2a3-120">솔루션 탐색기에서 **Win양식 Withefsample** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-120">In Solution Explorer, right-click on the **WinFormswithEFSample** project</span></span>
-   <span data-ttu-id="af2a3-121">**NuGet 패키지 관리 ...** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-121">Select **Manage NuGet Packages…**</span></span>
-   <span data-ttu-id="af2a3-122">NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-122">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
-   <span data-ttu-id="af2a3-123">**설치** 클릭</span><span class="sxs-lookup"><span data-stu-id="af2a3-123">Click **Install**</span></span>  
    > [!NOTE]
    > <span data-ttu-id="af2a3-124">EntityFramework 어셈블리 외에 System.componentmodel에 대 한 참조도 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-124">In addition to the EntityFramework assembly a reference to System.ComponentModel.DataAnnotations is also added.</span></span> <span data-ttu-id="af2a3-125">프로젝트에 System.object에 대 한 참조가 있는 경우 EntityFramework 패키지를 설치 하면 제거 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-125">If the project has a reference to System.Data.Entity, then it will be removed when the EntityFramework package is installed.</span></span> <span data-ttu-id="af2a3-126">System.object 어셈블리는 Entity Framework 6 응용 프로그램에 더 이상 사용 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-126">The System.Data.Entity assembly is no longer used for Entity Framework 6 applications.</span></span>

## <a name="implementing-ilistsource-for-collections"></a><span data-ttu-id="af2a3-127">컬렉션에 대해 IListSource 구현</span><span class="sxs-lookup"><span data-stu-id="af2a3-127">Implementing IListSource for Collections</span></span>

<span data-ttu-id="af2a3-128">Windows Forms를 사용할 때 정렬에 양방향 데이터 바인딩을 사용 하려면 컬렉션 속성에서 IListSource 인터페이스를 구현 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-128">Collection properties must implement the IListSource interface to enable two-way data binding with sorting when using Windows Forms.</span></span> <span data-ttu-id="af2a3-129">이렇게 하려면 System.collections.objectmodel.observablecollection를 확장 하 여 IListSource 기능을 추가 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-129">To do this we are going to extend ObservableCollection to add IListSource functionality.</span></span>

-   <span data-ttu-id="af2a3-130">프로젝트에 **ObservableListSource** 클래스를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-130">Add an **ObservableListSource** class to the project:</span></span>
    -   <span data-ttu-id="af2a3-131">프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-131">Right-click on the project name</span></span>
    -   <span data-ttu-id="af2a3-132">**새 항목 추가&gt; 를** 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-132">Select **Add -&gt; New Item**</span></span>
    -   <span data-ttu-id="af2a3-133">클래스 를 선택 하 고 클래스 이름으로 **ObservableListSource** 를 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-133">Select **Class** and enter **ObservableListSource** for the class name</span></span>
-   <span data-ttu-id="af2a3-134">기본적으로 생성 된 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-134">Replace the code generated by default with the following code:</span></span>

<span data-ttu-id="af2a3-135">*이 클래스를 사용 하면 양방향 데이터 바인딩과 정렬을 사용할 수 있습니다. 클래스는 system.collections.objectmodel.observablecollection&lt;T&gt; 에서 파생 되며 IListSource의 명시적 구현을 추가 합니다. IListSource의 GetList () 메서드는 System.collections.objectmodel.observablecollection와 동기화 상태로 유지 되는 IBindingList 구현을 반환 하도록 구현 됩니다. ToBindingList에 의해 생성 된 IBindingList 구현에서는 정렬을 지원 합니다. ToBindingList 확장 메서드는 EntityFramework 어셈블리에서 정의 됩니다.*</span><span class="sxs-lookup"><span data-stu-id="af2a3-135">*This class enables two-way data binding as well as sorting. The class derives from ObservableCollection&lt;T&gt; and adds an explicit implementation of IListSource. The GetList() method of IListSource is implemented to return an IBindingList implementation that stays in sync with the ObservableCollection. The IBindingList implementation generated by ToBindingList supports sorting. The ToBindingList extension method is defined in the EntityFramework assembly.*</span></span>

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

## <a name="define-a-model"></a><span data-ttu-id="af2a3-136">모델 정의</span><span class="sxs-lookup"><span data-stu-id="af2a3-136">Define a Model</span></span>

<span data-ttu-id="af2a3-137">이 연습에서는 Code First 또는 EF Designer를 사용 하 여 모델을 구현 하도록 선택할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-137">In this walkthrough you can chose to implement a model using Code First or the EF Designer.</span></span> <span data-ttu-id="af2a3-138">다음 두 섹션 중 하나를 완료 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-138">Complete one of the two following sections.</span></span>

### <a name="option-1-define-a-model-using-code-first"></a><span data-ttu-id="af2a3-139">옵션 1: Code First를 사용 하 여 모델 정의</span><span class="sxs-lookup"><span data-stu-id="af2a3-139">Option 1: Define a Model using Code First</span></span>

<span data-ttu-id="af2a3-140">이 섹션에서는 Code First를 사용 하 여 모델 및 연결 된 데이터베이스를 만드는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-140">This section shows how to create a model and its associated database using Code First.</span></span> <span data-ttu-id="af2a3-141">다음 섹션으로 건너뜁니다 (**옵션 2: Database First를 사용 하 여 EF** designer를 사용 하 여 데이터베이스에서 모델을 리버스 엔지니어링 하는 경우 Database First)를 사용 하 여 모델을 정의 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-141">Skip to the next section (**Option 2: Define a model using Database First)** if you would rather use Database First to reverse engineer your model from a database using the EF designer</span></span>

<span data-ttu-id="af2a3-142">Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-142">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span>

-   <span data-ttu-id="af2a3-143">프로젝트에 새 **Product** 클래스 추가</span><span class="sxs-lookup"><span data-stu-id="af2a3-143">Add a new **Product** class to project</span></span>
-   <span data-ttu-id="af2a3-144">기본적으로 생성 된 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-144">Replace the code generated by default with the following code:</span></span>

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

-   <span data-ttu-id="af2a3-145">**범주** 클래스를 프로젝트에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-145">Add a **Category** class to the project.</span></span>
-   <span data-ttu-id="af2a3-146">기본적으로 생성 된 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-146">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="af2a3-147">엔터티를 정의 하는 것 외에도 **DbContext** 에서 파생 되 고 **&lt;&gt; dbset** 엔터티 속성을 노출 하는 클래스를 정의 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-147">In addition to defining entities, you need to define a class that derives from **DbContext** and exposes **DbSet&lt;TEntity&gt;** properties.</span></span> <span data-ttu-id="af2a3-148">**Dbset** 속성을 사용 하면 컨텍스트에서 모델에 포함 하려는 형식을 알 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-148">The **DbSet** properties let the context know which types you want to include in the model.</span></span> <span data-ttu-id="af2a3-149">**DbContext** 및 **Dbset** 형식은 entityframework 어셈블리에서 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-149">The **DbContext** and **DbSet** types are defined in the EntityFramework assembly.</span></span>

<span data-ttu-id="af2a3-150">DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-150">An instance of the DbContext derived type manages the entity objects during run time, which includes populating objects with data from a database, change tracking, and persisting data to the database.</span></span>

-   <span data-ttu-id="af2a3-151">새 **제품 컨텍스트** 클래스를 프로젝트에 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-151">Add a new **ProductContext** class to the project.</span></span>
-   <span data-ttu-id="af2a3-152">기본적으로 생성 된 코드를 다음 코드로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-152">Replace the code generated by default with the following code:</span></span>

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

<span data-ttu-id="af2a3-153">프로젝트를 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-153">Compile the project.</span></span>

### <a name="option-2-define-a-model-using-database-first"></a><span data-ttu-id="af2a3-154">옵션 2: Database First를 사용 하 여 모델 정의</span><span class="sxs-lookup"><span data-stu-id="af2a3-154">Option 2: Define a model using Database First</span></span>

<span data-ttu-id="af2a3-155">이 섹션에서는 EF designer를 사용 하 여 데이터베이스에서 모델을 리버스 엔지니어링 하는 Database First를 사용 하는 방법을 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-155">This section shows how to use Database First to reverse engineer your model from a database using the EF designer.</span></span> <span data-ttu-id="af2a3-156">이전 섹션을 완료 한 경우 (**옵션 1: Code First)** 를 사용 하 여 모델을 정의한 다음이 섹션을 건너뛰고 **지연 로드** 섹션으로 바로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-156">If you completed the previous section (**Option 1: Define a model using Code First)**, then skip this section and go straight to the **Lazy Loading** section.</span></span>

#### <a name="create-an-existing-database"></a><span data-ttu-id="af2a3-157">기존 데이터베이스 만들기</span><span class="sxs-lookup"><span data-stu-id="af2a3-157">Create an Existing Database</span></span>

<span data-ttu-id="af2a3-158">일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-158">Typically when you are targeting an existing database it will already be created, but for this walkthrough we need to create a database to access.</span></span>

<span data-ttu-id="af2a3-159">Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-159">The database server that is installed with Visual Studio is different depending on the version of Visual Studio you have installed:</span></span>

-   <span data-ttu-id="af2a3-160">Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-160">If you are using Visual Studio 2010 you'll be creating a SQL Express database.</span></span>
-   <span data-ttu-id="af2a3-161">Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-161">If you are using Visual Studio 2012 then you'll be creating a [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) database.</span></span>

<span data-ttu-id="af2a3-162">계속 해 서 데이터베이스를 생성 해 보겠습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-162">Let's go ahead and generate the database.</span></span>

-   <span data-ttu-id="af2a3-163">**보기-&gt; 서버 탐색기**</span><span class="sxs-lookup"><span data-stu-id="af2a3-163">**View -&gt; Server Explorer**</span></span>
-   <span data-ttu-id="af2a3-164">**데이터 연결-&gt; 연결 추가** ...를 마우스 오른쪽 단추로 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-164">Right click on **Data Connections -&gt; Add Connection…**</span></span>
-   <span data-ttu-id="af2a3-165">서버 탐색기 데이터베이스에 연결 하지 않은 경우 Microsoft SQL Server를 데이터 원본으로 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-165">If you haven’t connected to a database from Server Explorer before you’ll need to select Microsoft SQL Server as the data source</span></span>

    ![데이터 소스 변경](~/ef6/media/changedatasource.png)

-   <span data-ttu-id="af2a3-167">설치한 항목에 따라 LocalDB 또는 SQL Express에 연결 하 고 데이터베이스 이름으로 **제품** 을 입력 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-167">Connect to either LocalDB or SQL Express, depending on which one you have installed, and enter **Products** as the database name</span></span>

    ![연결 LocalDB 추가](~/ef6/media/addconnectionlocaldb.png)

    ![연결 Express 추가](~/ef6/media/addconnectionexpress.png)

-   <span data-ttu-id="af2a3-170">**확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-170">Select **OK** and you will be asked if you want to create a new database, select **Yes**</span></span>

    ![데이터베이스 만들기](~/ef6/media/createdatabase.png)

-   <span data-ttu-id="af2a3-172">이제 새 데이터베이스가 서버 탐색기에 표시 되 면 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-172">The new database will now appear in Server Explorer, right-click on it and select **New Query**</span></span>
-   <span data-ttu-id="af2a3-173">다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-173">Copy the following SQL into the new query, then right-click on the query and select **Execute**</span></span>

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

#### <a name="reverse-engineer-model"></a><span data-ttu-id="af2a3-174">리버스 엔지니어링 모델</span><span class="sxs-lookup"><span data-stu-id="af2a3-174">Reverse Engineer Model</span></span>

<span data-ttu-id="af2a3-175">Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하 여 모델을 만들 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-175">We’re going to make use of Entity Framework Designer, which is included as part of Visual Studio, to create our model.</span></span>

-   <span data-ttu-id="af2a3-176">**프로젝트-&gt; 새 항목 추가 ...**</span><span class="sxs-lookup"><span data-stu-id="af2a3-176">**Project -&gt; Add New Item…**</span></span>
-   <span data-ttu-id="af2a3-177">왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델</span><span class="sxs-lookup"><span data-stu-id="af2a3-177">Select **Data** from the left menu and then **ADO.NET Entity Data Model**</span></span>
-   <span data-ttu-id="af2a3-178">이름으로 **제품 모델** 을 입력 하 고 **확인을** 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-178">Enter **ProductModel** as the name and click **OK**</span></span>
-   <span data-ttu-id="af2a3-179">그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-179">This launches the **Entity Data Model Wizard**</span></span>
-   <span data-ttu-id="af2a3-180">**데이터베이스에서 생성** 을 선택 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-180">Select **Generate from Database** and click **Next**</span></span>

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   <span data-ttu-id="af2a3-182">첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 연결 문자열 이름으로 **제품 컨텍스트** 를 입력 한 후 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-182">Select the connection to the database you created in the first section, enter **ProductContext** as the name of the connection string and click **Next**</span></span>

    ![연결 선택](~/ef6/media/chooseyourconnection.png)

-   <span data-ttu-id="af2a3-184">' 테이블 ' 옆의 확인란을 클릭 하 여 모든 테이블을 가져온 다음 ' 마침 '을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-184">Click the checkbox next to ‘Tables’ to import all tables and click ‘Finish’</span></span>

    ![개체 선택](~/ef6/media/chooseyourobjects.png)

<span data-ttu-id="af2a3-186">리버스 엔지니어링 프로세스가 완료 되 면 새 모델이 프로젝트에 추가 되 고 Entity Framework Designer에서 볼 수 있도록 열립니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-186">Once the reverse engineer process completes the new model is added to your project and opened up for you to view in the Entity Framework Designer.</span></span> <span data-ttu-id="af2a3-187">또한 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 App.config 파일이 프로젝트에 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-187">An App.config file has also been added to your project with the connection details for the database.</span></span>

#### <a name="additional-steps-in-visual-studio-2010"></a><span data-ttu-id="af2a3-188">Visual Studio 2010의 추가 단계</span><span class="sxs-lookup"><span data-stu-id="af2a3-188">Additional Steps in Visual Studio 2010</span></span>

<span data-ttu-id="af2a3-189">Visual Studio 2010에서 작업 하는 경우 EF6 코드 생성을 사용 하도록 EF designer를 업데이트 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-189">If you are working in Visual Studio 2010 then you will need to update the EF designer to use EF6 code generation.</span></span>

-   <span data-ttu-id="af2a3-190">EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-190">Right-click on an empty spot of your model in the EF Designer and select **Add Code Generation Item…**</span></span>
-   <span data-ttu-id="af2a3-191">왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-191">Select **Online Templates** from the left menu and search for **DbContext**</span></span>
-   <span data-ttu-id="af2a3-192">**C\#에 대해 EF DbContext 생성기를 선택 하** 고 이름으로 **ProductsModel** 를 입력 한 다음 추가를 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-192">Select the **EF 6.x DbContext Generator for C\#,** enter **ProductsModel** as the name and click Add</span></span>

#### <a name="updating-code-generation-for-data-binding"></a><span data-ttu-id="af2a3-193">데이터 바인딩에 대 한 코드 생성 업데이트</span><span class="sxs-lookup"><span data-stu-id="af2a3-193">Updating code generation for data binding</span></span>

<span data-ttu-id="af2a3-194">EF는 T4 템플릿을 사용 하 여 모델에서 코드를 생성 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-194">EF generates code from your model using T4 templates.</span></span> <span data-ttu-id="af2a3-195">Visual Studio와 함께 제공 되거나 Visual Studio 갤러리에서 다운로드 한 템플릿은 일반적인 용도로 사용 하기 위한 것입니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-195">The templates shipped with Visual Studio or downloaded from the Visual Studio gallery are intended for general purpose use.</span></span> <span data-ttu-id="af2a3-196">즉, 이러한 템플릿에서 생성 된 엔터티에는 간단한 ICollection&lt;T&gt; 속성이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-196">This means that the entities generated from these templates have simple ICollection&lt;T&gt; properties.</span></span> <span data-ttu-id="af2a3-197">그러나 데이터 바인딩을 수행할 때 IListSource를 구현 하는 컬렉션 속성을 갖는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-197">However, when doing data binding it is desirable to have collection properties that implement IListSource.</span></span> <span data-ttu-id="af2a3-198">위의 ObservableListSource 클래스를 만들었으므로 이제이 클래스를 사용 하 여 템플릿을 수정 하겠습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-198">This is why we created the ObservableListSource class above and we are now going to modify the templates to make use of this class.</span></span>

-   <span data-ttu-id="af2a3-199">**솔루션 탐색기** 를 열고 **제품 모델 .edmx** 파일을 찾습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-199">Open the **Solution Explorer** and find **ProductModel.edmx** file</span></span>
-   <span data-ttu-id="af2a3-200">**ProductModel.tt** 파일을 찾습니다 .이 파일은 제품 모델 .edmx 파일 아래에 중첩 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-200">Find the **ProductModel.tt** file which will be nested under the ProductModel.edmx file</span></span>

    ![제품 모델 템플릿](~/ef6/media/productmodeltemplate.png)

-   <span data-ttu-id="af2a3-202">ProductModel.tt 파일을 두 번 클릭 하 여 Visual Studio 편집기에서 엽니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-202">Double-click on the ProductModel.tt file to open it in the Visual Studio editor</span></span>
-   <span data-ttu-id="af2a3-203">"**ICollection**"의 두 항목을 찾아 "**ObservableListSource**"로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-203">Find and replace the two occurrences of “**ICollection**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="af2a3-204">296 및 484 줄에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-204">These are located at approximately lines 296 and 484.</span></span>
-   <span data-ttu-id="af2a3-205">"**Hashset**"의 첫 번째 항목을 찾아 "**ObservableListSource**"로 바꿉니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-205">Find and replace the first occurrence of “**HashSet**” with “**ObservableListSource**”.</span></span> <span data-ttu-id="af2a3-206">이 항목은 약 50 줄에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-206">This occurrence is located at approximately line 50.</span></span> <span data-ttu-id="af2a3-207">코드에서 나중에 발견 된 두 번째 HashSet을 바꾸지 **마십시오** .</span><span class="sxs-lookup"><span data-stu-id="af2a3-207">**Do not** replace the second occurrence of HashSet found later in the code.</span></span>
-   <span data-ttu-id="af2a3-208">ProductModel.tt 파일을 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-208">Save the ProductModel.tt file.</span></span> <span data-ttu-id="af2a3-209">이로 인해 엔터티가 다시 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-209">This should cause the code for entities to be regenerated.</span></span> <span data-ttu-id="af2a3-210">코드가 자동으로 다시 생성 되지 않는 경우 ProductModel.tt를 마우스 오른쪽 단추로 클릭 하 고 "사용자 지정 도구 실행"을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-210">If the code does not regenerate automatically, then right click on ProductModel.tt and choose “Run Custom Tool”.</span></span>

<span data-ttu-id="af2a3-211">이제 ProductModel.tt 아래에 중첩 된 Category.cs 파일을 열면 Products 컬렉션에 **ObservableListSource&lt;Product&gt;** 형식이 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-211">If you now open the Category.cs file (which is nested under ProductModel.tt) then you should see that the Products collection has the type **ObservableListSource&lt;Product&gt;**.</span></span>

<span data-ttu-id="af2a3-212">프로젝트를 컴파일합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-212">Compile the project.</span></span>

## <a name="lazy-loading"></a><span data-ttu-id="af2a3-213">지연 로드</span><span class="sxs-lookup"><span data-stu-id="af2a3-213">Lazy Loading</span></span>

<span data-ttu-id="af2a3-214">**Product** 클래스의 **Category** 클래스 및 **Category** 속성에 대 한 **Products** 속성은 탐색 속성입니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-214">The **Products** property on the **Category** class and **Category** property on the **Product** class are navigation properties.</span></span> <span data-ttu-id="af2a3-215">Entity Framework 탐색 속성은 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-215">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span>

<span data-ttu-id="af2a3-216">EF는 탐색 속성에 처음 액세스할 때 데이터베이스에서 관련 엔터티를 자동으로 로드 하는 옵션을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-216">EF gives you an option of loading related entities from the database automatically the first time you access the navigation property.</span></span> <span data-ttu-id="af2a3-217">이 유형의 로드 (지연 로드)를 사용 하면 각 탐색 속성에 처음 액세스할 때 내용이 컨텍스트에 아직 없는 경우 데이터베이스에 대해 별도의 쿼리가 실행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-217">With this type of loading (called lazy loading), be aware that the first time you access each navigation property a separate query will be executed against the database if the contents are not already in the context.</span></span>

<span data-ttu-id="af2a3-218">POCO 엔터티 형식을 사용 하는 경우 EF는 런타임 중에 파생 된 프록시 형식의 인스턴스를 만든 다음 클래스의 가상 속성을 재정의 하 여 로드 후크를 추가 함으로써 지연 로드를 달성 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-218">When using POCO entity types, EF achieves lazy loading by creating instances of derived proxy types during runtime and then overriding virtual properties in your classes to add the loading hook.</span></span> <span data-ttu-id="af2a3-219">관련 개체의 지연 로드를 얻으려면 탐색 속성 getter를 **public** 및 **virtual** (Visual Basic에서**재정의** 가능)로 선언 하 고 클래스는 **sealed** 가 아니어야 합니다 (Visual Basic에서**NotOverridable** ).</span><span class="sxs-lookup"><span data-stu-id="af2a3-219">To get lazy loading of related objects, you must declare navigation property getters as **public** and **virtual** (**Overridable** in Visual Basic), and you class must not be **sealed** (**NotOverridable** in Visual Basic).</span></span> <span data-ttu-id="af2a3-220">Database First 사용 하는 경우 지연 로드를 사용할 수 있도록 탐색 속성이 자동으로 가상으로 만들어집니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-220">When using Database First navigation properties are automatically made virtual to enable lazy loading.</span></span> <span data-ttu-id="af2a3-221">Code First 섹션에서는 동일한 이유로 탐색 속성을 가상으로 만들도록 선택 했습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-221">In the Code First section we chose to make the navigation properties virtual for the same reason</span></span>

## <a name="bind-object-to-controls"></a><span data-ttu-id="af2a3-222">컨트롤에 개체 바인딩</span><span class="sxs-lookup"><span data-stu-id="af2a3-222">Bind Object to Controls</span></span>

<span data-ttu-id="af2a3-223">모델에 정의 된 클래스를이 WinForms 응용 프로그램의 데이터 원본으로 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-223">Add the classes that are defined in the model as data sources for this WinForms application.</span></span>

-   <span data-ttu-id="af2a3-224">주 메뉴에서 **프로젝트-&gt; 새 데이터 소스 추가** ...를 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-224">From the main menu, select **Project -&gt; Add New Data Source …**</span></span>
    <span data-ttu-id="af2a3-225">Visual Studio 2010에서는 **데이터-&gt; 새 데이터 소스 추가**...를 선택 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-225">(in Visual Studio 2010, you need to select **Data -&gt; Add New Data Source…**)</span></span>
-   <span data-ttu-id="af2a3-226">데이터 소스 형식 선택 창에서 **개체** 를 선택 하 고 **다음** 을 클릭 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-226">In the Choose a Data Source Type window, select **Object** and click **Next**</span></span>
-   <span data-ttu-id="af2a3-227">데이터 개체 선택 대화 상자에서 Win펼침을 두 번 선택 하 고 **범주** 를 선택 합니다. 범주 데이터 원본에서 제품의 속성을 통해 제품 데이터 원본을 선택할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-227">In the Select the Data Objects dialog, unfold the **WinFormswithEFSample** two times and select **Category** There is no need to select the Product data source, because we will get to it through the Product’s property on the Category data source.</span></span>

    ![데이터 원본](~/ef6/media/datasource.png)

-   <span data-ttu-id="af2a3-229">**마침을 클릭 합니다.**</span><span class="sxs-lookup"><span data-stu-id="af2a3-229">Click **Finish.**</span></span>
    <span data-ttu-id="af2a3-230">데이터 소스 창이 표시 되지 않으면 **보기-&gt; 기타 창&gt; -데이터 원본** 을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-230">If the Data Sources window is not showing up, select **View -&gt; Other Windows-&gt; Data Sources**</span></span>
-   <span data-ttu-id="af2a3-231">데이터 소스 창이 자동으로 숨겨지지 않도록 고정 아이콘을 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-231">Press the pin icon, so the Data Sources window does not auto hide.</span></span> <span data-ttu-id="af2a3-232">창이 이미 표시 되는 경우 새로 고침 단추를 눌러야 할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-232">You may need to hit the refresh button if the window was already visible.</span></span>

    ![데이터 원본 2](~/ef6/media/datasource2.png)

-   <span data-ttu-id="af2a3-234">솔루션 탐색기에서 **Form1.cs** 파일을 두 번 클릭 하 여 디자이너에서 기본 폼을 엽니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-234">In Solution Explorer, double-click the **Form1.cs** file to open the main form in designer.</span></span>
-   <span data-ttu-id="af2a3-235">**범주** 데이터 원본을 선택 하 고 폼에서 끌어 옵니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-235">Select the **Category** data source and drag it on the form.</span></span> <span data-ttu-id="af2a3-236">기본적으로 새 DataGridView (**Categorydatagridview**) 및 탐색 도구 모음 컨트롤이 디자이너에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-236">By default, a new DataGridView (**categoryDataGridView**) and Navigation toolbar controls are added to the designer.</span></span> <span data-ttu-id="af2a3-237">이러한 컨트롤은 생성 되는 BindingSource (**Categorybindingsource**) 및 바인딩 탐색기 (**categorybindingsource**) 구성 요소에 바인딩됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-237">These controls are bound to the BindingSource (**categoryBindingSource**) and Binding Navigator (**categoryBindingNavigator**) components that are created as well.</span></span>
-   <span data-ttu-id="af2a3-238">**Categorydatagridview**에서 열을 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-238">Edit the columns on the **categoryDataGridView**.</span></span> <span data-ttu-id="af2a3-239">**CategoryId** 열을 읽기 전용으로 설정 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-239">We want to set the **CategoryId** column to read-only.</span></span> <span data-ttu-id="af2a3-240">**CategoryId** 속성의 값은 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-240">The value for the **CategoryId** property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="af2a3-241">DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 열 편집 ...을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-241">Right-click the DataGridView control and select Edit Columns…</span></span>
    -   <span data-ttu-id="af2a3-242">CategoryId 열을 선택 하 고 ReadOnly를 True로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-242">Select the CategoryId column and set ReadOnly to True</span></span>
    -   <span data-ttu-id="af2a3-243">확인을 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-243">Press OK</span></span>
-   <span data-ttu-id="af2a3-244">범주 데이터 원본에서 제품을 선택 하 고 폼으로 끕니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-244">Select Products from under the Category data source and drag it on the form.</span></span> <span data-ttu-id="af2a3-245">제품 Datagridview 및 제품 Bindingsource가 양식에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-245">The productDataGridView and productBindingSource are added to the form.</span></span>
-   <span data-ttu-id="af2a3-246">제품 Datagridview에서 열을 편집 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-246">Edit the columns on the productDataGridView.</span></span> <span data-ttu-id="af2a3-247">CategoryId 및 Category 열을 숨기고 ProductId를 읽기 전용으로 설정 하려고 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-247">We want to hide the CategoryId and Category columns and set ProductId to read-only.</span></span> <span data-ttu-id="af2a3-248">ProductId 속성의 값은 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-248">The value for the ProductId property is generated by the database after we save the data.</span></span>
    -   <span data-ttu-id="af2a3-249">DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 **열 편집**...을 선택 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-249">Right-click the DataGridView control and select **Edit Columns...**.</span></span>
    -   <span data-ttu-id="af2a3-250">**ProductId** 열을 선택 하 고 **ReadOnly** 를 **True**로 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-250">Select the **ProductId** column and set **ReadOnly** to **True**.</span></span>
    -   <span data-ttu-id="af2a3-251">**CategoryId** 열을 선택 하 고 **제거** 단추를 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-251">Select the **CategoryId** column and press the **Remove** button.</span></span> <span data-ttu-id="af2a3-252">**Category** 열을 사용 하 여 동일한 작업을 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-252">Do the same with the **Category** column.</span></span>
    -   <span data-ttu-id="af2a3-253">**확인**을 누릅니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-253">Press **OK**.</span></span>

    <span data-ttu-id="af2a3-254">지금까지 DataGridView 컨트롤과 디자이너의 BindingSource 구성 요소를 연결 했습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-254">So far, we associated our DataGridView controls with BindingSource components in the designer.</span></span> <span data-ttu-id="af2a3-255">다음 섹션에서는 코드를 코드 뒤에 추가 하 여 DbContext에서 현재 추적 하는 엔터티 컬렉션에 categoryBindingSource를 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-255">In the next section we will add code to the code behind to set categoryBindingSource.DataSource to the collection of entities that are currently tracked by DbContext.</span></span> <span data-ttu-id="af2a3-256">범주에서 제품을 끌어다 놓으면 WinForms는 productsBindingSource 속성을 categoryBindingSource로 설정 하 고 productsBindingSource 속성을 Products로 설정 하는 작업을 처리 했습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-256">When we dragged-and-dropped Products from under the Category, the WinForms took care of setting up the productsBindingSource.DataSource property to categoryBindingSource and productsBindingSource.DataMember property to Products.</span></span> <span data-ttu-id="af2a3-257">이 바인딩으로 인해 현재 선택 된 범주에 속하는 제품만 products Datagridview에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-257">Because of this binding, only the products that belong to the currently selected Category will be displayed in the productDataGridView.</span></span>
-   <span data-ttu-id="af2a3-258">마우스 오른쪽 단추를 클릭 하 고 **사용**을 선택 하 여 탐색 도구 모음에서 **저장** 단추를 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-258">Enable the **Save** button on the Navigation toolbar by clicking the right mouse button and selecting **Enabled**.</span></span>

    ![양식 1 디자이너](~/ef6/media/form1-designer.png)

-   <span data-ttu-id="af2a3-260">단추를 두 번 클릭 하 여 저장 단추에 대 한 이벤트 처리기를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-260">Add the event handler for the save button by double-clicking on the button.</span></span> <span data-ttu-id="af2a3-261">이렇게 하면 이벤트 처리기가 추가 되 고 폼의 코드 숨김으로 이동 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-261">This will add the event handler and bring you to the code behind for the form.</span></span> <span data-ttu-id="af2a3-262">**CategoryBindingNavigatorSaveItem\_Click** 이벤트 처리기에 대 한 코드는 다음 섹션에 추가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-262">The code for the **categoryBindingNavigatorSaveItem\_Click** event handler will be added in the next section.</span></span>

## <a name="add-the-code-that-handles-data-interaction"></a><span data-ttu-id="af2a3-263">데이터 상호 작용을 처리 하는 코드 추가</span><span class="sxs-lookup"><span data-stu-id="af2a3-263">Add the Code that Handles Data Interaction</span></span>

<span data-ttu-id="af2a3-264">이제 제품 컨텍스트를 사용 하 여 데이터 액세스를 수행 하는 코드를 추가 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-264">We'll now add the code to use the ProductContext to perform data access.</span></span> <span data-ttu-id="af2a3-265">아래와 같이 주 폼 창의 코드를 업데이트 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-265">Update the code for the main form window as shown below.</span></span>

<span data-ttu-id="af2a3-266">이 코드는 장기적으로 실행 되는 제품 컨텍스트의 인스턴스를 선언 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-266">The code declares a long-running instance of ProductContext.</span></span> <span data-ttu-id="af2a3-267">제품 컨텍스트 개체는 데이터베이스에 데이터를 쿼리하고 저장 하는 데 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-267">The ProductContext object is used to query and save data to the database.</span></span> <span data-ttu-id="af2a3-268">그런 다음, 제품 컨텍스트 인스턴스의 Dispose () 메서드를 재정의 된 OnClosing 메서드에서 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-268">The Dispose() method on the ProductContext instance is then called from the overridden OnClosing method.</span></span> <span data-ttu-id="af2a3-269">코드 주석은 코드가 수행 하는 작업에 대 한 세부 정보를 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-269">The code comments provide details about what the code does.</span></span>

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

## <a name="test-the-windows-forms-application"></a><span data-ttu-id="af2a3-270">Windows Forms 응용 프로그램 테스트</span><span class="sxs-lookup"><span data-stu-id="af2a3-270">Test the Windows Forms Application</span></span>

-   <span data-ttu-id="af2a3-271">응용 프로그램을 컴파일 및 실행 하 고 기능을 테스트할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-271">Compile and run the application and you can test out the functionality.</span></span>

    ![저장 하기 전 1 양식](~/ef6/media/form1beforesave.png)

-   <span data-ttu-id="af2a3-273">저장 한 후에는 저장소에서 생성 된 키가 화면에 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-273">After saving the store generated keys are shown on the screen.</span></span>

    ![저장 후 1 양식](~/ef6/media/form1aftersave.png)

-   <span data-ttu-id="af2a3-275">Code First 사용 하는 경우 **Win양식 Withefsample. 제품** 데이터베이스를 만드는 것도 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="af2a3-275">If you used Code First, then you will also see that a **WinFormswithEFSample.ProductContext** database is created for you.</span></span>

    ![서버 개체 탐색기](~/ef6/media/serverobjexplorer.png)
