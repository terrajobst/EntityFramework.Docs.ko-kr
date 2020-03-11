---
title: Database First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: cc6ffdb3-388d-4e79-a201-01ec2577c949
ms.openlocfilehash: d40cff4ddccf43a394ef4f244653372a5a89b05a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415174"
---
# <a name="database-first"></a>Database First
이 비디오 및 단계별 연습은 Entity Framework를 사용 하 여 Database First 개발에 대 한 소개를 제공 합니다. Database First를 사용 하 여 기존 데이터베이스에서 모델을 리버스 엔지니어링할 수 있습니다. 모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다. 응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.

## <a name="watch-the-video"></a>비디오 보기
이 비디오는 Entity Framework를 사용 하 여 Database First 개발에 대 한 소개를 제공 합니다. Database First를 사용 하 여 기존 데이터베이스에서 모델을 리버스 엔지니어링할 수 있습니다. 모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다. 응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.

**작성자**: [Rowan Miller](https://romiller.com/)

**비디오**: [wmv](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.wmv) | [MP4](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-mp4video-databasefirst.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/8/F/0/8F0B5F63-4939-4DC8-A726-FF139B37F8D8/HDI-ITPro-MSDN-winvideo-databasefirst.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2010 또는 Visual Studio 2012 이상이 설치 되어 있어야 합니다.

Visual Studio 2010을 사용 하는 경우 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 도 설치 해야 합니다.

 

## <a name="1-create-an-existing-database"></a>1. 기존 데이터베이스를 만듭니다.

일반적으로 기존 데이터베이스를 대상으로 하는 경우에는 이미 생성 되지만이 연습에서는 액세스할 데이터베이스를 만들어야 합니다.

Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.
-   Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 데이터베이스를 만듭니다.

 

계속 해 서 데이터베이스를 생성 해 보겠습니다.

-   Visual Studio를 엽니다.
-   **뷰&gt; 서버 탐색기**
-   데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.
-   서버 탐색기 데이터베이스에 연결 하지 않은 경우 Microsoft SQL Server를 데이터 원본으로 선택 해야 합니다.

    ![데이터 원본 선택](~/ef6/media/selectdatasource.png)

-   설치한 항목에 따라 LocalDB 또는 SQL Express에 연결 하 고 **Databasefirst. 블로그** 를 데이터베이스 이름으로 입력 합니다.

    ![Sql Express 연결 DF](~/ef6/media/sqlexpressconnectiondf.png)

    ![LocalDB 연결 DF](~/ef6/media/localdbconnectiondf.png)

-   **확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.

    ![데이터베이스 만들기 대화 상자](~/ef6/media/createdatabasedialog.png)

-   이제 새 데이터베이스가 서버 탐색기에 표시 되 면 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.
-   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.

``` SQL
CREATE TABLE [dbo].[Blogs] (
    [BlogId] INT IDENTITY (1, 1) NOT NULL,
    [Name] NVARCHAR (200) NULL,
    [Url]  NVARCHAR (200) NULL,
    CONSTRAINT [PK_dbo.Blogs] PRIMARY KEY CLUSTERED ([BlogId] ASC)
);

CREATE TABLE [dbo].[Posts] (
    [PostId] INT IDENTITY (1, 1) NOT NULL,
    [Title] NVARCHAR (200) NULL,
    [Content] NTEXT NULL,
    [BlogId] INT NOT NULL,
    CONSTRAINT [PK_dbo.Posts] PRIMARY KEY CLUSTERED ([PostId] ASC),
    CONSTRAINT [FK_dbo.Posts_dbo.Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [dbo].[Blogs] ([BlogId]) ON DELETE CASCADE
);
```

## <a name="2-create-the-application"></a>2. 응용 프로그램 만들기

간단 하 게 유지 하기 위해 Database First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.
-   **Databasefirstsample** 을 이름으로 입력 합니다.
-   **확인**을 선택합니다.

 

## <a name="3-reverse-engineer-model"></a>3. 리버스 엔지니어링 모델

Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하 여 모델을 만들 예정입니다.

-   **프로젝트-새 항목 추가&gt; ...**
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델
-   이름으로 **BloggingModel** 를 입력 하 고 **확인을** 클릭 합니다.
-   그러면 **엔터티 데이터 모델 마법사** 가 시작 됩니다.
-   **데이터베이스에서 생성** 을 선택 하 고 **다음** 을 클릭 합니다.

    ![마법사 1 단계](~/ef6/media/wizardstep1.png)

-   첫 번째 섹션에서 만든 데이터베이스에 대 한 연결을 선택 하 고 연결 문자열 이름으로 **BloggingContext** 를 입력 하 고 **다음** 을 클릭 합니다.

    ![마법사 2 단계](~/ef6/media/wizardstep2.png)

-   ' 테이블 ' 옆의 확인란을 클릭 하 여 모든 테이블을 가져온 다음 ' 마침 '을 클릭 합니다.

    ![마법사 3 단계](~/ef6/media/wizardstep3.png)

 

리버스 엔지니어링 프로세스가 완료 되 면 새 모델이 프로젝트에 추가 되 고 Entity Framework Designer에서 볼 수 있도록 열립니다. 또한 데이터베이스에 대 한 연결 세부 정보를 사용 하 여 App.config 파일이 프로젝트에 추가 되었습니다.

![모델 초기](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010의 추가 단계

Visual Studio 2010에서 작업 하는 경우 Entity Framework 최신 버전으로 업그레이드 하기 위해 몇 가지 추가 단계를 수행 해야 합니다. 업그레이드는 향상 된 API 화면에 대 한 액세스를 제공 하 고, 사용 하기 쉽고, 최신 버그 수정 기능을 제공 하기 때문에 중요 합니다.

먼저 NuGet에서 최신 버전의 Entity Framework을 가져와야 합니다.

-   **프로젝트 – NuGet 패키지를 관리&gt;** ...
    * **Nuget 패키지 관리 ...** 옵션이 없는 경우 [최신 버전의 nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 을 설치 해야* 합니다.
-   **온라인** 탭을 선택 합니다.
-   **Entityframework** 패키지를 선택 합니다.
-   **설치**를 클릭합니다.

다음으로 Entity Framework의 이후 버전에 도입 된 DbContext API를 사용 하는 코드를 생성 하기 위해 모델을 교환 해야 합니다.

-   EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.
-   왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.
-   **C\#에 대해 EF 5.X DbContext 생성기** 를 선택 하 고 이름으로 **BloggingModel** 를 입력 한 다음 **추가** 를 클릭 합니다.

    ![DbContext 템플릿](~/ef6/media/dbcontexttemplate.png)

 

## <a name="4-reading--writing-data"></a>4. 데이터 쓰기 & 읽기

이제 모델을 사용 하 여 일부 데이터에 액세스 하는 데 사용할 수 있습니다. 데이터에 액세스 하는 데 사용할 클래스는 EDMX 파일을 기반으로 자동으로 생성 됩니다.

*이 스크린샷은 visual Studio 2012에서 가져온 것입니다. Visual Studio 2010를 사용 하는 경우 BloggingModel.tt 및 BloggingModel.Context.tt 파일은 EDMX 파일 아래에 중첩 되지 않고 직접 프로젝트 바로 아래에 있습니다.*

![생성 된 클래스 DF](~/ef6/media/generatedclassesdf.png)

 

아래와 같이 Program.cs에서 Main 메서드를 구현 합니다. 이 코드는 컨텍스트의 새 인스턴스를 만든 다음이를 사용 하 여 새 블로그를 삽입 합니다. 그런 다음 LINQ 쿼리를 사용 하 여 데이터베이스에서 제목별로 사전순으로 정렬 된 모든 블로그를 검색 합니다.

``` csharp
class Program
{
    static void Main(string[] args)
    {
        using (var db = new BloggingContext())
        {
            // Create and save a new Blog
            Console.Write("Enter a name for a new Blog: ");
            var name = Console.ReadLine();

            var blog = new Blog { Name = name };
            db.Blogs.Add(blog);
            db.SaveChanges();

            // Display all Blogs from the database
            var query = from b in db.Blogs
                        orderby b.Name
                        select b;

            Console.WriteLine("All blogs in the database:");
            foreach (var item in query)
            {
                Console.WriteLine(item.Name);
            }

            Console.WriteLine("Press any key to exit...");
            Console.ReadKey();
        }
    }
}
```

이제 응용 프로그램을 실행 하 고 테스트할 수 있습니다.

```console
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```
 

## <a name="5-dealing-with-database-changes"></a>5. 데이터베이스 변경 처리

이제 데이터베이스 스키마를 변경할 때 이러한 변경을 수행할 때 이러한 변경 내용을 반영 하기 위해 모델을 업데이트 해야 합니다.

첫 번째 단계는 데이터베이스 스키마를 변경 하는 것입니다. 사용자 테이블을 스키마에 추가 하겠습니다.

-   Databasefirst를 마우스 오른쪽 단추로 클릭 하 고 서버 탐색기의 블로그 데이터베이스를 클릭 한 다음 **새 쿼리** 를 선택 **합니다.**
-   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.

``` SQL
CREATE TABLE [dbo].[Users]
(
    [Username] NVARCHAR(50) NOT NULL PRIMARY KEY,  
    [DisplayName] NVARCHAR(MAX) NULL
)
```

이제 스키마가 업데이트 되었으므로 모델을 해당 변경 내용으로 업데이트할 수 있습니다.

-   EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 ' 데이터베이스에서 모델 업데이트 ... '를 선택 하면 업데이트 마법사가 시작 됩니다.
-   업데이트 마법사의 추가 탭에서 테이블 옆의 확인란을 선택 합니다 .이는 스키마에서 새 테이블을 추가 하려고 함을 나타냅니다.
    *새로 고침 탭에는 업데이트 중에 변경 내용이 확인 될 모델의 기존 테이블이 표시 됩니다. 삭제 탭에는 스키마에서 제거 되 고 업데이트의 일부로 모델에서 제거 되는 모든 테이블이 표시 됩니다. 이러한 두 탭에 대 한 정보는 자동으로 검색 되며 정보 제공을 위해서만 제공 되며 설정을 변경할 수 없습니다.*

    ![새로 고침 마법사](~/ef6/media/refreshwizard.png)

-   업데이트 마법사에서 마침을 클릭 합니다.

 

이제 모델은 데이터베이스에 추가한 사용자 테이블에 매핑되는 새 사용자 엔터티를 포함 하도록 업데이트 됩니다.

![모델이 업데이트 됨](~/ef6/media/modelupdated.png)

## <a name="summary"></a>요약

이 연습에서는 기존 데이터베이스를 기반으로 EF Designer에서 모델을 만들 수 있도록 하는 Database First 개발을 살펴보았습니다. 그런 다음이 모델을 사용 하 여 데이터베이스에서 일부 데이터를 읽고 씁니다. 마지막으로, 데이터베이스 스키마에 대 한 변경 내용을 반영 하도록 모델을 업데이트 했습니다.
