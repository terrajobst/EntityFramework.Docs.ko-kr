---
title: 자동 추적 엔터티 연습-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
caps.latest.revision: 3
ms.openlocfilehash: d07ae727ffba60a9296b34b9261acd99f7e8e3b6
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122349"
---
# <a name="self-tracking-entities-walkthrough"></a>자동 추적 엔터티 연습
> [!IMPORTANT]
> 자동 추적 엔터티 템플릿을 더 이상 권장하지 않습니다. 이 템플릿은 기존 응용 프로그램을 지원하는 용도로만 제공될 것입니다. 응용 프로그램에서 연결이 끊긴 엔터티 그래프를 사용해야 하는 경우 커뮤니티에서 적극적으로 개발한 자동 추적 엔터티와 비슷한 기술인 [추적 가능 엔터티](http://trackableentities.github.io/) 같은 다른 대안을 고려하거나 하위 수준 변경 내용 추적 API를 사용하여 사용자 지정 코드를 작성하는 방법을 고려해 보세요.

이 연습에서는 Windows Communication Foundation (WCF) 서비스 엔터티 그래프를 반환 하는 작업을 노출 하는 시나리오를 보여 줍니다. 다음, 클라이언트 응용 프로그램에서는 해당 그래프를 조작 하 고 유효성을 검사 하 고 Entity Framework를 사용 하 여 데이터베이스에 업데이트를 저장 하는 서비스 작업에 수정 내용을 전송 합니다.

이 연습을 완료 하기 전에 읽어야 하는 [자동 추적 엔터티](index.md) 페이지입니다.

이 연습에서는 다음 작업을 수행합니다.

-   에 액세스 하려면 데이터베이스를 만듭니다.
-   모델이 포함 된 클래스 라이브러리를 만듭니다.
-   자동 추적 엔터티 생성기 템플릿을를 교환 합니다.
-   엔터티 클래스를 별도 프로젝트로 이동합니다.
-   쿼리 엔터티를 저장 하는 작업을 노출 하는 WCF 서비스를 만듭니다.
-   응용 프로그램 (콘솔 및 WPF) 서비스를 사용 하는 클라이언트를 만듭니다.

이 연습에서는 Database First 사용 하지만 동일한 기술을 Model First를 동일 하 게 적용 합니다.

## <a name="pre-requisites"></a>필수 조건

이 연습을 완료 하려면 최신 버전의 Visual Studio를 해야 합니다.

## <a name="create-a-database"></a>데이터베이스 만들기

Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.

-   Visual Studio 2012를 사용 하는 경우 다음 만들려는 LocalDB 데이터베이스입니다.
-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.

데이터베이스를 생성 해 보겠습니다.

-   Visual Studio를 엽니다.
-   **보기-&gt; 서버 탐색기**
-   마우스 오른쪽 단추로 클릭 **데이터 연결-&gt; 연결 추가 중...**
-   선택 해야 하기 전에 서버 탐색기에서 데이터베이스에 연결 하지 않았으면 **Microsoft SQL Server** 데이터 원본으로
-   LocalDB 또는 어느에 따라 설치한 SQL Express에 연결
-   입력 **STESample** 데이터베이스 이름으로
-   선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**
-   새 데이터베이스 서버 탐색기에 나타납니다.
-   Visual Studio 2012를 사용 하는 경우
    -   서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 선택 **새 쿼리**
    -   새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **실행**
-   Visual Studio 2010을 사용 하는 경우
    -   선택 **데이터&gt; Transact SQL 편집기-&gt; 새 쿼리 연결 하는 중...**
    -   입력 **.\\ SQLEXPRESS** 서버 이름 및 클릭으로 **확인**
    -   선택 된 **STESample** 쿼리 편집기의 맨 위에 있는 드롭다운에서 아래로 데이터베이스
    -   새 쿼리를 다음과 같은 SQL 복사 후 선택한 쿼리를 마우스 오른쪽 단추로 클릭 **SQL 실행**

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

    SET IDENTITY_INSERT [dbo].[Blogs] ON
    INSERT INTO [dbo].[Blogs] ([BlogId], [Name], [Url]) VALUES (1, N'ADO.NET Blog', N'blogs.msdn.com/adonet')
    SET IDENTITY_INSERT [dbo].[Blogs] OFF
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'Intro to EF', N'Interesting stuff...', 1)
    INSERT INTO [dbo].[Posts] ([Title], [Content], [BlogId]) VALUES (N'What is New', N'More interesting stuff...', 1)
