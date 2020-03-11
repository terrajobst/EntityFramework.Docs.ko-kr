---
title: WPF를 사용 하 여 데이터 바인딩-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e90d48e6-bea5-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 1933988277d3be8fecc02fced3293f2b7f80c901
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414100"
---
# <a name="databinding-with-wpf"></a>WPF를 사용 하 여 데이터 바인딩
이 단계별 연습에서는 POCO 형식을 "마스터-세부 정보" 폼의 WPF 컨트롤에 바인딩하는 방법을 보여 줍니다. 응용 프로그램은 Entity Framework Api를 사용 하 여 데이터베이스의 데이터로 개체를 채우고, 변경 내용을 추적 하 고, 데이터를 데이터베이스에 저장 합니다.

이 모델은 일 대 다 관계에 참여 하는 두 가지 유형, 즉 **Category** (principal\\master) 및 **Product** (종속\\세부 정보)를 정의 합니다. 그런 다음 Visual Studio 도구를 사용 하 여 모델에 정의 된 형식을 WPF 컨트롤에 바인딩합니다. WPF 데이터 바인딩 프레임 워크를 사용 하면 관련 개체 간을 탐색할 수 있습니다. 즉, 마스터 뷰에서 행을 선택 하면 세부 정보 보기가 해당 자식 데이터로 업데이트 됩니다.

이 연습의 스크린 샷 및 코드 목록은 Visual Studio 2013에서 가져온 것 이지만 Visual Studio 2012 또는 Visual Studio 2010를 사용 하 여이 연습을 완료할 수 있습니다.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>WPF 데이터 소스를 만드는 데 ' Object ' 옵션 사용

이전 버전의 Entity Framework에서는 EF Designer를 사용 하 여 만든 모델을 기반으로 새 데이터 원본을 만들 때 **데이터베이스** 옵션을 사용 하는 것이 좋습니다. 이는 디자이너가 EntityObject에서 파생 된 엔터티 클래스 및 ObjectContext에서 파생 된 컨텍스트를 생성 하기 때문입니다. 데이터베이스 옵션을 사용 하면이 API 화면과 상호 작용 하는 데 가장 적합 한 코드를 작성 하는 데 도움이 됩니다.

Visual Studio 2012 및 Visual Studio 2013에 대 한 EF Designer는 간단한 POCO 엔터티 클래스와 함께 DbContext에서 파생 되는 컨텍스트를 생성 합니다. Visual Studio 2010을 사용 하는 경우이 연습의 뒷부분에서 설명 하는 대로 DbContext를 사용 하는 코드 생성 템플릿으로 교체 하는 것이 좋습니다.

DbContext API surface를 사용 하는 경우이 연습에 표시 된 것 처럼 새 데이터 원본을 만들 때 **개체** 옵션을 사용 해야 합니다.

필요한 경우 EF Designer를 사용 하 여 만든 모델에 대 한 [ObjectContext 기반 코드 생성으로 되돌릴](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) 수 있습니다.

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2013, Visual Studio 2012 또는 Visual Studio 2010이 설치 되어 있어야 합니다.

