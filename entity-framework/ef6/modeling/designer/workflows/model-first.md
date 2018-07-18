---
title: EF6 먼저 모델
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
caps.latest.revision: 3
ms.openlocfilehash: e7876776ed0dee764d5ced97b863a3580e02e20b
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122269"
---
# <a name="model-first"></a>먼저 모델
이 비디오 및 단계별 연습에서는 Entity Framework를 사용 하 여 Model First 개발에 대 한 소개를 제공 합니다. 먼저 모델을 사용 하면 Entity Framework 디자이너를 사용 하 여 새 모델을 만들고 다음 모델에서 데이터베이스 스키마를 생성할 수 있습니다. 모델은 EDMX 파일 (.edmx 확장명)에 저장 됩니다 및 보고 Entity Framework 디자이너에서 편집할 수 있습니다. 응용 프로그램에서 상호 작용 하는 클래스는 EDMX 파일에서 자동으로 생성 됩니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오 및 단계별 연습에서는 Entity Framework를 사용 하 여 Model First 개발에 대 한 소개를 제공 합니다. 먼저 모델을 사용 하면 Entity Framework 디자이너를 사용 하 여 새 모델을 만들고 다음 모델에서 데이터베이스 스키마를 생성할 수 있습니다. 모델은 EDMX 파일 (.edmx 확장명)에 저장 됩니다 및 보고 Entity Framework 디자이너에서 편집할 수 있습니다. 응용 프로그램에서 상호 작용 하는 클래스는 EDMX 파일에서 자동으로 생성 됩니다.