```

## <a name="create-the-model"></a>모델 만들기

Up,에서는 먼저 모델에 삽입할 프로젝트입니다.

-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Visual C\#**  왼쪽된 창에서 다음 **클래스 라이브러리**
-   입력 **STESample** 이름과 클릭 **확인**

이제 간단한 모델을 데이터베이스에 액세스 하려면 EF 디자이너에서 만들어 보겠습니다.

-   **프로젝트-&gt; 새 항목 추가...**
-   선택 **데이터** 왼쪽된 창에서 다음 **ADO.NET 엔터티 데이터 모델**
-   입력 **BloggingModel** 이름과 클릭 **확인**
-   선택 **데이터베이스에서 생성** 를 클릭 하 고 **다음**
-   이전 섹션에서 만든 데이터베이스에 대 한 연결 정보를 입력 합니다.
-   입력 **BloggingContext** 연결 문자열 및 클릭에 대 한 이름으로 **다음**
-   옆에 확인란 **테이블** 를 클릭 하 고 **마침**

## <a name="swap-to-ste-code-generation"></a>STE 코드 생성을 교환 합니다.

이제 기본 코드 생성 및 교환 작업을 자동 추적 엔터티를 사용 하지 않도록 설정 해야 합니다.

### <a name="if-you-are-using-visual-studio-2012"></a>Visual Studio 2012를 사용 하는 경우

-   확장 **BloggingModel.edmx** 에 **솔루션 탐색기** 삭제 하 고는 **BloggingModel.tt** 고 **BloggingModel.Context.tt** 
     *기본 코드 생성을 사용 하지 않도록 설정 됩니다*
-   EF 디자이너 화면에서 빈 영역을 마우스 오른쪽 단추로 클릭 **코드 생성 항목 추가...**
-   선택 **Online** 검색에 대 한 확인 하 고 왼쪽된 창에서 **STE 생성기**
-   선택 된 **C에 대 한 STE 생성기\#**  템플릿에 입력 **STETemplate** 이름과 클릭 **추가**
-   합니다 **STETemplate.tt** 하 고 **STETemplate.Context.tt** 파일은 BloggingModel.edmx 파일 아래에 중첩 추가

### <a name="if-you-are-using-visual-studio-2010"></a>Visual Studio 2010을 사용 하는 경우

-   EF 디자이너 화면에서 빈 영역을 마우스 오른쪽 단추로 클릭 **코드 생성 항목 추가...**
-   선택 **코드** 왼쪽된 창에서 다음 **ADO.NET 자동 추적 엔터티 생성기**
-   입력 **STETemplate** 이름과 클릭 **추가**
-   합니다 **STETemplate.tt** 하 고 **STETemplate.Context.tt** 파일은 프로젝트에 직접 추가

## <a name="move-entity-types-into-separate-project"></a>엔터티 형식은 별도 프로젝트로 이동

자동 추적 엔터티를 사용 하는 클라이언트 응용 프로그램 모델에서 생성 된 엔터티 클래스에 대 한 액세스를 해야 합니다. 클라이언트 응용 프로그램 전체 모델을 노출 하지 않으려는 것 때문에 엔터티 클래스에는 별도 프로젝트로 이동 하겠습니다.

첫 번째 단계는 기존 프로젝트에서 엔터티 클래스 생성을 중지 하려면:

-   마우스 오른쪽 단추로 클릭 **STETemplate.tt** 에 **솔루션 탐색기** 선택한 **속성**
-   에 **속성** 창 지우기 **TextTemplatingFileGenerator** 에서 합니다 **CustomTool** 속성
-   확장 **STETemplate.tt** 에 **솔루션 탐색기** 그 아래에 중첩 된 모든 파일을 삭제 하 고

새 프로젝트를 추가 하 여 엔터티 클래스를 생성 하겠습니다 다음으로,

-   **파일만&gt; 추가-&gt; 프로젝트...**
-   선택 **Visual C\#**  왼쪽된 창에서 다음 **클래스 라이브러리**
-   입력 **STESample.Entities** 이름과 클릭 **확인**
-   **프로젝트-&gt; 기존 항목 추가...**
-   로 이동 합니다 **STESample** 프로젝트 폴더
-   보려는 선택 **모든 파일 (\*.\*)**
-   선택 된 **STETemplate.tt** 파일
-   옆에 드롭다운 화살표를 클릭 합니다 **추가** 단추를 선택 **링크로 추가**

    ![AddLinkedTemplate](~/ef6/media/addlinkedtemplate.png)

또한 엔터티 클래스에는 동일한 컨텍스트에서 네임 스페이스에 생성 되도록 것입니다. 이 바로 응용 프로그램 전체에서 추가 해야 하는 문을 사용 하 여의 수를 줄입니다.

-   연결 된 마우스 오른쪽 단추로 클릭 **STETemplate.tt** 에 **솔루션 탐색기** 선택한 **속성**
-   에 **속성** 창 집합 **사용자 지정 도구 Namespace** 에 **STESample**

STE 템플릿에 의해 생성 된 코드에 대 한 참조 해야 합니다. **System.Runtime.Serialization** 컴파일하기 위해. 이 라이브러리는 WCF에 필요한 **DataContract** 하 고 **DataMember** 직렬화 가능 엔터티 형식에 사용 되는 특성입니다.

-   마우스 오른쪽 단추로 클릭 합니다 **STESample.Entities** 프로젝트 **솔루션 탐색기** 선택한 **참조 추가...**
    -   Visual Studio 2012-확인란 옆 **System.Runtime.Serialization** 를 클릭 하 고 **확인**
    -   Visual Studio 2010-에서 선택 **System.Runtime.Serialization** 를 클릭 하 고 **확인**

마지막으로,이 컨텍스트에서 사용 하 여 프로젝트에 엔터티 형식에 대 한 참조를 해야 합니다.

-   마우스 오른쪽 단추로 클릭 합니다 **STESample** 프로젝트 **솔루션 탐색기** 선택한 **참조 추가...**
    -   Visual Studio 2012-에서 선택 **솔루션** 왼쪽된 창에서 확인란 옆에 **STESample.Entities** 를 클릭 하 고 **확인**
    -   Visual Studio 2010-에서 선택 합니다 **프로젝트** 탭을 선택 **STESample.Entities** 를 클릭 하 고 **확인**

>[!NOTE]
> 엔터티 형식은 별도 프로젝트를 이동 하는 또 다른 옵션의 기본 위치에서 연결 하는 것이 아니라 템플릿 파일을 옮기는 것입니다. 이 작업을 수행 하는 경우 업데이트 해야 합니다는 **inputFile** .edmx 파일에 상대 경로 제공 하는 템플릿에 변수 (이 예제는 **... \\BloggingModel.edmx**).

## <a name="create-a-wcf-service"></a>WCF 서비스 만들기

이제 데이터를 노출 하는 WCF 서비스를 추가 하려면, 프로젝트를 만들어 시작 하겠습니다.

-   **파일만&gt; 추가-&gt; 프로젝트...**
-   선택 **Visual C\#**  왼쪽된 창에서 다음 **WCF 서비스 응용 프로그램**
-   입력 **STESample.Service** 이름과 클릭 **확인**
-   에 대 한 참조를 추가 합니다 **System.Data.Entity** 어셈블리
-   에 대 한 참조를 추가 합니다 **STESample** 하 고 **STESample.Entities** 프로젝트

런타임 시 검색 되도록이 프로젝트에 EF 연결 문자열을 복사 해야 합니다.

-   열기를 **App.Config** 에 대 한 파일을 * * STESample * * 프로젝트 및 복사 합니다 **connectionStrings** 요소
-   붙여넣기를 **connectionStrings** 의 자식 요소로 요소를 **구성** 의 요소를 **Web.Config** 파일을 **STESample.Service** 프로젝트

이제 실제 서비스를 구현 하는 시간입니다.

-   오픈 **IService1.cs** 내용을 다음 코드로 바꾸고

``` csharp
    using System.Collections.Generic;
    using System.ServiceModel;

    namespace STESample.Service
    {
        [ServiceContract]
        public interface IService1
        {
            [OperationContract]
            List<Blog> GetBlogs();

            [OperationContract]
            void UpdateBlog(Blog blog);
        }
    }
