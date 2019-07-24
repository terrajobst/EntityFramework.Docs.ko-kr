---
title: WinForms를 사용 하 여 데이터 바인딩-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80fc5062-2f1c-4dbd-ab6e-b99496784b36
ms.openlocfilehash: ad55ef4d496bbfe30eafcab9811c92989066519f
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306556"
---
# <a name="databinding-with-winforms"></a>WinForms를 사용 하 여 데이터 바인딩
이 단계별 연습에서는 POCO 형식을 "마스터-세부 정보" 폼의 WinForms (Window Forms) 컨트롤에 바인딩하는 방법을 보여 줍니다. 응용 프로그램은 Entity Framework를 사용 하 여 데이터베이스의 데이터로 개체를 채우고, 변경 내용을 추적 하 고, 데이터를 데이터베이스에 보관 합니다.

이 모델은 일 대 다 관계에 참여 하는 두 가지 형식을 정의 합니다. 범주 (주\\마스터) 및 제품 (종속\\정보)입니다. 그런 다음 Visual Studio 도구를 사용 하 여 모델에 정의 된 형식을 WinForms 컨트롤에 바인딩합니다. WinForms 데이터 바인딩 프레임 워크를 사용 하면 관련 개체 간을 탐색할 수 있습니다. 즉, 마스터 뷰에서 행을 선택 하면 세부 정보 보기가 해당 자식 데이터로 업데이트 됩니다.

이 연습의 스크린 샷 및 코드 목록은 Visual Studio 2013에서 가져온 것 이지만 Visual Studio 2012 또는 Visual Studio 2010를 사용 하 여이 연습을 완료할 수 있습니다.

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2013, Visual Studio 2012 또는 Visual Studio 2010이 설치 되어 있어야 합니다.

Visual Studio 2010을 사용 하는 경우 NuGet도 설치 해야 합니다. 자세한 내용은 [NuGet 설치](http://docs.nuget.org/docs/start-here/installing-nuget)를 참조 하세요.

## <a name="create-the-application"></a>애플리케이션 만들기

-   Visual Studio를 엽니다.
-   **파일-&gt; 새로 만들기&gt; -프로젝트 ....**
-   왼쪽 창에서 **windows** 를 선택 하 고 오른쪽 창에서 **windows 양식 응용 프로그램** 을 선택 합니다.
-   **Win양식 Withefsample** 을 이름으로 입력 합니다.
-   **확인**을 선택합니다.

## <a name="install-the-entity-framework-nuget-package"></a>Entity Framework NuGet 패키지를 설치 합니다.

-   솔루션 탐색기에서 **Win양식 Withefsample** 프로젝트를 마우스 오른쪽 단추로 클릭 합니다.
-   **NuGet 패키지 관리 ...** 를 선택 합니다.
-   NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.
-   **설치** 클릭  
    > [!NOTE]
    > EntityFramework 어셈블리 외에 System.componentmodel에 대 한 참조도 추가 됩니다. 프로젝트에 System.object에 대 한 참조가 있는 경우 EntityFramework 패키지를 설치 하면 제거 됩니다. System.object 어셈블리는 Entity Framework 6 응용 프로그램에 더 이상 사용 되지 않습니다.

## <a name="implementing-ilistsource-for-collections"></a>컬렉션에 대해 IListSource 구현

Windows Forms를 사용할 때 정렬에 양방향 데이터 바인딩을 사용 하려면 컬렉션 속성에서 IListSource 인터페이스를 구현 해야 합니다. 이렇게 하려면 System.collections.objectmodel.observablecollection를 확장 하 여 IListSource 기능을 추가 하겠습니다.

-   프로젝트에 **ObservableListSource** 클래스를 추가 합니다.
    -   프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.
    -   **새 항목 추가&gt; 를** 선택 합니다.
    -   클래스 **를 선택 하** 고 클래스 이름으로 **ObservableListSource** 를 입력 합니다.
-   기본적으로 생성 된 코드를 다음 코드로 바꿉니다.

*이 클래스를 사용 하면 양방향 데이터 바인딩과 정렬을 사용할 수 있습니다. 클래스는 system.collections.objectmodel.observablecollection&lt;T&gt; 에서 파생 되며 IListSource의 명시적 구현을 추가 합니다. IListSource의 GetList () 메서드는 System.collections.objectmodel.observablecollection와 동기화 상태로 유지 되는 IBindingList 구현을 반환 하도록 구현 됩니다. ToBindingList에 의해 생성 된 IBindingList 구현에서는 정렬을 지원 합니다. ToBindingList 확장 메서드는 EntityFramework 어셈블리에서 정의 됩니다.*

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

## <a name="define-a-model"></a>모델 정의

이 연습에서는 Code First 또는 EF Designer를 사용 하 여 모델을 구현 하도록 선택할 수 있습니다. 다음 두 섹션 중 하나를 완료 합니다.

### <a name="option-1-define-a-model-using-code-first"></a>옵션 1: Code First를 사용 하 여 모델 정의

이 섹션에서는 Code First를 사용 하 여 모델 및 연결 된 데이터베이스를 만드는 방법을 보여 줍니다. 다음 섹션으로 건너뜁니다 (**옵션 2: Database First를 사용 하 여 EF** designer를 사용 하 여 데이터베이스에서 모델을 리버스 엔지니어링 하는 경우 Database First)를 사용 하 여 모델을 정의 합니다.

Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다.

-   프로젝트에 새 **Product** 클래스 추가
-   기본적으로 생성 된 코드를 다음 코드로 바꿉니다.

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

-   **범주** 클래스를 프로젝트에 추가 합니다.
-   기본적으로 생성 된 코드를 다음 코드로 바꿉니다.

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

엔터티를 정의 하는 것 외에도 **DbContext** 에서 파생 되 고 **&lt;&gt; dbset** 엔터티 속성을 노출 하는 클래스를 정의 해야 합니다. **Dbset** 속성을 사용 하면 컨텍스트에서 모델에 포함 하려는 형식을 알 수 있습니다. **DbContext** 및 **Dbset** 형식은 entityframework 어셈블리에서 정의 됩니다.

DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.

-   새 **제품 컨텍스트** 클래스를 프로젝트에 추가 합니다.
-   기본적으로 생성 된 코드를 다음 코드로 바꿉니다.

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

### <a name="option-2-define-a-model-using-database-first"></a>옵션 2: Database First를 사용 하 여 모델 정의

이 섹션에서는 EF designer를 사용 하 여 데이터베이스에서 모델을 리버스 엔지니어링 하는 Database First를 사용 하는 방법을 보여 줍니다. 이전 섹션을 완료 한 경우 (**옵션 1: Code First)** 를 사용 하 여 모델을 정의한 다음이 섹션을 건너뛰고 **지연 로드** 섹션으로 바로 이동 합니다.

#### <a name="create-an-existing-database"></a>기존 데이터베이스 만들기

일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.

Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.
-   Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) 데이터베이스를 만듭니다.

