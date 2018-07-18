---
title: 공간-EF 디자이너-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 06baa6e1-d680-4a95-845b-81305c87a962
caps.latest.revision: 3
ms.openlocfilehash: 2332818275763fd90274f426b7fced4c3c14ac17
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122554"
---
# <a name="spatial---ef-designer"></a>공간-EF 디자이너
> [!NOTE]
> **EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

비디오 및 단계별 연습에는 Entity Framework 디자이너를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다. 또한 LINQ 쿼리를 사용 하 여 두 위치 사이의 거리를 찾으려고 하는 방법을 보여 줍니다.

이 연습에서는 새 데이터베이스를 만들려면 Model First 사용 됩니다 있지만 EF 디자이너를 사용 하 여 사용할 수도 있습니다는 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 기존 데이터베이스에 매핑할 워크플로.

공간 형식 지원 Entity Framework 5에서 도입 되었습니다. 공간 형식, 열거형 및 테이블 반환 함수 같은 새 기능을 사용 하려면 해야 대상 지정 하는.NET Framework 4.5를 참고 합니다. Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.

공간 데이터 형식을 사용 하 여 공간 지원에는 Entity Framework 공급자를 사용 해야 있습니다. 참조 [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md) 자세한 내용은 합니다.

기본 공간 데이터 유형은 두 가지가: geography 및 geometry 합니다. 타원 데이터를 저장 하는 지리 데이터 형식 (예를 들어, GPS 위도 및 경도 좌표)입니다. 기 하 도형 데이터 형식은 유클리드 (평면) 좌표계를 나타냅니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오에는 Entity Framework 디자이너를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다. 또한 LINQ 쿼리를 사용 하 여 두 위치 사이의 거리를 찾으려고 하는 방법을 보여 줍니다.

**제공한**: Julia Kornich

**비디오**: [WMV](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.wmv) | [MP4](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-mp4video-spatialwithdesigner.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/E/C/9/EC9E6547-8983-4C1F-A919-D33210E4B213/HDI-ITPro-MSDN-winvideo-spatialwithdesigner.zip)

## <a name="pre-requisites"></a>필수 조건

Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio 2012 열기
2.  에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**
3.  왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿
4.  입력 **SpatialEFDesigner** 고 프로젝트의 이름으로 **확인**

## <a name="create-a-new-model-using-the-ef-designer"></a>EF 디자이너를 사용 하 여 새 모델 만들기

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**
2.  선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서
3.  입력 **UniversityModel.edmx** 클릭 한 다음 확인 하 고 파일 이름에 대 한 **추가**
4.  엔터티 데이터 모델 마법사 페이지에서 선택 **빈 모델** Model 콘텐츠 선택 대화 상자에서
5.  클릭 **마침**

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.

마법사에서는 다음 작업을 수행합니다.

-   개념적 모델, 저장소 모델 및 이들 간의 매핑을 정의 하는 EnumTestModel.edmx 파일이 생성 됩니다. 생성 된 메타 데이터 파일을 어셈블리에 포함 되므로 출력 어셈블리에 포함 하려면.edmx 파일의 메타 데이터 아티팩트 처리 속성을 설정 합니다.
-   다음 어셈블리에 대 한 참조 추가: EntityFramework 고 System.ComponentModel.DataAnnotations, System.Data.Entity 합니다.
-   UniversityModel.tt 파일과 UniversityModel.Context.tt 만들고.edmx 파일에 추가 합니다. T4 템플릿 파일에 이러한.edmx 모델의 엔터티에 매핑되는 POCO 형식과 파생 된 DbContext 형식을 정의 하는 코드를 생성 합니다.

## <a name="add-a-new-entity-type"></a>새 엔터티 형식을 추가 합니다.

