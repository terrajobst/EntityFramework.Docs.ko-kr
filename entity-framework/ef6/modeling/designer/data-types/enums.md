---
title: 열거형 지원-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 92a763b84a04d3ce7ec0853ef2a4852356cf7997
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182515"
---
# <a name="enum-support---ef-designer"></a>열거형 지원-EF 디자이너
> [!NOTE]
> **EF5** 이상-이 페이지에서 설명 하는 기능, api 등은 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

이 비디오 및 단계별 연습에서는 Entity Framework Designer에서 열거형 형식을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.

이 연습에서는 Model First 사용 하 여 새 데이터베이스를 만들지만 EF Designer를 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 워크플로와 함께 사용 하 여 기존 데이터베이스에 매핑할 수도 있습니다.

열거형 지원은 Entity Framework 5에서 도입 되었습니다. 열거형, 공간 데이터 형식 및 테이블 반환 함수와 같은 새로운 기능을 사용 하려면 .NET Framework 4.5를 대상으로 해야 합니다. Visual Studio 2012는 기본적으로 .NET 4.5를 대상으로 합니다.

Entity Framework에서 열거형은 **Byte**, **Int16**, **Int32**, **Int64** 또는 **SByte**의 기본 형식을 가질 수 있습니다.

## <a name="watch-the-video"></a>비디오 시청
이 비디오는 Entity Framework Designer에서 열거형 형식을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법도 보여 줍니다.

제공한 **사람**: 줄리아 Kornich