계속 해 서 데이터베이스를 생성 해 보겠습니다.

-   **보기-&gt; 서버 탐색기**
-   **데이터 연결-&gt; 연결 추가** ...를 마우스 오른쪽 단추로 클릭 합니다.
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

-   **프로젝트-&gt; 새 항목 추가 ...**
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델
-   이름으로 **제품 모델** 을 입력 하 고 **확인을** 클릭 합니다.
-   그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.
-   **데이터베이스에서 생성** 을 선택 하 고 **다음** 을 클릭 합니다.

    ![ChooseModelContents](~/ef6/media/choosemodelcontents.png)

-   첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 연결 문자열 이름으로 **제품 컨텍스트** 를 입력 한 후 **다음** 을 클릭 합니다.

    ![연결 선택](~/ef6/media/chooseyourconnection.png)

-   ' 테이블 ' 옆의 확인란을 클릭 하 여 모든 테이블을 가져온 다음 ' 마침 '을 클릭 합니다.

    ![개체 선택](~/ef6/media/chooseyourobjects.png)

리버스 엔지니어링 프로세스가 완료 되 면 새 모델이 프로젝트에 추가 되 고 Entity Framework Designer에서 볼 수 있도록 열립니다. 또한 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 App.config 파일이 프로젝트에 추가 되었습니다.

#### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010의 추가 단계

Visual Studio 2010에서 작업 하는 경우 EF6 코드 생성을 사용 하도록 EF designer를 업데이트 해야 합니다.

-   EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.
-   왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.
-   **C\#에 대해 EF DbContext 생성기를 선택 하** 고 이름으로 **ProductsModel** 를 입력 한 다음 추가를 클릭 합니다.

#### <a name="updating-code-generation-for-data-binding"></a>데이터 바인딩에 대 한 코드 생성 업데이트

EF는 T4 템플릿을 사용 하 여 모델에서 코드를 생성 합니다. Visual Studio와 함께 제공 되거나 Visual Studio 갤러리에서 다운로드 한 템플릿은 일반적인 용도로 사용 하기 위한 것입니다. 즉, 이러한 템플릿에서 생성 된 엔터티에는 간단한 ICollection&lt;T&gt; 속성이 있습니다. 그러나 데이터 바인딩을 수행할 때 IListSource를 구현 하는 컬렉션 속성을 갖는 것이 좋습니다. 위의 ObservableListSource 클래스를 만들었으므로 이제이 클래스를 사용 하 여 템플릿을 수정 하겠습니다.

