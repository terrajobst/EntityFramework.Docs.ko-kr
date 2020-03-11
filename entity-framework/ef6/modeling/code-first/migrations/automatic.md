---
title: 자동 Code First 마이그레이션-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0eb86787-2161-4cb4-9cb8-67c5d6e95650
ms.openlocfilehash: 2713afaf09707b7696e90464aac9945c2d82d274
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415702"
---
# <a name="automatic-code-first-migrations"></a>자동 Code First 마이그레이션
자동 마이그레이션을 사용 하면 각 변경 내용에 대 한 코드 파일을 프로젝트에 포함 하지 않고도 Code First 마이그레이션를 사용할 수 있습니다. 모든 변경 내용을 자동으로 적용할 수 있는 것은 아닙니다. 예를 들어, 열 이름을 바꾸면 코드 기반 마이그레이션을 사용 해야 합니다.

> [!NOTE]
> 이 문서에서는 기본 시나리오에서 Code First 마이그레이션를 사용 하는 방법을 알고 있다고 가정 합니다. 그렇지 않으면 계속 하기 전에 [Code First 마이그레이션](~/ef6/modeling/code-first/migrations/index.md) 읽어야 합니다.

## <a name="recommendation-for-team-environments"></a>팀 환경에 대 한 권장 사항

자동 및 코드 기반 마이그레이션을 섞어서 수 있지만이는 팀 개발 시나리오에서 권장 되지 않습니다. 소스 제어를 사용 하는 개발자 팀의 일부인 경우에는 순수 하 게 자동 마이그레이션 또는 순수 코드 기반 마이그레이션을 사용 해야 합니다. 자동 마이그레이션의 제한 사항을 고려 하 여 팀 환경에서 코드 기반 마이그레이션을 사용 하는 것이 좋습니다.

## <a name="building-an-initial-model--database"></a>초기 모델 및 데이터베이스 만들기

마이그레이션 사용을 시작하기 전에 먼저 사용할 프로젝트와 Code First 모델이 필요합니다. 이 연습에서는 정식 **Blog** 및 **Post** 모델을 사용하려고 합니다.

-   새 **MigrationsAutomaticDemo** 콘솔 응용 프로그램 만들기
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

-   응용 프로그램을 실행 하면 **MigrationsAutomaticCodeDemo 컨텍스트** 데이터베이스가 생성 되는 것을 볼 수 있습니다.

    ![데이터베이스 LocalDB](~/ef6/media/databaselocaldb.png)

## <a name="enabling-migrations"></a>마이그레이션을 사용하도록 설정

이제 모델을 약간 변경할 시간입니다.

-   Blog 클래스에 Url 속성을 소개해 보겠습니다.

``` csharp
    public string Url { get; set; }
```

