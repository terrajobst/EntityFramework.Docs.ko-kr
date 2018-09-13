---
title: 열거형 지원-EF 디자이너-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: c6ae6d8f-1ace-47db-ad47-b1718f1ba082
ms.openlocfilehash: 4c2562ade28dc30df20eb2a35c179582bf6e0777
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490794"
---
# <a name="enum-support---ef-designer"></a>열거형 지원-EF 디자이너
> [!NOTE]
> **EF5 이상만** -Api 기능 등이이 페이지에 설명 된 Entity Framework 5에서 도입 되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

이 비디오 및 단계별 연습에서는 Entity Framework 디자이너를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.

이 연습에서는 새 데이터베이스를 만들려면 Model First 사용 됩니다 있지만 EF 디자이너를 사용 하 여 사용할 수도 있습니다는 [Database First](~/ef6/modeling/designer/workflows/database-first.md) 기존 데이터베이스에 매핑할 워크플로.

열거형 지원 Entity Framework 5에서 도입 되었습니다. 테이블 반환 함수, 열거형 및 공간 데이터 형식 등 새 기능을 사용 하려면.NET Framework 4.5 대상으로 해야 합니다. Visual Studio 2012는 기본적으로.NET 4.5를 대상으로 합니다.

Entity Framework의 열거형에는 다음 기본 형식이 있을 수 있습니다: **바이트**, **Int16**를 **Int32**를 **Int64** , 또는 **SByte**합니다.

## <a name="watch-the-video"></a>비디오를 시청 하세요.
이 비디오에는 Entity Framework 디자이너를 사용 하 여 열거형을 사용 하는 방법을 보여 줍니다. 또한 LINQ 쿼리에서 열거형을 사용 하는 방법을 보여 줍니다.

**제공한**: Julia Kornich

