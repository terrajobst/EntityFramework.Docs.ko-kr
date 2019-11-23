---
title: Model First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: e1b9c319-bb8a-4417-ac94-7890f257e7f6
ms.openlocfilehash: 1b37805beb3d33f0b6dad2577a8abb3ea8f7b1e4
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182436"
---
# <a name="model-first"></a>Model First
이 비디오 및 단계별 연습은 Entity Framework를 사용 하 여 Model First 개발에 대 한 소개를 제공 합니다. Model First를 사용 하면 Entity Framework Designer를 사용 하 여 새 모델을 만든 다음 모델에서 데이터베이스 스키마를 생성할 수 있습니다. 모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다. 응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.

## <a name="watch-the-video"></a>비디오 시청
이 비디오 및 단계별 연습은 Entity Framework를 사용 하 여 Model First 개발에 대 한 소개를 제공 합니다. Model First를 사용 하면 Entity Framework Designer를 사용 하 여 새 모델을 만든 다음 모델에서 데이터베이스 스키마를 생성할 수 있습니다. 모델은 EDMX 파일 (.edmx 확장명)에 저장 되며 Entity Framework Designer에서 보고 편집할 수 있습니다. 응용 프로그램에서 상호 작용 하는 클래스가 EDMX 파일에서 자동으로 생성 됩니다.

