---
title: 자동 Code First 마이그레이션을-EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 256d1c774a2165dc12daf3d04550566c1a44b751
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250454"
---
# <a name="automatic-code-first-migrations"></a>자동 Code First 마이그레이션
자동 마이그레이션은 사용 하면 각 변경에 대 한 프로젝트에서 코드 파일을 사용 하지 않고도 Code First 마이그레이션을 사용 수 있습니다. 모든 변경 내용을 자동으로 적용할 수 있습니다-열 이름 바꾸기 코드 기반 마이그레이션 사용 해야 하는 예를 들어 있습니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션을 사용 하는 방법을 알고 있다고 가정 합니다. 이렇게 하지 않으면 경우 읽기 해야 [Code First 마이그레이션을](~/ef6/modeling/code-first/migrations/index.md) 계속 하기 전에 합니다.

## <a name="recommendation-for-team-environments"></a>팀 환경에 대 한 권장 사항

자동 및 코드 기반 마이그레이션이 섞어서 읽기 쉽게 표시할 수 있지만이 팀 개발 시나리오에서 권장 되지 않습니다. 소스 제어를 사용 하는 개발자 팀의 일원인 경우 순수 하 게 자동 마이그레이션 또는 순수 하 게 코드 기반 마이그레이션 중 사용 해야 합니다. 자동 마이그레이션 제한 사항이 지정 된 팀 환경에서 코드 기반 마이그레이션을 사용 하는 것이 좋습니다.

## <a name="building-an-initial-model--database"></a>초기 모델 및 데이터베이스 만들기

마이그레이션 사용을 시작하기 전에 먼저 사용할 프로젝트와 Code First 모델이 필요합니다. 이 연습에서는 정식 **Blog** 및 **Post** 모델을 사용하려고 합니다.

-   새 **MigrationsAutomaticDemo** 콘솔 응용 프로그램
-   최신 버전의 **EntityFramework** NuGet 패키지를 프로젝트에 추가합니다.
    -   **도구 –&gt; 라이브러리 패키지 관리자 –&gt; 패키지 관리자 콘솔**
    -   **Install-Package EntityFramework** 명령 실행
-   아래 표시된 코드가 있는 **Model.cs** 파일을 추가합니다. 이 코드에서는 도메인 모델을 구성하는 단일 **Blog** 클래스와 EF Code First 컨텍스트인 **BlogContext** 클래스를 정의합니다

  ``` csharp
      using System.Data.Entity;
      using System.Collections.Generic;
      using System.ComponentModel.DataAnnotations;
      using System.Data.Entity.Infrastructure;

      namespace MigrationsAutomaticDemo
      {
          public class BlogContext : DbContext
          {
              public DbSet<Blog> Blogs { get; set; }
          }

          public class Blog
          {
              public int BlogId { get; set; }
              public string Name { get; set; }
          }
      }
  ```

-   이제 모델이 준비되었으므로 데이터 액세스를 수행하는 데 사용할 시간입니다. **Program.cs** 파일을 아래 표시된 코드로 업데이트합니다.

  ``` csharp
      using System;
      using System.Collections.Generic;
      using System.Linq;
      using System.Text;

      namespace MigrationsAutomaticDemo
      {
          class Program
          {
              static void Main(string[] args)
              {
                  using (var db = new BlogContext())
                  {
                      db.Blogs.Add(new Blog { Name = "Another Blog " });
                      db.SaveChanges();

                      foreach (var blog in db.Blogs)
                      {
                          Console.WriteLine(blog.Name);
                      }
                  }

                  Console.WriteLine("Press any key to exit...");
                  Console.ReadKey();
              }
          }
      }
  ```