**비디오**: [wmv](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [wmv (ZIP)](https://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>필수 구성 요소

이 연습을 완료 하려면 Visual Studio 2012, Ultimate, Premium, Professional 또는 Web Express edition을 설치 해야 합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio 2012 열기
2.  **파일** 메뉴에서 **새로 만들기**를 가리킨 다음 **프로젝트** 를 클릭 합니다.
3.  왼쪽 창에서 **Visual C\#** 을 클릭 한 다음 **콘솔** 템플릿을 선택 합니다.
4.  프로젝트 이름으로 **Enumefdesigner** 를 입력 하 고 **확인** 을 클릭 합니다.

## <a name="create-a-new-model-using-the-ef-designer"></a>EF Designer를 사용 하 여 새 모델 만들기

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 단추로 클릭 하 고 **추가**를 가리킨 다음 **새 항목** 을 클릭 합니다.
2.  왼쪽 메뉴에서 **데이터** 를 선택 하 고 템플릿 창에서 **ADO.NET 엔터티 데이터 모델** 를 선택 합니다.
3.  파일 이름으로 **Enumtestmodel .edmx** 를 입력 한 다음 **추가** 를 클릭 합니다.
4.  엔터티 데이터 모델 마법사 페이지의 Model 콘텐츠 선택 대화 상자에서 **빈 모델** 을 선택 합니다.
5.  **마침** 클릭

모델 편집을 위한 디자인 화면을 제공 하는 Entity Designer 표시 됩니다.

마법사에서는 다음 작업을 수행합니다.

-   개념적 모델, 저장소 모델 및 두 모델 간의 매핑을 정의 하는 EnumTestModel .edmx 파일을 생성 합니다. 생성 된 메타 데이터 파일이 어셈블리에 포함 되도록 .edmx 파일의 메타 데이터 아티팩트 처리 속성을 출력 어셈블리에 포함 하도록 설정 합니다.
-   EntityFramework, System.componentmodel 및 System.object 어셈블리에 대 한 참조를 추가 합니다.
-   EnumTestModel.tt 및 EnumTestModel.Context.tt 파일을 만들고 .edmx 파일 아래에 추가 합니다. 이러한 T4 템플릿 파일은 .edmx 모델의 엔터티에 매핑되는 DbContext 파생 형식 및 POCO 형식을 정의 하는 코드를 생성 합니다.

## <a name="add-a-new-entity-type"></a>새 엔터티 형식 추가

1.  디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 하 고 **추가-&gt; 엔터티**를 선택 합니다. 새 엔터티 대화 상자가 나타납니다.
2.  유형 이름에 대해 **학과** 를 지정 하 고 키 속성 이름에 **DepartmentID** 를 지정 하 고 형식을 **Int32** 로 그대로 둡니다.
3.  **확인**을 클릭합니다.
4.  엔터티를 마우스 오른쪽 단추로 클릭 하 고 **추가 새로 만들기-&gt; 스칼라 속성** 을 선택 합니다.
5.  새 속성의 이름을 **Name** 으로 바꿉니다.
6.  새 속성의 형식을 **int32** (기본적으로 새 속성은 문자열 형식)로 변경 하 여 형식을 변경 하 고 속성 창를 연 다음 형식 속성을 **int32** 로 변경 합니다.
7.  다른 스칼라 속성을 추가 하 고 **예산**으로 이름을 변경 하 고, 형식을 **10 진수로** 변경 합니다.

## <a name="add-an-enum-type"></a>열거형 형식 추가

1.  Entity Framework Designer에서 Name 속성을 마우스 오른쪽 단추로 클릭 하 고 **열거형으로 변환** 을 선택 합니다.

    ![열거형으로 변환](~/ef6/media/converttoenum.png)

2.  **열거 추가** 대화 상자에서 Enum 형식 이름에 **DepartmentNames** 를 입력 하 고, 기본 형식을 **Int32**로 변경한 후 형식에 영어, 수학 및 경제를 추가 합니다.

    ![열거형 형식 추가](~/ef6/media/addenumtype.png)

3.  **확인을** 누릅니다.
4.  모델을 저장 하 고 프로젝트를 빌드합니다.
    > [!NOTE]
    > 빌드하면 매핑되지 않은 엔터티와 연결에 대 한 경고가 오류 목록 표시 될 수 있습니다. 모델에서 데이터베이스를 생성 하는 것을 선택한 후 오류가 사라집니다. 이러한 경고는 무시 해도 됩니다.

속성 창를 살펴보면 이름 속성의 형식이 **DepartmentNames** 로 변경 되 고 새로 추가 된 열거형 형식이 형식 목록에 추가 된 것을 알 수 있습니다.

모델 브라우저 창으로 전환 하면 형식이 열거형 형식 노드에도 추가 된 것을 확인할 수 있습니다.

![모델 브라우저](~/ef6/media/modelbrowser.png)

>[!NOTE]
> 마우스 오른쪽 단추를 클릭 하 고 **열거형 형식 추가**를 선택 하 여이 창에서 새 열거형 형식을 추가할 수도 있습니다. 형식이 생성 되 면 형식 목록에 표시 되 고 속성에 연결할 수 있습니다.

## <a name="generate-database-from-model"></a>모델에서 데이터베이스 생성

이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.

1.  Entity Designer 화면의 빈 공간을 마우스 오른쪽 단추로 클릭 하 고 **모델에서 데이터베이스 생성** 을 선택 합니다.
2.  데이터베이스 생성 마법사의 데이터 연결 선택 대화 상자가 표시 됩니다. **새 연결** 단추 지정 **(localdb)\\** 데이터베이스에 대 한 서버 이름 및 **Enumtest** 에 대해 mssqllocaldb를 클릭 하 고 **확인** 을 클릭 합니다.
3.  새 데이터베이스를 만들지 여부를 묻는 대화 상자가 표시 되 면 **예**를 클릭 합니다.
4.  **다음** 을 클릭 하 고 데이터베이스 만들기 마법사에서 데이터베이스를 만들기 위한 ddl (데이터 정의 언어)을 생성 합니다. 생성 된 Ddl은 요약 및 설정 대화 상자에 표시 되며, 해당 ddl에는 열거형 형식에 매핑되는 테이블에 대 한 정의가 포함 되지 않습니다.
5.  마침 **을 클릭 하면 DDL** 스크립트가 실행 되지 않습니다 .를 클릭 합니다.
6.  데이터베이스 만들기 마법사는 다음 작업을 수행 합니다. T-sql 편집기에서 **enumtest** 를 엽니다. .edmx 파일의 저장소 스키마 및 매핑 섹션은 app.config 파일에 연결 문자열 정보를 추가 합니다.
7.  T-sql 편집기에서 마우스 오른쪽 단추를 클릭 하 고 서버에 연결 대화 상자가 나타납니다 .를 선택 하 고 2 단계의 연결 정보를 입력 한 다음 **연결** 을 **클릭 합니다.**
8.  생성 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 고침** 을 선택 합니다.

## <a name="persist-and-retrieve-data"></a>데이터 유지 및 검색

Main 메서드가 정의 된 Program.cs 파일을 엽니다. Main 함수에 다음 코드를 추가 합니다. 이 코드는 컨텍스트에 새 부서 개체를 추가 합니다. 그런 다음 데이터를 저장 합니다. 또한이 코드는 이름이 DepartmentNames 인 부서를 반환 하는 LINQ 쿼리를 실행 합니다.

``` csharp
using (var context = new EnumTestModelContainer())
{
    context.Departments.Add(new Department{ Name = DepartmentNames.English });

    context.SaveChanges();

    var department = (from d in context.Departments
                        where d.Name == DepartmentNames.English
                        select d).FirstOrDefault();

    Console.WriteLine(
        "DepartmentID: {0} and Name: {1}",
        department.DepartmentID,  
        department.Name);
}
```

애플리케이션을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```console
DepartmentID: 1 Name: English
```

데이터베이스의 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 **새로 고침**을 선택 합니다. 그런 다음 테이블에서 마우스 오른쪽 단추를 클릭 하 고 **데이터 보기**를 선택 합니다.

## <a name="summary"></a>요약

이 연습에서는 Entity Framework Designer를 사용 하 여 열거형 형식을 매핑하는 방법 및 코드에서 열거형을 사용 하는 방법을 살펴보았습니다. 
