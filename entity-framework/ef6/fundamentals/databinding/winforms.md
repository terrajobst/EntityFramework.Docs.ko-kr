---
title: WinForms-EF6 사용 하 여 데이터 바인딩
author: divega
ms.date: 2016-10-23
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: 7ceb8e85fe3d8f5ab9a5e58ef9c84599585d8f77
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994531"
---
# <a name="databinding-with-winforms"></a>WinForms 사용 하 여 데이터 바인딩
이 단계별 연습에는 POCO 형식 "마스터-세부 정보" 형태로 Window Forms (WinForms) 컨트롤에 바인딩하는 방법을 보여 줍니다. Entity Framework를 사용 하 여 데이터베이스에서 데이터를 사용 하 여 개체를 채우기, 변경 내용 추적 및 데이터베이스에 데이터를 유지 하는 응용 프로그램입니다.

모델에 일 대 다 관계에 참여 하는 두 가지 형식을 정의 합니다: 범주 (주\\마스터) 및 제품 (종속\\세부 정보). 그런 다음 Visual Studio 도구는 WinForms 컨트롤 모델에서 정의 된 형식에 바인딩할 사용 됩니다. WinForms 데이터 바인딩 프레임 워크에 관련 된 개체 사이 탐색할 수 있습니다: 세부 정보 뷰에서 해당 자식 데이터를 사용 하 여 업데이트를 사용 하면 마스터 뷰에 행을 선택 합니다.

스크린샷 및이 연습의 코드 샘플은 Visual Studio 2013에서 가져옵니다 하지만 Visual Studio 2012 또는 Visual Studio 2010을 사용 하 여이 연습을 완료할 수 있습니다.

## <a name="pre-requisites"></a>필수 조건

이 연습을 완료 하려면 Visual Studio 2012 또는 Visual Studio 2010 설치, Visual Studio 2013이 설치 해야 합니다.

Visual Studio 2010을 사용 하는 경우 NuGet을 설치 하려면 수도 있습니다. 자세한 내용은 [NuGet 설치](http://docs.nuget.org/docs/start-here/installing-nuget)합니다.

## <a name="create-the-application"></a>응용 프로그램 만들기

-   Visual Studio를 엽니다.
-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Windows** 왼쪽된 창에서와 **Windows FormsApplication** 오른쪽 창에서
-   입력 **WinFormswithEFSample** 이름으로
-   **확인**을 선택합니다.

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet 패키지를 설치 합니다.

-   솔루션 탐색기에서 마우스 오른쪽 단추로 클릭 합니다 **WinFormswithEFSample** 프로젝트
-   선택 **NuGet 패키지 관리...**
-   NuGet 패키지 관리 대화 상자에서 선택 합니다 **Online** 탭을 선택 합니다 **EntityFramework** 패키지
-   클릭 **설치**  
    > [!NOTE]
    > EntityFramework 어셈블리 외에도 System.ComponentModel.DataAnnotations에 대 한 참조도 추가 됩니다. 프로젝트 System.Data.Entity에 대 한 참조가 있으면 다음 제거 됩니다 EntityFramework 패키지를 설치한 경우. System.Data.Entity 어셈블리는 Entity Framework 6 응용 프로그램에 더 이상 사용 됩니다.

## <a name="implementing-ilistsource-for-collections"></a>컬렉션에 대 한 IListSource 구현

컬렉션 속성에는 Windows Forms를 사용 하는 경우 정렬 된 양방향 데이터 바인딩을 사용할 수 있도록 IListSource 인터페이스를 구현 해야 합니다. 이렇게 하려면 IListSource 기능을 추가 하는 ObservableCollection 확장 하려고 합니다.

-   추가 된 **ObservableListSource** 프로젝트에 클래스:
    -   프로젝트 이름을 마우스 오른쪽 단추로 클릭
    -   선택 **추가-&gt; 새 항목**
    -   선택 **클래스** enter **ObservableListSource** 클래스 이름
-   다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.

*이 클래스를 사용 하면 양방향 데이터 바인딩 뿐만 아니라 정렬 합니다. 클래스는 ObservableCollection에서 파생 됩니다&lt;T&gt; IListSource의 명시적 구현을 추가 합니다. IListSource GetList() 메서드의 ObservableCollection와 동기화 상태로 유지 되는 IBindingList 구현을 반환 하도록 구현 됩니다. IBindingList 구현 ToBindingList에서 생성 된 정렬을 지원 합니다. ToBindingList 확장 메서드는 EntityFramework 어셈블리에 정의 됩니다.*

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
                return _bindingList  (_bindingList = this.ToBindingList());
            }
        }
    }