-   응용 프로그램을 실행 하 고 확인할 수 있습니다는 **MigrationsAutomaticCodeDemo.BlogContext** 데이터베이스 생성 됩니다.

    ![LocalDB 데이터베이스](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>마이그레이션을 사용하도록 설정

이제 모델을 약간 변경할 시간입니다.

-   Blog 클래스에 Url 속성을 소개해 보겠습니다.

``` csharp
    public string Url { get; set; }
```

응용 프로그램을 다시 실행하는 경우 InvalidOperationException이 발생합니다. 이 예외에서는 *'BlogContext' 컨텍스트를 지원하는 모델이 변경되었습니다. Code First 마이그레이션을 사용하여 데이터베이스를 업데이트하는 것이 좋습니다.* 라고 나타냅니다([*http://go.microsoft.com/fwlink/?LinkId=238269*](http://go.microsoft.com/fwlink/?LinkId=238269)*).*

예외에서 암시하듯이 이제 Code First 마이그레이션 사용을 시작할 시간입니다. 자동 마이그레이션을 사용 하려고 하므로 여기서을 지정 합니다 **– EnableAutomaticMigrations** 전환 합니다.

-   실행 합니다 **Enable-migrations – EnableAutomaticMigrations** 패키지 관리자 콘솔이 명령에서 명령에 추가 **마이그레이션을** 프로젝트 폴더입니다. 이 새 폴더에 파일이 하나 포함 되어 있습니다.

-   **Configuration 클래스입니다.** 이 클래스를 사용하면 컨텍스트에 맞게 마이그레이션이 작동하는 방식을 구성할 수 있습니다. 이 연습에서는 기본 구성만 사용하겠습니다.
    *프로젝트에는 Code First 컨텍스트가 하나만 있으므로 Enable-Migrations는 이 구성이 적용되는 컨텍스트 유형을 자동으로 채웁니다.*

 

## <a name="your-first-automatic-migration"></a>첫 번째 자동 마이그레이션

Code First 마이그레이션에는 익숙해져야 하는 다음 두 가지 기본 명령이 있습니다.

-   **Add-Migration**은 마지막 마이그레이션을 만든 후에 대한 변경 내용에 따라 다음 마이그레이션을 스캐폴드합니다
-   **Update-Database**는 보류 중인 모든 마이그레이션을 데이터베이스에 적용합니다.

방지 하려고 추가 마이그레이션 (하지 않는 한 사용을 실제로 필요)에 초점을 자동으로 Code First 마이그레이션을 수 있도록 계산 하 고 변경 내용을 적용 합니다. 사용해 보겠습니다 **Update-database** 모델에 변경 내용을 푸시 하도록 Code First 마이그레이션을 가져오려면 (새 **Blog.Ur**l 속성) 데이터베이스에 합니다.

-   실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다.

**MigrationsAutomaticDemo.BlogContext** 데이터베이스는 이제 포함 하도록 업데이트 합니다 **Url** 열에는 **블로그** 테이블.

 

## <a name="your-second-automatic-migration"></a>두 번째 자동 마이그레이션

다른 변경 하 고 Code First 마이그레이션을 데이터베이스에 변경 내용을 자동으로 푸시 하도록 만들어 보겠습니다.

-   새 **Post** 클래스도 추가하겠습니다.

``` csharp
    public class Post
    {
        public int PostId { get; set; }
        [MaxLength(200)]
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
```

-   또한 **Blog** 클래스에 **Posts** 컬렉션도 추가하여 **Blog**와 **Post** 간 관계의 다른 끝 부분을 만듭니다.

``` csharp
    public virtual List<Post> Posts { get; set; }
```

이제 사용할 **Update-database** 데이터베이스를 최신 상태로 설정 합니다. 이번에는 Code First 마이그레이션이 실행되는 SQL을 확인할 수 있도록 **-Verbose** 플래그를 지정하겠습니다.

-   패키지 관리자 콘솔에서 **Update-Database -Verbose** 명령을 실행합니다.

## <a name="adding-a-code-based-migration"></a>마이그레이션에 따라 코드를 추가 합니다.

이제 코드 기반 마이그레이션에 대 한 사용 하고자 할 수 있습니다 것에 대해 살펴보겠습니다.

-   추가 해 보겠습니다를 **등급** 속성을 **블로그** 클래스

``` csharp
    public int Rating { get; set; }
```

만 실행할 수 있습니다 **Update-database** 를 데이터베이스에 이러한 변경 내용을 푸시합니다. 그러나 추가 될 예정 비허용 **Blogs.Rating** 열을 테이블의 모든 기존 데이터가 있으면이 할당 되는 새 열에 대 한 데이터 형식의 CLR 기본 (등급은 정수, 그러니까 **0**). 그러나 **Blogs** 테이블의 기존 행이 적절한 등급으로 시작되도록 기본값인 **3**을 지정하려고 합니다.
우리는를 편집할 수 있도록이 변경 아웃 코드 기반 마이그레이션에 쓸 추가 마이그레이션 명령을 사용해 보겠습니다. 합니다 **Add-migration** 명령을 사용 하면 이러한 마이그레이션 이름을 지정 하겠습니다 우리를 **AddBlogRating**합니다.

-   실행 합니다 **Add-migration AddBlogRating** 패키지 관리자 콘솔에서 명령을 합니다.
-   에 **마이그레이션을** 폴더 이제 새 **AddBlogRating** 마이그레이션. 마이그레이션 파일 이름이 사전 순서 지정에 도움이 되도록 타임 스탬프를 사용 하 여 고정 됩니다. Blog.Rating (아래 코드에서 줄 10)에 대 한 기본값은 3 지정 하려면 생성된 된 코드 편집

*마이그레이션에는 일부 메타 데이터를 캡처하는 코드 숨김 파일을 있습니다. 이 메타 데이터는 Code First 마이그레이션을이 코드 기반 마이그레이션을 시작 하기 전에 수행한 자동 마이그레이션을 복제할 수 있습니다. 다른 개발자가이 마이그레이션을 실행 하려는 경우 또는 응용 프로그램을 배포할 때 이것이 중요 합니다.*

``` csharp
    namespace MigrationsAutomaticDemo.Migrations
    {
        using System;
        using System.Data.Entity.Migrations;

        public partial class AddBlogRating : DbMigration
        {
            public override void Up()
            {
                AddColumn("Blogs", "Rating", c => c.Int(nullable: false, defaultValue: 3));
            }

            public override void Down()
            {
                DropColumn("Blogs", "Rating");
            }
        }
    }
```

편집한 마이그레이션이 좋아 보이므로 **Update-Database**를 사용하여 데이터베이스를 최신 상태로 만들겠습니다.

-   실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다.

## <a name="back-to-automatic-migrations"></a>자동 마이그레이션은 돌아가기

간단한 변경에 대 한 자동 마이그레이션을 다시 전환 하려면 무료 됩니다. 각 코드 기반 마이그레이션에 대 한 코드 숨김 파일에 저장 하는 메타 데이터에 따라 올바른 순서로 자동 및 코드 기반 마이그레이션을 수행 하는 code First 마이그레이션의 고려 합니다.

-   모델을 Post.Abstract 속성을 추가 해 보겠습니다

``` csharp
    public string Abstract { get; set; }
```

이제 사용할 수 있습니다 **Update-database** 이 변경 된 자동 마이그레이션을 사용 하 여 데이터베이스를 푸시 하려면 Code First 마이그레이션을 가져오려고 합니다.

-   실행 합니다 **Update-database** 패키지 관리자 콘솔에서 명령을 합니다.

## <a name="summary"></a>요약

이 연습에서는 적용할 자동 마이그레이션을 사용 하는 방법을 살펴보았습니다 모델 데이터베이스로 변경 합니다. 스 캐 폴드 더 많이 제어 해야 하는 경우 자동 마이그레이션 사이 코드 기반 마이그레이션을 실행 하는 방법을 살펴보았습니다.