```

-   오픈 **Service1.svc** 내용을 다음 코드로 바꾸고

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.Linq;

    namespace STESample.Service
    {
        public class Service1 : IService1
        {
            /// <summary>
            /// Gets all the Blogs and related Posts.
            /// </summary>
            public List<Blog> GetBlogs()
            {
                using (BloggingContext context = new BloggingContext())
                {
                    return context.Blogs.Include("Posts").ToList();
                }
            }

            /// <summary>
            /// Updates Blog and its related Posts.
            /// </summary>
            public void UpdateBlog(Blog blog)
            {
                using (BloggingContext context = new BloggingContext())
                {
                    try
                    {
                        // TODO: Perform validation on the updated order before applying the changes.

                        // The ApplyChanges method examines the change tracking information
                        // contained in the graph of self-tracking entities to infer the set of operations
                        // that need to be performed to reflect the changes in the database.
                        context.Blogs.ApplyChanges(blog);
                        context.SaveChanges();

                    }
                    catch (UpdateException)
                    {
                        // To avoid propagating exception messages that contain sensitive data to the client tier
                        // calls to ApplyChanges and SaveChanges should be wrapped in exception handling code.
                        throw new InvalidOperationException("Failed to update. Try your request again.");
                    }
                }
            }        
        }
    }
```