Visual Studio 2010을 사용 하는 경우 NuGet도 설치 해야 합니다. 자세한 내용은 [NuGet 설치](https://docs.microsoft.com/nuget/install-nuget-client-tools)를 참조 하세요.  

## <a name="create-the-application"></a>애플리케이션 만들기

-   Visual Studio를 엽니다.
-   **파일&gt; 새&gt; 프로젝트 ....**
-   왼쪽 창에서 **Windows** 를 선택 하 고 오른쪽 창에서 **WPFApplication** 를 선택 합니다.
-   이름으로 **WPFwithEFSample** 입력
-    **확인을** 선택 합니다.

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet 패키지를 설치 합니다.

-   솔루션 탐색기에서 **Win양식 Withefsample** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.
-   **NuGet 패키지 관리 ...** 를 선택 합니다.
-   NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.
-   **설치**를 클릭합니다.  
    >[!NOTE]
    > EntityFramework 어셈블리 외에 System.componentmodel에 대 한 참조도 추가 됩니다. 프로젝트에 System.object에 대 한 참조가 있는 경우 EntityFramework 패키지를 설치 하면 제거 됩니다. System.object 어셈블리는 Entity Framework 6 응용 프로그램에 더 이상 사용 되지 않습니다.

## <a name="define-a-model"></a>모델 정의

이 연습에서는 Code First 또는 EF Designer를 사용 하 여 모델을 구현 하도록 선택할 수 있습니다. 다음 두 섹션 중 하나를 완료 합니다.

### <a name="option-1-define-a-model-using-code-first"></a>옵션 1: Code First을 사용 하 여 모델 정의

이 섹션에서는 Code First를 사용 하 여 모델 및 연결 된 데이터베이스를 만드는 방법을 보여 줍니다. EF designer를 사용 하 여 데이터베이스에서 모델을 리버스 엔지니어링 하기 위해 Database First를 사용 하는 경우 다음 섹션 (**옵션 2: Database First을 사용 하 여 모델 정의)** 으로 건너뜁니다.

Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다.

-   WPFwithEFSample에 새 클래스를 추가 합니다 **.**
    -   프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.
    -   **추가**, **새 항목** 을 차례로 선택 합니다.
    -   클래스 **를 선택 하** 고 클래스 이름에 대 한 **Product** 입력
-    **Product** 클래스 정의를 다음 코드로 바꿉니다.

``` csharp
    namespace WPFwithEFSample
    {
        public class Product
        {
            public int ProductId { get; set; }
            public string Name { get; set; }

            public int CategoryId { get; set; }
            public virtual Category Category { get; set; }
        }
    }

-   Add a **Category** class with the following definition:

    using System.Collections.ObjectModel;

    namespace WPFwithEFSample
    {
        public class Category
        {
            public Category()
            {
                this.Products = new ObservableCollection<Product>();
            }

            public int CategoryId { get; set; }
            public string Name { get; set; }

            public virtual ObservableCollection<Product> Products { get; private set; }
        }
    }
```

**Product** 클래스의 **Category** 클래스 및 **Category** 속성에 대 한 **Products** 속성은 탐색 속성입니다. Entity Framework 탐색 속성은 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.

엔터티를 정의 하는 것 외에도 DbContext에서 파생 되 고 DbSet&lt;&gt; 속성을 노출 하는 클래스를 정의 해야 합니다. DbSet&lt;&gt; 속성을 사용 하면 컨텍스트에서 모델에 포함 하려는 형식을 알 수 있습니다.

DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.

-   다음 정의를 사용 하 여 새 **제품 컨텍스트** 클래스를 프로젝트에 추가 합니다.

``` csharp
    using System.Data.Entity;

    namespace WPFwithEFSample
    {
        public class ProductContext : DbContext
        {
            public DbSet<Category> Categories { get; set; }
            public DbSet<Product> Products { get; set; }
        }
    }
```

프로젝트를 컴파일합니다.

### <a name="option-2-define-a-model-using-database-first"></a>옵션 2: Database First을 사용 하 여 모델 정의

이 섹션에서는 EF designer를 사용 하 여 데이터베이스에서 모델을 리버스 엔지니어링 하는 Database First를 사용 하는 방법을 보여 줍니다. 이전 섹션을 완료 한 경우 (**옵션 1: Code First를 사용 하 여 모델 정의)** 이 섹션을 건너뛰고 **지연 로드** 섹션으로 바로 이동 합니다.

#### <a name="create-an-existing-database"></a>기존 데이터베이스 만들기

일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.

Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.
-   Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스를 만듭니다.

계속 해 서 데이터베이스를 생성 해 보겠습니다.

-   **뷰&gt; 서버 탐색기**
-   데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.
-   서버 탐색기 데이터베이스에 연결 하지 않은 경우 Microsoft SQL Server를 데이터 원본으로 선택 해야 합니다.

    ![데이터 소스 변경](~/ef6/media/changedatasource.png)

-   설치한 항목에 따라 LocalDB 또는 SQL Express에 연결 하 고 데이터베이스 이름으로 **제품** 을 입력 합니다.

    ![연결 LocalDB 추가](~/ef6/media/addconnectionlocaldb.png)

    ![연결 Express 추가](~/ef6/media/addconnectionexpress.png)

-   **확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.

    ![데이터베이스 만들기](~/ef6/media/createdatabase.png)

-   이제 새 데이터베이스가 서버 탐색기에 표시 되 면 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.
-   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.

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

#### <a name="reverse-engineer-model"></a>리버스 엔지니어링 모델

Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하 여 모델을 만들 예정입니다.

-   **프로젝트-새 항목 추가&gt; ...**
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델
-   이름으로 **제품 모델** 을 입력 하 고 **확인을** 클릭 합니다.
-   그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.
-   **데이터베이스에서 생성** 을 선택 하 고 **다음** 을 클릭 합니다.

    ![모델 콘텐츠 선택](~/ef6/media/choosemodelcontents.png)

-   첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 연결 문자열 이름으로 **제품 컨텍스트** 를 입력 한 후 **다음** 을 클릭 합니다.

    ![연결 선택](~/ef6/media/chooseyourconnection.png)

-   ' 테이블 ' 옆의 확인란을 클릭 하 여 모든 테이블을 가져온 다음 ' 마침 '을 클릭 합니다.

    ![개체 선택](~/ef6/media/chooseyourobjects.png)

리버스 엔지니어링 프로세스가 완료 되 면 새 모델이 프로젝트에 추가 되 고 Entity Framework Designer에서 볼 수 있도록 열립니다. 또한 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 App.config 파일이 프로젝트에 추가 되었습니다.

#### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010의 추가 단계

Visual Studio 2010에서 작업 하는 경우 EF6 코드 생성을 사용 하도록 EF designer를 업데이트 해야 합니다.

-   EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.
-   왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.
-   **C\#에 대해 EF 6.X DbContext 생성기를 선택 하** 고 이름으로 **ProductsModel** 를 입력 한 다음 추가를 클릭 합니다.

#### <a name="updating-code-generation-for-data-binding"></a>데이터 바인딩에 대 한 코드 생성 업데이트

EF는 T4 템플릿을 사용 하 여 모델에서 코드를 생성 합니다. Visual Studio와 함께 제공 되거나 Visual Studio 갤러리에서 다운로드 한 템플릿은 일반적인 용도로 사용 하기 위한 것입니다. 즉, 이러한 템플릿에서 생성 된 엔터티에는 간단한 ICollection&lt;T&gt; 속성이 있습니다. 그러나 WPF를 사용 하 여 데이터 바인딩을 수행할 때 WPF가 컬렉션에 대 한 변경 내용을 추적할 수 있도록 컬렉션 속성에 대해 **system.collections.objectmodel.observablecollection** 를 사용 하는 것이 좋습니다. 이를 위해 System.collections.objectmodel.observablecollection를 사용 하도록 템플릿을 수정 하겠습니다.

-   **솔루션 탐색기** 를 열고 **제품 모델 .edmx** 파일을 찾습니다.
-   **ProductModel.tt** 파일을 찾습니다 .이 파일은 제품 모델 .edmx 파일 아래에 중첩 될 수 있습니다.

    ![WPF 제품 모델 템플릿](~/ef6/media/wpfproductmodeltemplate.png)

-   ProductModel.tt 파일을 두 번 클릭 하 여 Visual Studio 편집기에서 엽니다.
-   "**ICollection**"의 두 항목을 찾아 "**system.collections.objectmodel.observablecollection**"로 바꿉니다. 이러한 설정은 약 296 및 484 줄에 있습니다.
-   "**Hashset**"의 첫 번째 항목을 찾아 "**system.collections.objectmodel.observablecollection**"로 바꿉니다. 이러한 발생은 약 50 줄에 있습니다. 코드에서 나중에 발견 된 두 번째 HashSet을 바꾸지 **마십시오** .
-   "**System.object**"를 찾아 "system.object"를 "**system.object**"로 바꿉니다. 이는 약 424 줄에 있습니다.
-   ProductModel.tt 파일을 저장 합니다. 이로 인해 엔터티가 다시 생성 됩니다. 코드가 자동으로 다시 생성 되지 않는 경우 ProductModel.tt를 마우스 오른쪽 단추로 클릭 하 고 "사용자 지정 도구 실행"을 선택 합니다.

이제 ProductModel.tt 아래에 중첩 된 Category.cs 파일을 열면 Products 컬렉션에 **system.collections.objectmodel.observablecollection&lt;Product&gt;** 형식이 표시 됩니다.

프로젝트를 컴파일합니다.

## <a name="lazy-loading"></a>지연 로드

**Product** 클래스의 **Category** 클래스 및 **Category** 속성에 대 한 **Products** 속성은 탐색 속성입니다. Entity Framework 탐색 속성은 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.

EF는 탐색 속성에 처음 액세스할 때 데이터베이스에서 관련 엔터티를 자동으로 로드 하는 옵션을 제공 합니다. 이 유형의 로드 (지연 로드)를 사용 하면 각 탐색 속성에 처음 액세스할 때 내용이 컨텍스트에 아직 없는 경우 데이터베이스에 대해 별도의 쿼리가 실행 됩니다.

POCO 엔터티 형식을 사용 하는 경우 EF는 런타임 중에 파생 된 프록시 형식의 인스턴스를 만든 다음 클래스의 가상 속성을 재정의 하 여 로드 후크를 추가 함으로써 지연 로드를 달성 합니다. 관련 개체의 지연 로드를 얻으려면 탐색 속성 getter를 **public** 및 **virtual** (Visual Basic에서**재정의** 가능)로 선언 하 고 클래스는 **sealed** 가 아니어야 합니다 (Visual Basic에서**NotOverridable** ). Database First 사용 하는 경우 지연 로드를 사용할 수 있도록 탐색 속성이 자동으로 가상으로 만들어집니다. Code First 섹션에서는 동일한 이유로 탐색 속성을 가상으로 만들도록 선택 했습니다.

## <a name="bind-object-to-controls"></a>컨트롤에 개체 바인딩

모델에 정의 된 클래스를이 WPF 응용 프로그램에 대 한 데이터 소스로 추가 합니다.

-   솔루션 탐색기에서 Mainwindow.xaml를 두 번 클릭 하 여 기본 폼을 엽니다 **.**
-   주 메뉴에서 **프로젝트-&gt; 새 데이터 소스 추가** ...를 선택 합니다.
    Visual Studio 2010에서는 데이터&gt; 선택 하 고 **새 데이터 소스 추가**...를 선택 해야 합니다.
-   데이터 소스 형식 선택 창에서 **개체** 를 선택 하 고 **다음** 을 클릭 합니다.
-   데이터 개체 선택 대화 상자에서 **WPFwithEFSample** 두 번 펼침 **범주** 를 선택 합니다.  
    ***범주** 데이터 원본에서 **제품**의 속성을 통해 **제품** 데이터 원본을 선택할 필요가 없습니다.*  

    ![데이터 개체 선택](~/ef6/media/selectdataobjects.png)

-   **마침**을 클릭합니다.
-   데이터 소스 창이 표시 되지 않으면 Mainwindow.xaml 창 옆에 있는 데이터 소스 창이 열립니다.  ***다른 Windows&gt; 데이터 소스를&gt;** 선택* 합니다.
-   데이터 소스 창이 자동으로 숨겨지지 않도록 고정 아이콘을 누릅니다. 창이 이미 표시 되는 경우 새로 고침 단추를 눌러야 할 수 있습니다.

    ![데이터 원본](~/ef6/media/datasources.png)

-    **범주** 데이터 원본을 선택 하 고 폼에서 끌어 옵니다.

이 소스를 끌 때 발생 하는 작업은 다음과 같습니다.

-   **Categoryviewsource** 리소스 및 **CATEGORYDATAGRID** 컨트롤이 XAML에 추가 되었습니다. 
-   부모 그리드 요소의 DataContext 속성이 "{StaticResource **Categoryviewsource** }" (으)로 설정 되었습니다. **Categoryviewsource** 리소스는 외부\\부모 그리드 요소에 대 한 바인딩 소스로 사용 됩니다. 그러면 내부 그리드 요소가 부모 그리드에서 DataContext 값을 상속 합니다 (categoryDataGrid의 System.windows.controls.itemscontrol.itemssource 속성은 "{Binding}"으로 설정 됨).

``` xml
    <Window.Resources>
        <CollectionViewSource x:Key="categoryViewSource"
                                d:DesignSource="{d:DesignInstance {x:Type local:Category}, CreateList=True}"/>
    </Window.Resources>
    <Grid DataContext="{StaticResource categoryViewSource}">
        <DataGrid x:Name="categoryDataGrid" AutoGenerateColumns="False" EnableRowVirtualization="True"
                    ItemsSource="{Binding}" Margin="13,13,43,191"
                    RowDetailsVisibilityMode="VisibleWhenSelected">
            <DataGrid.Columns>
                <DataGridTextColumn x:Name="categoryIdColumn" Binding="{Binding CategoryId}"
                                    Header="Category Id" Width="SizeToHeader"/>
                <DataGridTextColumn x:Name="nameColumn" Binding="{Binding Name}"
                                    Header="Name" Width="SizeToHeader"/>
            </DataGrid.Columns>
        </DataGrid>
    </Grid>
```

## <a name="adding-a-details-grid"></a>세부 정보 그리드 추가

이제 범주를 표시 하는 그리드를 사용 하 여 연결 된 제품을 표시 하는 세부 정보 표를 추가 합니다.

-    **범주** 데이터 원본 아래에서 **Products** 속성을 선택 하 고 폼으로 끕니다.
    -   **CategoryProductsViewSource** 리소스 및 **제품 DATAGRID** 그리드는 XAML에 추가 됩니다.
    -   이 리소스의 바인딩 경로를 Products로 설정 합니다.
    -   WPF 데이터 바인딩 프레임 워크는 선택한 범주와 관련 된 제품만 제품 **datagrid** 에 표시 되도록 합니다.
-   도구 상자에서 **단추** 를 폼으로 끌어 옵니다. **Name** 속성을 **buttonsave** 로 설정 하 고 **Content** 속성을 **저장**으로 설정 합니다.

폼은 다음과 같이 표시 됩니다.

![디자이너](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>데이터 상호 작용을 처리 하는 코드 추가

주 창에 일부 이벤트 처리기를 추가할 때입니다.

-   XAML 창에서 **&lt;창** 요소를 클릭 하면 주 창이 선택 됩니다.
-   **속성** 창의 오른쪽 위에서 **이벤트** 를 선택 하 고 **로드** 된 레이블의 오른쪽에 있는 텍스트 상자를 두 번 클릭 합니다.

    ![주 창 속성](~/ef6/media/mainwindowproperties.png)

-   또한 디자이너에서 저장 단추를 두 번 클릭 하 여 **저장** 단추에 대 한 **클릭** 이벤트를 추가 합니다. 

그러면 폼의 코드 숨김으로 이동 하 고, 이제 제품 컨텍스트를 사용 하 여 데이터 액세스를 수행 하는 코드를 편집 합니다. 아래와 같이 Mainwindow.xaml에 대 한 코드를 업데이트 합니다.

이 코드는 장기적으로 실행 되는 **제품 컨텍스트의**인스턴스를 선언 합니다. **제품 컨텍스트** 개체는 데이터베이스에 데이터를 쿼리하고 저장 하는 데 사용 됩니다. 그러면 **제품 컨텍스트** 인스턴스의 **Dispose ()** 가 재정의 된 **onclosing** 메서드에서 호출 됩니다. 코드 주석은 코드가 수행 하는 작업에 대 한 세부 정보를 제공 합니다.

``` csharp
    using System.Data.Entity;
    using System.Linq;
    using System.Windows;

    namespace WPFwithEFSample
    {
        public partial class MainWindow : Window
        {
            private ProductContext _context = new ProductContext();
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                System.Windows.Data.CollectionViewSource categoryViewSource =
                    ((System.Windows.Data.CollectionViewSource)(this.FindResource("categoryViewSource")));

                // Load is an extension method on IQueryable,
                // defined in the System.Data.Entity namespace.
                // This method enumerates the results of the query,
                // similar to ToList but without creating a list.
                // When used with Linq to Entities this method
                // creates entity objects and adds them to the context.
                _context.Categories.Load();

                // After the data is loaded call the DbSet<T>.Local property
                // to use the DbSet<T> as a binding source.
                categoryViewSource.Source = _context.Categories.Local;
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                // When you delete an object from the related entities collection
                // (in this case Products), the Entity Framework doesn’t mark
                // these child entities as deleted.
                // Instead, it removes the relationship between the parent and the child
                // by setting the parent reference to null.
                // So we manually have to delete the products
                // that have a Category reference set to null.

                // The following code uses LINQ to Objects
                // against the Local collection of Products.
                // The ToList call is required because otherwise the collection will be modified
                // by the Remove call while it is being enumerated.
                // In most other situations you can use LINQ to Objects directly
                // against the Local property without using ToList first.
                foreach (var product in _context.Products.Local.ToList())
                {
                    if (product.Category == null)
                    {
                        _context.Products.Remove(product);
                    }
                }

                _context.SaveChanges();
                // Refresh the grids so the database generated values show up.
                this.categoryDataGrid.Items.Refresh();
                this.productsDataGrid.Items.Refresh();
            }

            protected override void OnClosing(System.ComponentModel.CancelEventArgs e)
            {
                base.OnClosing(e);
                this._context.Dispose();
            }
        }

    }
```

## <a name="test-the-wpf-application"></a>WPF 응용 프로그램 테스트

-   애플리케이션을 컴파일하고 실행합니다. Code First 사용 하는 경우 **WPFwithEFSample 컨텍스트** 데이터베이스가 생성 된 것을 알 수 있습니다.
-   *데이터베이스에서 기본 키가 생성 되기 때문* 에 맨 위 표에 범주 이름을 입력 하 고 아래쪽 표에 있는 제품 이름에는 ID 열에 아무것도 입력 하지 않습니다.

    ![새 범주 및 제품을 포함 하는 주 창](~/ef6/media/screen1.png)

-   **저장** 단추를 눌러 데이터를 데이터베이스에 저장 합니다.

DbContext의 **SaveChanges ()** 에 대 한 호출 후 id는 데이터베이스에서 생성 된 값으로 채워집니다. **SaveChanges ()** 후에 **Refresh ()** 를 호출 했으므로 **DataGrid** 컨트롤도 새 값으로 업데이트 됩니다.

![Id가 채워진 주 창](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>추가 리소스

WPF를 사용 하 여 컬렉션에 데이터를 바인딩하는 방법에 대해 자세히 알아보려면 WPF 설명서에서 [이 항목](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) 을 참조 하세요.  
