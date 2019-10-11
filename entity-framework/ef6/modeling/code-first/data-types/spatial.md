---
title: 공간-Code First-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d617aed1-15f2-48a9-b187-186991c666e3
ms.openlocfilehash: 018f480c1f0f1e74fc9f7a8950a6880e96f1facc
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182658"
---
# <a name="spatial---code-first"></a>공간-Code First
> [!NOTE]
> **EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

비디오 및 단계별 연습에서는 Entity Framework Code First를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다. LINQ 쿼리를 사용 하 여 두 위치 간의 거리를 찾는 방법도 보여 줍니다.

이 연습에서는 Code First 사용 하 여 새 데이터베이스를 만들지만 [기존 데이터베이스에 Code First를](~/ef6/modeling/code-first/workflows/existing-database.md)사용할 수도 있습니다.

공간 형식 지원은 Entity Framework 5에서 도입 되었습니다. 공간 형식, 열거형 및 테이블 반환 함수와 같은 새 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다. Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.

공간 데이터 형식을 사용 하려면 공간이 지원 되는 Entity Framework 공급자도 사용 해야 합니다. 자세한 내용은 [공간 형식에 대 한 공급자 지원](~/ef6/fundamentals/providers/spatial-support.md) 을 참조 하세요.

두 가지 주요 공간 데이터 형식인 geography 및 geometry가 있습니다. Geography 데이터 형식은 타원 데이터 (예: GPS 위도 및 경도 좌표)를 저장 합니다. Geometry 데이터 형식은 유클리드 (평면) 좌표계를 나타냅니다.

## <a name="watch-the-video"></a>비디오 시청
이 비디오는 Entity Framework Code First를 사용 하 여 공간 형식을 매핑하는 방법을 보여 줍니다. LINQ 쿼리를 사용 하 여 두 위치 간의 거리를 찾는 방법도 보여 줍니다.

**제공**: 줄리아 Kornich