-   **솔루션 탐색기** 를 열고 **제품 모델 .edmx** 파일을 찾습니다.
-   **ProductModel.tt** 파일을 찾습니다 .이 파일은 제품 모델 .edmx 파일 아래에 중첩 될 수 있습니다.

    ![제품 모델 템플릿](~/ef6/media/productmodeltemplate.png)

-   ProductModel.tt 파일을 두 번 클릭 하 여 Visual Studio 편집기에서 엽니다.
-   "**ICollection**"의 두 항목을 찾아 "**ObservableListSource**"로 바꿉니다. 296 및 484 줄에 있습니다.
-   "**Hashset**"의 첫 번째 항목을 찾아 "**ObservableListSource**"로 바꿉니다. 이 항목은 약 50 줄에 있습니다. 코드에서 나중에 발견 된 두 번째 HashSet을 바꾸지 **마십시오** .
-   ProductModel.tt 파일을 저장 합니다. 이로 인해 엔터티가 다시 생성 됩니다. 코드가 자동으로 다시 생성 되지 않는 경우 ProductModel.tt를 마우스 오른쪽 단추로 클릭 하 고 "사용자 지정 도구 실행"을 선택 합니다.

이제 ProductModel.tt 아래에 중첩 된 Category.cs 파일을 열면 Products 컬렉션에 **ObservableListSource&lt;Product&gt;** 형식이 표시 됩니다.

프로젝트를 컴파일합니다.

## <a name="lazy-loading"></a>지연 로드

**Product** 클래스의 **Category** 클래스 및 **Category** 속성에 대 한 **Products** 속성은 탐색 속성입니다. Entity Framework 탐색 속성은 두 엔터티 형식 간의 관계를 탐색 하는 방법을 제공 합니다.

EF는 탐색 속성에 처음 액세스할 때 데이터베이스에서 관련 엔터티를 자동으로 로드 하는 옵션을 제공 합니다. 이 유형의 로드 (지연 로드)를 사용 하면 각 탐색 속성에 처음 액세스할 때 내용이 컨텍스트에 아직 없는 경우 데이터베이스에 대해 별도의 쿼리가 실행 됩니다.

POCO 엔터티 형식을 사용 하는 경우 EF는 런타임 중에 파생 된 프록시 형식의 인스턴스를 만든 다음 클래스의 가상 속성을 재정의 하 여 로드 후크를 추가 함으로써 지연 로드를 달성 합니다. 관련 개체의 지연 로드를 얻으려면 탐색 속성 getter를 **public** 및 **virtual** (Visual Basic에서**재정의** 가능)로 선언 하 고 클래스는 **sealed** 가 아니어야 합니다 (Visual Basic에서**NotOverridable** ). Database First 사용 하는 경우 지연 로드를 사용할 수 있도록 탐색 속성이 자동으로 가상으로 만들어집니다. Code First 섹션에서는 동일한 이유로 탐색 속성을 가상으로 만들도록 선택 했습니다.

## <a name="bind-object-to-controls"></a>컨트롤에 개체 바인딩

모델에 정의 된 클래스를이 WinForms 응용 프로그램의 데이터 원본으로 추가 합니다.

-   주 메뉴에서 **프로젝트-&gt; 새 데이터 소스 추가** ...를 선택 합니다.
    Visual Studio 2010에서는 **데이터-&gt; 새 데이터 소스 추가**...를 선택 해야 합니다.
-   데이터 소스 형식 선택 창에서 **개체** 를 선택 하 고 **다음** 을 클릭 합니다.
-   데이터 개체 선택 대화 상자에서 Win펼침을 두  번 선택 하 고 **범주** 를 선택 합니다. 범주 데이터 원본에서 제품의 속성을 통해 제품 데이터 원본을 선택할 필요가 없습니다.

    ![데이터 원본](~/ef6/media/datasource.png)

-   **마침을 클릭 합니다.** 데이터 소스 *창이 표시 되지 않으면 * * * 보기-&gt; 기타 창-&gt; 데이터 원본을 선택* 합니다. 
    *
-   데이터 소스 창이 자동으로 숨겨지지 않도록 고정 아이콘을 누릅니다. 창이 이미 표시 되는 경우 새로 고침 단추를 눌러야 할 수 있습니다.

    ![데이터 원본 2](~/ef6/media/datasource2.png)