1.  디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 **추가-&gt; 엔터티**, 새 Entity 대화 상자 표시
2.  지정할 **University** 유형에 대 한 이름을 지정 하 고 지정 **UniversityID** 형식으로 유지 키 속성 이름에 대 한 **Int32**
3.  **확인**을 클릭합니다.
4.  엔터티를 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 스칼라 속성**
5.  새 속성 이름 바꾸기 **이름**
6.  다른 스칼라 속성을 추가 하 고 이름을 **위치** 속성 창을 열고 새 속성의 형식을 변경 **Geography**
7.  모델을 저장 하 고 프로젝트를 빌드하십시오
    > [!NOTE]
    > 를 빌드할 때 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다. 오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 했습니다 때문에 이러한 경고를 무시할 수 있습니다.

## <a name="generate-database-from-model"></a>모델에서 데이터베이스 생성

이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.

1.  Entity Designer 화면에서 빈 공간을 마우스 오른쪽 단추로 클릭 **모델에서 데이터베이스 생성**
2.  데이터베이스 생성 마법사의 데이터 연결 대화 상자는 선택 표시를 클릭 합니다 **새 연결** 지정 단추 **(localdb)\\mssqllocaldb** 고서버이름에대한 **대학** 데이터베이스 및 클릭 **확인**
3.  새 데이터베이스를 만들 것인지 묻는 대화 상자가 팝업을 누릅니다 **예**합니다.
4.  클릭 **다음** 데이터베이스 만들기 마법사의 요약 및 설정 대화 상자 참고 DDL에 대 한 정의 포함 하지 않는에 표시 되는 데이터베이스를 만드는 중 생성된 된 DDL에 대 한 DDL (데이터 정의 언어)을 생성 하 고는 열거형 형식에 매핑되는 테이블
5.  클릭 **완료** 마침을 클릭 하면 DDL 스크립트 실행 되지 않습니다.
6.  다음을 수행 하는 데이터베이스 만들기 마법사: 엽니다는 **UniversityModel.edmx.sql** 추가 연결 문자열 정보를 App.config 파일에는 EDMX의 저장소 스키마 및 매핑 섹션을 생성 하는 T-SQL 편집기에서에서 파일
7.  T-SQL 편집기에서 마우스 오른쪽 단추를 클릭 하 고 선택 **Execute** 나타나면 서버 대화 상자에는 연결 2 단계의 연결 정보를 입력 하 고 클릭 **연결**
8.  생성된 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**

## <a name="persist-and-retrieve-data"></a>유지 하 고 데이터를 검색 합니다.

Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다. 다음 코드를 Main 함수에 추가 합니다.

두 가지 새로운 University 개체를 컨텍스트에 추가 하는 코드입니다. 공간 속성 DbGeography.FromText 메서드를 사용 하 여 초기화 됩니다. 지리 지점 나타낸 WellKnownText 메서드에 전달 됩니다. 다음 코드는 데이터를 저장합니다. 그런 다음는 LINQ 쿼리 해당 위치가 지정된 된 위치에 가장 가까운 University 개체를 반환 하는 생성 되 고 실행 합니다.

``` csharp
using (var context = new UniversityModelContainer())
{
    context.Universities.Add(new University()
    {
        Name = "Graphic Design Institute",
        Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
    });

    context.Universities.Add(new University()
    {
        Name = "School of Fine Art",
        Location = DbGeography.FromText("POINT(-122.335197 47.646711)"),
    });

    context.SaveChanges();

    var myLocation = DbGeography.FromText("POINT(-122.296623 47.640405)");

    var university = (from u in context.Universities
                                orderby u.Location.Distance(myLocation)
                                select u).FirstOrDefault();

    Console.WriteLine(
        "The closest University to you is: {0}.",
        university.Name);
}
```

응용 프로그램을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```
The closest University to you is: School of Fine Art.
```

데이터베이스에서 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**합니다. 그런 다음 선택한 테이블에서 마우스 오른쪽 단추를 클릭 **데이터 보기**합니다.

## <a name="summary"></a>요약

이 연습에서는 Entity Framework 디자이너를 사용 하 여 공간 형식을 매핑하는 방법 및 코드에서 공간 형식을 사용 하는 방법을 살펴보았습니다. 
