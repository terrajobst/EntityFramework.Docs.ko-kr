---
title: WPF를 가진 데이터 바인딩 - EF6
author: divega
ms.date: 04/02/2020
ms.assetid: e90d48e6-bea7785-47ef-b756-7b89cce4daf0
ms.openlocfilehash: 6908e2a7597d0c199620c6015ed3ea06226c5ea9
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80639138"
---
> [!IMPORTANT]
> **이 문서는 .NET 프레임워크에서만 WPF에 대해 유효합니다.**
>
> 이 문서에서는 .NET 프레임워크에서 WPF에 대한 데이터 바인딩에 대해 설명합니다. 새 .NET Core 프로젝트의 경우 엔터티 프레임워크 6 대신 [EF 코어를](/ef/core) 사용하는 것이 좋습니다. EF 코어의 데이터 바인딩에 대한 설명서는 [문제 #778](https://github.com/dotnet/EntityFramework.Docs/issues/778)추적됩니다.

# <a name="databinding-with-wpf"></a>WPF를 사용한 데이터 바인딩
이 단계별 연습에서는 POCO 형식을 WPF 컨트롤에 "마스터 세부 정보" 형식으로 바인딩하는 방법을 보여 주며 응용 프로그램은 Entity Framework API를 사용하여 개체에 데이터베이스의 데이터를 채우고, 변경 내용을 추적하고, 데이터베이스에 데이터를 유지합니다.

모델은 일대다 관계에 참여하는 두 가지 유형인 **범주(주** \\마스터)와\\ **제품(종속** 세부 정보)을 정의합니다. 그런 다음 Visual Studio 도구를 사용하여 모델에 정의된 형식을 WPF 컨트롤에 바인딩합니다. WPF 데이터 바인딩 프레임워크를 사용하면 관련 개체 간에 탐색할 수 있습니다.

이 연습의 스크린샷및 코드 목록은 Visual Studio 2013에서 가져온 것이지만 Visual Studio 2012 또는 Visual Studio 2010을 통해 이 연습을 완료할 수 있습니다.

## <a name="use-the-object-option-for-creating-wpf-data-sources"></a>WPF 데이터 원본을 만들기 위한 '개체' 옵션 사용

이전 버전의 엔터티 프레임워크에서는 EF 디자이너로 만든 모델을 기반으로 새 데이터 원본을 만들 때 **데이터베이스** 옵션을 사용하는 것이 좋습니다. 이는 디자이너가 EntityContext에서 파생된 컨텍스트 및 EntityObject에서 파생된 엔터티 클래스에서 파생된 컨텍스트를 생성하기 때문입니다. 데이터베이스 옵션을 사용하면 이 API 표면과 상호 작용하는 데 가장 적합한 코드를 작성하는 데 도움이 됩니다.

EF 비주얼 스튜디오 2012 및 비주얼 스튜디오 2013에 대 한 EF 디자이너 간단한 POCO 엔터티 클래스와 함께 DbContext에서 파생 된 컨텍스트를 생성 합니다. Visual Studio 2010에서는 이 연습의 후반부에서 설명한 대로 DbContext를 사용하는 코드 생성 템플릿으로 바꾸는 것이 좋습니다.

DbContext API 표면을 사용할 때는 이 연습과 같이 새 데이터 원본을 만들 때 **개체** 옵션을 사용해야 합니다.

필요한 경우 EF 디자이너로 만든 모델에 대해 [ObjectContext 기반 코드 생성으로 되돌릴](~/ef6/modeling/designer/codegen/legacy-objectcontext.md) 수 있습니다.

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료하려면 Visual Studio 2013, Visual Studio 2012 또는 Visual Studio 2010을 설치해야 합니다.

Visual Studio 2010을 사용하는 경우 NuGet도 설치해야 합니다. 자세한 내용은 [NuGet 설치를](https://docs.microsoft.com/nuget/install-nuget-client-tools)참조하십시오.  

## <a name="create-the-application"></a>애플리케이션 만들기

-   Visual Studio 열기
-   **파일&gt; -&gt; 새로운 - 프로젝트....**
-   왼쪽 창에서 **창** 선택 및 **WPF오른쪽** 창에서 응용 프로그램
-    **이름으로 WPFwithEFSample을** 입력합니다.
-    **확인** 선택

## <a name="install-the-entity-framework-nuget-package"></a>엔터티 프레임워크 NuGet 패키지 설치

-   솔루션 탐색기에서 **WinFormswithEFSample** 프로젝트를 마우스 오른쪽 버튼으로 클릭합니다.
-   **NuGet 패키지 관리를 선택합니다...**
-   NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택하고 **EntityFramework** 패키지를 선택합니다.
-   **설치를 클릭합니다.**  
    >[!NOTE]
    > EntityFramework 어셈블리 외에도 System.ComponentModel.DataAnnotations에 대한 참조도 추가됩니다. 프로젝트에 System.Data.Entity에 대한 참조가 있는 경우 EntityFramework 패키지가 설치되면 제거됩니다. System.Data.Entity 어셈블리는 더 이상 엔터티 프레임워크 6 응용 프로그램에 사용되지 않습니다.

## <a name="define-a-model"></a>모델 정의

이 연습에서는 코드 퍼스트 또는 EF 디자이너를 사용하여 모델을 구현하도록 선택할 수 있습니다. 다음 두 섹션 중 하나를 완료합니다.

### <a name="option-1-define-a-model-using-code-first"></a>옵션 1: 코드 우선을 사용하여 모델 정의

이 섹션에서는 Code First를 사용하여 모델 및 관련 데이터베이스를 만드는 방법을 보여 주며 이 섹션에서는 EF 디자이너를 사용하여 데이터베이스에서 모델을 리버스 엔지니어링하려면 다음**섹션(옵션 2: 데이터베이스 를 먼저 사용하여 모델 정의)으로** 건너뜁니다.

Code First 개발을 사용하는 경우 일반적으로 개념적(도메인) 모델을 정의하는 .NET Framework 클래스를 작성합니다.

-    **WPFwithEFSample에** 새 클래스를 추가합니다.
    -   프로젝트 이름을 마우스 오른쪽 버튼으로 클릭
    -   추가 다음 **새 항목** **선택**
    -   **클래스를** 선택하고 클래스 이름에 대한 **제품을** 입력합니다.
-    **제품** 클래스 정의를 다음 코드로 바꿉니다.

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

**Product** 클래스의 **범주** 클래스 및 **범주** 속성의 Products **속성은** 탐색 속성입니다. 엔터티 Framework에서 탐색 속성은 두 엔터티 형식 간의 관계를 탐색하는 방법을 제공합니다.

엔터티를 정의하는 것 외에도 DbContext에서 파생되고 DbSet&lt;TEntity&gt; 속성을 노출하는 클래스를 정의해야 합니다. DbSet&lt;TEntity&gt; 속성을 사용하면 컨텍스트에서 모델에 포함할 형식을 알 수 있습니다.

DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리하며, 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 대한 데이터 유지가 포함됩니다.

-   다음 정의로 프로젝트에 새 **ProductContext** 클래스를 추가합니다.

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

### <a name="option-2-define-a-model-using-database-first"></a>옵션 2: 데이터베이스 를 먼저 사용하여 모델 정의

이 섹션에서는 데이터베이스 First를 사용하여 EF 디자이너를 사용하여 데이터베이스에서 모델을 리버스 엔지니어링하는 방법을 보여 줍니다. 이전**섹션(옵션 1: 코드 우선을 사용하여 모델 정의)을**완료한 경우 이 섹션을 건너뛰고 **지연 로드** 섹션으로 바로 이동합니다.

#### <a name="create-an-existing-database"></a>기존 데이터베이스 만들기

일반적으로 기존 데이터베이스를 대상으로 하는 경우 이미 만들어지지만 이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.

Visual Studio에 설치된 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용하는 경우 SQL Express 데이터베이스를 만듭니다.
-   Visual Studio 2012를 사용하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스를 만듭니다.

데이터베이스를 생성해 보겠습니다.

-   **보기&gt; - 서버 탐색기**
-   데이터 연결을 마우스 오른쪽 버튼으로 클릭 **-&gt; 연결 추가...**
-   Microsoft SQL Server를 데이터 원본으로 선택하기 전에 서버 탐색기의 데이터베이스에 연결하지 않은 경우

    ![데이터 소스 변경](~/ef6/media/changedatasource.png)

-   설치한 로컬DB 또는 SQL Express에 연결하고 **제품을** 데이터베이스 이름으로 입력합니다.

    ![연결 로컬DB 추가](~/ef6/media/addconnectionlocaldb.png)

    ![연결 익스프레스 추가](~/ef6/media/addconnectionexpress.png)

-   **확인을** 선택하면 새 데이터베이스를 만들지 묻는 메시지가 표시되고 **예(예)를** 선택합니다.

    ![데이터베이스 만들기](~/ef6/media/createdatabase.png)

-   이제 새 데이터베이스가 서버 탐색기에서 나타나고 마우스 오른쪽 단추로 클릭하고 **새 쿼리를** 선택합니다.
-   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭하고 **실행을** 선택합니다.

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

#### <a name="reverse-engineer-model"></a>리버스 엔지니어 모델

Visual Studio의 일부로 포함된 엔터티 프레임워크 디자이너를 사용하여 모델을 만들 겠습니다.

-   **프로젝트&gt; - 새 항목 추가...**
-   왼쪽 메뉴에서 **데이터를** 선택한 다음 **엔터티 데이터 모델을 ADO.NET**
-   제품 모델을 이름으로 입력하고 **확인을** **클릭합니다.**
-   그러면 **엔터티 데이터 모델 마법사가** 시작됩니다.
-   **데이터베이스에서 생성을** 선택하고 **다음을 클릭합니다.**

    ![모델 콘텐츠 선택](~/ef6/media/choosemodelcontents.png)

-   첫 번째 섹션에서 만든 데이터베이스에 대한 연결을 선택하고 연결 문자열의 이름으로 **ProductContext를** 입력하고 **다음을 클릭합니다.**

    ![연결 선택](~/ef6/media/chooseyourconnection.png)

-   '테이블' 옆의 확인란을 클릭하여 모든 테이블을 가져오고 '완료'를 클릭합니다.

    ![개체 선택](~/ef6/media/chooseyourobjects.png)

리버스 엔지니어 프로세스가 완료되면 새 모델이 프로젝트에 추가되고 엔터티 프레임워크 디자이너에서 볼 수 있도록 열립니다. 데이터베이스에 대한 연결 세부 정보가 있는 App.config 파일도 프로젝트에 추가되었습니다.

#### <a name="additional-steps-in-visual-studio-2010"></a>비주얼 스튜디오 2010의 추가 단계

Visual Studio 2010에서 작업하는 경우 EF6 코드 생성을 사용하려면 EF 디자이너를 업데이트해야 합니다.

-   EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 버튼으로 클릭하고 **코드 생성 항목 추가를 선택합니다...**
-   왼쪽 메뉴에서 **온라인 템플릿을** 선택하고 **DbContext를** 검색합니다.
-   **\#C에 대한 EF 6.x DbContext 생성기를** 선택하고 **제품 모델을** 이름으로 입력하고 추가를 클릭합니다.

#### <a name="updating-code-generation-for-data-binding"></a>데이터 바인딩을 위한 코드 생성 업데이트

EF는 T4 템플릿을 사용하여 모델에서 코드를 생성합니다. Visual Studio와 함께 제공되거나 Visual Studio 갤러리에서 다운로드한 템플릿은 범용용입니다. 즉, 이러한 템플릿에서 생성된 엔터티에는 간단한&lt;&gt; ICollection T 속성이 있습니다. 그러나 WPF를 사용하여 데이터 바인딩을 수행할 때WPF가 컬렉션에 대한 변경 내용을 추적할 수 있도록 컬렉션 속성에 **대해 ObservableCollection를** 사용하는 것이 좋습니다. 이를 위해 ObservableCollection를 사용하도록 템플릿을 수정합니다.

-   솔루션 **탐색기를** 열고 **ProductModel.edmx** 파일을 찾습니다.
-   ProductModel.edmx 파일 아래에 중첩될 **ProductModel.tt** 파일 찾기

    ![WPF 제품 모델 템플릿](~/ef6/media/wpfproductmodeltemplate.png)

-   ProductModel.tt 파일을 두 번 클릭하여 Visual Studio 편집기에서 엽니다.
-   **"ICollection"** 의 두 발생을 **"관찰 가능 컬렉션"으로**찾아 서 바꿉습니다. 이들은 대략 296 호선과 484 호선에 있습니다.
-   **"해시셋"의**첫 번째 발생을 **"관찰 가능 컬렉션"으로**찾아 바꿉습니다. 이 발생은 약 50줄에 있습니다. 코드의 나중에 발견된 HashSet의 두 번째 발생을 **대체하지 마십시오.**
-   **"System.Collections.Generic"의**유일한 발생을 찾아서 **"System.Collections.ObjectModel"로 바꿉습니다.** 이 곳은 424호선에 있습니다.
-   ProductModel.tt 파일을 저장합니다. 이렇게 하면 엔터티에 대한 코드가 다시 생성됩니다. 코드가 자동으로 재생성되지 않으면 ProductModel.tt 마우스 오른쪽 버튼으로 클릭하고 "사용자 지정 도구 실행"을 선택합니다.

이제 Category.cs 파일(ProductModel.tt 아래에 중첩)을 열면 제품 컬렉션에 **Observable&lt;제품&gt;** 이 유형이 있음을 알 수 있습니다.

프로젝트를 컴파일합니다.

## <a name="lazy-loading"></a>지연 로드

**Product** 클래스의 **범주** 클래스 및 **범주** 속성의 Products **속성은** 탐색 속성입니다. 엔터티 Framework에서 탐색 속성은 두 엔터티 형식 간의 관계를 탐색하는 방법을 제공합니다.

EF는 탐색 속성에 처음 액세스할 때 데이터베이스에서 관련 엔터티를 자동으로 로드하는 옵션을 제공합니다. 이러한 유형의 로드(지연 로드라고 함)를 사용하면 각 탐색 속성에 처음 액세스할 때 내용이 컨텍스트에 없는 경우 데이터베이스에 대해 별도의 쿼리가 실행됩니다.

POCO 엔터티 형식을 사용하는 경우 EF는 런타임 중에 파생 프록시 형식의 인스턴스를 만든 다음 클래스의 가상 속성을 재정의하여 로드 후크를 추가하여 지연 로드를 달성합니다. 관련 개체의 지연 로드를 얻으려면 탐색 속성 getter를 **공용** 및 **가상(Visual** Basic에서**재정의 가능)으로** 선언해야 하며 클래스는 **봉인되지** 않아야 합니다(Visual Basic에서**초과 ridable).** 데이터베이스 First 탐색 속성을 사용하는 경우 지연 로드를 사용하도록 가상으로 자동으로 만들어집니다. 코드 첫 번째 섹션에서 동일한 이유로 탐색 속성을 가상으로 만들기로 결정했습니다.

## <a name="bind-object-to-controls"></a>개체를 컨트롤에 바인딩

모델에 정의된 클래스를 이 WPF 응용 프로그램의 데이터 원본으로 추가합니다.

-   솔루션 탐색기에서 **MainWindow.xaml을** 두 번 클릭하여 기본 양식을 엽니다.
-   메인 메뉴에서 **프로젝트 선택&gt; - 새 데이터 원본 추가 ...**
    (Visual Studio 2010에서 데이터를 선택해야 합니다 **-&gt; 새 데이터 원본 추가...**)
-   데이터 원본 유형 창 선택에서 **개체를** 선택하고 **다음을 클릭합니다.**
-   데이터 개체 선택 대화 상자에서 **WPFwithEFSample를** 두 번 펼치고 **범주를** 선택합니다.  
    ***범주** 데이터 원본의 **제품**속성을 통해 제품 데이터 원본을 선택할 필요가 없으므로 **제품** 데이터 원본을 선택할 필요가 없습니다.*  

    ![데이터 개체 선택](~/ef6/media/selectdataobjects.png)

-   **마침**을 클릭합니다.
-   데이터 원본 창 이 MainWindow.xaml 창 옆에 *열리면 데이터 원본 창이 표시되지 않으면 **보기 -&gt; 다른&gt; Windows- 데이터 원본을** 선택합니다.*
-   핀 아이콘을 누르면 데이터 원본 창이 자동으로 숨겨지지 않습니다. 창이 이미 표시되어 있는 경우 새로 고침 단추를 눌러야 할 수 있습니다.

    ![솔루션 탐색기](~/ef6/media/datasources.png)

-    **범주** 데이터 원본을 선택하고 양식에 드래그합니다.

이 소스를 드래그할 때 다음과 같은 일이 발생했습니다.

-   **범주뷰소스** 리소스 및 **범주DataGrid** 컨트롤이 XAML에 추가되었습니다. 
-   상위 그리드 요소의 DataContext 속성이 "{StaticResource **범주ViewSource** }"로 설정되었습니다.**categoryViewSource** 리소스외부\\부모 Grid 요소에 대 한 바인딩 소스 역할을 합니다. 그런 다음 내부 그리드 요소는 상위 그리드에서 DataContext 값을 상속합니다(범주DataGrid의 ItemsSource 속성은 "{Binding}"로 설정됩니다).

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

이제 범주를 표시하는 그리드가 있으므로 관련 제품을 표시하기 위해 세부 정보 표를 추가해 보겠습니다.

-    **범주** 데이터 원본 아래에서 **Products** 속성을 선택하고 양식에 끕을 끕습니다.
    -   **범주ProductsViewSource** 리소스 및 **제품데이터 그리드가** XAML에 추가됩니다.
    -   이 리소스의 바인딩 경로가 제품으로 설정됩니다.
    -   WPF 데이터 바인딩 프레임워크는 선택한 범주와 관련된 제품만 **productDataGrid에** 표시되도록 합니다.
-   도구 상자에서 **단추를** 양식으로 끕습니다. **Name** 속성을 **buttonSave로** 설정하고 **콘텐츠** 속성을 **저장하도록 설정합니다.**

양식은 다음과 유사해야 합니다.

![Designer](~/ef6/media/designer.png) 

## <a name="add-code-that-handles-data-interaction"></a>데이터 상호 작용을 처리하는 코드 추가

일부 이벤트 처리기를 기본 창에 추가할 차례입니다.

-   XAML 창에서 ** &lt;창** 요소를 클릭하면 주 창이 선택됩니다.
-   **속성** 창에서 오른쪽 상단의 **이벤트를** 선택한 다음 **로드된** 레이블의 오른쪽에 있는 텍스트 상자를 두 번 클릭합니다.

    ![기본 창 속성](~/ef6/media/mainwindowproperties.png)

-   또한 디자이너에서 저장 단추를 두 번 클릭하여 **저장** 단추에 대한 **클릭** 이벤트를 추가합니다. 

이렇게 하면 양식뒤에 있는 코드로 이동하면 이제 ProductContext를 사용하여 데이터 액세스를 수행하기 위해 코드를 편집합니다. 아래 와 같이 MainWindow의 코드를 업데이트합니다.

코드는 **ProductContext**의 장기 실행 인스턴스를 선언합니다. **ProductContext** 개체는 데이터를 쿼리하고 데이터베이스에 저장하는 데 사용됩니다. 그런 다음 제품 **컨텍스트** 인스턴스의 **Dispose()가** 재정의된 **OnClosing** 메서드에서 호출됩니다.코드 주석은 코드가 수행하는 작업에 대한 세부 정보를 제공합니다.

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

-   애플리케이션을 컴파일하고 실행합니다. 코드 퍼스트를 사용한 경우 **WPFwithEFSample.ProductContext** 데이터베이스가 만들어집니다.
-   맨 위 표에 범주 이름을 입력하고 아래쪽 표의 제품 이름을 *입력하려면 데이터베이스에서 기본 키가 생성되므로 ID 열에 아무 것도 입력하지 마십시오.*

    ![새로운 카테고리와 제품이 있는 메인 윈도우](~/ef6/media/screen1.png)

-   **저장** 버튼을 눌러 데이터를 데이터베이스에 저장합니다.

DbContext의 **SaveChanges()를**호출한 후 인덱스는 데이터베이스에서 생성된 값으로 채워집니다. **SaveChanges()** 후에 **새로 고침()을** 호출했기 때문에 **DataGrid** 컨트롤도 새 값으로 업데이트됩니다.

![아이디가 채워진 주 창](~/ef6/media/screen2.png)

## <a name="additional-resources"></a>추가 리소스

WPF를 사용하여 컬렉션에 대한 데이터 바인딩에 대해 자세히 알아보려면 WPF 설명서에서 [이 항목을](https://docs.microsoft.com/dotnet/framework/wpf/data/data-binding-overview#binding-to-collections) 참조하세요.  