응용 프로그램을 다시 실행 하는 경우 *데이터베이스를 만든 후에 ' BlogContext ' 컨텍스트를 지 원하는 모델이 변경 되었다는 InvalidOperationException을 얻게 됩니다. Code First 마이그레이션를 사용 하 여 데이터베이스를 업데이트 하는 것이 좋습니다 (* [ *http://go.microsoft.com/fwlink/?LinkId=238269* ](https://go.microsoft.com/fwlink/?LinkId=238269) *).*

예외에서 암시하듯이 이제 Code First 마이그레이션 사용을 시작할 시간입니다. 자동 마이그레이션을 사용 하려고 하기 때문에 **– enableautomatic** migration 스위치를 지정 합니다.

-   패키지 관리자 콘솔에서 **사용-마이그레이션-Enable자동 마이그레이션** 명령을 실행 합니다 .이 명령은 **마이그레이션** 폴더를 프로젝트에 추가 했습니다. 이 새 폴더에는 하나의 파일이 포함 됩니다.

-   **Configuration 클래스입니다.** 이 클래스를 사용하면 컨텍스트에 맞게 마이그레이션이 작동하는 방식을 구성할 수 있습니다. 이 연습에서는 기본 구성만 사용하겠습니다.
    *프로젝트에는 Code First 컨텍스트가 하나만 있으므로 Enable-Migrations는 이 구성이 적용되는 컨텍스트 유형을 자동으로 채웁니다.*

 

## <a name="your-first-automatic-migration"></a>첫 번째 자동 마이그레이션

Code First 마이그레이션에는 익숙해져야 하는 다음 두 가지 기본 명령이 있습니다.

-   **Add-Migration**은 마지막 마이그레이션을 만든 후에 대한 변경 내용에 따라 다음 마이그레이션을 스캐폴드합니다
-   **Update-Database**는 보류 중인 모든 마이그레이션을 데이터베이스에 적용합니다.

추가 마이그레이션 (필요한 경우는 제외)을 사용 하지 않도록 하 고, 변경 내용을 자동으로 계산 하 고 적용할 Code First 마이그레이션에 집중 하는 것이 좋습니다. **업데이트-데이터베이스** 를 사용 하 여 모델 (새 **Blog. Ur**l 속성)의 변경 내용을 데이터베이스로 푸시하는 Code First 마이그레이션을 가져옵니다.

-   패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다.

이제 **MigrationsAutomaticDemo 컨텍스트** 데이터베이스는 **블로그** 테이블에 **Url** 열을 포함 하도록 업데이트 되었습니다.

 

## <a name="your-second-automatic-migration"></a>두 번째 자동 마이그레이션

다른 변경을 수행 하 고 변경 내용을 데이터베이스에 자동으로 푸시 Code First 마이그레이션 합니다.

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

이제 **업데이트-데이터베이스** 를 사용 하 여 데이터베이스를 최신 상태로 만듭니다. 이번에는 Code First 마이그레이션이 실행되는 SQL을 확인할 수 있도록 **-Verbose** 플래그를 지정하겠습니다.

-   패키지 관리자 콘솔에서 **Update-Database -Verbose** 명령을 실행합니다.

## <a name="adding-a-code-based-migration"></a>코드 기반 마이그레이션 추가

이제에 대 한 코드 기반 마이그레이션을 사용 하고자 하는 항목을 살펴보겠습니다.

-   **블로그** 클래스에 **등급** 속성을 추가 해 보겠습니다.

``` csharp
    public int Rating { get; set; }
```

**업데이트 데이터베이스** 를 실행 하 여 이러한 변경 내용을 데이터베이스에 푸시할 수 있습니다. 그러나 null을 **허용 하지 않는**블로그를 추가 하 고 **있습니다. 평점** 열. 테이블에 기존 데이터가 있으면 새 열에 대 한 데이터 형식의 CLR 기본값 (등급은 정수)을 할당 받게 됩니다. 그러나 **Blogs** 테이블의 기존 행이 적절한 등급으로 시작되도록 기본값인 **3**을 지정하려고 합니다.
추가 마이그레이션 명령을 사용 하 여이 변경 내용을 코드 기반 마이그레이션에 기록 하 여 편집할 수 있도록 하겠습니다. **마이그레이션 추가** 명령을 사용 하 여 이러한 마이그레이션의 이름을 지정할 수 있습니다. 우리는 우리가 **AddBlogRating**.

-   패키지 관리자 콘솔에서 **AddBlogRating 추가-마이그레이션** 명령을 실행 합니다.
-   이제 마이그레이션 **폴더에** 새 **AddBlogRating** 마이그레이션이 있습니다. 마이그레이션 파일 이름은 순서를 지정 하는 데 도움이 되는 타임 스탬프로 미리 고정 되어 있습니다. 생성 된 코드를 편집 하 여 블로그의 기본값 3을 지정 합니다 (아래 코드에서 줄 10).

*또한 마이그레이션에는 일부 메타 데이터를 캡처하는 코드 숨김이 있습니다. 이 메타 데이터를 사용 하면 Code First 마이그레이션이 코드 기반 마이그레이션 전에 수행한 자동 마이그레이션을 복제할 수 있습니다. 이는 다른 개발자가 마이그레이션을 실행 하려는 경우 나 응용 프로그램을 배포 해야 하는 경우에 중요 합니다.*

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

-   패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다.

## <a name="back-to-automatic-migrations"></a>자동 마이그레이션으로 돌아가기

이제 간단한 변경 내용에 대 한 자동 마이그레이션으로 다시 전환할 수 있습니다. Code First 마이그레이션는 각 코드 기반 마이그레이션의 코드 기반 파일에 저장 되는 메타 데이터를 기반으로 자동 및 코드 기반 마이그레이션을 올바른 순서로 수행 합니다.

-   모델에 대 한 추상 속성을 추가 해 보겠습니다.

``` csharp
    public string Abstract { get; set; }
```

이제 **업데이트-데이터베이스** 를 사용 하 여 자동 마이그레이션을 사용 하 여이 변경 내용을 데이터베이스에 푸시할 Code First 마이그레이션 가져올 수 있습니다.

-   패키지 관리자 콘솔에서 **업데이트 데이터베이스** 명령을 실행 합니다.

## <a name="summary"></a>요약

이 연습에서는 자동 마이그레이션을 사용 하 여 모델 변경 사항을 데이터베이스로 푸시하는 방법에 대해 살펴보았습니다. 또한 더 많은 제어가 필요한 경우 자동 마이그레이션 사이에서 코드 기반 마이그레이션을 스 캐 폴드 하 고 실행 하는 방법도 살펴보았습니다.
