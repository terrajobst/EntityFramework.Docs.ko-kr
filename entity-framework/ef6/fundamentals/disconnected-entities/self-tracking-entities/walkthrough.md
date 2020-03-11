---
title: 자동 추적 엔터티 연습-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b21207c9-1d95-4aa3-ae05-bc5fe300dab0
ms.openlocfilehash: 9bd644461f50a7eff1006cb8866ca9a3b08b6b8d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416146"
---
# <a name="self-tracking-entities-walkthrough"></a>자동 추적 엔터티 연습
> [!IMPORTANT]
> 자동 추적 엔터티 템플릿을 더 이상 권장하지 않습니다. 이 템플릿은 기존 애플리케이션을 지원하는 용도로만 제공될 것입니다. 애플리케이션에서 연결이 끊긴 엔터티 그래프를 사용해야 하는 경우 커뮤니티에서 적극적으로 개발한 자동 추적 엔터티와 비슷한 기술인 [추적 가능 엔터티](https://trackableentities.github.io/) 같은 다른 대안을 고려하거나 하위 수준 변경 내용 추적 API를 사용하여 사용자 지정 코드를 작성하는 방법을 고려해 보세요.

이 연습에서는 Windows Communication Foundation (WCF) 서비스에서 엔터티 그래프를 반환 하는 작업을 노출 하는 시나리오를 보여 줍니다. 그런 다음 클라이언트 응용 프로그램은 해당 그래프를 조작 하 고 Entity Framework를 사용 하 여 업데이트의 유효성을 검사 하 고 데이터베이스에 저장 하는 서비스 작업에 수정 내용을 전송 합니다.

이 연습을 완료 하기 전에 [자동 추적 엔터티](index.md) 페이지를 확인 해야 합니다.

이 연습에서는 다음 작업을 수행합니다.

-   액세스할 데이터베이스를 만듭니다.
-   모델을 포함 하는 클래스 라이브러리를 만듭니다.
-   자동 추적 엔터티 생성기 템플릿으로 바꿉니다.
-   엔터티 클래스를 개별 프로젝트로 이동 합니다.
-   엔터티를 쿼리하고 저장 하는 작업을 노출 하는 WCF 서비스를 만듭니다.
-   서비스를 사용 하는 클라이언트 응용 프로그램 (콘솔 및 WPF)을 만듭니다.

이 연습에서는 Database First을 사용 하지만 동일한 기술이 Model First에 동일 하 게 적용 됩니다.

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 최신 버전의 Visual Studio가 필요 합니다.

## <a name="create-a-database"></a>데이터베이스 만들기

Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2012을 사용 하는 경우 LocalDB 데이터베이스를 만듭니다.
-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.

계속 해 서 데이터베이스를 생성 해 보겠습니다.

-   Visual Studio를 엽니다.
-   **뷰&gt; 서버 탐색기**
-   데이터 연결을 마우스 오른쪽 단추로 클릭 하 **&gt; 연결 추가** ...를 클릭 합니다.
-   서버 탐색기 데이터베이스에 연결 하지 않은 경우 **Microsoft SQL Server** 를 데이터 원본으로 선택 해야 합니다.
-   설치한 항목에 따라 LocalDB 또는 SQL Express에 연결
-   데이터베이스 이름으로 **STESample** 을 입력 합니다.
-   **확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.
-   이제 새 데이터베이스가 서버 탐색기에 표시 됩니다.
-   Visual Studio 2012을 사용 하는 경우
    -   서버 탐색기에서 데이터베이스를 마우스 오른쪽 단추로 클릭 하 고 **새 쿼리** 를 선택 합니다.
    -   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **실행** 을 선택 합니다.
-   Visual Studio 2010을 사용 하는 경우
    -   **데이터&gt; Transact-sql 편집기-&gt; 새 쿼리 연결** ...을 선택 합니다.
    -   \\SQLEXPRESS를 서버 이름으로 입력 하 고 **확인을** 클릭 **합니다.**
    -   쿼리 편집기 위쪽의 드롭다운에서 **STESample** 데이터베이스를 선택 합니다.
    -   다음 SQL을 새 쿼리에 복사한 다음 쿼리를 마우스 오른쪽 단추로 클릭 하 고 **Sql 실행** 을 선택 합니다.

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

먼저 모델을 배치할 프로젝트가 필요 합니다.

-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 창에서 **Visual C\#** 를 선택 하 고 **클래스 라이브러리** 를 선택 합니다.
-   이름으로 **STESample** 를 입력 하 고 **확인을** 클릭 합니다.

이제 EF Designer에서 간단한 모델을 만들어 데이터베이스에 액세스 합니다.

-   **프로젝트-새 항목 추가&gt; ...**
-   왼쪽 창에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델
-   이름으로 **BloggingModel** 를 입력 하 고 **확인을** 클릭 합니다.
-   **데이터베이스에서 생성** 을 선택 하 고 **다음** 을 클릭 합니다.
-   이전 섹션에서 만든 데이터베이스에 대 한 연결 정보를 입력 합니다.
-   연결 문자열 이름으로 **BloggingContext** 를 입력 하 고 **다음** 을 클릭 합니다.
-   **테이블** 옆의 확인란을 선택 하 고 **마침** 을 클릭 합니다.

## <a name="swap-to-ste-code-generation"></a>붙여넣기 코드 생성으로 전환

이제 기본 코드 생성을 사용 하지 않도록 설정 하 고 자동 추적 엔터티로 교환 해야 합니다.

### <a name="if-you-are-using-visual-studio-2012"></a>Visual Studio 2012을 사용 하는 경우

-   **솔루션 탐색기** 에서 **BloggingModel** 를 확장 하 고 **BloggingModel.tt** 및
    **BloggingModel.Context.tt** 를 삭제 합니다. *이렇게 하면 기본 코드 생성이 사용 되지 않습니다* .
-   EF 디자이너 화면에서 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.
-   왼쪽 창에서 **온라인** 을 선택 하 고 **붙여넣기 생성기** 를 검색 합니다.
-   **붙여넣기 생성기 For C\#** 템플릿을 선택 하 고 이름으로 **STETemplate** 를 입력 한 다음 **추가** 를 클릭 합니다.
-   **STETemplate.tt** 및 **STETemplate.Context.tt** 파일은 BloggingModel 파일 아래에 중첩 되어 추가 됩니다.

### <a name="if-you-are-using-visual-studio-2010"></a>Visual Studio 2010을 사용 하는 경우

-   EF 디자이너 화면에서 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.
-   왼쪽 창에서 **코드** 를 선택한 다음 **자동 추적 엔터티 생성기를 ADO.NET** .
-   이름으로 **STETemplate** 를 입력 하 고 **추가** 를 클릭 합니다.
-   **STETemplate.tt** 및 **STETemplate.Context.tt** 파일은 프로젝트에 직접 추가 됩니다.

## <a name="move-entity-types-into-separate-project"></a>엔터티 형식을 개별 프로젝트로 이동

자동 추적 엔터티를 사용 하려면 클라이언트 응용 프로그램에 모델에서 생성 된 엔터티 클래스에 대 한 액세스 권한이 있어야 합니다. 전체 모델을 클라이언트 응용 프로그램에 노출 하지 않으려는 경우 엔터티 클래스를 개별 프로젝트로 이동 하겠습니다.

첫 번째 단계는 기존 프로젝트에서 엔터티 클래스를 생성 하는 것을 중지 하는 것입니다.

-   **솔루션 탐색기** 에서 **STETemplate.tt** 을 마우스 오른쪽 단추로 클릭 하 고 **속성** 을 선택 합니다.
-   **속성** 창의 **사용자 지정 도구** 속성에서 **texttemplatingfilegenerator** 의 선택을 취소 합니다.
-   **솔루션 탐색기** 에서 **STETemplate.tt** 를 확장 하 고 그 아래 중첩 된 모든 파일을 삭제 합니다.

다음으로 새 프로젝트를 추가 하 고 여기에 엔터티 클래스를 생성 하겠습니다.

-   **파일&gt; 추가&gt; 프로젝트 ...**
-   왼쪽 창에서 **Visual C\#** 를 선택 하 고 **클래스 라이브러리** 를 선택 합니다.
-   이름으로 STESample를 입력 하 고 **확인을** 클릭 **합니다.**
-   **프로젝트-기존 항목 추가&gt; ...**
-   **STESample** 프로젝트 폴더로 이동 합니다.
-   모든 파일을 보려면 선택 **합니다 (\*\*).**
-   **STETemplate.tt** 파일 선택
-   **추가** 단추 옆에 있는 드롭다운 화살표를 클릭 하 고 **링크로 추가** 를 선택 합니다.

    ![연결 된 템플릿 추가](~/ef6/media/addlinkedtemplate.png)

또한 컨텍스트와 동일한 네임 스페이스에서 엔터티 클래스가 생성 되도록 합니다. 이렇게 하면 응용 프로그램 전체에서 추가 해야 하는 using 문 수가 줄어듭니다.

-   **솔루션 탐색기** 에서 연결 된 **STETemplate.tt** 를 마우스 오른쪽 단추로 클릭 하 고 **속성** 을 선택 합니다.
-   **속성** 창에서 **사용자 지정 도구 네임 스페이스** 를 **STESample** 로 설정 합니다.

붙여넣기 템플릿에서 생성 된 코드에는 컴파일을 위해 **system.object** 에 대 한 참조가 필요 합니다. 이 라이브러리는 serialize 할 수 있는 엔터티 형식에서 사용 되는 WCF **DataContract** 및 **DataMember** 특성에 필요 합니다.

-   **솔루션 탐색기** 에서 **STESample** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    -   Visual Studio 2012에서- **system.web** 옆의 확인란을 선택 하 고 **확인** 을 클릭 합니다.
    -   Visual Studio 2010에서, 선택 하 고 **확인** 을 클릭 합니다 **.**

마지막으로 컨텍스트를 포함 하는 프로젝트에는 엔터티 형식에 대 한 참조가 필요 합니다.

-   **솔루션 탐색기** 에서 **STESample** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **참조 추가** ...를 선택 합니다.
    -   Visual Studio 2012-왼쪽 창에서 **솔루션** 을 선택 하 고 STESample 옆에 있는 확인란을 선택 하 고 **확인** 을 클릭 **합니다.**
    -   Visual Studio 2010- **프로젝트** 탭을 선택 하 고 STESample를 선택한 다음 **확인** 을 클릭 **합니다.**

>[!NOTE]
> 엔터티 형식을 개별 프로젝트로 이동 하는 또 다른 옵션은 템플릿 파일을 기본 위치에서 연결 하는 대신 이동 하는 것입니다. 이렇게 하려면 템플릿에서 **inputFile** 변수를 업데이트 하 여 edmx 파일 (이 예제에서는 **..\\BloggingModel**)에 대 한 상대 경로를 제공 해야 합니다.

## <a name="create-a-wcf-service"></a>WCF 서비스 만들기

이제 데이터를 노출 하는 WCF 서비스를 추가할 때 프로젝트를 만들어 시작 합니다.

-   **파일&gt; 추가&gt; 프로젝트 ...**
-   왼쪽 창에서 **Visual C\#** 를 선택 하 고 **WCF 서비스 응용 프로그램** 을 선택 합니다.
-   이름으로 STESample를 입력 하 고 **확인을** 클릭 **합니다.**
-   System.object 어셈블리에 참조를 추가 합니다 **.**
-   **STESample** 및 **STESample** 프로젝트에 대 한 참조 추가

런타임에 발견 되도록 EF 연결 문자열을이 프로젝트에 복사 해야 합니다.

-    **STESample **프로젝트에 대 한 app.config 파일을 열고 **connectionStrings** 요소를 복사 합니다 **.**
-   **ConnectionStrings** 요소를 STESample **프로젝트에서 web.config 파일의** **구성** 요소에 대 한 자식 요소로 붙여넣습니다 **.**

이제 실제 서비스를 구현할 때입니다.

-   **IService1.cs** 를 열고 내용을 다음 코드로 바꿉니다.

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

-   Service1을 열고 내용을 다음 코드로 바꿉니다 **.**

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

-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 창에서 **Visual C\#** 를 선택 하 고 **콘솔 응용 프로그램** 을 선택 합니다.
-   이름으로 **STESample. ConsoleTest** 를 입력 하 고 **확인** 을 클릭 합니다.
-   **STESample** 프로젝트에 대 한 참조 추가

WCF 서비스에 대 한 서비스 참조가 필요 합니다.

-   **솔루션 탐색기** 에서 **STESample ConsoleTest** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **서비스 참조 추가 ...** 를 선택 합니다.
-   **검색** 을 클릭 합니다.
-   네임 스페이스로 **BloggingService** 를 입력 하 고 **확인을** 클릭 합니다.

이제 서비스를 사용 하는 코드를 작성할 수 있습니다.

-   **Program.cs** 를 열고 내용을 다음 코드로 바꿉니다.

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

이제 애플리케이션을 실행하여 작동하는지 확인할 수 있습니다.

-   **솔루션 탐색기** 에서 STESample ConsoleTest 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **디버그-&gt; 새 인스턴스 시작** 을 선택 **합니다.**

응용 프로그램이 실행 되 면 다음과 같은 출력이 표시 됩니다.

```console
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

-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 창에서 **Visual C\#** 를 선택 하 고 **WPF 응용 프로그램** 을 선택 합니다.
-   이름으로 **STESample. WPFTest** 를 입력 하 고 **확인** 을 클릭 합니다.
-   **STESample** 프로젝트에 대 한 참조 추가

WCF 서비스에 대 한 서비스 참조가 필요 합니다.

-   **솔루션 탐색기** 에서 **STESample WPFTest** 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **서비스 참조 추가 ...** 를 선택 합니다.
-   **검색** 을 클릭 합니다.
-   네임 스페이스로 **BloggingService** 를 입력 하 고 **확인을** 클릭 합니다.

이제 서비스를 사용 하는 코드를 작성할 수 있습니다.

-   **Mainwindow.xaml** 를 열고 내용을 다음 코드로 바꿉니다.

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

-   Mainwindow.xaml (**MainWindow.xaml.cs**)에 대 한 코드를 열고 내용을 다음 코드로 바꿉니다.

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

이제 애플리케이션을 실행하여 작동하는지 확인할 수 있습니다.

-   **솔루션 탐색기** 에서 STESample WPFTest 프로젝트를 마우스 오른쪽 단추로 클릭 하 고 **디버그-&gt; 새 인스턴스 시작** 을 선택 **합니다.**
-   화면을 사용 하 여 데이터를 조작 하 고 **저장** 단추를 사용 하 여 서비스를 통해 데이터를 저장할 수 있습니다.

![WPF 주 창](~/ef6/media/wpf.png)