-   솔루션 탐색기에서 **Form1.cs** 파일을 두 번 클릭 하 여 디자이너에서 기본 폼을 엽니다.
-   **범주** 데이터 원본을 선택 하 고 폼에서 끌어 옵니다. 기본적으로 새 DataGridView (**Categorydatagridview**) 및 탐색 도구 모음 컨트롤이 디자이너에 추가 됩니다. 이러한 컨트롤은 생성 되는 BindingSource (**Categorybindingsource**) 및 바인딩 탐색기 (**categorybindingsource**) 구성 요소에 바인딩됩니다.
-   **Categorydatagridview**에서 열을 편집 합니다. **CategoryId** 열을 읽기 전용으로 설정 하려고 합니다. **CategoryId** 속성의 값은 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.
    -   DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 열 편집 ...을 선택 합니다.
    -   CategoryId 열을 선택 하 고 ReadOnly를 True로 설정 합니다.
    -   확인을 누릅니다.
-   범주 데이터 원본에서 제품을 선택 하 고 폼으로 끕니다. 제품 Datagridview 및 제품 Bindingsource가 양식에 추가 됩니다.
-   제품 Datagridview에서 열을 편집 합니다. CategoryId 및 Category 열을 숨기고 ProductId를 읽기 전용으로 설정 하려고 합니다. ProductId 속성의 값은 데이터를 저장 한 후 데이터베이스에서 생성 됩니다.
    -   DataGridView 컨트롤을 마우스 오른쪽 단추로 클릭 하 고 **열 편집**...을 선택 합니다.
    -   **ProductId** 열을 선택 하 고 **ReadOnly** 를 **True**로 설정 합니다.
    -   **CategoryId** 열을 선택 하 고 **제거** 단추를 누릅니다. **Category** 열을 사용 하 여 동일한 작업을 수행 합니다.
    -   **확인**을 누릅니다.

    지금까지 DataGridView 컨트롤과 디자이너의 BindingSource 구성 요소를 연결 했습니다. 다음 섹션에서는 코드를 코드 뒤에 추가 하 여 DbContext에서 현재 추적 하는 엔터티 컬렉션에 categoryBindingSource를 설정 합니다. 범주에서 제품을 끌어다 놓으면 WinForms는 productsBindingSource 속성을 categoryBindingSource로 설정 하 고 productsBindingSource 속성을 Products로 설정 하는 작업을 처리 했습니다. 이 바인딩으로 인해 현재 선택 된 범주에 속하는 제품만 products Datagridview에 표시 됩니다.
-   마우스 오른쪽 단추를 클릭 하 고 **사용**을 선택 하 여 탐색 도구 모음에서 **저장** 단추를 사용 하도록 설정 합니다.

    ![양식 1 디자이너](~/ef6/media/form1-designer.png)

-   단추를 두 번 클릭 하 여 저장 단추에 대 한 이벤트 처리기를 추가 합니다. 이렇게 하면 이벤트 처리기가 추가 되 고 폼의 코드 숨김으로 이동 합니다. **CategoryBindingNavigatorSaveItem\_Click** 이벤트 처리기에 대 한 코드는 다음 섹션에 추가 됩니다.

## <a name="add-the-code-that-handles-data-interaction"></a>데이터 상호 작용을 처리 하는 코드 추가

이제 제품 컨텍스트를 사용 하 여 데이터 액세스를 수행 하는 코드를 추가 합니다. 아래와 같이 주 폼 창의 코드를 업데이트 합니다.

이 코드는 장기적으로 실행 되는 제품 컨텍스트의 인스턴스를 선언 합니다. 제품 컨텍스트 개체는 데이터베이스에 데이터를 쿼리하고 저장 하는 데 사용 됩니다. 그런 다음, 제품 컨텍스트 인스턴스의 Dispose () 메서드를 재정의 된 OnClosing 메서드에서 호출 합니다. 코드 주석은 코드가 수행 하는 작업에 대 한 세부 정보를 제공 합니다.

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

-   응용 프로그램을 컴파일 및 실행 하 고 기능을 테스트할 수 있습니다.

    ![저장 하기 전 1 양식](~/ef6/media/form1beforesave.png)

-   저장 한 후에는 저장소에서 생성 된 키가 화면에 표시 됩니다.

    ![저장 후 1 양식](~/ef6/media/form1aftersave.png)

-   Code First 사용 하는 경우 **Win양식 Withefsample. 제품** 데이터베이스를 만드는 것도 표시 됩니다.

    ![서버 개체 탐색기](~/ef6/media/serverobjexplorer.png)