## <a name="consume-the-service-from-a-console-application"></a>콘솔 응용 프로그램에서 서비스 사용

서비스를 사용 하는 콘솔 응용 프로그램을 만들어 보겠습니다.

-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Visual C\#**  왼쪽된 창에서 다음 **콘솔 응용 프로그램**
-   입력 **STESample.ConsoleTest** 이름과 클릭 **확인**
-   에 대 한 참조를 추가 합니다 **STESample.Entities** 프로젝트

WCF 서비스에 대 한 서비스 참조 필요

-   마우스 오른쪽 단추로 클릭 합니다 **STESample.ConsoleTest** 프로젝트 **솔루션 탐색기** 선택한 **서비스 참조 추가...**
-   클릭 **검색**
-   입력 **BloggingService** 네임 스페이스와 클릭 **확인**

이제 서비스를 사용 하는 일부 코드를 작성할 수 했습니다.

-   오픈 **Program.cs** 그 내용을 다음 코드로 바꿉니다.

``` csharp
    using STESample.ConsoleTest.BloggingService;
    using System;
    using System.Linq;

    namespace STESample.ConsoleTest
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Print out the data before we change anything
                Console.WriteLine("Initial Data:");
                DisplayBlogsAndPosts();

                // Add a new Blog and some Posts
                AddBlogAndPost();
                Console.WriteLine("After Adding:");
                DisplayBlogsAndPosts();

                // Modify the Blog and one of its Posts
                UpdateBlogAndPost();
                Console.WriteLine("After Update:");
                DisplayBlogsAndPosts();

                // Delete the Blog and its Posts
                DeleteBlogAndPost();
                Console.WriteLine("After Delete:");
                DisplayBlogsAndPosts();

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            static void DisplayBlogsAndPosts()
            {
                using (var service = new Service1Client())
                {
                    // Get all Blogs (and Posts) from the service
                    // and print them to the console
                    var blogs = service.GetBlogs();
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(blog.Name);
                        foreach (var post in blog.Posts)
                        {
                            Console.WriteLine(" - {0}", post.Title);
                        }
                    }
                }

                Console.WriteLine();
                Console.WriteLine();
            }

            static void AddBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Create a new Blog with a couple of Posts
                    var newBlog = new Blog
                    {
                        Name = "The New Blog",
                        Posts =
                        {
                            new Post { Title = "Welcome to the new blog"},
                            new Post { Title = "What's new on the new blog"}
                        }
                    };

                    // Save the changes using the service
                    service.UpdateBlog(newBlog);
                }
            }

            static void UpdateBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The New Blog
                    var blog = blogs.First(b => b.Name == "The New Blog");

                    // Update the Blogs name
                    blog.Name = "The Not-So-New Blog";

                    // Update one of the related posts
                    blog.Posts.First().Content = "Some interesting content...";

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }

            static void DeleteBlogAndPost()
            {
                using (var service = new Service1Client())
                {
                    // Get all the Blogs
                    var blogs = service.GetBlogs();

                    // Use LINQ to Objects to find The Not-So-New Blog
                    var blog = blogs.First(b => b.Name == "The Not-So-New Blog");

                    // Mark all related Posts for deletion
                    // We need to call ToList because each Post will be removed from the
                    // Posts collection when we call MarkAsDeleted
                    foreach (var post in blog.Posts.ToList())
                    {
                        post.MarkAsDeleted();
                    }

                    // Mark the Blog for deletion
                    blog.MarkAsDeleted();

                    // Save the changes using the service
                    service.UpdateBlog(blog);
                }
            }
        }
    }
```

이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.

-   마우스 오른쪽 단추로 클릭 합니다 **STESample.ConsoleTest** 프로젝트 **솔루션 탐색기** 선택한 **디버그-&gt; 새 인스턴스 시작**

응용 프로그램을 실행 하는 경우 다음과 같은 출력이 표시 됩니다.

```
Initial Data:
ADO.NET Blog
- Intro to EF
- What is New

After Adding:
ADO.NET Blog
- Intro to EF
- What is New
The New Blog
- Welcome to the new blog
- What's new on the new blog

After Update:
ADO.NET Blog
- Intro to EF
- What is New
The Not-So-New Blog
- Welcome to the new blog
- What's new on the new blog

After Delete:
ADO.NET Blog
- Intro to EF
- What is New

Press any key to exit...
```