**작성자**: [Rowan Miller](https://romiller.com/)

**비디오**: [wmv](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.wmv) | [MP4](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-mp4video-modelfirst.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/5/B/1/5B1C338C-AFA7-4F68-B304-48BB008146EF/HDI-ITPro-MSDN-winvideo-modelfirst.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2010 또는 Visual Studio 2012이 설치 되어 있어야 합니다.

Visual Studio 2010을 사용 하는 경우 [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 도 설치 해야 합니다.

## <a name="1-create-the-application"></a>1. 응용 프로그램 만들기

간단 하 게 유지 하기 위해 Model First를 사용 하 여 데이터 액세스를 수행 하는 기본 콘솔 응용 프로그램을 빌드 하겠습니다.

-   Visual Studio를 엽니다.
-   **파일&gt; 새&gt; 프로젝트 ...**
-   왼쪽 메뉴 및 **콘솔 응용 프로그램** 에서 **Windows** 를 선택 합니다.
-   **Modelfirstsample** 을 이름으로 입력 합니다.
-   **확인**을 선택합니다.

## <a name="2-create-model"></a>2. 모델 만들기

Visual Studio의 일부로 포함 된 Entity Framework Designer를 사용 하 여 모델을 만들 예정입니다.

-   **프로젝트-새 항목 추가&gt; ...**
-   왼쪽 메뉴에서 **데이터** 를 선택 하 고 **ADO.NET** 를 선택 엔터티 데이터 모델
-   이름으로 **BloggingModel** 를 입력 하 고 **확인**을 클릭 하면 엔터티 데이터 모델 마법사가 시작 됩니다.
-   **빈 모델** 을 선택 하 고 **마침** 을 클릭 합니다.

    ![빈 모델 만들기](~/ef6/media/createemptymodel.png)

빈 모델을 사용 하 여 Entity Framework Designer를 엽니다. 이제 모델에 엔터티, 속성 및 연결을 추가 하기 시작할 수 있습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **속성** 을 선택 합니다.
-   속성 창 **엔터티 컨테이너 이름을** **BloggingContext**
    변경 합니다. *이 이름은 사용자를 위해 생성 되는 파생 컨텍스트의 이름이 며, 컨텍스트는 데이터베이스와의 세션을 나타내며,이를 통해 데이터를 쿼리하고 저장할 수 있습니다* .
-   디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **추가 새-&gt; 엔터티 ...** 를 선택 합니다.
-   **블로그** 를 엔터티 이름으로 입력 하 고 키 이름으로 **BlogId** 를 입력 하 고 **확인을** 클릭 합니다.

    ![블로그 엔터티 추가](~/ef6/media/addblogentity.png)

-   디자인 화면에서 새 엔터티를 마우스 오른쪽 단추로 클릭 하 고 **추가 새-&gt; 스칼라 속성**을 선택 하 고 **이름** 을 속성 이름으로 입력 합니다.
-   **Url** 속성을 추가 하려면이 프로세스를 반복 합니다.
-   디자인 화면에서 **url** 속성을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 하 속성 창 **Nullable** 설정을 **True** 로 변경
    합니다. *그러면 url을 할당 하지 않고 데이터베이스에 블로그를 저장할 수 있습니다* .
-   방금 학습 된 기법을 사용 하 여 **PostId** key 속성이 있는 **Post** 엔터티를 추가 합니다.
-   **게시** 엔터티에 **제목** 및 **내용** 스칼라 속성 추가

이제 두 개의 엔터티가 있으므로 두 엔터티 간에 연결 (또는 관계)을 추가 합니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **추가 새-&gt; 연결** ...을 선택 합니다.
-   복합성이 **하나** 이 고 다른 끝점을 사용 하 **여 복합성이** **많은**
    에 대 한 관계 지점의 한쪽 **끝을 만듭니다** . 즉, *블로그의 게시물이 많고 게시물 하나가 블로그 하나에 속합니다* .
-   **' Post ' 엔터티 상자에 외래 키 속성 추가** 가 선택 되어 있는지 확인 하 고 **확인** 을 클릭 합니다.

    ![연결 MF 추가](~/ef6/media/addassociationmf.png)

이제에서 데이터베이스를 생성 하 고 데이터를 읽고 쓰는 데 사용할 수 있는 간단한 모델을 만들었습니다.

![모델 초기](~/ef6/media/modelinitial.png)

### <a name="additional-steps-in-visual-studio-2010"></a>Visual Studio 2010의 추가 단계

Visual Studio 2010에서 작업 하는 경우 Entity Framework 최신 버전으로 업그레이드 하기 위해 몇 가지 추가 단계를 수행 해야 합니다. 업그레이드는 향상 된 API 화면에 대 한 액세스를 제공 하 고, 사용 하기 쉽고, 최신 버그 수정 기능을 제공 하기 때문에 중요 합니다.

먼저 NuGet에서 최신 버전의 Entity Framework을 가져와야 합니다.

-   **프로젝트 – NuGet 패키지를 관리&gt;** ...
    * **Nuget 패키지 관리 ...** 옵션이 없는 경우 [최신 버전의 nuget](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) 을 설치 해야* 합니다.
-   **온라인** 탭을 선택 합니다.
-   **Entityframework** 패키지를 선택 합니다.
-   **설치** 클릭

다음으로 Entity Framework의 이후 버전에 도입 된 DbContext API를 사용 하는 코드를 생성 하기 위해 모델을 교환 해야 합니다.

-   EF 디자이너에서 모델의 빈 지점을 마우스 오른쪽 단추로 클릭 하 고 **코드 생성 항목 추가** ...를 선택 합니다.
-   왼쪽 메뉴에서 **온라인 템플릿** 을 선택 하 고 **DbContext** 를 검색 합니다.
-   **C\#에 대해 EF 5.X DbContext 생성기** 를 선택 하 고 이름으로 **BloggingModel** 를 입력 한 다음 **추가** 를 클릭 합니다.

    ![DbContext 템플릿](~/ef6/media/dbcontexttemplate.png)

## <a name="3-generating-the-database"></a>3. 데이터베이스 생성

모델을 통해 모델을 사용 하 여 데이터를 저장 하 고 검색 하는 데 사용할 수 있는 데이터베이스 스키마를 계산할 수 Entity Framework.

Visual Studio와 함께 설치 되는 데이터베이스 서버는 설치한 Visual Studio 버전에 따라 다릅니다.

-   Visual Studio 2010을 사용 하는 경우 SQL Express 데이터베이스를 만듭니다.
-   Visual Studio 2012을 사용 하는 경우 [LocalDB](https://msdn.microsoft.com/library/hh510202(v=sql.110).aspx) 데이터베이스를 만듭니다.

계속 해 서 데이터베이스를 생성 해 보겠습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성** ...을 선택 합니다.
-   **새 연결** ...을 클릭 합니다. 사용 중인 Visual Studio 버전에 따라 LocalDB 또는 SQL Express를 지정 하 고 데이터베이스 이름으로 **Modelfirst** 를 입력 합니다.

    ![LocalDB 연결 MF](~/ef6/media/localdbconnectionmf.png)

    ![Sql Express 연결 MF](~/ef6/media/sqlexpressconnectionmf.png)

-   **확인** 을 선택 하 고 새 데이터베이스를 만들지 여부를 묻는 메시지가 표시 되 면 **예** 를 선택 합니다.
-   **다음** 을 선택 하면 Entity Framework Designer는 데이터베이스 스키마를 만드는 스크립트를 계산 합니다.
-   스크립트가 표시 되 면 **마침** 을 클릭 하면 스크립트가 프로젝트에 추가 되 고 열립니다.
-   스크립트를 마우스 오른쪽 단추로 클릭 하 고 **실행**을 선택 합니다. 사용 중인 Visual Studio 버전에 따라 연결할 데이터베이스를 지정 하 고 LocalDB 또는 SQL Server Express를 지정 하 라는 메시지가 표시 됩니다.

## <a name="4-reading--writing-data"></a>4. 데이터 쓰기 & 읽기

이제 모델을 사용 하 여 일부 데이터에 액세스 하는 데 사용할 수 있습니다. 데이터에 액세스 하는 데 사용할 클래스는 EDMX 파일을 기반으로 자동으로 생성 됩니다.

*이 스크린샷은 visual Studio 2012에서 가져온 것입니다. Visual Studio 2010를 사용 하는 경우 BloggingModel.tt 및 BloggingModel.Context.tt 파일은 EDMX 파일 아래에 중첩 되지 않고 직접 프로젝트 바로 아래에 있습니다.*

![생성 된 클래스](~/ef6/media/generatedclasses.png)

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

## <a name="5-dealing-with-model-changes"></a>5. 모델 변경 처리

이제 모델을 변경할 때 이러한 변경 작업을 수행 하면 데이터베이스 스키마도 업데이트 해야 합니다.

모델에 새 사용자 엔터티를 추가 하는 것부터 시작 합니다.

-   키 이름 및 **문자열** 을 키 이름 **으로 사용 하 여 새** **사용자** 엔터티 이름을 키에 대 한 속성 형식으로 추가 합니다.

    ![사용자 엔터티 추가](~/ef6/media/adduserentity.png)

-   디자인 화면에서 **username** 속성을 마우스 오른쪽 단추로 클릭 하 고 **속성**을 선택 하 속성 창 **MaxLength** 설정을 **50**
    하 여 *사용자 이름에 저장할 수 있는 데이터를 50 자로 제한* 합니다.
-   **사용자** 엔터티에 **DisplayName** 스칼라 속성 추가

이제 업데이트 된 모델이 있으며 새 사용자 엔터티 유형을 수용 하기 위해 데이터베이스를 업데이트할 준비가 되었습니다.

-   디자인 화면을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성**...을 선택 하 여 업데이트 된 모델을 기반으로 스키마를 다시 만드는 스크립트를 계산 Entity Framework 합니다.
-   **마침** 클릭
-   기존 DDL 스크립트와 모델의 매핑 및 저장소 부분을 덮어쓰는 방법에 대 한 경고 메시지가 표시 될 수 있습니다. 이러한 경고에 대해 모두 **예** 를 클릭 합니다.
-   데이터베이스를 만들기 위해 업데이트 된 SQL 스크립트가 열립니다.  
    *생성 된 스크립트는 모든 기존 테이블을 삭제 한 다음 스키마를 처음부터 다시 만듭니다. 로컬 개발에는 적용 되지만 이미 배포 된 데이터베이스에 변경 내용을 푸시할 수는 없습니다. 이미 배포 된 데이터베이스에 변경 내용을 게시 해야 하는 경우에는 스크립트를 편집 하거나 스키마 비교 도구를 사용 하 여 마이그레이션 스크립트를 계산 해야 합니다.*
-   스크립트를 마우스 오른쪽 단추로 클릭 하 고 **실행**을 선택 합니다. 사용 중인 Visual Studio 버전에 따라 연결할 데이터베이스를 지정 하 고 LocalDB 또는 SQL Server Express를 지정 하 라는 메시지가 표시 됩니다.

## <a name="summary"></a>요약

이 연습에서는 EF Designer에서 모델을 만든 다음 해당 모델에서 데이터베이스를 생성할 수 있도록 하는 Model First 개발에 대해 살펴보았습니다. 그런 다음 모델을 사용 하 여 데이터베이스에서 일부 데이터를 읽고 씁니다. 마지막으로 모델을 업데이트 한 다음 모델과 일치 하도록 데이터베이스 스키마를 다시 만들었습니다.