**작성자**: [Rowan Miller](http://romiller.com/)

**비디오**: [WMV](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>필수 조건

Visual Studio 2010 해야 하거나이 연습을 완료 하려면 Visual Studio 2012를 설치 합니다.

Visual Studio 2010을 사용 하는 경우 해야 할 [NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 설치 합니다.

## <a name="1-create-the-application"></a>1. 응용 프로그램 만들기

간단 하 게 데이터 액세스를 수행 하 여 Model First를 사용 하는 기본적인 콘솔 응용 프로그램을 빌드 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일만&gt; 새로운 기능-&gt; 프로젝트...**
-   선택 **Windows** 왼쪽된 메뉴에서 및 **콘솔 응용 프로그램**
-   입력 **ModelFirstSample** 이름으로
-   **확인**을 선택합니다.

## <a name="2-create-model"></a>2. 모델 만들기

모델을 만들려면 Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하겠습니다.

-   **프로젝트-&gt; 새 항목 추가...**
-   선택 **데이터** 왼쪽된 메뉴에서 차례로 **ADO.NET 엔터티 데이터 모델**
-   입력 **BloggingModel** 이름과 클릭 **확인**, 엔터티 데이터 모델 마법사가 시작 됩니다
-   선택 **빈 모델** 를 클릭 하 고 **마침**

    ![CreateEmptyModel](~/ef6/media/createemptymodel.png)

빈 모델을 사용 하 여 Entity Framework 디자이너 열려 있습니다. 이제 모델에 엔터티, 속성 및 연결 추가 시작할 수 있습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 **속성**
-   속성 창 변경에서 합니다 **엔터티 컨테이너 이름을** 하 **BloggingContext**
    *컨텍스트에 자동으로 생성 되는 파생 컨텍스트의 이름입니다 데이터베이스를 쿼리하고 데이터를 저장할 수 있어를 사용 하 여 세션을 나타냅니다.*
-   디자인 화면을 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 엔터티 중...**
-   입력 **블로그** 엔터티 이름으로 및 **BlogId** 키 이름과 클릭 **확인**

    ![AddBlogEntity](~/ef6/media/addblogentity.png)

-   디자인 화면에 새 엔터티를 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 스칼라 속성**를 입력 **이름** 속성의 이름으로 합니다.
-   추가 하는이 프로세스를 반복 하는 **Url** 속성입니다.
-   마우스 오른쪽 단추로 클릭 합니다 **Url** 디자인 화면에 속성 **속성**, 속성 창 변경에서를 **Nullable** 설정을 **True** 
     *Url을 할당 하지 않고 데이터베이스에 블로그를 저장할 수 있도록*
-   방금 학습 기법을 사용 하 여 추가 **게시물** 사용 하 여 엔터티를 **PostId** 키 속성
-   추가 **제목** 및 **콘텐츠** 스칼라 속성을 **Post** 엔터티

엔터티의 몇 만들었으므로 서로 연결 (또는 관계)을 추가할 시간 됩니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 연결 하는 중...**
-   확인 하는 관계의 한쪽 끝 **블로그** 복합성을 사용 하 여 **하나** 이 고 다른 끝점에 **Post** 복합성을 사용 하 여 **많은** 
     *즉 블로그 많은 게시물에 블로그 게시물 속해*
-   확인 합니다 **외래 키 속성 'Post' 엔터티에 추가할** 상자 체크 인 되 고 클릭 **확인**

    ![AddAssociationMF](~/ef6/media/addassociationmf.png)

이제 것에서 데이터베이스를 생성 하 고 데이터 읽기 및 쓰기에 사용할 수 있는 간단한 모델입니다.

![ModelInitial](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010의 추가 단계

Visual Studio 2010에서 작업 하는 경우 Entity Framework의 최신 버전으로 업그레이드 하기 위해 수행 해야 할 몇 가지 추가 단계가 있습니다. 최신 버그 픽스 뿐만 아니라 훨씬 쉽게 사용할 수 있는 향상 된 API 화면 액세스를 제공 하기 때문에 업그레이드 하는 것이 중요 합니다.

먼저를 해야 NuGet에서 최신 버전의 Entity Framework를 가져오려고 합니다.

-   **프로젝트&gt; NuGet 패키지 관리... ** 
     *없다면는 **NuGet 패키지 관리... ** 를 설치 해야 하는 옵션을 [최신 버전의 NuGet](http://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)*
-   선택 된 **Online** 탭
-   선택 된 **EntityFramework** 패키지
-   클릭 **설치**

다음으로, Entity Framework의 이후 버전에서 도입 된 DbContext api를 사용 하는 코드를 생성할 모델 교환 해야 합니다.

-   EF 디자이너에서 모델의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 선택 **코드 생성 항목 추가...**
-   선택 **온라인 템플릿을** 검색에 대 한 확인 하 고 왼쪽된 메뉴에서 **DbContext**
-   선택 된 EF **5.x C에 대 한 DbContext 생성기\#** 를 입력 **BloggingModel** 이름과 클릭 **추가**

    ![DbContextTemplate](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. 데이터베이스를 생성합니다.

Entity Framework 모델 지정 되 면 저장 하 고 모델을 사용 하 여 데이터를 검색할 수 있도록 하는 데이터베이스 스키마를 계산할 수 있습니다.

Visual Studio와 함께 설치 되는 데이터베이스 서버 설치한 Visual Studio의 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만드는 됩니다.
-   Visual Studio 2012를 사용 하는 경우를 만들 수는 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 데이터베이스입니다.

데이터베이스를 생성 해 보겠습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 **모델에서 데이터베이스 생성 중...**
-   클릭 **새 연결...** LocalDB 또는 SQL Express를 사용 중인 Visual Studio 버전을 지정 하 고 입력 **ModelFirst.Blogging** 데이터베이스 이름으로 합니다.

    ![LocalDBConnectionMF](~/ef6/media/localdbconnectionmf.png)

    ![SqlExpressConnectionMF](~/ef6/media/sqlexpressconnectionmf.png)

-   선택 **확인** 를 묻는 새 데이터베이스를 만들려는 경우 **예**
-   선택 **다음** 및 Entity Framework 디자이너는 데이터베이스 스키마를 만드는 스크립트를 계산 합니다.
-   스크립트 표시 되 면 클릭 **완료** 및 스크립트 프로젝트에 추가 되며 열
-   선택한 스크립트를 마우스 오른쪽 단추로 클릭 **Execute**LocalDB 지정 연결할 데이터베이스를 지정 하 라는 메시지가 표시 됩니다, SQL Server Express를 사용 하는 Visual Studio의 버전에 따라

## <a name="4-reading--writing-data"></a>4. 읽기 및 데이터 쓰기

이제 모델이 준비 일부 데이터 액세스를 사용 하는 시간입니다. 클래스를 예정 하는 데 데이터에 액세스 되 고 자동으로 생성 된 EDMX 파일에 따라 합니다.

*이 스크린 샷에서 Visual Studio 2012, Visual Studio 2010을 사용 하는 경우는 BloggingModel.tt 이며 BloggingModel.Context.tt 파일 프로젝트 아래에 직접 EDMX 파일 아래에 중첩 하지 않고입니다.*

![GeneratedClasses](~/ef6/media/generatedclasses.png)

아래 표시 된 것과 같이 Program.cs의 Main 메서드를 구현 합니다. 이 코드는이 컨텍스트의 새 인스턴스를 만듭니다 및 다음 새 블로그 삽입을 사용 하 여 합니다. 다음 LINQ 쿼리를 사용 하 여 제목으로 사전순으로 정렬 하는 데이터베이스에서 모든 블로그를 검색 하려면.

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

```
Enter a name for a new Blog: ADO.NET Blog
All blogs in the database:
ADO.NET Blog
Press any key to exit...
```

## <a name="5-dealing-with-model-changes"></a>5. 모델 변경 처리

이제 데이터베이스 스키마를 업데이트 해야 하는 이러한 변경을 수행 하는 경우 모델을를 일부 변경 하는 시간입니다.

모델에 새 사용자 엔터티를 추가 하 여 시작 하겠습니다.

-   새 **사용자** 엔터티 이름의 **Username** 키 이름으로 및 **문자열** 키에 대 한 속성 형식으로

    ![AddUserEntity](~/ef6/media/adduserentity.png)

-   마우스 오른쪽 단추로 클릭는 **사용자 이름** 디자인 화면에 속성 **속성**에서 속성 창 변경 합니다 **MaxLength** 로 설정 **50 ** 
     *50 자로 사용자 이름을 저장할 수 있는 데이터 제한*
-   추가 된 **DisplayName** 스칼라 속성을 합니다 **사용자** 엔터티

이제 업데이트 된 모델을 하 고이 새 사용자 엔터티 형식에 맞게 데이터베이스를 업데이트할 준비가 완료 됩니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 **모델에서 데이터베이스 생성 중...** , Entity Framework에는 스크립트를 업데이트 된 모델을 기반으로 하는 스키마를 다시 계산 됩니다.
-   클릭 **마침**
-   기존 DDL 스크립트 및 모델의 매핑 및 저장소 부분을 덮어쓰기에 대 한 경고를 수신, 클릭 수 있습니다 **예** 모두 이러한 경고에 대 한
-   데이터베이스를 만드는 업데이트 된 SQL 스크립트를 열  
    *생성 되는 스크립트를 기존 테이블을 모두 삭제 하 고 스키마를 처음부터 다시 합니다. 이 로컬 개발에 대해 작동할 수 있지만 이미 배포 된 데이터베이스에 변경 내용을 푸시하기 위한 실행 가능한 아닙니다. 이미 배포 된 데이터베이스에 변경 내용을 게시 해야 할 경우 스크립트를 편집 하거나 스키마 비교 도구를 사용 하 여 마이그레이션 스크립트를 계산 해야 합니다.*
-   선택한 스크립트를 마우스 오른쪽 단추로 클릭 **Execute**LocalDB 지정 연결할 데이터베이스를 지정 하 라는 메시지가 표시 됩니다, SQL Server Express를 사용 하는 Visual Studio의 버전에 따라

## <a name="summary"></a>요약

이 연습의 첫 번째 모델 개발에 살펴보았습니다는 수 있었습니다 EF 디자이너에서 모델을 만들고 해당 모델에서 데이터베이스를 생성 합니다. 그런 다음 데이터베이스에서 일부 데이터를 쓰고 읽기 모델을 사용 합니다. 마지막으로 모델을 업데이트 하 고 모델과 일치 하도록 데이터베이스 스키마를 다시 생성 합니다.