## <a name="consume-the-service-from-a-wpf-application"></a>WPF 응용 프로그램에서 서비스 사용

서비스를 사용 하는 WPF 응용 프로그램을 만들어 보겠습니다.

-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Visual C\#**  왼쪽된 창에서 다음 **WPF 응용 프로그램**
-   입력 **STESample.WPFTest** 이름과 클릭 **확인**
-   에 대 한 참조를 추가 합니다 **STESample.Entities** 프로젝트

WCF 서비스에 대 한 서비스 참조 필요

-   마우스 오른쪽 단추로 클릭 합니다 **STESample.WPFTest** 프로젝트 **솔루션 탐색기** 선택한 **서비스 참조 추가...**
-   클릭 **검색**
-   입력 **BloggingService** 네임 스페이스와 클릭 **확인**

이제 서비스를 사용 하는 일부 코드를 작성할 수 했습니다.

-   오픈 **MainWindow.xaml** 그 내용을 다음 코드로 바꿉니다.

``` xaml
    <Window
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:STESample="clr-namespace:STESample;assembly=STESample.Entities"
        mc:Ignorable="d" x:Class="STESample.WPFTest.MainWindow"
        Title="MainWindow" Height="350" Width="525" Loaded="Window_Loaded">

        <Window.Resources>
            <CollectionViewSource
                x:Key="blogViewSource"
                d:DesignSource="{d:DesignInstance {x:Type STESample:Blog}, CreateList=True}"/>
            <CollectionViewSource
                x:Key="blogPostsViewSource"
                Source="{Binding Posts, Source={StaticResource blogViewSource}}"/>
        </Window.Resources>

        <Grid DataContext="{StaticResource blogViewSource}">
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding}" Margin="10,10,10,179">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding BlogId}" Header="Id" Width="Auto" IsReadOnly="True" />
                    <DataGridTextColumn Binding="{Binding Name}" Header="Name" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Url}" Header="Url" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <DataGrid AutoGenerateColumns="False" EnableRowVirtualization="True"
                      ItemsSource="{Binding Source={StaticResource blogPostsViewSource}}" Margin="10,145,10,38">
                <DataGrid.Columns>
                    <DataGridTextColumn Binding="{Binding PostId}" Header="Id" Width="Auto"  IsReadOnly="True"/>
                    <DataGridTextColumn Binding="{Binding Title}" Header="Title" Width="Auto"/>
                    <DataGridTextColumn Binding="{Binding Content}" Header="Content" Width="Auto"/>
                </DataGrid.Columns>
            </DataGrid>
            <Button Width="68" Height="23" HorizontalAlignment="Right" VerticalAlignment="Bottom"
                    Margin="0,0,10,10" Click="buttonSave_Click">Save</Button>
        </Grid>
    </Window>
```

-   MainWindow에 대 한 코드 숨김 엽니다 (**MainWindow.xaml.cs**) 콘텐츠를 다음 코드로 바꾸고

``` csharp
    using STESample.WPFTest.BloggingService;
    using System.Collections.Generic;
    using System.Linq;
    using System.Windows;
    using System.Windows.Data;

    namespace STESample.WPFTest
    {
        public partial class MainWindow : Window
        {
            public MainWindow()
            {
                InitializeComponent();
            }

            private void Window_Loaded(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Find the view source for Blogs and populate it with all Blogs (and related Posts)
                    // from the Service. The default editing functionality of WPF will allow the objects
                    // to be manipulated on the screen.
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }

            private void buttonSave_Click(object sender, RoutedEventArgs e)
            {
                using (var service = new Service1Client())
                {
                    // Get the blogs that are bound to the screen
                    var blogsViewSource = (CollectionViewSource)this.FindResource("blogViewSource");
                    var blogs = (List<Blog>)blogsViewSource.Source;

                    // Save all Blogs and related Posts
                    foreach (var blog in blogs)
                    {
                        service.UpdateBlog(blog);
                    }

                    // Re-query for data to get database-generated keys etc.
                    blogsViewSource.Source = service.GetBlogs().ToList();
                }
            }
        }
    }
```

이제 응용 프로그램을 실행하여 작동하는지 확인할 수 있습니다.

-   마우스 오른쪽 단추로 클릭 합니다 **STESample.WPFTest** 프로젝트 **솔루션 탐색기** 선택한 **디버그-&gt; 새 인스턴스 시작**
-   화면을 사용 하 여 데이터를 조작 하 고 사용 하 여 서비스를 통해 저장 된 **저장할** 단추

![WPF](~/ef6/media/wpf.png)