```

## <a name="define-a-model"></a>모델 정의

이 연습에서는 있습니다 Code First 또는 EF 디자이너를 사용 하 여 모델을 구현 하도록 선택 했습니다. 다음 두 섹션 중 하나를 수행 합니다.

### <a name="option-1-define-a-model-using-code-first"></a>옵션 1: Code First를 사용 하 여 모델 정의

이 섹션에는 모델과 Code First를 사용 하 여 연결된 된 데이터베이스를 만드는 방법을 보여 줍니다. 다음 섹션을 건너뜁니다 (**옵션 2: Database First를 사용 하 여 모델 정의)** 사용 하려는 Database First 순서를 반대로 바꿀 경우 EF 디자이너를 사용 하 여 데이터베이스에서 모델을 설계 합니다.

Code First 개발을 사용 하는 경우 일반적으로 개념적 (도메인) 모델을 정의 하는.NET Framework 클래스를 작성 하 여 시작 합니다.

-   새 **제품** 프로젝트에 클래스
-   다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.

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

-   추가 된 **범주** 프로젝트에 클래스입니다.
-   다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.

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

파생 된 클래스를 정의 해야 엔터티를 정의 하는 것 외에도 **DbContext** 노출 **DbSet&lt;TEntity&gt;**  속성입니다. 합니다 **DbSet** 모델에 포함 하려는 유형을 알고 있어야 하는 컨텍스트 속성을 사용 합니다. 합니다 **DbContext** 하 고 **DbSet** 형식은 EntityFramework 어셈블리에 정의 됩니다.

런타임에 데이터베이스에서 데이터를 사용 하 여 개체를 채우는 포함 하는 엔터티 개체를 관리 하는 파생 된 DbContext 형식의 인스턴스, 데이터베이스, 추적 및 저장 데이터를 변경 합니다.

-   새 **ProductContext** 프로젝트에 클래스입니다.
-   다음 코드를 사용 하 여 기본적으로 생성 된 코드를 바꿉니다.

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

프로젝트를 컴파일합니다.

### <a name="option-2-define-a-model-using-database-first"></a>Database First를 사용 하 여 모델을 정의 하는 옵션 2:

이 섹션에서는 Database First 리버스 엔지니어링을 사용 하는 방법을 EF 디자이너를 사용 하 여 데이터베이스에서 모델을 보여 줍니다. 이전 섹션을 완료 하는 경우 (**옵션 1: Code First를 사용 하 여 모델 정의)**, 그런 다음이 섹션을 건너뛰고 바로 이동 합니다 **지연 로드** 섹션입니다.

#### <a name="create-an-existing-database"></a>기존 데이터베이스를 만들려면

일반적으로 이미 만들어집니다, 기존 데이터베이스를 대상으로 할 때 있지만이 연습에 액세스 하려면 데이터베이스를 만들려고 해야 합니다.

Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.
-   Visual Studio 2012를 사용 하는 경우를 만들 수는 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스입니다.

데이터베이스를 생성 해 보겠습니다.

-   **보기-&gt; 서버 탐색기**
-   마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**
-   Microsoft SQL Server 데이터 원본으로 선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않은 경우

    ![ChangeDataSource](~/ef6/media/changedatasource.png)

-   LocalDB 또는 어느에 따라 설치한 SQL Express에 연결 하 고 입력 **제품** 데이터베이스 이름으로

    ![AddConnectionLocalDB](~/ef6/media/addconnectionlocaldb.png)

    ![AddConnectionExpress](~/ef6/media/addconnectionexpress.png)

-   선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**

    ![CreateDatabase](~/ef6/media/createdatabase.png)

-   새 데이터베이스 이제 서버 탐색기에서 마우스 나타나고 선택 **새 쿼리**
-   새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**

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

#### <a name="reverse-engineer-model"></a>모델 리버스 엔지니어링

모델을 만들려면 Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하겠습니다.

-   **프로젝트-&gt; 새 항목 추가...**
-   선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**
-   입력 **ProductModel** 이름과 클릭 **확인**
-   그러면는 **엔터티 데이터 모델 마법사**
-   선택 **데이터베이스에서 생성** 를 클릭 하 고 **다음**

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   첫 번째 섹션에서 만든 데이터베이스에 연결을 선택, 입력 **ProductContext** 연결 문자열 및 클릭의 이름으로 **다음**

    ![ChooseYourConnection](~/ef6/media/chooseyourconnection.png)

-   모든 테이블을 가져오고 '마침' 클릭 '테이블' 옆의 확인란을 클릭 합니다.

    ![ChooseYourObjects](~/ef6/media/chooseyourobjects.png)

리버스 엔지니어링 프로세스가 완료 되 면 새 모델 프로젝트에 추가 되 고 Entity Framework 디자이너에서 확인할 수 있게 합니다. 또한 App.config 파일을 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 프로젝트에 추가 되었습니다.

#### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010의 추가 단계

Visual Studio 2010에서 작업 하는 경우 EF6 코드 생성을 사용 하 여 EF 디자이너를 업데이트 해야 합니다.

-   EF 디자이너에서 모델의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **코드 생성 항목 추가...**
-   선택 **온라인 템플릿을** 검색에 대 한 확인 하 고 왼쪽된 메뉴에서 **DbContext**
-   선택 합니다 **EF 6.x C에 대 한 DbContext 생성기\#를** 입력 **ProductsModel** 이름으로 추가 클릭 합니다.

#### <a name="updating-code-generation-for-data-binding"></a>데이터 바인딩에 대 한 코드 생성 업데이트

EF T4 템플릿을 사용 하 여 모델에서 코드를 생성 합니다. Visual Studio를 함께 사용 하거나 Visual Studio 갤러리에서 다운로드 한 템플릿에 범용 사용이 됩니다. 즉, 이러한 템플릿에서 생성 한 엔터티 간단한 ICollection&lt;T&gt; 속성입니다. 그러나 데이터 바인딩을 수행할 때 것 컬렉션 속성이 IListSource 구현 하는 것이 바람직합니다. 이 때문에 위의 ObservableListSource 클래스를 만들었습니다 및 있도록 템플릿을 수정 하려고 이제이 클래스를 사용 합니다.

-   엽니다는 **솔루션 탐색기** 찾고 **ProductModel.edmx** 파일
-   찾을 합니다 **ProductModel.tt** ProductModel.edmx 파일 아래에 중첩 될는 파일

    ![ProductModelTemplate](~/ef6/media/productmodeltemplate.png)

-   Visual Studio 편집기에서 열려는 ProductModel.tt 파일을 두 번 클릭
-   찾기 및 바꾸기 2 개 "**ICollection**"with"**ObservableListSource**"입니다. 이들은 296 및 484 약 줄 위치입니다.
-   찾기 및 바꾸기 첫 번째로 나타나는 "**HashSet**"with"**ObservableListSource**"입니다. 이 항목은 약 50 줄에 있습니다. **그렇지 않은** HashSet 코드 나중에 두 번째 항목으로 바꿉니다.
-   ProductModel.tt 파일을 저장 합니다. 이 인해 다시 생성 하는 엔터티에 대 한 코드를 해야 합니다. 코드를 자동으로 다시 생성 하지 않습니다, 경우 ProductModel.tt 마우스 오른쪽 단추로 클릭 하 고 "사용자 지정 도구 실행"을 선택 합니다.

이제 (함은 ProductModel.tt 아래에 중첩) Category.cs 파일을 열 경우 제품 컬렉션 형식에 표시 되어야 **ObservableListSource&lt;제품&gt;** 합니다.

프로젝트를 컴파일합니다.

## <a name="lazy-loading"></a>지연 로드

**제품** 속성에는 **범주** 클래스 및 **범주** 속성에는 **제품** 클래스는 탐색 속성. Entity Framework, 탐색 속성에는 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.

EF는 관련된 엔터티 로드 데이터베이스에서 자동으로 처음 탐색 속성에 액세스 하는 옵션을 제공 합니다. 이 유형의 로드 (지연 로딩 이라고 함)는 각 탐색 속성에 액세스 하는 처음으로 별도 쿼리가 실행 됩니다 데이터베이스에 대 한 내용이 없는 경우 컨텍스트에서 알아야 합니다.

POCO 엔터티 형식을 사용 하는 경우 EF는 런타임에 파생된 프록시 형식의 인스턴스를 만들고 다음 로드 후크를 추가 하려면 클래스에서 가상 속성을 재정의 하 여 지연 로딩을 달성 합니다. 관련된 개체의 지연 로드를 가져오려면 선언 해야 탐색으로 속성 getter **공용** 하 고 **가상** (**Overridable** Visual basic에서), 있습니다 클래스가 아니어야 하 고 **봉인** (**NotOverridable** Visual basic에서). 데이터베이스를 사용 하는 경우 첫 번째 탐색 속성이 자동으로 지연 로드를 사용 하도록 설정 하려면 가상 만들어집니다. 같은 이유로 가상 탐색 속성을 확인 하기로 코드 첫 번째 섹션에서

## <a name="bind-object-to-controls"></a>컨트롤에 개체 바인딩

이 WinForms 응용 프로그램에 대 한 데이터 원본으로 모델에 정의 된 클래스를 추가 합니다.

-   주 메뉴에서 선택 **프로젝트-&gt; 새 데이터 소스 추가...**
    (Visual Studio 2010에서 선택 해야 **데이터-&gt; 새 데이터 소스 추가...** )
-   데이터 원본 유형 창에서 선택 **개체** 를 클릭 하 고 **다음**
-   데이터 개체 대화 상자 선택에서 펼침 합니다 **WinFormswithEFSample** 두 시간과 선택 **범주** 있습니다 이므로 제품 데이터 원본을 선택 하지 않아도 제품의 진행 과정을 살펴보겠습니다 범주 데이터 원본에 대 한 속성입니다.

    ![DataSource](~/ef6/media/datasource.png)

-   클릭 **완료.** 
     *데이터 소스 창에서 표시 되지 않으면, 선택 * * * 보기-&gt; 다른 Windows-&gt; 데이터 원본**
-   고정 아이콘을 하므로 데이터 소스 창에서는 자동 숨기기 단추를 누릅니다. 창의 창이 이미 열려 있는 경우 새로 고침 단추를 적중 해야 합니다.

    ![DataSource2](~/ef6/media/datasource2.png)

-   솔루션 탐색기에서 두 번 클릭 합니다 **Form1.cs** 파일을 기본 폼 디자이너에서 엽니다.
-   선택 된 **범주** 데이터 원본 및 폼에 놓습니다. 기본적으로 새 DataGridView (**categoryDataGridView**) 탐색 도구 모음 컨트롤 디자이너에 추가 됩니다. 이러한 컨트롤은 BindingSource에 바인딩됩니다 (**categoryBindingSource**) 및 바인딩 Navigator (**categoryBindingNavigator**)도 생성 된 구성 요소입니다.
-   열을 편집 합니다 **categoryDataGridView**합니다. 설정 해야 합니다 **CategoryId** 읽기 전용으로 열입니다. 에 대 한 값을 **CategoryId** 속성 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.
    -   DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 열 편집을 선택 하는 중...
    -   CategoryId 열을 선택 하 고 ReadOnly를 True로 설정
    -   확인
-   범주 데이터 원본에서 제품을 선택 하 고 폼에 놓습니다. ProductBindingSource 고 productDataGridView 폼에 추가 됩니다.
-   productDataGridView에서 열을 편집 합니다. CategoryId 및 범주 열을 숨기고 ProductId를 읽기 전용으로 설정 하려고 합니다. ProductId 속성의 값은 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.
    -   DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 선택 **열 편집...** .
    -   선택 된 **ProductId** 열 집합과 **ReadOnly** 에 **True**합니다.
    -   선택 합니다 **CategoryId** 열 및 키를 눌러 합니다 **제거** 단추입니다. 동일한 작업을 수행 합니다 **범주** 열입니다.
    -   키를 눌러 **확인**합니다.

    지금 DataGridView 컨트롤이 BindingSource 구성 요소 디자이너를 사용 하 여 연결 합니다. 다음 섹션에서는 categoryBindingSource.DataSource DbContext에서 현재 추적 되는 엔터티의 컬렉션을 설정 하려면 코드를 코드 숨김에 추가 합니다. 때 끌어 놓은 제품 범주에는 WinForms에서 수행한 할까요 productsBindingSource.DataSource 속성 제품 categoryBindingSource 및 productsBindingSource.DataMember 속성을 설정 하는 합니다. 이 바인딩 때문에 현재 선택 된 범주에 속하는 제품만 productDataGridView에 표시 됩니다.
-   사용 하도록 설정 합니다 **저장** 마우스 오른쪽 단추를 클릭 하 고 선택 하 여 탐색 도구 모음 단추 **Enabled**.

    ![Form1 디자이너](~/ef6/media/form1-designer.png)

-   저장에 대 한 이벤트 처리기를 추가 단추를 두 번 클릭 하면 단추입니다. 이벤트 처리기를 추가 되 고 폼의 코드 숨김 표시 됩니다. 에 대 한 코드를 **categoryBindingNavigatorSaveItem\_클릭** 이벤트 처리기는 다음 섹션에 추가 됩니다.

## <a name="add-the-code-that-handles-data-interaction"></a>데이터 상호 작용을 처리 하는 코드를 추가 합니다.

이제 데이터 액세스를 수행 하는 ProductContext를 사용 하도록 코드를 추가 합니다. 아래와 같이 기본 폼 창에 대 한 코드를 업데이트 합니다.

코드는 ProductContext의 장기 실행 인스턴스를 선언합니다. ProductContext 개체를 쿼리하고 데이터를 데이터베이스에 저장 됩니다. ProductContext 인스턴스에서 dispose () 메서드 재정의 OnClosing 메서드에서 호출 됩니다. 코드 주석을 코드 수행 작업에 대 한 세부 정보를 제공 합니다.

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

## <a name="test-the-windows-forms-application"></a>Windows Forms 응용 프로그램 테스트

-   컴파일 및 실행 하는 응용 프로그램 기능을 테스트할 수 있습니다.

    ![Form1BeforeSave](~/ef6/media/form1beforesave.png)

-   저장 한 후 저장소 생성 키를 화면에 표시 됩니다.

    ![Form1AfterSave](~/ef6/media/form1aftersave.png)

-   Code First에서 사용 되는 경우 표시 됩니다는 **WinFormswithEFSample.ProductContext** 데이터베이스 생성 됩니다.

    ![ServerObjExplorer](~/ef6/media/serverobjexplorer.png)