**비디오**: [WMV](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.wmv)@NO__T-[1MP4](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-mp4video-spatialwithcodefirst.m4v) | [WMV (ZIP)](https://download.microsoft.com/download/9/1/3/913EA17E-6F97-41D8-A4FE-805A0D83D26A/HDI-ITPro-MSDN-winvideo-spatialwithcodefirst.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio 2012 열기
2.  **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.
3.  왼쪽 창에서 **Visual C @ no__t-1**을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
4.  프로젝트 이름으로 **SpatialCodeFirst** 를 입력 하 고 **확인을** 클릭 합니다.

## <a name="define-a-new-model-using-code-first"></a>Code First를 사용 하 여 새 모델 정의

Code First 개발을 사용 하는 경우 일반적으로 개념 (도메인) 모델을 정의 하는 .NET Framework 클래스를 작성 하는 것으로 시작 합니다. 아래 코드는 대학 클래스를 정의 합니다.

대학에는 DbGeography 형식의 Location 속성이 있습니다. DbGeography 형식을 사용 하려면 system.xml 어셈블리에 참조를 추가 하 고 문을 사용 하 여 system.string을 추가 해야 합니다.

Program.cs 파일을 열고 다음 using 문을 파일 맨 위에 붙여넣습니다.

``` csharp
using System.Data.Spatial;
```

Program.cs 파일에 다음 대학 클래스 정의를 추가 합니다.

``` csharp
public class University  
{
    public int UniversityID { get; set; }
    public string Name { get; set; }
    public DbGeography Location { get; set; }
}
```

## <a name="define-the-dbcontext-derived-type"></a>DbContext 파생 형식 정의

엔터티를 정의 하는 것 외에도 DbContext에서 파생 되는 클래스를 정의 하 고 DbSet @ no__t-0D@ no__t-1 속성을 노출 해야 합니다. DbSet @ no__t-0D@ no__t 속성을 사용 하면 모델에 포함 하려는 유형을 컨텍스트에 알 수 있습니다.

DbContext 파생 형식의 인스턴스는 런타임 중에 엔터티 개체를 관리 합니다. 여기에는 데이터베이스의 데이터로 개체 채우기, 변경 내용 추적 및 데이터베이스에 데이터 유지가 포함 됩니다.

DbContext 및 DbSet 형식은 EntityFramework 어셈블리에서 정의 됩니다. EntityFramework NuGet 패키지를 사용 하 여이 DLL에 대 한 참조를 추가 합니다.

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 합니다.
2.  **NuGet 패키지 관리 ...** 를 선택 합니다.
3.  NuGet 패키지 관리 대화 상자에서 **온라인** 탭을 선택 하 고 **entityframework** 패키지를 선택 합니다.
4.  **설치** 클릭

EntityFramework 어셈블리 외에도 System.componentmodel 어셈블리에 대 한 참조도 추가 됩니다.

Program.cs 파일의 맨 위에 다음 using 문을 추가 합니다.

``` csharp
using System.Data.Entity;
```

Program.cs에서 컨텍스트 정의를 추가 합니다. 

``` csharp
public partial class UniversityContext : DbContext
{
    public DbSet<University> Universities { get; set; }
}
```

## <a name="persist-and-retrieve-data"></a>데이터 유지 및 검색

Main 메서드가 정의 된 Program.cs 파일을 엽니다. Main 함수에 다음 코드를 추가 합니다.

이 코드는 두 개의 새 대학 개체를 컨텍스트에 추가 합니다. 공간 속성은 DbGeography. FromText 메서드를 사용 하 여 초기화 됩니다. WellKnownText로 표시 된 geography point는 메서드에 전달 됩니다. 그런 다음 코드에서 데이터를 저장 합니다. 그런 다음 해당 위치가 지정 된 위치와 가장 가까운 대학 개체를 반환 하는 LINQ 쿼리가 생성 되 고 실행 됩니다.

``` csharp
using (var context = new UniversityContext ())
{
    context.Universities.Add(new University()
        {
            Name = "Graphic Design Institute",
            Location = DbGeography.FromText("POINT(-122.336106 47.605049)"),
        });

    context. Universities.Add(new University()
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

애플리케이션을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```console
The closest University to you is: School of Fine Art.
```

## <a name="view-the-generated-database"></a>생성 된 데이터베이스 보기

응용 프로그램을 처음 실행 하면 Entity Framework 데이터베이스를 만듭니다. Visual Studio 2012가 설치 되어 있으므로 데이터베이스가 LocalDB 인스턴스에 생성 됩니다. 기본적으로 Entity Framework은 파생 컨텍스트의 정규화 된 이름 (이 예제에서는 **SpatialCodeFirst UniversityContext**) 뒤에 데이터베이스 이름을 부여 합니다. 기존 데이터베이스가 사용 되는 이후 시간입니다.  

데이터베이스를 만든 후 모델을 변경 하는 경우 Code First 마이그레이션를 사용 하 여 데이터베이스 스키마를 업데이트 해야 합니다. 마이그레이션을 사용 하는 예제는 [새 데이터베이스에 대 한 Code First를](~/ef6/modeling/code-first/workflows/new-database.md) 참조 하세요.

데이터베이스 및 데이터를 보려면 다음을 수행 합니다.

1.  Visual Studio 2012 주 메뉴에서 **보기** - @ no__t **SQL Server 개체 탐색기**를 선택 합니다.
2.  LocalDB가 서버 목록에 없으면 **SQL Server** 에서 마우스 오른쪽 단추를 클릭 하 고 추가를 선택 **SQL Server** 기본 **Windows 인증** 을 사용 하 여 localdb 인스턴스에 연결 합니다.
3.  LocalDB 노드 확장
4.  **데이터베이스** 폴더를 펼침 하 여 새 데이터베이스를 확인 하 고 **대학** 테이블로 이동 합니다.
5.  데이터를 보려면 테이블을 마우스 오른쪽 단추로 클릭 하 고 **데이터 보기** 를 선택 합니다.

## <a name="summary"></a>요약

이 연습에서는 Entity Framework Code First에서 공간 형식을 사용 하는 방법을 살펴보았습니다. 