**비디오**: [WMV](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.wmv) | [MP4](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-mp4video-enumwithdesiger.m4v) | [WMV (ZIP)](http://download.microsoft.com/download/0/7/A/07ADECC9-7893-415D-9F20-8B97D46A37EC/HDI-ITPro-MSDN-winvideo-enumwithdesiger.zip)

## <a name="pre-requisites"></a>필수 조건

Visual Studio 2012 Ultimate, Premium, Professional, Web Express edition이 연습을 완료 하려면 설치 해야 합니다.

## <a name="set-up-the-project"></a>프로젝트 설정

1.  Visual Studio 2012 열기
2.  에 **파일** 메뉴에서 **새로 만들기**를 클릭 하 고 **프로젝트**
3.  왼쪽된 창에서 클릭 **Visual C\#** 를 선택한 후 합니다 **콘솔** 템플릿
4.  입력 **EnumEFDesigner** 고 프로젝트의 이름으로 **확인**

## <a name="create-a-new-model-using-the-ef-designer"></a>EF 디자이너를 사용 하 여 새 모델 만들기

1.  솔루션 탐색기에서 프로젝트 이름을 마우스 오른쪽 **추가**를 클릭 하 고 **새 항목**
2.  선택 **데이터** 선택 고 왼쪽된 메뉴에서 **ADO.NET Entity Data Model** 템플릿 창에서
3.  입력 **EnumTestModel.edmx** 클릭 한 다음 확인 하 고 파일 이름에 대 한 **추가**
4.  엔터티 데이터 모델 마법사 페이지에서 선택 **빈 모델** Model 콘텐츠 선택 대화 상자에서
5.  클릭 **마침**

모델 편집을 위해 디자인 화면을 제공 하는 Entity Designer에 표시 됩니다.

마법사에서는 다음 작업을 수행합니다.

-   개념적 모델, 저장소 모델 및 이들 간의 매핑을 정의 하는 EnumTestModel.edmx 파일이 생성 됩니다. 생성 된 메타 데이터 파일을 어셈블리에 포함 되므로 출력 어셈블리에 포함 하려면.edmx 파일의 메타 데이터 아티팩트 처리 속성을 설정 합니다.
-   다음 어셈블리에 대 한 참조 추가: EntityFramework 고 System.ComponentModel.DataAnnotations, System.Data.Entity 합니다.
-   EnumTestModel.tt 파일과 EnumTestModel.Context.tt 만들고.edmx 파일에 추가 합니다. T4 템플릿 파일에 이러한.edmx 모델의 엔터티에 매핑되는 POCO 형식과 파생 된 DbContext 형식을 정의 하는 코드를 생성 합니다.

## <a name="add-a-new-entity-type"></a>새 엔터티 형식을 추가 합니다.

1.  디자인 화면의 빈 영역을 마우스 오른쪽 단추로 클릭 **추가-&gt; 엔터티**, 새 Entity 대화 상자 표시
2.  지정할 **부서** 유형에 대 한 이름을 지정 하 고 지정 **DepartmentID** 형식으로 유지 키 속성 이름에 대 한 **Int32**
3.  **확인**을 클릭합니다.
4.  엔터티를 마우스 오른쪽 단추로 클릭 **새로 추가-&gt; 스칼라 속성**
5.  새 속성 이름 바꾸기 **이름**
6.  새 속성의 형식을 변경 **Int32** (기본적으로 새 속성은 문자열 형식의) 유형을 변경 하려면 속성 창을 열고 유형 속성을 변경 **Int32**
7.  다른 스칼라 속성을 추가 하 고 이름을 **예산**, 유형을 변경 하 여 **10 진수**

## <a name="add-an-enum-type"></a>열거형 형식 추가

1.  Entity Framework 디자이너에서 Name 속성을 단추로 **열거형으로 변환**

    ![열거형으로 변환](~/ef6/media/converttoenum.png)

2.  에 **추가 열거형** 대화 상자 유형을 **DepartmentNames** 열거형 형식 이름에 대 한 기본 유형을 변경 하 여 **Int32**, 다음 형식으로 다음 멤버를 추가 하 고: 영어 수치 연산 및 경제성

    ![열거형 형식 추가](~/ef6/media/addenumtype.png)

3.  키를 눌러 **확인**
4.  모델을 저장 하 고 프로젝트를 빌드하십시오
    > [!NOTE]
    > 를 빌드할 때 매핑되지 않은 엔터티 및 연결에 대 한 경고는 오류 목록에 나타날 수 있습니다. 오류 사라집니다을 모델에서 데이터베이스를 생성 하도록 선택 했습니다 때문에 이러한 경고를 무시할 수 있습니다.

속성 창을 보면 Name 속성의 형식에 변경 되었는지 알 수 있습니다 **DepartmentNames** 새로 추가 된 열거형 형식의 목록에 추가 되었습니다.

모델 브라우저 창으로 전환 하면 형식에서 열거형 형식 노드를 추가한 표시 메시지가 표시 됩니다.

![모델 브라우저](~/ef6/media/modelbrowser.png)

>[!NOTE]
> 또한 새 열거형이이 창에서 마우스 오른쪽 단추를 클릭 하 고 선택 하 여 추가할 수 있습니다 **열거형 형식 추가**합니다. 형식이 생성 되 면 형식 목록에 표시 됩니다 및 속성을 사용 하 여 연결할 수 있습니다.

## <a name="generate-database-from-model"></a>모델에서 데이터베이스 생성

이제 모델을 기반으로 하는 데이터베이스를 생성할 수 있습니다.

1.  Entity Designer 화면에서 빈 공간을 마우스 오른쪽 단추로 클릭 **모델에서 데이터베이스 생성**
2.  데이터베이스 생성 마법사의 데이터 연결 대화 상자는 선택 표시를 클릭 합니다 **새 연결** 지정 단추 **(localdb)\\mssqllocaldb** 고서버이름에대한 **EnumTest** 데이터베이스 및 클릭 **확인**
3.  새 데이터베이스를 만들 것인지 묻는 대화 상자가 팝업을 누릅니다 **예**합니다.
4.  클릭 **다음** 데이터베이스 만들기 마법사의 요약 및 설정 대화 상자 참고 DDL에 대 한 정의 포함 하지 않는에 표시 되는 데이터베이스를 만드는 중 생성된 된 DDL에 대 한 DDL (데이터 정의 언어)을 생성 하 고는 열거형 형식에 매핑되는 테이블
5.  클릭 **완료** 마침을 클릭 하면 DDL 스크립트 실행 되지 않습니다.
6.  다음을 수행 하는 데이터베이스 만들기 마법사: 엽니다는 **EnumTest.edmx.sql** 추가 연결 문자열 정보를 App.config 파일에는 EDMX의 저장소 스키마 및 매핑 섹션을 생성 하는 T-SQL 편집기에서에서 파일
7.  T-SQL 편집기에서 마우스 오른쪽 단추를 클릭 하 고 선택 **Execute** 나타나면 서버 대화 상자에는 연결 2 단계의 연결 정보를 입력 하 고 클릭 **연결**
8.  생성된 된 스키마를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**

## <a name="persist-and-retrieve-data"></a>유지 하 고 데이터를 검색 합니다.

Main 메서드에 정의 되어 있는 Program.cs 파일을 엽니다. 다음 코드를 Main 함수에 추가 합니다. 새 부서 개체 컨텍스트를 추가 하는 코드입니다. 데이터를 저장합니다. 코드는 또한 여기서 name은 DepartmentNames.English 부서를 반환 하는 LINQ 쿼리를 실행 합니다.

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

응용 프로그램을 컴파일하고 실행합니다. 프로그램에서는 다음이 출력됩니다.

```
DepartmentID: 1 Name: English
```

데이터베이스에서 데이터를 보려면 SQL Server 개체 탐색기에서 데이터베이스 이름을 마우스 오른쪽 단추로 클릭 하 고 선택 **새로 고침**합니다. 그런 다음 선택한 테이블에서 마우스 오른쪽 단추를 클릭 **데이터 보기**합니다.

## <a name="summary"></a>요약

이 연습에서는 Entity Framework 디자이너를 사용 하 여 열거형 형식을 매핑하는 방법 및 코드의 열거형을 사용 하는 방법을 살펴보았습니다. 
