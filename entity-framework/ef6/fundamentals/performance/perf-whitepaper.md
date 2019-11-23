---
title: EF4, EF5 및 EF6에 대 한 성능 고려 사항
author: divega
ms.date: 10/23/2016
ms.assetid: d6d5a465-6434-45fa-855d-5eb48c61a2ea
ms.openlocfilehash: 07eb605f0d39f0c1bcfe781540525180f0dd0b22
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181662"
---
# <a name="performance-considerations-for-ef-4-5-and-6"></a>EF 4, 5 및 6에 대 한 성능 고려 사항
David Obando, Eric Dettinger 및 기타

게시일: 4 월 2012

마지막 업데이트: 5 월 2014

------------------------------------------------------------------------

## <a name="1-introduction"></a>1. 소개

개체-관계형 매핑 프레임 워크를 사용 하면 개체 지향 응용 프로그램의 데이터 액세스에 대 한 추상화를 편리 하 게 제공할 수 있습니다. .NET 응용 프로그램의 경우 Microsoft의 권장 O/RM이 Entity Framework 됩니다. 그러나 모든 추상화를 사용 하면 성능이 중요할 수 있습니다.

이 백서에는 Entity Framework를 사용 하 여 응용 프로그램을 개발할 때 성능에 영향을 줄 수 있는 Entity Framework 내부 알고리즘을 이해 하 고 조사를 위한 팁을 제공할 수 있는 성능 고려 사항이 나와 있습니다. Entity Framework를 사용 하는 응용 프로그램의 성능 향상 웹에서 이미 사용할 수 있는 성능에 대 한 유용한 토픽이 많이 있으며, 가능한 경우 이러한 리소스를 가리키기도 시도 했습니다.

성능은 까다로운 주제입니다. 이 백서는 Entity Framework를 사용 하는 응용 프로그램에 대 한 성능 관련 결정을 내리는 데 도움이 되는 리소스로 사용 됩니다. 성능을 보여 주기 위해 일부 테스트 메트릭을 포함 했지만 이러한 메트릭은 응용 프로그램에서 볼 수 있는 성능에 대 한 절대 표시기로 제공 되지 않습니다.

실용적인 목적으로이 문서에서는 Entity Framework 4가 .NET 4.0에서 실행 되 고 Entity Framework 5와 6이 .NET 4.5에서 실행 된다고 가정 합니다. Entity Framework 5에 대 한 성능 향상의 대부분은 .NET 4.5와 함께 제공 되는 핵심 구성 요소 내에 있습니다.

Entity Framework 6은 대역 외 릴리스 이며 .NET과 함께 제공 되는 Entity Framework 구성 요소에 의존 하지 않습니다. Entity Framework 6은 .NET 4.0 및 .NET 4.5 둘 다에서 작동 하며, .NET 4.0에서 업그레이드 하지 않았지만 응용 프로그램에서 최신 Entity Framework 비트가 필요한 사용자에 게 큰 성능 이점을 제공할 수 있습니다. 이 문서에 Entity Framework 6 인 경우이 문서를 작성할 당시에 사용할 수 있는 최신 버전을 참조 하세요. 버전 6.1.0.

## <a name="2-cold-vs-warm-query-execution"></a>2. 콜드 및 웜 쿼리 실행

지정 된 모델에 대 한 쿼리가 처음으로 수행 되는 경우 Entity Framework은 모델을 로드 하 고 유효성을 검사 하기 위해 백그라운드에서 많은 작업을 수행 합니다. 이 첫 번째 쿼리를 "콜드" 쿼리로 자주 참조 합니다.  이미 로드 된 모델에 대 한 추가 쿼리는 "웜" 쿼리 라고 하며 훨씬 더 빠릅니다.

Entity Framework를 사용 하 여 쿼리를 실행할 때 시간이 소요 되는 위치를 개략적으로 확인 하 고 Entity Framework 6에서 개선 되는 기능을 확인할 수 있습니다.

**첫 번째 쿼리 실행-콜드 쿼리**

| 코드 사용자 쓰기                                                                                     | 작업                    | EF4 성능 영향                                                                                                                                                                                                                                                                                                                                                                                                        | EF5 성능 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                    | EF6 성능 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | 컨텍스트 만들기          | 보통                                                                                                                                                                                                                                                                                                                                                                                                                        | 보통                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | 쿼리 식 생성 | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                           | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| `  var c1 = q1.First();`                                                                             | LINQ 쿼리 실행      | -메타 데이터 로드: 높음 (캐시 됨) <br/> -뷰 생성: 매우 높음, 캐시 됨 <br/> -매개 변수 평가: 중형 <br/> -쿼리 변환: 중형 <br/> -구체화 generation: Medium (캐시 됨) <br/> -데이터베이스 쿼리 실행: 잠재적으로 높음 <br/> + 연결 열기 <br/> + ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중형 <br/> -Id 조회: 중간 | -메타 데이터 로드: 높음 (캐시 됨) <br/> -뷰 생성: 매우 높음, 캐시 됨 <br/> -매개 변수 평가: 낮음 <br/> -쿼리 변환: Medium (캐시 됨) <br/> -구체화 generation: Medium (캐시 됨) <br/> -데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리) <br/> + 연결 열기 <br/> + ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중형 <br/> -Id 조회: 중간 | -메타 데이터 로드: 높음 (캐시 됨) <br/> -뷰 생성: 보통 (캐시 됨) <br/> -매개 변수 평가: 낮음 <br/> -쿼리 변환: Medium (캐시 됨) <br/> -구체화 generation: Medium (캐시 됨) <br/> -데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리) <br/> + 연결 열기 <br/> + ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: Medium (EF5 보다 빠름) <br/> -Id 조회: 중간 |
| `}`                                                                                                  | 연결. 닫기          | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                           | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |


**두 번째 쿼리 실행-웜 쿼리**

| 코드 사용자 쓰기                                                                                     | 작업                    | EF4 성능 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | EF5 성능 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | EF6 성능 영향                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|:-----------------------------------------------------------------------------------------------------|:--------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `using(var db = new MyContext())` <br/> `{`                                                          | 컨텍스트 만들기          | 보통                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | 보통                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var q1 = ` <br/> `    from c in db.Customers` <br/> `    where c.Id == id1` <br/> `    select c;` | 쿼리 식 생성 | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `  var c1 = q1.First();`                                                                             | LINQ 쿼리 실행      | -메타 데이터 ~~로드~~ 조회: ~~높음, 캐시 된~~ 낮음 <br/> - ~~생성~~ 조회 조회: ~~매우 높음, 캐시 된~~ 낮음 <br/> -매개 변수 평가: 중형 <br/> -쿼리 ~~변환~~ 조회: 중형 <br/> -구체화 ~~생성~~ 조회: ~~보통 이지만 캐시 된~~ 낮음 <br/> -데이터베이스 쿼리 실행: 잠재적으로 높음 <br/> + 연결 열기 <br/> + ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중형 <br/> -Id 조회: 중간 | -메타 데이터 ~~로드~~ 조회: ~~높음, 캐시 된~~ 낮음 <br/> - ~~생성~~ 조회 조회: ~~매우 높음, 캐시 된~~ 낮음 <br/> -매개 변수 평가: 낮음 <br/> -쿼리 ~~변환~~ 조회: ~~보통 이지만 캐시 된~~ 낮음 <br/> -구체화 ~~생성~~ 조회: ~~보통 이지만 캐시 된~~ 낮음 <br/> -데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리) <br/> + 연결 열기 <br/> + ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: 중형 <br/> -Id 조회: 중간 | -메타 데이터 ~~로드~~ 조회: ~~높음, 캐시 된~~ 낮음 <br/> - ~~생성~~ 조회 보기: ~~보통 이지만 캐시 된~~ 낮음 <br/> -매개 변수 평가: 낮음 <br/> -쿼리 ~~변환~~ 조회: ~~보통 이지만 캐시 된~~ 낮음 <br/> -구체화 ~~생성~~ 조회: ~~보통 이지만 캐시 된~~ 낮음 <br/> -데이터베이스 쿼리 실행: 잠재적으로 높음 (경우에 따라 더 나은 쿼리) <br/> + 연결 열기 <br/> + ExecuteReader <br/> + DataReader.Read <br/> 개체 구체화: Medium (EF5 보다 빠름) <br/> -Id 조회: 중간 |
| `}`                                                                                                  | 연결. 닫기          | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | 낮음                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |


콜드 쿼리와 웜 쿼리의 성능 비용을 줄이는 몇 가지 방법이 있으며, 다음 섹션에서이에 대해 살펴보겠습니다. 특히 미리 생성 된 뷰를 사용 하 여 콜드 쿼리에서 모델 로드 비용을 줄이는 것을 살펴보겠습니다 .이는 뷰 생성 중에 발생 하는 성능 pains를 완화 하는 데 도움이 됩니다. 웜 쿼리의 경우 쿼리 계획 캐싱, 추적 쿼리 및 다른 쿼리 실행 옵션을 다룹니다.

### <a name="21-what-is-view-generation"></a>2.1 뷰 생성 이란?

뷰 생성을 이해 하려면 먼저 "매핑 뷰"를 이해 해야 합니다. 매핑 뷰는 각 엔터티 집합 및 연결에 대 한 매핑에 지정 된 변환의 실행 가능한 표현입니다. 내부적으로 이러한 매핑 뷰는 CQTs (정식 쿼리 트리)의 형태를 사용 합니다. 매핑 뷰에는 다음과 같은 두 가지 유형이 있습니다.

-   쿼리 뷰: 데이터베이스 스키마에서 개념적 모델로 이동 하는 데 필요한 변환을 나타냅니다.
-   업데이트 뷰: 개념적 모델에서 데이터베이스 스키마로 이동 하는 데 필요한 변환을 나타냅니다.

개념적 모델은 다양 한 방식으로 데이터베이스 스키마와 다를 수 있습니다. 예를 들어 하나의 단일 테이블을 사용 하 여 두 개의 서로 다른 엔터티 형식에 대 한 데이터를 저장할 수 있습니다. 상속 및 특수 한 매핑은 매핑 뷰의 복잡성에서 역할을 수행 합니다.

매핑 사양에 따라 이러한 뷰를 계산 하는 프로세스는 뷰 생성을 호출 하는 것입니다. 뷰 생성은 모델이 로드 될 때 또는 빌드 시 "미리 생성 된 뷰"를 사용 하 여 동적으로 발생 될 수 있습니다. 후자는 Entity SQL 문 형식으로 C\# 또는 VB 파일에 serialize 됩니다.

뷰가 생성 될 때에도 유효성 검사가 수행 됩니다. 성능 관점에서 볼 때 대부분의 뷰 생성 비용은 실제로 엔터티 간의 연결이 적합 하 고 지원 되는 모든 작업에 대해 올바른 카디널리티를 갖도록 하는 뷰의 유효성을 검사 하는 것입니다.

엔터티 집합에 대 한 쿼리가 실행 되 면 쿼리는 해당 쿼리 뷰와 결합 되며이 컴퍼지션의 결과는 계획 컴파일러를 통해 실행 되어 백업 저장소에서 이해할 수 있는 쿼리 표현을 만듭니다. SQL Server의 경우이 컴파일의 최종 결과는 T-sql SELECT 문이 됩니다. 엔터티 집합에 대 한 업데이트를 처음 수행 하는 경우 유사한 프로세스를 통해 업데이트 뷰를 실행 하 여 대상 데이터베이스에 대 한 DML 문으로 변환 합니다.

### <a name="22-factors-that-affect-view-generation-performance"></a>2.2 뷰 생성 성능에 영향을 주는 요소

뷰 생성 단계의 성능은 모델의 크기에 따라 달라 지 며 모델을 상호 연결 하는 방법에 따라 달라 집니다. 두 엔터티가 상속 체인이 나 연결을 통해 연결 된 경우 연결 된 것으로 간주 됩니다. 마찬가지로 두 테이블이 외래 키를 통해 연결 된 경우 연결 됩니다. 스키마의 연결 된 엔터티 및 테이블 수가 늘어나면 뷰 생성 비용이 증가 합니다.

뷰를 생성 하 고 유효성을 검사 하는 데 사용 하는 알고리즘은 최악의 경우에는 유용 하지만 약간의 최적화를 사용 하 여이를 개선 합니다. 성능에 부정적인 영향을 주는 가장 큰 요인은 다음과 같습니다.

-   엔터티 수와 이러한 엔터티 간의 연결 크기를 참조 하는 모델 크기
-   모델 복잡성, 특히 많은 수의 형식을 포함 하는 상속.
-   외래 키 연결 대신 독립 연결을 사용 합니다.

작고 간단한 모델의 경우 비용이 미리 생성 된 보기를 사용 하지 않도록 충분히 작을 수 있습니다. 모델 크기와 복잡성이 증가 함에 따라 보기 생성 및 유효성 검사의 비용을 줄이는 데 사용할 수 있는 몇 가지 옵션이 있습니다.

### <a name="23-using-pre-generated-views-to-decrease-model-load-time"></a>2.3 미리 생성 된 뷰를 사용 하 여 모델 로드 시간 줄이기

Entity Framework 6에서 미리 생성 된 뷰를 사용 하는 방법에 대 한 자세한 내용은 [미리 생성 된 매핑 보기](~/ef6/fundamentals/performance/pre-generated-views.md) 를 참조 하십시오.

#### <a name="231-pre-generated-views-using-the-entity-framework-power-tools-community-edition"></a>Entity Framework Power Tools Community Edition을 사용 하 여 미리 생성 된 뷰 2.3.1

[Entity Framework 6 파워 도구 Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition) 을 사용 하 여 모델 클래스 파일을 마우스 오른쪽 단추로 클릭 하 고 Entity Framework 메뉴를 사용 하 여 "뷰 생성"을 선택 하 여 EDMX 및 Code First 모델의 뷰를 생성할 수 있습니다. Entity Framework Power Tools Community Edition은 DbContext 파생 컨텍스트에서만 작동 합니다.

#### <a name="232-how-to-use-pre-generated-views-with-a-model-created-by-edmgen"></a>Edmgen.exe에서 만든 모델을 사용 하 여 미리 생성 된 뷰를 사용 하는 방법 2.3.2

Edmgen.exe는 .NET과 함께 제공 되 고 Entity Framework 4 및 5와 함께 작동 하지만 Entity Framework 6에서는 작동 하지 않는 유틸리티입니다. Edmgen.exe를 사용 하면 명령줄에서 모델 파일, 개체 계층 및 뷰를 생성할 수 있습니다. 출력 중 하나는 선택, VB 또는 C\#언어의 뷰 파일이 됩니다. 각 엔터티 집합에 대 한 Entity SQL 코드 조각이 포함 된 코드 파일입니다. 미리 생성 된 뷰를 사용 하도록 설정 하려면 프로젝트에 파일을 포함 하기만 하면 됩니다.

모델에 대 한 스키마 파일을 수동으로 편집 하는 경우 뷰 파일을 다시 생성 해야 합니다. **/Mode: ViewGeneration** 플래그를 사용 하 여 edmgen.exe를 실행 하 여이 작업을 수행할 수 있습니다.

#### <a name="233-how-to-use-pre-generated-views-with-an-edmx-file"></a>2.3.3 EDMX 파일을 사용 하 여 미리 생성 된 뷰를 사용 하는 방법

Edmgen.exe를 사용 하 여 EDMX 파일에 대 한 보기를 생성할 수도 있습니다. 이전에 참조 한 MSDN 항목에서는이 작업을 수행 하기 위해 빌드 전 이벤트를 추가 하는 방법에 대해 설명 합니다. 그러나이 작업은 복잡 하며 가능 하지 않은 경우도 있습니다. 일반적으로 모델이 edmx 파일에 있을 때 T4 템플릿을 사용 하 여 뷰를 생성 하는 것이 더 쉽습니다.

ADO.NET 팀 블로그는 뷰를 생성 하기 위해 T4 템플릿을 사용 하는 방법에 설명 하는 게시물 ( \<http://blogs.msdn.com/b/adonet/archive/2008/06/20/how-to-use-a-t4-template-for-view-generation.aspx>)합니다. 이 게시물에는 다운로드 하 여 프로젝트에 추가할 수 있는 템플릿이 포함 되어 있습니다. 템플릿은 Entity Framework의 첫 번째 버전용으로 작성 되었기 때문에 최신 버전의 Entity Framework에서 작동 하지 않을 수 있습니다. 그러나 Visual Studio 갤러리에서 Entity Framework 4 및 5에 대 한 최신 보기 생성 템플릿 집합을 다운로드할 수 있습니다.

-   VB.NET: \<http://visualstudiogallery.msdn.microsoft.com/118b44f2-1b91-4de2-a584-7a680418941d>
-   C\#: \<http://visualstudiogallery.msdn.microsoft.com/ae7730ce-ddab-470f-8456-1b313cd2c44d>

Entity Framework 6을 사용 하는 경우에서 가져올 수 있습니다 뷰 생성 T4 템플릿 아래에 있는 Visual Studio 갤러리 \<http://visualstudiogallery.msdn.microsoft.com/18a7db90-6705-4d19-9dd1-0a6c23d0751f>합니다.

### <a name="24-reducing-the-cost-of-view-generation"></a>2.4 보기 생성 비용 절감

미리 생성 된 뷰를 사용 하면 뷰 생성 비용을 모델 로드 (런타임)에서 디자인 시간으로 이동 합니다. 이렇게 하면 런타임 시 시작 성능이 향상 되지만 개발 하는 동안에도 보기 생성의 어려움을 겪을 수 있습니다. 컴파일 시간과 런타임에 뷰 생성 비용을 줄이는 데 도움이 되는 몇 가지 추가 트릭을 사용할 수 있습니다.

#### <a name="241-using-foreign-key-associations-to-reduce-view-generation-cost"></a>2.4.1는 외래 키 연결을 사용 하 여 보기 생성 비용 절감

모델의 연결을 독립 연결에서 외래 키 연결로 전환 하는 것이 보기 생성에 소요 된 시간을 크게 향상 시키는 여러 가지 사례를 살펴보았습니다.

이러한 개선을 보여 주기 위해 Edmgen.exe를 사용 하 여 두 가지 버전의 Navision 모델을 생성 했습니다. *참고: Navision 모델에 대 한 설명은 부록 C를 참조 하세요.* Navision 모델은 매우 많은 엔터티 및 관계 간의 관계로 인해이 연습에서 흥미롭습니다.

이 매우 큰 모델의 한 버전은 외래 키 연결을 사용 하 여 생성 되 고 다른 버전은 독립 연결을 사용 하 여 생성 되었습니다. 그런 다음 각 모델에 대 한 뷰를 생성 하는 데 걸린 시간을 시간 지정 했습니다. Entity Framework 5 테스트는 EntityViewGenerator 클래스의 GenerateViews () 메서드를 사용 하 여 뷰를 생성 하 고, Entity Framework 6 테스트는 StorageMappingItemCollection 클래스의 GenerateViews () 메서드를 사용 했습니다. 이는 Entity Framework 6 코드 베이스에서 발생 한 코드 재구성으로 인해 발생 합니다.

Entity Framework 5를 사용 하 여, 외래 키가 있는 모델에 대 한 생성 보기는 랩 컴퓨터에서 65 분이 걸렸습니다. 독립 연결을 사용 하는 모델에 대 한 뷰를 생성 하는 데 걸리는 시간을 알 수 없습니다. 매월 업데이트를 설치 하기 위해 랩에 컴퓨터가 다시 부팅 되기 전에 한 달 동안 테스트를 실행 하 고 있습니다.

Entity Framework 6을 사용 하는 경우 외래 키가 있는 모델에 대 한 생성 보기는 동일한 랩 컴퓨터에서 28 초가 걸렸습니다. 독립 연결을 사용 하는 모델에 대 한 생성 보기는 58 초 걸렸습니다. 해당 뷰 생성 코드에서 6 Entity Framework에 대 한 향상 된 기능은 많은 프로젝트에서 시작 시간을 단축 하기 위해 미리 생성 된 뷰가 필요 하지 않음을 의미 합니다.

Entity Framework 4 및 5의 미리 생성 보기는 Edmgen.exe 또는 Entity Framework 파워 도구를 사용 하 여 수행할 수 있습니다. Entity Framework 6은 Entity Framework 파워 도구를 통해 또는 [미리 생성 된 매핑 보기](~/ef6/fundamentals/performance/pre-generated-views.md)에서 설명한 대로 프로그래밍 방식으로 뷰 생성을 수행할 수 있습니다.

##### <a name="2411-how-to-use-foreign-keys-instead-of-independent-associations"></a>독립 연결 대신 외래 키를 사용 하는 방법 2.4.1.1

Visual Studio에서 Edmgen.exe 또는 Entity Designer를 사용 하는 경우 기본적으로 FKs를 가져오고, FKs와 IAs 간을 전환 하는 단일 확인란 또는 명령줄 플래그를 사용 합니다.

Code First 모델이 클 경우 독립 연결을 사용 하는 것은 뷰 생성에 동일한 효과를 가집니다. 종속 개체에 대 한 클래스에 외래 키 속성을 포함 하 여 이러한 영향을 방지할 수 있습니다. 하지만 일부 개발자는 개체 모델을 polluting 하는 것으로 간주 합니다. 이 주제에 대 한 자세한 정보를 찾을 수 있습니다 \<http://blog.oneunicorn.com/2011/12/11/whats-the-deal-with-mapping-foreign-keys-using-the-entity-framework/>합니다.

| 사용 하는 경우      | 단계                                                                                                                                                                                                                                                                                                                              |
|:----------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Entity Designer | 두 엔터티 간의 연결을 추가한 후에는 참조 제약 조건이 있는지 확인 합니다. 참조 제약 조건은 독립적인 연결 대신 외래 키를 사용 Entity Framework에 게 알려 줍니다. 추가 세부 정보를 참조 하세요 \<http://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx>합니다. |
| EDMGen          | Edmgen.exe를 사용 하 여 데이터베이스에서 파일을 생성 하는 경우 외래 키가 적용 되 고 모델에 추가 됩니다. EDMGen에 의해 노출 된 다양 한 옵션에 대 한 자세한 내용은 방문 [http://msdn.microsoft.com/library/bb387165.aspx](https://msdn.microsoft.com/library/bb387165.aspx)합니다.                           |
| Code First      | Code First를 사용할 때 종속 개체에 외래 키 속성을 포함 하는 방법에 대 한 자세한 내용은 [Code First 규칙](~/ef6/modeling/code-first/conventions/built-in.md) 항목의 "관계 규칙" 섹션을 참조 하세요.                                                                                              |

#### <a name="242-moving-your-model-to-a-separate-assembly"></a>모델을 별도 어셈블리로 이동 2.4.2 sections

모델이 응용 프로그램의 프로젝트에 직접 포함 되어 있고 빌드 전 이벤트 또는 T4 템플릿을 통해 뷰를 생성 하는 경우 모델이 변경 되지 않은 경우에도 프로젝트를 다시 빌드할 때마다 생성 및 유효성 검사가 수행 됩니다. 모델을 별도의 어셈블리로 이동 하 고 응용 프로그램의 프로젝트에서 참조 하는 경우에는 모델을 포함 하는 프로젝트를 다시 빌드할 필요 없이 응용 프로그램을 다른 방법으로 변경할 수 있습니다.

*참고:* 모델을 개별 어셈블리로 이동할 때  모델에 대 한 연결 문자열을 클라이언트 프로젝트의 응용 프로그램 구성 파일에 복사 해야 합니다.

#### <a name="243-disable-validation-of-an-edmx-based-model"></a>2.4.3 edmx 기반 모델의 유효성 검사를 사용 하지 않도록 설정

모델을 변경 하지 않은 경우에도 EDMX 모델은 컴파일 시간에 유효성이 검사 됩니다. 모델의 유효성이 이미 검사 된 경우 속성 창에서 "빌드 유효성 검사" 속성을 false로 설정 하 여 컴파일 타임에 유효성 검사를 표시 하지 않을 수 있습니다. 매핑 또는 모델을 변경 하는 경우 유효성 검사를 일시적으로 다시 사용 하도록 설정 하 여 변경 내용을 확인할 수 있습니다.

Entity Framework 6의 Entity Framework Designer에 대 한 성능이 향상 되었으며 "빌드의 유효성 검사" 비용은 이전 버전의 디자이너 에서보다 훨씬 낮습니다.

## <a name="3-caching-in-the-entity-framework"></a>3 Entity Framework 캐싱

Entity Framework에는 기본 제공 되는 다음과 같은 형식의 캐싱이 있습니다.

1.  개체 캐싱 – ObjectContext 인스턴스에 기본 제공 되는 ObjectStateManager는 해당 인스턴스를 사용 하 여 검색 된 개체의 메모리에서 추적을 유지 합니다. 이를 첫 번째 수준의 캐시 라고도 합니다.
2.  쿼리 계획 캐싱-쿼리가 두 번 이상 실행 될 때 생성 된 저장소 명령을 다시 사용 합니다.
3.  메타 데이터 캐싱-동일한 모델에 대 한 여러 연결에서 모델에 대 한 메타 데이터를 공유 합니다.

EF에서 제공 하는 캐시 외에도 줄 바꿈 공급자 라는 특수 한 종류의 ADO.NET 데이터 공급자를 사용 하 여 데이터베이스에서 검색 된 결과에 대 한 캐시를 사용 하 여 Entity Framework를 확장할 수 있습니다 .이는 두 번째 수준 캐싱이 라고도 합니다.

### <a name="31-object-caching"></a>3.1 개체 캐싱

기본적으로 쿼리 결과에서 엔터티가 반환 될 때 EF가이를 구체화 하기 직전에 ObjectContext는 동일한 키를 가진 엔터티가 해당 ObjectStateManager에 이미 로드 되었는지 여부를 확인 합니다. 동일한 키를 가진 엔터티가 이미 있는 경우 EF는 쿼리 결과에이를 포함 합니다. EF는 데이터베이스에 대해 계속 쿼리를 실행 하지만이 동작은 엔터티를 여러 번 구체화 하는 비용을 많이 우회할 수 있습니다.

#### <a name="311-getting-entities-from-the-object-cache-using-dbcontext-find"></a>DbContext Find를 사용 하 여 개체 캐시에서 엔터티 가져오기 3.1.1

일반 쿼리와 달리 DbSet (EF 4.1에서는 처음으로 포함 된 Api)의 Find 메서드는 데이터베이스에 대해 쿼리를 실행 하기 전에 메모리에서 검색을 수행 합니다. 두 개의 다른 ObjectContext 인스턴스에는 별도의 개체 캐시가 있음을 의미 하는 두 개의 서로 다른 ObjectStateManager 인스턴스가 있습니다.

Find는 기본 키 값을 사용 하 여 컨텍스트에 의해 추적 된 엔터티를 찾으려고 시도 합니다. 엔터티가 컨텍스트에 있지 않으면 쿼리를 실행 하 고 데이터베이스에 대해 계산 하며, 엔터티가 컨텍스트 또는 데이터베이스에 없는 경우 null이 반환 됩니다. 또한 Find는 컨텍스트에 추가 되었지만 아직 데이터베이스에 저장 되지 않은 엔터티를 반환 합니다.

찾기를 사용할 때 고려해 야 할 성능 고려 사항이 있습니다. 이 메서드를 호출 하면 기본적으로 데이터베이스에 대 한 커밋 보류 중인 변경 내용을 감지 하기 위해 개체 캐시의 유효성 검사가 트리거됩니다. 이 프로세스는 개체 캐시에 매우 많은 개체가 있거나 개체 캐시에 추가 되는 큰 개체 그래프에 있는 경우 비용이 매우 클 수 있지만, 사용 하지 않도록 설정할 수도 있습니다. 일부 경우에는 자동 검색을 사용 하지 않도록 설정할 때 Find 메서드를 호출할 때의 차이점을 확인할 수 있습니다. 하지만 개체가 실제로 캐시에 있는 경우와 데이터베이스에서 개체를 검색 해야 하는 경우에는 두 번째 크기를 인식 합니다. 다음은 5000 엔터티를 로드 하 여 밀리초 단위로 표현 된 마이크로 벤치 마크 중 일부를 사용 하 여 측정 한 예제 그래프입니다.

![.Net 4.5 로그 크기](~/ef6/media/net45logscale.png ".net 4.5-로그") 눈금 간격

자동 검색을 사용 하지 않도록 설정 된 찾기의 예:

``` csharp
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    context.Configuration.AutoDetectChangesEnabled = true;
    ...
```

Find 메서드를 사용할 때 고려해 야 할 사항은 다음과 같습니다.

1.  개체가 캐시에 없으면 Find의 이점이 부정 되지만 구문은 키로 쿼리 하는 것 보다 간단 합니다.
2.  자동 검색을 사용 하는 경우에는 찾기 방법의 비용이 한 크기 만큼 증가 하거나 모델의 복잡도와 개체 캐시에 있는 엔터티의 양에 따라 달라질 수 있습니다.

또한 Find는 찾으려는 엔터티만 반환 하 고, 개체 캐시에 아직 연결 되지 않은 경우 연결 된 엔터티를 자동으로 로드 하지 않는다는 점에 유의 하세요. 연결 된 엔터티를 검색 해야 하는 경우 즉시 로드와 함께 키로 쿼리를 사용할 수 있습니다. 자세한 내용은 **8.1 지연 로드 및 즉시 로드**를 참조 하세요.

#### <a name="312-performance-issues-when-the-object-cache-has-many-entities"></a>개체 캐시에 많은 엔터티가 있는 경우 성능 문제 3.1.2

개체 캐시를 사용 하면 Entity Framework의 전체 응답성을 높일 수 있습니다. 그러나 개체 캐시가 대량의 엔터티를 로드 한 경우 추가, 제거, 찾기, 항목, SaveChanges 등의 특정 작업에 영향을 줄 수 있습니다. 특히 DetectChanges에 대 한 호출을 트리거하는 작업은 매우 큰 개체 캐시에 의해 부정적인 영향을 받습니다. DetectChanges는 개체 그래프를 개체 상태 관리자와 동기화 하 고 해당 성능이 개체 그래프의 크기에 따라 직접 결정 됩니다. DetectChanges에 대 한 자세한 내용은 [POCO 엔터티의 변경 내용 추적](https://msdn.microsoft.com/library/dd456848.aspx)을 참조 하세요.

Entity Framework 6을 사용 하는 경우 개발자는 컬렉션을 반복 하 고 인스턴스당 한 번 추가를 호출 하는 대신 DbSet에서 직접 AddRange 및 RemoveRange를 호출할 수 있습니다. 범위 메서드를 사용 하는 경우의 이점은 추가 된 각 엔터티에 대해 한 번이 아니라 전체 엔터티 집합에 대해 한 번만 DetectChanges 비용을 지불 하는 것입니다.

### <a name="32-query-plan-caching"></a>3.2 쿼리 계획 캐싱

쿼리가 처음 실행 될 때 내부 계획 컴파일러를 통해 개념적 쿼리를 저장 명령으로 변환 합니다 (예: SQL Server에 대해 실행 될 때 실행 되는 T-sql).  쿼리 계획 캐싱이 설정 된 경우 다음에 쿼리를 실행할 때 계획 컴파일러를 우회 하 여 실행을 위해 쿼리 계획 캐시에서 직접 store 명령이 검색 됩니다.

쿼리 계획 캐시는 동일한 AppDomain 내의 ObjectContext 인스턴스 간에 공유 됩니다. 쿼리 계획 캐싱을 활용 하려면 ObjectContext 인스턴스를 보관할 필요가 없습니다.

#### <a name="321-some-notes-about-query-plan-caching"></a>쿼리 계획 캐싱에 대 한 몇 가지 참고 사항 3.2.1

-   쿼리 계획 캐시는 Entity SQL, LINQ to Entities 및 CompiledQuery 개체의 모든 쿼리 유형에 대해 공유 됩니다.
-   기본적으로 EntityCommand 또는 ObjectQuery를 통해 실행 되는지에 관계 없이 쿼리 계획 캐싱은 Entity SQL 쿼리에 대해 사용 하도록 설정 됩니다. .NET 4.5에서 Entity Framework의 LINQ to Entities 쿼리에 대해서도 기본적으로 사용 하도록 설정 되며, Entity Framework 6
    -   EnablePlanCaching 속성 (EntityCommand 또는 ObjectQuery)을 false로 설정 하 여 쿼리 계획 캐싱을 사용 하지 않도록 설정할 수 있습니다. 예를 들면 다음과 같습니다.
``` csharp
                    var query = from customer in context.Customer
                                where customer.CustomerId == id
                                select new
                                {
                                    customer.CustomerId,
                                    customer.Name
                                };
                    ObjectQuery oQuery = query as ObjectQuery;
                    oQuery.EnablePlanCaching = false;
```
-   매개 변수가 있는 쿼리의 경우 매개 변수의 값을 변경 하면 캐시 된 쿼리가 계속 적중 됩니다. 그러나 매개 변수의 패싯 (예: 크기, 전체 자릿수 또는 소수 자릿수)을 변경 하면 캐시의 다른 항목에 도달 하 게 됩니다.
-   Entity SQL 사용 하는 경우 쿼리 문자열은 키의 일부입니다. 쿼리가 기능적으로 동일한 경우에도 쿼리를 변경 하면 다른 캐시 항목이 생성 됩니다. 여기에는 대/소문자 또는 공백을 변경 하는 작업이 포함 됩니다.
-   LINQ를 사용 하는 경우 쿼리를 처리 하 여 키의 일부를 생성 합니다. 따라서 LINQ 식을 변경 하면 다른 키가 생성 됩니다.
-   기타 기술 제한이 적용 될 수 있습니다. 자세한 내용은 Autocompiled 쿼리를 참조 하세요.

#### <a name="322-cache-eviction-algorithm"></a>3.2.2 캐시 제거 알고리즘

내부 알고리즘의 작동 방식을 이해 하면 쿼리 계획 캐싱을 사용 하거나 사용 하지 않도록 설정 하는 경우를 파악할 수 있습니다. 정리 알고리즘은 다음과 같습니다.

1.  캐시에 설정 된 항목 수 (800)가 포함 되 면 주기적으로 (분당 한 번) 캐시를 스윕 하는 타이머가 시작 됩니다.
2.  캐시를 사용 하는 동안 LFRU (가장 자주 사용 하지 않음)의 캐시에서 항목이 제거 됩니다. 이 알고리즘은 꺼낼 항목을 결정할 때 적중 횟수와 보존 기간을 모두 고려 합니다.
3.  각 캐시 스윕의 끝에는 캐시에 800 항목이 다시 포함 됩니다.

모든 캐시 항목은 제거할 항목을 결정할 때 동일 하 게 처리 됩니다. 즉, CompiledQuery에 대 한 store 명령이 Entity SQL 쿼리에 대 한 저장소 명령과 동일한 제거 가능성을 갖습니다.

캐시에 800 엔터티가 있는 경우 캐시 제거 타이머가 시작 되지만이 타이머가 시작 된 후에만 캐시가 스윕 60 초입니다. 즉, 최대 60 초 동안 캐시가 상당히 커질 수 있습니다.

#### <a name="323-test-metrics-demonstrating-query-plan-caching-performance"></a>쿼리 계획 캐싱 성능을 보여 주는 3.2.3 테스트 메트릭

응용 프로그램의 성능에 대 한 쿼리 계획 캐싱의 영향을 보여 주기 위해 Navision 모델에 대해 많은 Entity SQL 쿼리를 실행 한 테스트를 수행 했습니다. Navision 모델에 대 한 설명 및 실행 된 쿼리 유형에 대해서는 부록을 참조 하세요. 이 테스트에서는 먼저 쿼리 목록을 반복 하 고 각 쿼리를 한 번 실행 하 여 캐시에 추가 합니다 (캐싱이 설정 된 경우). 이 단계는 시간이 초과 되었습니다. 다음으로, 캐시 비우기가 수행 될 수 있도록 60 초 넘게 주 스레드를 대기 시킵니다. 마지막으로 캐시 된 쿼리를 실행 하기 위해 목록 전체를 반복 합니다. 또한 각 쿼리 집합이 실행 되기 전에 SQL Server 계획 캐시가 플러시되고 쿼리 계획 캐시에서 제공 하는 혜택을 정확 하 게 반영 합니다.

##### <a name="3231-test-results"></a>3.2.3.1 테스트 결과

| 테스트                                                                   | 캐시 없음 EF5 | EF5 캐시 됨 | 캐시 없음 EF6 | EF6 캐시 됨 |
|:-----------------------------------------------------------------------|:-------------|:-----------|:-------------|:-----------|
| 모든 18723 쿼리 열거                                          | 124          | 125.4      | 124.3        | 125.3      |
| 스윕 방지 (복잡성에 관계 없이 첫 번째 800 쿼리만)  | 41.7         | 5.5        | 40.5         | 5.4        |
| AggregatingSubtotals 쿼리만 (178 total-sweep을 방지 하는) | 39.5         | 4.5        | 38.1         | 4.6        |

*모든 시간 (초)입니다.*

도덕적-여러 개의 고유 쿼리를 실행 하는 경우 (예: 동적으로 생성 된 쿼리) 캐싱이 도움이 되지 않으며 캐시의 결과로 생성 되는 캐시를 사용 하면 실제로 계획 캐싱에 가장 유용한 쿼리를 유지할 수 있습니다.

AggregatingSubtotals 쿼리는를 사용 하 여 테스트 한 쿼리와 가장 복잡 합니다. 예상 대로 쿼리가 복잡할수록 쿼리 계획 캐싱에서 더 많은 이점을 얻게 됩니다.

CompiledQuery는 계획을 캐시 하는 LINQ 쿼리 이기 때문에 CompiledQuery와 동등한 Entity SQL 쿼리의 비교 결과는 유사 해야 합니다. 실제로 응용 프로그램에 동적 Entity SQL 쿼리가 많이 있는 경우 캐시를 쿼리를 사용 하 여 채우면 캐시에서 플러시된 경우에도 CompiledQueries이 "디컴파일"로 인해 효과적으로 발생 합니다. 이 시나리오에서는 동적 쿼리에서 캐싱을 사용 하지 않도록 설정 하 여 CompiledQueries의 우선 순위를 지정 함으로써 성능을 향상 시킬 수 있습니다. 물론, 아직 동적 쿼리 대신 매개 변수가 있는 쿼리를 사용 하도록 앱을 다시 작성 하는 것이 좋습니다.

### <a name="33-using-compiledquery-to-improve-performance-with-linq-queries"></a>3.3 LINQ 쿼리를 사용 하 여 성능을 향상 시키기 위해 CompiledQuery 사용

테스트 결과, CompiledQuery를 사용 하면 autocompiled LINQ 쿼리를 통해 7%의 이점을 누릴 수 있습니다. 즉, Entity Framework 스택에서 코드를 실행 하는 데 시간을 7% 줄일 수 있습니다. 응용 프로그램이 7% 더 빠르게 수행 되는 것은 아닙니다. 일반적으로 EF 5.0에서 CompiledQuery 개체를 작성 하 고 유지 관리 하는 데 드는 비용은 이점과 비교할 때 문제가 되지 않을 수 있습니다. 진행이 다를 수 있으므로 프로젝트에 추가 푸시가 필요한 경우이 옵션을 연습 합니다. CompiledQueries는 ObjectContext 파생 모델과만 호환 되며 DbContext 파생 모델과 호환 되지 않습니다.

CompiledQuery을 만들고 호출 하는 방법에 대 한 자세한 내용은 [컴파일된 쿼리 (LINQ to Entities)](https://msdn.microsoft.com/library/bb896297.aspx)를 참조 하세요.

CompiledQuery를 사용할 때 수행 해야 하는 두 가지 고려 사항이 있습니다. 즉, 정적 인스턴스를 사용 하는 데 필요한 요구 사항 및 composability를 사용 하 여 발생 하는 문제입니다. 다음은 이러한 두 가지 고려 사항에 대 한 자세한 설명입니다.

#### <a name="331-use-static-compiledquery-instances"></a>3.3.1 Use static CompiledQuery instances

LINQ 쿼리를 컴파일하면 시간이 많이 소요 되기 때문에 데이터베이스에서 데이터를 인출 해야 할 때마다이 작업을 수행 하지 않으려고 합니다. CompiledQuery 인스턴스를 사용 하면 한 번만 컴파일하거나 여러 번 실행할 수 있지만, 다시 컴파일하는 대신 매번 동일한 CompiledQuery 인스턴스를 다시 사용 하려면 신중 하 게 사용 해야 합니다. CompiledQuery 인스턴스를 저장 하는 데 정적 멤버를 사용 해야 합니다. 그렇지 않으면 어떤 이점도 표시 되지 않습니다.

예를 들어, 선택한 범주에 대 한 제품 표시를 처리 하는 다음 메서드 본문이 페이지에 있다고 가정 합니다.

``` csharp
    // Warning: this is the wrong way of using CompiledQuery
    using (NorthwindEntities context = new NorthwindEntities())
    {
        string selectedCategory = this.categoriesList.SelectedValue;

        var productsForCategory = CompiledQuery.Compile<NorthwindEntities, string, IQueryable<Product>>(
            (NorthwindEntities nwnd, string category) =>
                nwnd.Products.Where(p => p.Category.CategoryName == category)
        );

        this.productsGrid.DataSource = productsForCategory.Invoke(context, selectedCategory).ToList();
        this.productsGrid.DataBind();
    }

    this.productsGrid.Visible = true;
```

이 경우 메서드가 호출 될 때마다 즉석에서 새 CompiledQuery 인스턴스를 만듭니다. 쿼리 계획 캐시에서 저장소 명령을 검색 하 여 성능상의 이점을 확인 하는 대신 새 인스턴스를 만들 때마다 CompiledQuery가 계획 컴파일러를 진행 합니다. 실제로 메서드를 호출할 때마다 새 CompiledQuery 항목을 사용 하 여 쿼리 계획 캐시에 polluting 됩니다.

대신, 컴파일된 쿼리의 정적 인스턴스를 만들려는 경우 메서드가 호출 될 때마다 동일한 컴파일된 쿼리를 호출 합니다. 이를 위한 한 가지 방법은 개체 컨텍스트의 멤버로 CompiledQuery 인스턴스를 추가 하는 것입니다.  그런 다음 도우미 메서드를 통해 CompiledQuery에 액세스 하 여 작업을 간단 하 게 만들 수 있습니다.

``` csharp
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IEnumerable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
            );

        public IEnumerable<Product> GetProductsForCategory(string categoryName)
        {
            return productsForCategoryCQ.Invoke(this, categoryName).ToList();
        }
```

이 도우미 메서드는 다음과 같이 호출 됩니다.

``` csharp
    this.productsGrid.DataSource = context.GetProductsForCategory(selectedCategory);
```

#### <a name="332-composing-over-a-compiledquery"></a>CompiledQuery에 대 한 3.3.2 작성

LINQ 쿼리를 통해 작성 하는 기능은 매우 유용 합니다. 이렇게 하려면 *Skip ()* 또는 *Count ()* 와 같은 IQueryable 후에 메서드를 호출 하면 됩니다. 그러나 이렇게 하면 기본적으로 새 IQueryable 개체가 반환 됩니다. 기술적으로 CompiledQuery을 통해 작성 하지 않아도 되는 것은 아니지만, 이렇게 하면 계획 컴파일러를 통해 다시 전달 해야 하는 새 IQueryable 개체가 생성 됩니다.

일부 구성 요소는 구성 된 IQueryable 개체를 사용 하 여 고급 기능을 사용 하도록 설정 합니다. 예: ASP. 네트워크 GridView는 SelectMethod 속성을 통해 IQueryable 개체에 데이터 바인딩할 수 있습니다. 그러면 GridView가이 IQueryable 개체를 통해 데이터 모델에 대 한 정렬과 페이징을 수행할 수 있도록 구성 됩니다. 여기에서 볼 수 있듯이 GridView에 대해 CompiledQuery를 사용 하면 컴파일된 쿼리에 적중 되지 않지만 새로운 autocompiled 쿼리가 생성 됩니다.

이를 실행할 수 있는 한 가지 위치는 쿼리에 프로그레시브 필터를 추가 하는 경우입니다. 예를 들어, 선택 사항인 필터 (예: Country 및 OrdersCount)에 대 한 몇 가지 드롭다운 목록을 포함 하는 고객 페이지가 있다고 가정 합니다. CompiledQuery의 IQueryable 결과에 대해 이러한 필터를 작성할 수 있지만 이렇게 하면 새 쿼리가 실행 될 때마다 계획 컴파일러를 통해 새 쿼리가 생성 됩니다.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployee();

        if (this.orderCountFilterList.SelectedItem.Value != defaultFilterText)
        {
            int orderCount = int.Parse(orderCountFilterList.SelectedValue);
            myCustomers = myCustomers.Where(c => c.Orders.Count > orderCount);
        }

        if (this.countryFilterList.SelectedItem.Value != defaultFilterText)
        {
            myCustomers = myCustomers.Where(c => c.Address.Country == countryFilterList.SelectedValue);
        }

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 이 다시 컴파일이 발생 하지 않도록 하려면 CompiledQuery를 다시 작성 하 여 가능한 필터를 고려해 야 합니다.

``` csharp
    private static readonly Func<NorthwindEntities, int, int?, string, IQueryable<Customer>> customersForEmployeeWithFiltersCQ = CompiledQuery.Compile(
        (NorthwindEntities context, int empId, int? countFilter, string countryFilter) =>
            context.Customers.Where(c => c.Orders.Any(o => o.EmployeeID == empId))
            .Where(c => countFilter.HasValue == false || c.Orders.Count > countFilter)
            .Where(c => countryFilter == null || c.Address.Country == countryFilter)
        );
```

다음과 같은 UI에서 호출 됩니다.

``` csharp
    using (NorthwindEntities context = new NorthwindEntities())
    {
        int? countFilter = (this.orderCountFilterList.SelectedIndex == 0) ?
            (int?)null :
            int.Parse(this.orderCountFilterList.SelectedValue);

        string countryFilter = (this.countryFilterList.SelectedIndex == 0) ?
            null :
            this.countryFilterList.SelectedValue;

        IQueryable<Customer> myCustomers = context.InvokeCustomersForEmployeeWithFilters(
                countFilter, countryFilter);

        this.customersGrid.DataSource = myCustomers;
        this.customersGrid.DataBind();
    }
```

 여기서의 단점은 생성 된 저장소 명령은 항상 null 검사를 포함 하는 필터를 포함 하지만 데이터베이스 서버가 최적화 되려면 매우 간단 해야 합니다.

``` SQL
...
WHERE ((0 = (CASE WHEN (@p__linq__1 IS NOT NULL) THEN cast(1 as bit) WHEN (@p__linq__1 IS NULL) THEN cast(0 as bit) END)) OR ([Project3].[C2] > @p__linq__2)) AND (@p__linq__3 IS NULL OR [Project3].[Country] = @p__linq__4)
```

### <a name="34-metadata-caching"></a>3.4 메타 데이터 캐싱

Entity Framework는 메타 데이터 캐싱도 지원 합니다. 이는 기본적으로 동일한 모델에 대 한 여러 연결에서 유형 정보 및 데이터베이스 간 매핑 정보를 캐싱하는 것입니다. 메타 데이터 캐시는 AppDomain 마다 고유 합니다.

#### <a name="341-metadata-caching-algorithm"></a>3.4.1 메타 데이터 캐싱 알고리즘

1.  모델에 대 한 메타 데이터 정보는 각 EntityConnection에 대해 ItemCollection에 저장 됩니다.
    -   참고로 모델의 각 부분에 대해 서로 다른 ItemCollection 개체가 있습니다. 예를 들어, 데이터 모델에 대 한 정보를 포함 하는 데이터 집합 Itemcollections ObjectItemCollection에는 데이터 모델에 대 한 정보가 포함 되어 있습니다. EdmItemCollection에는 개념적 모델에 대 한 정보가 포함 되어 있습니다.

2.  두 연결이 동일한 연결 문자열을 사용 하는 경우 동일한 ItemCollection 인스턴스를 공유 합니다.
3.  기능적으로 동일 하지만 서로 다른 연결 문자열은 메타 데이터 캐시를 발생 시킬 수 있습니다. 연결 문자열을 토큰화 때문에 토큰의 순서를 변경 하면 공유 메타 데이터를 생성 해야 합니다. 하지만 기능적으로 보이는 두 연결 문자열은 토큰화 후 동일 하 게 계산 되지 않을 수도 있습니다.
4.  ItemCollection은 주기적으로 사용 하기 위해 확인 됩니다. 작업 영역에 최근에 액세스 하지 않은 것으로 확인 되 면 다음 캐시 스윕에서 정리 하도록 표시 됩니다.
5.  EntityConnection을 만들면 메타 데이터 캐시가 생성 됩니다 .이 경우에는 연결을 열 때까지 항목 컬렉션이 초기화 되지 않습니다. 이 작업 영역은 캐싱 알고리즘이 "사용 도중"이 아닌 것으로 확인 될 때까지 메모리에 유지 됩니다.

고객 자문 팀 설명 큰 모델을 사용 하는 경우 "사용 중단"를 방지 하기 위해 ItemCollection에 대 한 참조를 보유 하는 블로그 게시물을 작성 했습니다: \<http://blogs.msdn.com/b/appfabriccat/archive/2010/10/22/metadataworkspace-reference-in-wcf-services.aspx>합니다.

#### <a name="342-the-relationship-between-metadata-caching-and-query-plan-caching"></a>3.4.2 메타 데이터 캐싱 및 쿼리 계획 캐싱 간의 관계

쿼리 계획 캐시 인스턴스는 MetadataWorkspace의 저장소 형식 ItemCollection에 있습니다. 이는 지정 된 MetadataWorkspace를 사용 하 여 인스턴스화된 모든 컨텍스트에 대 한 쿼리에 캐시 된 저장소 명령이 사용 됨을 의미 합니다. 또한 두 개의 연결 문자열이 약간 다르고 토큰화 이후 일치 하지 않는 경우 다른 쿼리 계획 캐시 인스턴스가 사용 됩니다.

### <a name="35-results-caching"></a>3.5 결과 캐싱

결과 캐싱 ("두 번째 수준 캐싱"이 라고도 함)을 사용 하면 쿼리 결과를 로컬 캐시에 유지할 수 있습니다. 쿼리를 실행 하는 경우 저장소에 대해 쿼리 하기 전에 먼저 결과가 로컬에서 사용 가능한 지 확인 합니다. 결과 캐싱은 Entity Framework에서 직접 지원 되지 않지만 래핑 공급자를 사용 하 여 두 번째 수준 캐시를 추가할 수 있습니다. 두 번째 수준 캐시를 사용 하는 Alachisoft의 예제 래핑 공급자는 [NCache를 기반으로 하는 두 번째 수준 캐시 Entity Framework](https://www.alachisoft.com/ncache/entity-framework.html)입니다.

두 번째 수준 캐싱 구현은 LINQ 식이 평가 (및 funcletized) 되 고 쿼리 실행 계획이 첫 번째 수준 캐시에서 계산 되거나 검색 된 후에 발생 하는 삽입 된 기능입니다. 그런 다음 두 번째 수준 캐시는 원시 데이터베이스 결과만 저장 하므로 구체화 파이프라인은 나중에 계속 실행 됩니다.

#### <a name="351-additional-references-for-results-caching-with-the-wrapping-provider"></a>래핑 공급자를 사용 하 여 결과 캐싱에 대 한 추가 참조 3.5.1

-   Julie Lerman는 Windows Server AppFabric 캐싱을 사용 하도록 샘플 래핑 공급자를 업데이트 하는 방법을 포함 하는 "두 번째 수준 캐싱 Entity Framework 및 Windows Azure" MSDN 문서를 작성 했습니다. [https://msdn.microsoft.com/magazine/hh394143.aspx](https://msdn.microsoft.com/magazine/hh394143.aspx)
-   팀 블로그 Entity Framework 5에 대 한 캐싱 공급자와 함께 실행 하는 방법을 설명 하는 게시물에 Entity Framework 5를 사용 하 여 작업 하는 경우: \<http://blogs.msdn.com/b/adonet/archive/2010/09/13/ef-caching-with-jarek-kowalski-s-provider.aspx>합니다. 또한 프로젝트에 2 단계 캐싱 추가를 자동화 하는 데 도움이 되는 T4 템플릿도 포함 합니다.

## <a name="4-autocompiled-queries"></a>자동 컴파일된 쿼리 4 개

Entity Framework를 사용 하 여 데이터베이스에 대해 쿼리를 실행 하는 경우 결과를 실제로 구체화 하기 전에 일련의 단계를 수행 해야 합니다. 이러한 단계 중 하나는 쿼리 컴파일입니다. Entity SQL 쿼리는 자동으로 캐시 되기 때문에 성능이 좋은 것으로 알려져 있으므로 동일한 쿼리를 실행 하는 두 번째 또는 세 번째는 계획 컴파일러를 건너뛰고 캐시 된 계획을 대신 사용할 수 있습니다.

Entity Framework 5에서는 LINQ to Entities 쿼리를 위한 자동 캐싱도 도입 되었습니다. 이전 버전의 Entity Framework는 성능을 향상 시킬 수 있는 CompiledQuery를 만드는 것은 LINQ to Entities 쿼리를 캐시할 수 있기 때문에 일반적인 방법 이었습니다. 이제 CompiledQuery를 사용 하지 않고 캐싱이 자동으로 수행 되므로 "autocompiled 쿼리" 기능을 호출 합니다. 쿼리 계획 캐시 및 해당 메커니즘에 대 한 자세한 내용은 쿼리 계획 캐싱을 참조 하세요.

Entity Framework는 쿼리를 다시 컴파일해야 하는 경우를 감지 하 고 이전에 컴파일된 경우라도 쿼리를 호출할 때이를 수행 합니다. 쿼리가 다시 컴파일되는 일반적인 조건은 다음과 같습니다.

-   쿼리에 연결 된 MergeOption 변경 캐시 된 쿼리를 사용 하지 않고, 대신 계획 컴파일러를 다시 실행 하 고 새로 만든 계획이 캐시 됩니다.
-   ContextOptions의 값을 변경 합니다. UseCSharpNullComparisonBehavior. MergeOption을 변경 하는 것과 동일한 결과를 얻을 수 있습니다.

다른 조건으로 인해 쿼리에서 캐시를 사용 하지 못할 수 있습니다. 일반적인 예는 다음과 같습니다.

-   IEnumerable&lt;T&gt;를 사용 합니다.&lt;&gt;(T 값)을 포함 합니다.
-   상수를 사용 하 여 쿼리를 생성 하는 함수 사용.
-   매핑되지 않은 개체의 속성 사용
-   다시 컴파일해야 하는 다른 쿼리에 쿼리를 연결 합니다.

### <a name="41-using-ienumerablelttgtcontainslttgtt-value"></a>4.1 IEnumerable&lt;T&gt;를 사용 합니다. T 값&lt;T&gt;를 포함 합니다.

Entity Framework는 IEnumerable&lt;T&gt;를 호출 하는 쿼리를 캐시 하지 않습니다. 컬렉션의 값이 volatile로 간주 되기 때문에 메모리 내 컬렉션에 대 한&lt;T&gt;(T 값)를 포함 합니다. 다음 예제 쿼리는 캐시 되지 않으므로 항상 계획 컴파일러에 의해 처리 됩니다.

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var query = context.MyEntities
                    .Where(entity => ids.Contains(entity.Id));

    var results = query.ToList();
    ...
}
```

Contains를 포함 하는 IEnumerable의 크기는 쿼리 컴파일 속도 또는 속도를 결정 합니다. 위의 예제에 나와 있는 것과 같은 큰 컬렉션을 사용 하는 경우 성능이 크게 저하 될 수 있습니다.

Entity Framework 6에는 IEnumerable&lt;T&gt;하는 방법에 대 한 최적화가 포함 되어 있습니다. 쿼리가 실행 될 때&lt;T&gt;(T 값)을 포함 합니다. 생성 되는 SQL 코드는 생성 하 고 읽기는 훨씬 더 빠르며, 대부분의 경우 서버 에서도 더 빠르게 실행 됩니다.

### <a name="42-using-functions-that-produce-queries-with-constants"></a>4.2 상수를 사용 하 여 쿼리를 생성 하는 함수 사용

Skip (), Take (), Contains () 및 DefautIfEmpty () LINQ 연산자는 매개 변수를 사용 하 여 SQL 쿼리를 생성 하지 않고 대신 전달 되는 값을 상수로 추가 합니다. 이로 인해 동일 하지 않을 수 있는 쿼리는 EF stack과 데이터베이스 서버 모두에서 쿼리 계획 캐시를 polluting 하 고 후속 쿼리 실행에서 동일한 상수를 사용 하지 않으면 다시 사용 되지 않습니다. 예를 들면 다음과 같습니다.

``` csharp
var id = 10;
...
using (var context = new MyContext())
{
    var query = context.MyEntities.Select(entity => entity.Id).Contains(id);

    var results = query.ToList();
    ...
}
```

이 예에서는이 쿼리가 실행 될 때마다 id에 대해 다른 값을 사용 하 여 쿼리를 새 계획으로 컴파일합니다.

특히 페이징을 수행할 때 Skip 및 Take 사용에 주의 해야 합니다. EF6에서 이러한 메서드에는 이러한 메서드에 전달 된 변수를 캡처하여 SQLparameters로 변환할 수 있기 때문에 캐시 된 쿼리 계획을 다시 사용할 수 있도록 하는 람다 오버 로드가 있습니다. 이를 통해 Skip 및 Take에 대해 다른 상수로 지정 된 각 쿼리는 자체 쿼리 계획 캐시 항목을 가져올 수 있으므로 캐시를 깔끔하게 유지할 수도 있습니다.

다음 코드는 최적화 되지 않지만이 클래스의 쿼리를 exemplify 하는 데만 사용할 수 있습니다.

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

이 동일한 코드의 더 빠른 버전은 람다를 사용 하 여 Skip을 호출 하는 것과 관련이 있습니다.

``` csharp
var customers = context.Customers.OrderBy(c => c.LastName);
for (var i = 0; i < count; ++i)
{
    var currentCustomer = customers.Skip(() => i).FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

두 번째 조각은 쿼리가 실행 될 때마다 동일한 쿼리 계획이 사용 되므로 CPU 시간이 절약 되 고 쿼리 캐시의 polluting 방지 되기 때문에 최대 11%까지 더 빠르게 실행할 수 있습니다. 또한 건너뛸 매개 변수는 클로저 이므로 코드는 이제 다음과 같이 표시 될 수 있습니다.

``` csharp
var i = 0;
var skippyCustomers = context.Customers.OrderBy(c => c.LastName).Skip(() => i);
for (; i < count; ++i)
{
    var currentCustomer = skippyCustomers.FirstOrDefault();
    ProcessCustomer(currentCustomer);
}
```

### <a name="43-using-the-properties-of-a-non-mapped-object"></a>4.3 매핑되지 않은 개체의 속성 사용

쿼리에서 매핑되지 않은 개체 형식의 속성을 매개 변수로 사용 하는 경우 쿼리가 캐시 되지 않습니다. 예를 들면 다음과 같습니다.

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();

    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myObject.MyProperty)
                select entity;

   var results = query.ToList();
    ...
}
```

이 예제에서는 NonMappedType 클래스가 엔터티 모델의 일부가 아닌 것으로 가정 합니다. 이 쿼리는 매핑되지 않은 형식을 사용 하지 않고 쿼리에 대 한 매개 변수로 지역 변수를 사용 하도록 쉽게 변경할 수 있습니다.

``` csharp
using (var context = new MyContext())
{
    var myObject = new NonMappedType();
    var myValue = myObject.MyProperty;
    var query = from entity in context.MyEntities
                where entity.Name.StartsWith(myValue)
                select entity;

    var results = query.ToList();
    ...
}
```

이 경우 쿼리는 캐시 되 고 쿼리 계획 캐시에서 이점을 얻을 수 있습니다.

### <a name="44-linking-to-queries-that-require-recompiling"></a>4.4 다시 컴파일하지 않아도 되는 쿼리에 연결

위와 동일한 예제를 사용 하는 경우 다시 컴파일해야 하는 쿼리를 사용 하는 두 번째 쿼리가 있으면 전체 두 번째 쿼리도 다시 컴파일됩니다. 이 시나리오를 보여 주는 예제는 다음과 같습니다.

``` csharp
int[] ids = new int[10000];
...
using (var context = new MyContext())
{
    var firstQuery = from entity in context.MyEntities
                        where ids.Contains(entity.Id)
                        select entity;

    var secondQuery = from entity in context.MyEntities
                        where firstQuery.Any(otherEntity => otherEntity.Id == entity.Id)
                        select entity;

    var results = secondQuery.ToList();
    ...
}
```

이 예는 일반적 이지만 firstQuery에 연결 하 여 secondQuery를 캐시 하지 못하게 하는 방법을 보여 줍니다. FirstQuery가 다시 컴파일해야 하는 쿼리가 아니면 secondQuery가 캐시 됩니다.

## <a name="5-notracking-queries"></a>5 NoTracking 쿼리

### <a name="51-disabling-change-tracking-to-reduce-state-management-overhead"></a>5.1 상태 관리 오버 헤드를 줄이기 위해 변경 내용 추적을 사용 하지 않도록 설정

읽기 전용 시나리오에서 개체를 ObjectStateManager로 로드 하는 오버 헤드를 방지 하려는 경우 "추적 안 함" 쿼리를 실행할 수 있습니다.  변경 내용 추적은 쿼리 수준에서 사용 하지 않도록 설정할 수 있습니다.

그러나 변경 내용 추적을 사용 하지 않도록 설정 하면 개체 캐시가 효과적으로 해제 됩니다. 엔터티를 쿼리하면 이전에는 ObjectStateManager에서 구체화 된 쿼리 결과를 끌어 구체화를 건너뛸 수 없습니다. 동일한 컨텍스트에서 동일한 엔터티에 대해 반복적으로 쿼리 하는 경우 변경 내용 추적을 사용 하도록 설정 하면 성능상의 이점을 실제로 볼 수 있습니다.

ObjectContext를 사용 하 여 쿼리 하는 경우 ObjectQuery 및 ObjectSet 인스턴스는 설정 된 후 MergeOption을 사용 하 고이를 구성 하는 쿼리는 부모 쿼리의 유효 MergeOption을 상속 합니다. DbContext를 사용 하는 경우 DbSet에서 AsNoTracking () 한정자를 호출 하 여 추적을 사용 하지 않도록 설정할 수 있습니다.

#### <a name="511-disabling-change-tracking-for-a-query-when-using-dbcontext"></a>5.1.1 DbContext를 사용 하는 경우 쿼리에 대 한 변경 내용 추적 해제

쿼리의 AsNoTracking () 메서드에 대 한 호출을 연결 하 여 쿼리 모드를 NoTracking로 전환할 수 있습니다. ObjectQuery와 달리 DbContext API의 DbSet 및 Dbset 클래스에는 MergeOption에 대 한 mutable 속성이 없습니다.

``` csharp
    var productsForCategory = from p in context.Products.AsNoTracking()
                                where p.Category.CategoryName == selectedCategory
                                select p;


```

#### <a name="512-disabling-change-tracking-at-the-query-level-using-objectcontext"></a>5.1.2 ObjectContext를 사용 하 여 쿼리 수준에서 변경 내용 추적 해제

``` csharp
    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;

    ((ObjectQuery)productsForCategory).MergeOption = MergeOption.NoTracking;
```

#### <a name="513-disabling-change-tracking-for-an-entire-entity-set-using-objectcontext"></a>5.1.3 ObjectContext를 사용 하 여 전체 엔터티 집합에 대 한 변경 내용 추적 해제

``` csharp
    context.Products.MergeOption = MergeOption.NoTracking;

    var productsForCategory = from p in context.Products
                                where p.Category.CategoryName == selectedCategory
                                select p;
```

### <a name="52test-metrics-demonstrating-the-performance-benefit-of-notracking-queries"></a>5.2 쿼리 NoTracking의 성능 이점을 보여 주는 테스트 메트릭

이 테스트에서는 Navision 모델에 대 한 추적을 NoTracking 쿼리와 비교 하 여 ObjectStateManager를 채우는 비용을 확인 합니다. Navision 모델에 대 한 설명 및 실행 된 쿼리 유형에 대해서는 부록을 참조 하세요. 이 테스트에서는 쿼리 목록을 반복 하 고 각 쿼리를 한 번 실행 합니다. NoTracking 쿼리를 사용 하 고 "AppendOnly"의 기본 병합 옵션을 사용 하 여 한 번 테스트의 두 가지 변형을 실행 했습니다. 각 변형을 3 번 실행 하 고 평균 실행 값을 사용 합니다. 테스트 중에는 SQL Server에 대 한 쿼리 캐시를 지우고 다음 명령을 실행 하 여 tempdb를 축소 합니다.

1.  DBCC DROPCLEANBUFFERS
2.  DBCC FREEPROCCACHE
3.  DBCC SHRINKDATABASE (tempdb, 0)

테스트 결과 3 개 이상 실행 됩니다.

|                        | 추적 안 함 – 작업 집합 | 추적 안 함-시간 | 추가 전용 – 작업 집합 | 추가 전용 – 시간 |
|:-----------------------|:--------------------------|:-------------------|:--------------------------|:-------------------|
| **Entity Framework 5** | 460361728                 | 1163536 밀리초         | 596545536                 | 1273042 밀리초         |
| **Entity Framework 6** | 647127040                 | 190228 밀리초          | 832798720                 | 195521 밀리초          |

Entity Framework 5는 실행이 끝날 때 Entity Framework 6 보다 작은 메모리 사용 공간을 차지 합니다. Entity Framework 6에서 사용 하는 추가 메모리는 추가 메모리 구조와 새 기능 및 성능 향상을 위한 코드의 결과입니다.

또한 ObjectStateManager를 사용 하는 경우 메모리 사용 공간에 명확한 차이가 있습니다. 데이터베이스에서 구체화 한 모든 엔터티를 추적할 때 5 Entity Framework의 공간이 30% 늘어납니다. 이 작업을 수행 하는 경우 6%의 공간 크기가 28% Entity Framework.

시간을 기준으로이 테스트에서 우수함 6을 Entity Framework 하 여 Entity Framework 합니다. Entity Framework 6 Entity Framework 5에서 사용한 시간의 약 16%에 해당 하는 테스트를 완료 했습니다. 또한 Entity Framework 5는 ObjectStateManager를 사용 하는 경우 완료 하는 데 9% 더 많은 시간이 걸립니다. 반면, Entity Framework 6은 ObjectStateManager를 사용할 때 3% 더 많은 시간을 사용 합니다.

## <a name="6-query-execution-options"></a>6 쿼리 실행 옵션

Entity Framework는 다양 한 쿼리 방법을 제공 합니다. 다음 옵션을 살펴보고 각 옵션의 장단점을 비교 하 고 성능 특성을 검토 합니다.

-   LINQ to Entities.
-   추적 LINQ to Entities 없습니다.
-   ObjectQuery를 Entity SQL 합니다.
-   EntityCommand를 Entity SQL 합니다.
-   System.data.objects.objectcontext.executestorequery.
-   SqlQuery.
-   CompiledQuery.

### <a name="61-linq-to-entities-queries"></a>6.1 LINQ to Entities 쿼리

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

**들**

-   CUD 작업에 적합 합니다.
-   완전히 구체화 된 개체입니다.
-   프로그래밍 언어에 기본 제공 되는 구문을 사용 하 여 작성 하는 것이 가장 간단 합니다.
-   뛰어난 성능.

**단점**

-   특정 기술적 제한 사항 (예:
    -   외부 조인 쿼리에 대해 System.linq.enumerable.defaultifempty를 사용 하는 패턴은 Entity SQL의 단순 외부 조인 문 보다 더 복잡 한 쿼리를 생성 합니다.
    -   여전히 일반 패턴 일치와 마찬가지로를 사용할 수 없습니다.

### <a name="62-no-tracking-linq-to-entities-queries"></a>6.2 추적 LINQ to Entities 쿼리가 없습니다.

컨텍스트가 ObjectContext를 파생 하는 경우:

``` csharp
context.Products.MergeOption = MergeOption.NoTracking;
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
```

컨텍스트가 DbContext에서 파생 되는 경우:

``` csharp
var q = context.Products.AsNoTracking()
                        .Where(p => p.Category.CategoryName == "Beverages");
```

**들**

-   정기 LINQ 쿼리를 통해 성능이 향상 되었습니다.
-   완전히 구체화 된 개체입니다.
-   프로그래밍 언어에 기본 제공 되는 구문을 사용 하 여 작성 하는 것이 가장 간단 합니다.

**단점**

-   CUD 작업에 적합 하지 않습니다.
-   특정 기술적 제한 사항 (예:
    -   외부 조인 쿼리에 대해 System.linq.enumerable.defaultifempty를 사용 하는 패턴은 Entity SQL의 단순 외부 조인 문 보다 더 복잡 한 쿼리를 생성 합니다.
    -   여전히 일반 패턴 일치와 마찬가지로를 사용할 수 없습니다.

스칼라 속성을 프로젝션 하는 쿼리는 NoTracking가 지정 되지 않은 경우에도 추적 되지 않습니다. 예를 들면 다음과 같습니다.

``` csharp
var q = context.Products.Where(p => p.Category.CategoryName == "Beverages").Select(p => new { p.ProductName });
```

이 특정 쿼리는 명시적으로 NoTracking를 지정 하지 않지만 개체 상태 관리자에 알려진 형식을 구체화 하지 않으므로 구체화 된 결과가 추적 되지 않습니다.

### <a name="63-entity-sql-over-an-objectquery"></a>6.3 Entity SQL ObjectQuery

``` csharp
ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
```

**들**

-   CUD 작업에 적합 합니다.
-   완전히 구체화 된 개체입니다.
-   는 쿼리 계획 캐싱을 지원 합니다.

**단점**

-   에는 언어에 기본 제공 되는 쿼리 구문 보다 사용자 오류가 발생 하기 쉬운 텍스트 쿼리 문자열이 포함 됩니다.

### <a name="64-entity-sql-over-an-entity-command"></a>6.4 엔터티 명령 Entity SQL

``` csharp
EntityCommand cmd = eConn.CreateCommand();
cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
{
    while (reader.Read())
    {
        // manually 'materialize' the product
    }
}
```

**들**

-   .NET 4.0에서 쿼리 계획 캐싱을 지원 합니다. 계획 캐싱은 .NET 4.5의 다른 모든 쿼리 형식에서 지원 됩니다.

**단점**

-   에는 언어에 기본 제공 되는 쿼리 구문 보다 사용자 오류가 발생 하기 쉬운 텍스트 쿼리 문자열이 포함 됩니다.
-   CUD 작업에 적합 하지 않습니다.
-   결과는 자동으로 구체화 되지 않으므로 데이터 판독기에서 읽어야 합니다.

### <a name="65-sqlquery-and-executestorequery"></a>6.5 SqlQuery 및 System.data.objects.objectcontext.executestorequery

데이터베이스의 SqlQuery:

``` csharp
// use this to obtain entities and not track them
var q1 = context.Database.SqlQuery<Product>("select * from products");
```

DbSet의 SqlQuery:

``` csharp
// use this to obtain entities and have them tracked
var q2 = context.Products.SqlQuery("select * from products");
```

ExecyteStoreQuery:

``` csharp
var beverages = context.ExecuteStoreQuery<Product>(
@"     SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued, P.DiscontinuedDate
       FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
       WHERE        (C.CategoryName = 'Beverages')"
);
```

**들**

-   일반적으로 계획 컴파일러부터 가장 빠른 성능을 무시 합니다.
-   완전히 구체화 된 개체입니다.
-   DbSet에서 사용 될 경우 CUD 작업에 적합 합니다.

**단점**

-   쿼리가 텍스트 이며 오류가 발생 하기 쉽습니다.
-   쿼리는 개념적 의미 체계 대신 저장소 의미 체계를 사용 하 여 특정 백 엔드에 연결 됩니다.
-   상속이 있으면 작업 query는 요청 된 유형에 대 한 매핑 조건을 고려해 야 합니다.

### <a name="66-compiledquery"></a>6.6 CompiledQuery

``` csharp
private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
    (NorthwindEntities context, string categoryName) =>
        context.Products.Where(p => p.Category.CategoryName == categoryName)
        );
…
var q = context.InvokeProductsForCategoryCQ("Beverages");
```

**들**

-   는 일반 LINQ 쿼리에 비해 최대 7% 성능 향상을 제공 합니다.
-   완전히 구체화 된 개체입니다.
-   CUD 작업에 적합 합니다.

**단점**

-   복잡성 및 프로그래밍 오버 헤드가 증가 했습니다.
-   컴파일된 쿼리를 기반으로 작성 하는 경우 성능 향상이 손실 됩니다.
-   일부 LINQ 쿼리는 CompiledQuery으로 작성할 수 없습니다 (예: 무명 형식의 프로젝션).

### <a name="67-performance-comparison-of-different-query-options"></a>6.7 다른 쿼리 옵션의 성능 비교

컨텍스트 만들기가 시간 지정 되지 않은 간단한 마이크로 벤치 마크가 테스트에 추가 되었습니다. 제어 되는 환경에서 캐시 되지 않은 엔터티 집합에 대 한 5000 번 쿼리를 측정 했습니다. 이러한 숫자는 경고와 함께 사용 됩니다. 응용 프로그램에서 생성 된 실제 숫자를 반영 하지는 않지만, 대신 다른 쿼리 옵션을 비교할 때의 성능 차이를 매우 정확 하 게 측정 하는 것입니다. 새 컨텍스트를 만드는 비용을 제외 하 고 사과를 사과 합니다.

| EF  | 테스트                                 | 시간 (ms) | 메모리   |
|:----|:-------------------------------------|:----------|:---------|
| EF5 | ObjectContext ESQL                   | 2414      | 38801408 |
| EF5 | ObjectContext Linq 쿼리             | 2692      | 38277120 |
| EF5 | DbContext Linq 쿼리 추적 안 함     | 2818      | 41840640 |
| EF5 | DbContext Linq Query                 | 2930      | 41771008 |
| EF5 | ObjectContext Linq 쿼리 추적 안 함 | 3013      | 38412288 |
|     |                                      |           |          |
| EF6 | ObjectContext ESQL                   | 2059      | 46039040 |
| EF6 | ObjectContext Linq 쿼리             | 3074      | 45248512 |
| EF6 | DbContext Linq 쿼리 추적 안 함     | 3125      | 47575040 |
| EF6 | DbContext Linq Query                 | 3420      | 47652864 |
| EF6 | ObjectContext Linq 쿼리 추적 안 함 | 3593      | 45260800 |

![EF5 마이크로 벤치 마크, 5000 웜 반복](~/ef6/media/ef5micro5000warm.png)

![EF6 마이크로 벤치 마크, 5000 웜 반복](~/ef6/media/ef6micro5000warm.png)

마이크로 벤치 마크는 코드의 작은 변경 내용에 매우 중요 합니다. 이 경우 Entity Framework 5와 Entity Framework 6의 비용 간 차이는 [가로채기](~/ef6/fundamentals/logging-and-interception.md) 및 [트랜잭션 개선 사항이](~/ef6/saving/transactions.md)추가 되었기 때문입니다. 그러나 이러한 마이크로 벤치 마크 수치는 Entity Framework의 매우 작은 부분으로 증폭 된 비전입니다. 웜 쿼리의 실제 시나리오에서는 Entity Framework 5에서 Entity Framework 6으로 업그레이드할 때 성능 재발이 표시 되지 않습니다.

다른 쿼리 옵션의 실제 성능을 비교 하기 위해 다른 쿼리 옵션을 사용 하 여 범주 이름이 "음료" 인 모든 제품을 선택 하는 별도의 테스트 변형을 5 개 만들었습니다. 각 반복에는 컨텍스트를 만드는 비용과 반환 된 모든 엔터티를 구체화 하는 비용이 포함 됩니다. 1000 시간 반복의 합계를 계산 하기 전에 10 회 반복 실행이 시간 초과 되었습니다. 표시 되는 결과는 각 테스트의 5 번 실행에서 가져온 중앙값입니다. 자세한 내용은 테스트에 대 한 코드를 포함 하는 부록 B를 참조 하세요.

| EF  | 테스트                                        | 시간 (ms) | 메모리   |
|:----|:--------------------------------------------|:----------|:---------|
| EF5 | ObjectContext 엔터티 명령                | 621       | 39350272 |
| EF5 | 데이터베이스의 DbContext Sql 쿼리             | 825       | 37519360 |
| EF5 | ObjectContext 저장소 쿼리                   | 878       | 39460864 |
| EF5 | ObjectContext Linq 쿼리 추적 안 함        | 969       | 38293504 |
| EF5 | 개체 쿼리를 사용 하는 ObjectContext Entity Sql | 1089      | 38981632 |
| EF5 | ObjectContext 컴파일된 쿼리                | 1099      | 38682624 |
| EF5 | ObjectContext Linq 쿼리                    | 1152      | 38178816 |
| EF5 | DbContext Linq 쿼리 추적 안 함            | 1208      | 41803776 |
| EF5 | DbSet의 DbContext Sql 쿼리                | 1414      | 37982208 |
| EF5 | DbContext Linq Query                        | 1574      | 41738240 |
|     |                                             |           |          |
| EF6 | ObjectContext 엔터티 명령                | 480       | 47247360 |
| EF6 | ObjectContext 저장소 쿼리                   | 493       | 46739456 |
| EF6 | 데이터베이스의 DbContext Sql 쿼리             | 614       | 41607168 |
| EF6 | ObjectContext Linq 쿼리 추적 안 함        | 684       | 46333952 |
| EF6 | 개체 쿼리를 사용 하는 ObjectContext Entity Sql | 767       | 48865280 |
| EF6 | ObjectContext 컴파일된 쿼리                | 788       | 48467968 |
| EF6 | DbContext Linq 쿼리 추적 안 함            | 878       | 47554560 |
| EF6 | ObjectContext Linq 쿼리                    | 953       | 47632384 |
| EF6 | DbSet의 DbContext Sql 쿼리                | 1023      | 41992192 |
| EF6 | DbContext Linq Query                        | 1290      | 47529984 |


![EF5 웜 쿼리 1000 반복](~/ef6/media/ef5warmquery1000.png)

![EF6 웜 쿼리 1000 반복](~/ef6/media/ef6warmquery1000.png)

> [!NOTE]
> 완전성을 위해 EntityCommand에 대 한 Entity SQL 쿼리를 실행 하는 변형이 포함 되었습니다. 그러나 이러한 쿼리에 대 한 결과가 구체화 되지 않기 때문에 이러한 비교는 반드시 사과를 사과로 변환할 수 없습니다. 테스트에는 비교 fairer를 시도 하기 위해 구체화 하는 근접 한 근사값이 포함 됩니다.

이 종단 간 사례에서는 DbContext 초기화가 훨씬 더 빠르고 MetadataCollection&lt;T&gt; 조회를 포함 하 여 몇 가지 스택의 일부에 대 한 성능 향상으로 인해 Entity Framework 6 우수함 Entity Framework 5입니다.

## <a name="7-design-time-performance-considerations"></a>7 디자인 타임 성능 고려 사항

### <a name="71-inheritance-strategies"></a>7.1 상속 전략

Entity Framework를 사용 하는 경우 다른 성능 고려 사항은 사용 하는 상속 전략입니다. Entity Framework는 세 가지 기본 유형의 상속과 조합을 지원 합니다.

-   TPH (계층당 하나의 테이블) – 각 상속 집합이 행에 표시 되는 계층의 특정 형식을 나타내는 판별자 열이 있는 테이블에 매핑됩니다.
-   형식당 하나의 테이블 (TPT) – 각 형식에는 데이터베이스에 고유한 테이블이 있습니다. 자식 테이블은 부모 테이블에 포함 되지 않은 열만 정의 합니다.
-   TPC (클래스 단위)-각 형식에는 데이터베이스의 자체 전체 테이블이 있습니다. 자식 테이블은 부모 형식에 정의 된 필드를 포함 하 여 모든 필드를 정의 합니다.

모델에서 TPT 상속을 사용 하는 경우 생성 되는 쿼리는 다른 상속 전략을 사용 하 여 생성 되는 쿼리보다 복잡 하므로 저장소에서 실행 시간이 길어질 수 있습니다.  일반적으로 TPT 모델에 대 한 쿼리를 생성 하 고 결과 개체를 구체화 하는 데 더 많은 시간이 걸립니다.

"성능 고려 사항을 참조 Entity Framework에서 TPT (형식당 하나의 테이블) 상속을 사용 하는 경우" MSDN 블로그 게시: \<http://blogs.msdn.com/b/adonet/archive/2010/08/17/performance-considerations-when-using-tpt-table-per-type-inheritance-in-the-entity-framework.aspx>합니다.

#### <a name="711-avoiding-tpt-in-model-first-or-code-first-applications"></a>Model First 또는 Code First 응용 프로그램의 TPT 방지 7.1.1

TPT 스키마가 있는 기존 데이터베이스를 통해 모델을 만드는 경우에는 여러 가지 옵션을 사용할 수 없습니다. 그러나 Model First 또는 Code First를 사용 하 여 응용 프로그램을 만들 때 성능 문제에 대 한 TPT 상속을 피해 야 합니다.

Entity Designer 마법사에서 Model First 사용 하는 경우 모델의 모든 상속에 대 한 TPT을 얻게 됩니다. Model First를 사용 하 여 TPH 상속 전략으로 전환 하려는 경우 Visual Studio 갤러리에서 "엔터티 디자이너 데이터베이스 생성 전원 팩" 사용 가능한를 사용할 수 있습니다 ( \<http://visualstudiogallery.msdn.microsoft.com/df3541c3-d833-4b65-b942-989e7ec74c87/>)합니다.

Code First를 사용 하 여 상속 된 모델의 매핑을 구성 하는 경우 EF는 기본적으로 TPH를 사용 하므로 상속 계층 구조의 모든 엔터티는 동일한 테이블에 매핑됩니다. MSDN Magazine의 "코드 엔터티 Framework4.1에서 첫 번째." 문서 "매핑 Fluent API를 사용한" 섹션을 참조 하세요 ( [http://msdn.microsoft.com/magazine/hh126815.aspx](https://msdn.microsoft.com/magazine/hh126815.aspx)) 대 한 자세한 내용은 합니다.

### <a name="72-upgrading-from-ef4-to-improve-model-generation-time"></a>7.2 모델 생성 시간을 개선 하기 위해 EF4에서 업그레이드

모델의 SSDL (저장소 계층)을 생성 하는 알고리즘에 대 한 SQL Server의 향상 된 기능은 Entity Framework 5와 6에서 사용할 수 있으며, Visual Studio 2010 s p 1이 설치 될 때 Entity Framework 4로 업데이트 됩니다. 다음 테스트 결과는 매우 큰 모델 (이 경우 Navision 모델)을 생성할 때 향상 된 기능을 보여 줍니다. 자세한 내용은 부록 C를 참조 하세요.

모델에는 1005 엔터티 집합과 4227 연결 집합이 포함 되어 있습니다.

| 구성                              | 사용 된 시간 분석                                                                                                                                               |
|:-------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Visual Studio 2010, Entity Framework 4     | SSDL 생성: 2 hr 27 분 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> ObjectLayer 생성: 1 초 <br/> 생성 보기: 2 h 14 분 |
| Visual Studio 2010 SP1, Entity Framework 4 | SSDL 생성: 1 초 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> ObjectLayer 생성: 1 초 <br/> 생성 보기: 1 hr 53 분   |
| Visual Studio 2013, Entity Framework 5     | SSDL 생성: 1 초 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> ObjectLayer 생성: 1 초 <br/> 생성 보기: 65 분    |
| Visual Studio 2013, Entity Framework 6     | SSDL 생성: 1 초 <br/> 매핑 생성: 1 초 <br/> CSDL 생성: 1 초 <br/> ObjectLayer 생성: 1 초 <br/> 뷰 생성: 28 초.   |


SSDL을 생성할 때 클라이언트 개발 컴퓨터가 서버에서 결과가 반환 될 때까지 유휴 상태로 대기 하는 동안에는 SQL Server에서 로드가 거의 완전히 소비 됩니다. Dba는 특히 이러한 개선 사항을 이해 해야 합니다. 또한 기본적으로 모델 생성의 전체 비용이 생성 된 보기에서 발생 합니다.

### <a name="73-splitting-large-models-with-database-first-and-model-first"></a>7.3 Database First 및 Model First으로 많은 모델 분할

모델 크기가 늘어나면 디자이너 화면이 복잡해 집니다. 일반적으로 디자이너를 효과적으로 사용 하기 위해 너무 많은 엔터티가 300 개 이상인 모델을 고려 합니다. 큰 모델을 분할 하기 위한 여러 옵션을 설명 하는 다음 블로그 게시물: \<http://blogs.msdn.com/b/adonet/archive/2008/11/25/working-with-large-models-in-entity-framework-part-2.aspx>합니다.

첫 번째 버전의 Entity Framework에 대 한 게시물이 작성 되었지만 단계가 여전히 적용 됩니다.

### <a name="74-performance-considerations-with-the-entity-data-source-control"></a>7.4 엔터티 데이터 소스 컨트롤을 사용 하 여 성능 고려 사항

EntityDataSource 컨트롤을 사용 하는 웹 응용 프로그램의 성능이 상당히 deteriorates는 다중 스레드 성능 및 스트레스 테스트 사례를 살펴보았습니다. 근본 원인은 EntityDataSource가 웹 응용 프로그램에서 참조 하는 어셈블리에 대해 LoadFromAssembly를 반복적으로 호출 하 여 엔터티로 사용할 형식을 검색 하는 것입니다.

해결 방법은 EntityDataSource의 ContextTypeName를 파생 된 ObjectContext 클래스의 형식 이름으로 설정 하는 것입니다. 그러면 엔터티 형식에 대해 참조 된 모든 어셈블리를 검색 하는 메커니즘이 꺼집니다.

또한 ContextTypeName 필드를 설정 하면 리플렉션을 통해 어셈블리에서 형식을 로드할 수 없을 때 .NET 4.0의 EntityDataSource가 ReflectionTypeLoadException를 throw 하는 기능 문제가 방지 됩니다. 이 문제는 .NET 4.5에서 해결 되었습니다.

### <a name="75-poco-entities-and-change-tracking-proxies"></a>7.5 POCO 엔터티 및 변경 내용 추적 프록시

Entity Framework를 사용 하면 데이터 클래스 자체를 수정 하지 않고도 데이터 모델과 함께 사용자 지정 데이터 클래스를 사용할 수 있습니다. 즉, 기존 도메인 개체 등의 POCO(Plain Old CLR Object)를 데이터 모델과 함께 사용할 수 있습니다. 데이터 모델에 정의 된 엔터티에 매핑되는 이러한 POCO 데이터 클래스 (지 속성 무시 개체 라고도 함)는 엔터티 데이터 모델 도구에서 생성 된 엔터티 형식과 동일한 쿼리, 삽입, 업데이트 및 삭제 동작을 대부분 지원 합니다.

Entity Framework는 poco 형식에서 파생 된 프록시 클래스를 만들 수도 있습니다 .이 클래스는 POCO 엔터티에 대 한 지연 로드 및 자동 변경 내용 추적과 같은 기능을 사용 하도록 설정할 때 사용 됩니다. POCO 클래스에는 여기에 설명 된 대로 Entity Framework 프록시를 사용할 수 있도록 특정 요구 사항을 충족 해야 합니다. [http://msdn.microsoft.com/library/dd468057.aspx](https://msdn.microsoft.com/library/dd468057.aspx)합니다.

엔터티 속성의 값이 변경 될 때마다 추적 프록시가 개체 상태 관리자에 게 알리기 때문에 Entity Framework 엔터티의 실제 상태를 항상 알 수 있습니다. 이렇게 하려면 속성의 setter 메서드 본문에 알림 이벤트를 추가 하 고 해당 이벤트를 처리 하는 개체 상태 관리자를 수행 합니다. 프록시 엔터티를 만드는 것은 Entity Framework에서 만든 이벤트의 추가 된 집합으로 인해 프록시 POCO 엔터티를 만드는 것 보다 일반적으로 비용이 많이 듭니다.

POCO 엔터티에 변경 내용 추적 프록시가 없으면 엔터티의 콘텐츠를 이전에 저장 된 상태의 복사본과 비교 하 여 변경 내용을 찾을 수 있습니다. 이러한 심층 비교는 컨텍스트에 많은 엔터티가 있거나, 마지막 비교가 수행 된 이후에 변경 된 내용이 없는 경우에도 엔터티에 매우 많은 양의 속성이 있는 경우 시간이 오래 걸리는 프로세스입니다.

요약 하자면, 변경 내용 추적 프록시를 만들 때 성능 저하가 발생 하지만 변경 내용 추적을 통해 엔터티에 많은 속성이 있거나 모델에 많은 엔터티가 있는 경우 변경 내용 검색 프로세스의 속도를 높일 수 있습니다. 엔터티 양이 너무 많이 증가 하지 않는 적은 수의 속성을 가진 엔터티의 경우 변경 내용 추적 프록시가 그다지 유용 하지 않을 수 있습니다.

## <a name="8-loading-related-entities"></a>8 개 관련 엔터티 로드 중

### <a name="81-lazy-loading-vs-eager-loading"></a>8.1 지연 로드 대비 즉시 로드

Entity Framework는 대상 엔터티와 관련 된 엔터티를 로드 하는 여러 가지 방법을 제공 합니다. 예를 들어 제품을 쿼리 하는 경우 관련 주문이 개체 상태 관리자에 로드 되는 방법이 다릅니다. 성능 관점에서 볼 때 관련 엔터티를 로드할 때 가장 중요 한 질문은 지연 로드를 사용할지 즉시 로드를 사용할지 여부입니다.

즉시 로드를 사용 하는 경우 관련 엔터티가 대상 엔터티 집합과 함께 로드 됩니다. 쿼리에서 Include 문을 사용 하 여 가져올 관련 엔터티를 표시 합니다.

지연 로드를 사용 하는 경우 초기 쿼리는 대상 엔터티 집합에만 제공 됩니다. 그러나 탐색 속성에 액세스할 때마다 저장소에 대해 다른 쿼리를 실행 하 여 관련 엔터티를 로드 합니다.

엔터티가 로드 된 후에는 지연 로드를 사용 하 든 즉시 로드를 사용 하는지에 관계 없이 엔터티를 위한 추가 쿼리는 개체 상태 관리자에서 직접 로드 합니다.

### <a name="82-how-to-choose-between-lazy-loading-and-eager-loading"></a>8.2 지연 로드와 즉시 로드 중에서 선택 하는 방법

중요 한 것은 응용 프로그램에 대해 적절 한 선택을 수행할 수 있도록 지연 로드와 즉시 로드 간의 차이점을 이해 하는 것입니다. 이를 통해 데이터베이스에 대 한 여러 요청과 많은 페이로드를 포함할 수 있는 단일 요청 간의 균형을 평가할 수 있습니다. 응용 프로그램의 일부에서 즉시 로드를 사용 하 고 다른 부분의 지연 로드를 사용 하는 것이 적합할 수 있습니다.

내부적으로 발생 하는 상황에 대 한 예로 영국에 거주 하는 고객 및 주문 수를 쿼리해야 한다고 가정 합니다.

**즉시 로드 사용**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var ukCustomers = context.Customers.Include(c => c.Orders).Where(c => c.Address.Country == "UK");
    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

**지연 로드 사용**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    context.ContextOptions.LazyLoadingEnabled = true;

    //Notice that the Include method call is missing in the query
    var ukCustomers = context.Customers.Where(c => c.Address.Country == "UK");

    var chosenCustomer = AskUserToPickCustomer(ukCustomers);
    Console.WriteLine("Customer Id: {0} has {1} orders", customer.CustomerID, customer.Orders.Count);
}
```

즉시 로드를 사용 하는 경우 모든 고객 및 모든 주문을 반환 하는 단일 쿼리를 실행 합니다. 저장소 명령은 다음과 같습니다.

``` SQL
SELECT
[Project1].[C1] AS [C1],
[Project1].[CustomerID] AS [CustomerID],
[Project1].[CompanyName] AS [CompanyName],
[Project1].[ContactName] AS [ContactName],
[Project1].[ContactTitle] AS [ContactTitle],
[Project1].[Address] AS [Address],
[Project1].[City] AS [City],
[Project1].[Region] AS [Region],
[Project1].[PostalCode] AS [PostalCode],
[Project1].[Country] AS [Country],
[Project1].[Phone] AS [Phone],
[Project1].[Fax] AS [Fax],
[Project1].[C2] AS [C2],
[Project1].[OrderID] AS [OrderID],
[Project1].[CustomerID1] AS [CustomerID1],
[Project1].[EmployeeID] AS [EmployeeID],
[Project1].[OrderDate] AS [OrderDate],
[Project1].[RequiredDate] AS [RequiredDate],
[Project1].[ShippedDate] AS [ShippedDate],
[Project1].[ShipVia] AS [ShipVia],
[Project1].[Freight] AS [Freight],
[Project1].[ShipName] AS [ShipName],
[Project1].[ShipAddress] AS [ShipAddress],
[Project1].[ShipCity] AS [ShipCity],
[Project1].[ShipRegion] AS [ShipRegion],
[Project1].[ShipPostalCode] AS [ShipPostalCode],
[Project1].[ShipCountry] AS [ShipCountry]
FROM ( SELECT
      [Extent1].[CustomerID] AS [CustomerID],
       [Extent1].[CompanyName] AS [CompanyName],
       [Extent1].[ContactName] AS [ContactName],
       [Extent1].[ContactTitle] AS [ContactTitle],
       [Extent1].[Address] AS [Address],
       [Extent1].[City] AS [City],
       [Extent1].[Region] AS [Region],
       [Extent1].[PostalCode] AS [PostalCode],
       [Extent1].[Country] AS [Country],
       [Extent1].[Phone] AS [Phone],
       [Extent1].[Fax] AS [Fax],
      1 AS [C1],
       [Extent2].[OrderID] AS [OrderID],
       [Extent2].[CustomerID] AS [CustomerID1],
       [Extent2].[EmployeeID] AS [EmployeeID],
       [Extent2].[OrderDate] AS [OrderDate],
       [Extent2].[RequiredDate] AS [RequiredDate],
       [Extent2].[ShippedDate] AS [ShippedDate],
       [Extent2].[ShipVia] AS [ShipVia],
       [Extent2].[Freight] AS [Freight],
       [Extent2].[ShipName] AS [ShipName],
       [Extent2].[ShipAddress] AS [ShipAddress],
       [Extent2].[ShipCity] AS [ShipCity],
       [Extent2].[ShipRegion] AS [ShipRegion],
       [Extent2].[ShipPostalCode] AS [ShipPostalCode],
       [Extent2].[ShipCountry] AS [ShipCountry],
      CASE WHEN ([Extent2].[OrderID] IS NULL) THEN CAST(NULL AS int) ELSE 1 END AS [C2]
      FROM  [dbo].[Customers] AS [Extent1]
      LEFT OUTER JOIN [dbo].[Orders] AS [Extent2] ON [Extent1].[CustomerID] = [Extent2].[CustomerID]
      WHERE N'UK' = [Extent1].[Country]
)  AS [Project1]
ORDER BY [Project1].[CustomerID] ASC, [Project1].[C2] ASC
```

지연 로드를 사용 하는 경우 처음에 다음 쿼리를 실행 합니다.

``` SQL
SELECT
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[CompanyName] AS [CompanyName],
[Extent1].[ContactName] AS [ContactName],
[Extent1].[ContactTitle] AS [ContactTitle],
[Extent1].[Address] AS [Address],
[Extent1].[City] AS [City],
[Extent1].[Region] AS [Region],
[Extent1].[PostalCode] AS [PostalCode],
[Extent1].[Country] AS [Country],
[Extent1].[Phone] AS [Phone],
[Extent1].[Fax] AS [Fax]
FROM [dbo].[Customers] AS [Extent1]
WHERE N'UK' = [Extent1].[Country]
```

그리고 고객의 주문 탐색 속성에 액세스할 때마다 다음과 같은 다른 쿼리가 스토어에 대해 발급 됩니다.

``` SQL
exec sp_executesql N'SELECT
[Extent1].[OrderID] AS [OrderID],
[Extent1].[CustomerID] AS [CustomerID],
[Extent1].[EmployeeID] AS [EmployeeID],
[Extent1].[OrderDate] AS [OrderDate],
[Extent1].[RequiredDate] AS [RequiredDate],
[Extent1].[ShippedDate] AS [ShippedDate],
[Extent1].[ShipVia] AS [ShipVia],
[Extent1].[Freight] AS [Freight],
[Extent1].[ShipName] AS [ShipName],
[Extent1].[ShipAddress] AS [ShipAddress],
[Extent1].[ShipCity] AS [ShipCity],
[Extent1].[ShipRegion] AS [ShipRegion],
[Extent1].[ShipPostalCode] AS [ShipPostalCode],
[Extent1].[ShipCountry] AS [ShipCountry]
FROM [dbo].[Orders] AS [Extent1]
WHERE [Extent1].[CustomerID] = @EntityKeyValue1',N'@EntityKeyValue1 nchar(5)',@EntityKeyValue1=N'AROUT'
```

자세한 내용은 [관련 개체 로드](https://msdn.microsoft.com/library/bb896272.aspx)를 참조 하세요.

#### <a name="821-lazy-loading-versus-eager-loading-cheat-sheet"></a>8.2.1 지연 로드 및 즉시 로드 참고 자료 시트

즉시 로드와 지연 로드를 선택 하는 데에는 한 가지 크기의 제한이 없습니다. 잘 의사 결정을 내릴 수 있도록 두 전략 간의 차이점을 이해 하기 위해 먼저 시도 합니다. 또한 코드가 다음 시나리오에 적합 한지 고려해 야 합니다.

| 시나리오                                                                    | 제안 사항                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
|:----------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 인출 된 엔터티에서 많은 탐색 속성에 액세스 해야 하나요? | **아니요** -두 옵션을 모두 수행할 수 있습니다. 그러나 쿼리가 가져오는 페이로드가 너무 크지 않은 경우 즉시 로드를 사용 하면 개체를 구체화 하는 데 네트워크 왕복이 더 필요 하지 않으므로 성능이 향상 될 수 있습니다. <br/> <br/> **예** -엔터티에서 많은 탐색 속성에 액세스 해야 하는 경우 즉시 로드를 사용 하 여 쿼리에 여러 개의 include 문을 사용 하면 됩니다. 포함 하는 엔터티가 많을 수록 쿼리에서 반환 하는 페이로드가 커집니다. 쿼리에 세 개 이상의 엔터티를 포함 한 후에는 지연 로드로 전환 하는 것이 좋습니다. |
| 런타임에 필요한 데이터를 정확 하 게 알고 있나요?                   | 지연 로드는 사용자에 게 더 적합 합니다. 그렇지 않은 경우에는 필요 하지 않은 데이터에 대 한 쿼리를 종료할 수 있습니다. <br/> <br/> **예** -즉시 로드가 최상의 방법일 수 있습니다. 이를 통해 전체 집합을 더 빠르게 로드할 수 있습니다. 쿼리에서 매우 많은 양의 데이터를 인출 해야 하는데 속도가 너무 느리면 대신 지연 로드를 시도 하십시오.                                                                                                                                                                                                                                                       |
| 코드가 데이터베이스에서 지금 실행 되 고 있나요? (네트워크 대기 시간 증가)  | **아니요** -네트워크 대기 시간이 문제가 아니면 지연 로드를 사용 하면 코드를 간소화할 수 있습니다. 응용 프로그램의 토폴로지가 변경 될 수 있으므로 부여 된 데이터베이스에 대 한 근접성을 사용 하지 마십시오. <br/> <br/> **예** -네트워크가 문제인 경우 시나리오에 적합 한 항목을 결정할 수 있습니다. 일반적으로 즉시 로드는 왕복이 더 적기 때문에 더 효율적입니다.                                                                                                                                                                                                      |


#### <a name="822-performance-concerns-with-multiple-includes"></a>여러 포함의 성능 문제 8.2.2

서버 응답 시간 문제를 포함 하는 성능 관련 질문이 있는 경우 문제의 원인에는 여러 개의 Include 문이 있는 쿼리가 자주 발생 합니다. 쿼리에 관련 된 엔터티를 포함 하는 것은 매우 강력 하지만 내부적으로 발생 하는 일을 이해 하는 것이 중요 합니다.

내부 계획 컴파일러를 통해 저장 명령을 생성 하기 위해 여러 Include 문을 포함 하는 쿼리는 상대적으로 시간이 오래 걸립니다. 이 시간의 대부분은 결과 쿼리를 최적화 하는 데 소요 됩니다. 생성 된 저장소 명령은 매핑에 따라 각 포함에 대 한 외부 조인 또는 공용 구조체를 포함 합니다. 이와 같은 쿼리는 데이터베이스에서 큰 연결 된 그래프를 단일 페이로드에 acerbate, 특히 페이로드에 중복성이 많은 경우 (예: 여러 수준의 포함을 사용 하는 경우)에는 모든 대역폭 문제를 해결 합니다. 일대다 방향의 연결입니다.

ToTraceString을 사용 하 고 SQL Server Management Studio의 저장소 명령을 실행 하 여 페이로드 크기를 확인 하 여 쿼리가 과도 하 게 큰 페이로드를 반환 하는 경우를 확인할 수 있습니다. 이러한 경우 필요한 데이터를 가져오기 위해 쿼리에서 Include 문의 수를 줄일 수 있습니다. 또는 다음과 같이 더 작은 하위 쿼리 시퀀스로 쿼리를 중단할 수 있습니다.

**쿼리를 중단 하기 전에:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var customers = from c in context.Customers.Include(c => c.Orders)
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

**쿼리를 중단 한 후:**

``` csharp
using (NorthwindEntities context = new NorthwindEntities())
{
    var orders = from o in context.Orders
                 where o.Customer.LastName.StartsWith(lastNameParameter)
                 select o;

    orders.Load();

    var customers = from c in context.Customers
                    where c.LastName.StartsWith(lastNameParameter)
                    select c;

    foreach (Customer customer in customers)
    {
        ...
    }
}
```

이는 컨텍스트에서 id 확인 및 연결 픽스업을 자동으로 수행 해야 하는 기능을 사용 하기 때문에 추적 된 쿼리에만 적용 됩니다.

지연 로드와 마찬가지로 더 작은 페이로드에 대해 더 많은 쿼리가 사용 됩니다. 개별 속성의 프로젝션을 사용 하 여 각 엔터티에서 필요한 데이터만 명시적으로 선택할 수도 있지만이 경우에는 엔터티를 로드 하지 않으며 업데이트가 지원 되지 않습니다.

#### <a name="823-workaround-to-get-lazy-loading-of-properties"></a>8.2.3 속성의 지연 로드를 가져오는 방법

Entity Framework 현재 스칼라 또는 복합 속성의 지연 로드를 지원 하지 않습니다. 그러나 BLOB과 같은 커다란 개체를 포함 하는 테이블이 있는 경우 테이블 분할을 사용 하 여 대량 속성을 개별 엔터티로 구분할 수 있습니다. 예를 들어 varbinary photo 열을 포함 하는 Product 테이블이 있다고 가정 합니다. 쿼리에서이 속성에 액세스할 필요가 없는 경우 테이블 분할을 사용 하 여 일반적으로 필요한 엔터티 부분만 가져올 수 있습니다. 제품 사진을 나타내는 엔터티는 명시적으로 필요한 경우에만 로드 됩니다.

테이블 분할을 사용 하도록 설정 하는 방법을 보여주는 좋은 리소스는 Gil Fink "테이블 분할에서 Entity Framework" 블로그 게시물: \<http://blogs.microsoft.co.il/blogs/gilf/archive/2009/10/13/table-splitting-in-entity-framework.aspx>합니다.

## <a name="9-other-considerations"></a>9 기타 고려 사항

### <a name="91-server-garbage-collection"></a>9.1 서버 가비지 수집

일부 사용자는 가비지 수집기가 제대로 구성 되지 않은 경우 예상 되는 병렬 처리를 제한 하는 리소스 경합이 발생할 수 있습니다. 다중 스레드 시나리오 또는 서버 쪽 시스템과 유사한 응용 프로그램에서 EF를 사용할 때마다 서버 가비지 수집을 사용 하도록 설정 해야 합니다. 이 작업은 응용 프로그램 구성 파일의 간단한 설정을 통해 수행 됩니다.

``` xml
<?xmlversion="1.0" encoding="utf-8" ?>
<configuration>
        <runtime>
               <gcServer enabled="true" />
        </runtime>
</configuration>
```

이렇게 하면 스레드 경합이 줄어들고 CPU 포화 시나리오에서 최대 30%까지 처리량이 증가 합니다. 일반적으로 서버 가비지 컬렉션 뿐만 아니라 클래식 가비지 수집 (UI 및 클라이언트 쪽 시나리오에 맞게 조정 됨)을 사용 하 여 응용 프로그램의 동작 방식을 테스트 해야 합니다.

### <a name="92-autodetectchanges"></a>9.2 AutoDetectChanges

앞서 설명한 것 처럼 개체 캐시에 많은 엔터티가 있을 때 Entity Framework 성능 문제를 표시할 수 있습니다. Add, Remove, Find, Entry 및 SaveChanges와 같은 특정 작업은 개체 캐시가 얼마나 큰지를 기준으로 많은 양의 CPU를 사용할 수 있는 DetectChanges에 대 한 호출을 트리거합니다. 그 이유는 개체 캐시와 개체 상태 관리자가 컨텍스트에 수행 된 각 작업에서 가능한 한 동기화 된 상태를 유지 하려고 하기 때문입니다. 이렇게 하면 생성 된 데이터가 광범위 한 시나리오에서 정확 하 게 유지 됩니다.

일반적으로 응용 프로그램의 전체 수명 동안 Entity Framework의 자동 변경 검색을 사용 하도록 설정 하는 것이 좋습니다. 높은 CPU 사용량으로 인 한 시나리오의 영향을 받고 프로필이 DetectChanges에 대 한 호출 임을 나타내는 경우 코드의 중요 한 부분에서 AutoDetectChanges를 일시적으로 해제 하는 것이 좋습니다.

``` csharp
try
{
    context.Configuration.AutoDetectChangesEnabled = false;
    var product = context.Products.Find(productId);
    ...
}
finally
{
    context.Configuration.AutoDetectChangesEnabled = true;
}
```

AutoDetectChanges를 해제 하기 전에 엔터티에서 발생 하는 변경 내용에 대 한 특정 정보를 추적 하는 기능이 손실 될 Entity Framework 수 있다는 것을 이해 하는 것이 좋습니다. 올바르게 처리 되지 않으면 응용 프로그램에서 데이터가 일치 하지 않을 수 있습니다. AutoDetectChanges 해제에 대 한 자세한 내용은 읽을 \<http://blog.oneunicorn.com/2012/03/12/secrets-of-detectchanges-part-3-switching-off-automatic-detectchanges/>합니다.

### <a name="93-context-per-request"></a>9.3 요청 당 컨텍스트

가장 최적의 성능 환경을 제공 하기 위해 Entity Framework의 컨텍스트는 수명이 짧은 인스턴스로 사용 됩니다. 컨텍스트는 수명이 짧고 폐기 되기 때문에 가능 하면 매우 간단 하 고 메타 데이터를 다시 활용할 수 있도록 구현 되었습니다. 웹 시나리오에서는이를 염두에 두고 단일 요청 기간 이상으로 컨텍스트를 포함 하지 않는 것이 중요 합니다. 마찬가지로 비 웹 시나리오에서는 Entity Framework의 다양 한 캐싱 수준을 이해 하 여 컨텍스트를 삭제 해야 합니다. 일반적으로 응용 프로그램의 수명 내내 컨텍스트 인스턴스를 사용 하지 않아야 하 고 스레드 및 정적 컨텍스트 당 컨텍스트를 사용 하지 않아야 합니다.

### <a name="94-database-null-semantics"></a>9.4 데이터베이스 null 의미 체계

기본적으로 Entity Framework는 C\# null 비교 의미 체계가 있는 SQL 코드를 생성 합니다. 다음 예제 쿼리를 살펴 보십시오.

``` csharp
            int? categoryId = 7;
            int? supplierId = 8;
            decimal? unitPrice = 0;
            short? unitsInStock = 100;
            short? unitsOnOrder = 20;
            short? reorderLevel = null;

            var q = from p incontext.Products
                    wherep.Category.CategoryName == "Beverages"
                          || (p.CategoryID == categoryId
                                || p.SupplierID == supplierId
                                || p.UnitPrice == unitPrice
                                || p.UnitsInStock == unitsInStock
                                || p.UnitsOnOrder == unitsOnOrder
                                || p.ReorderLevel == reorderLevel)
                    select p;

            var r = q.ToList();
```

이 예제에서는 공급자 및 UnitPrice와 같이 엔터티의 nullable 속성에 대해 다양 한 nullable 변수를 비교 합니다. 이 쿼리에 대해 생성 된 SQL은 매개 변수 값이 열 값과 같은지 또는 매개 변수와 열 값이 모두 null 인지를 요청 합니다. 이렇게 하면 데이터베이스 서버에서 null을 처리 하는 방식이 숨겨지고 여러 데이터베이스 공급 업체에서 일관 된 C\# null 환경을 제공 하 게 됩니다. 반면에 생성 된 코드는 비트 복잡 쿼리의 where 문에 있는 비교의 양이 증가 하는 경우 제대로 수행 되지 않을 수 있습니다.

이러한 상황을 처리 하는 한 가지 방법은 데이터베이스 null 의미 체계를 사용 하는 것입니다. 이는 데이터베이스 엔진에서 null 값을 처리 하는 방법을 제공 하는 보다 간단한 SQL을 생성 Entity Framework 하기 때문에 C\# null 의미 체계와 다르게 동작할 수 있습니다. 데이터베이스 null 의미 체계는 컨텍스트 구성에 대해 하나의 단일 구성 줄을 사용 하 여 컨텍스트별으로 활성화 될 수 있습니다.

``` csharp
                context.Configuration.UseDatabaseNullSemantics = true;
```

데이터베이스 null 의미 체계를 사용 하는 경우 중소 규모의 쿼리에는 크게 성능 개선이 표시 되지 않지만 잠재적 null 비교가 많은 쿼리에서는 차이가 더 두드러집니다.

위의 예제 쿼리에서 성능 차이는 제어 되는 환경에서 실행 되는 마이크로 벤치 마크의 2% 미만 이었습니다.

### <a name="95-async"></a>9.5 비동기

Entity Framework 6에서는 .NET 4.5 이상에서 실행 될 때 비동기 작업에 대 한 지원이 도입 되었습니다. 대부분의 경우 IO 관련 경합이 있는 응용 프로그램은 비동기 쿼리 및 저장 작업을 사용 하는 데 가장 유용 합니다. 응용 프로그램의 IO 경합이 발생 하지 않는 경우에는 async를 사용 하는 것이 가장 좋습니다. 비동기를 사용 하는 경우 동기식으로 실행 되 고 동기 호출과 같은 시간 내에 결과를 반환 하는 것입니다. 또는 최악의 경우에는 단순히 비동기 작업에 대 한 실행을 연기 하 고 추가 tim을 추가 합니다. e-시나리오를 완료 합니다.

방문 비동기 응용 프로그램의 성능이 향상 됩니다 하는 경우를 결정 하는 데 도움이 되는 비동기 프로그래밍 작업에 대 한 내용은 [http://msdn.microsoft.com/library/hh191443.aspx ](https://msdn.microsoft.com/library/hh191443.aspx)합니다. Entity Framework에서 비동기 작업을 사용 하는 방법에 대 한 자세한 내용은 [비동기 쿼리 및 저장](~/ef6/fundamentals/async.md
)을 참조 하세요.

### <a name="96-ngen"></a>9.6 NGEN

Entity Framework 6은 .NET Framework의 기본 설치에는 제공 되지 않습니다. 따라서 Entity Framework 어셈블리는 기본적으로 NGEN이 아닙니다. 즉, 모든 Entity Framework 코드는 다른 MSIL 어셈블리와 동일한 JIT'ing 비용을 적용 합니다. 이렇게 하면 프로덕션 환경에서 응용 프로그램을 개발 하는 동안 F5 환경이 저하 될 수 있습니다. JIT'ing의 CPU 및 메모리 비용을 줄이기 위해 Entity Framework 이미지를 적절 하 게 NGEN 하는 것이 좋습니다. NGEN을 사용 하 여 Entity Framework 6의 시작 성능을 개선 하는 방법에 대 한 자세한 내용은 [ngen을 사용 하 여 시작 성능 향상](~/ef6/fundamentals/performance/ngen.md)을 참조 하세요.

### <a name="97-code-first-versus-edmx"></a>9.7 Code First 및 EDMX

개념적 모델의 메모리 내 표현 (개체), 저장소 스키마 (데이터베이스) 및 간의 매핑을 포함 하 여 개체 지향 프로그래밍과 관계형 데이터베이스 간의 임피던스 불일치 문제에 대 한 이유를 Entity Framework 합니다. 두. 이 메타 데이터를 엔터티 데이터 모델 또는 간단한 EDM 이라고 합니다. 이 EDM에서 Entity Framework는 뷰를 파생 시켜 메모리의 개체에서 데이터베이스로 데이터를 왕복 하 고 그 뒤로 이동 합니다.

개념적 모델, 저장소 스키마 및 매핑을 공식적으로 지정 하는 EDMX 파일에 Entity Framework를 사용 하는 경우 모델 로드 단계는 EDM이 올바른지 확인 해야 합니다. 예를 들어 매핑이 누락 되었는지 확인 해야 합니다. 뷰를 생성 한 다음 뷰의 유효성을 검사 하 고이 메타 데이터를 사용할 수 있도록 준비 합니다. 그런 다음 쿼리를 실행 하거나 새 데이터를 데이터 저장소에 저장할 수 있습니다.

Code First 접근 방식은 매우 정교한 엔터티 데이터 모델 생성기입니다. Entity Framework는 제공 된 코드에서 EDM을 생성 해야 합니다. 모델에 관련 된 클래스를 분석 하 고 규칙을 적용 하 고 흐름 API를 통해 모델을 구성 하 여이를 수행 합니다. EDM이 빌드된 후에는 기본적으로 프로젝트에 EDMX 파일이 있는 것과 동일한 방식으로 Entity Framework 동작 합니다. 따라서 Code First에서 모델을 빌드하면 EDMX가 있는 것과 비교 하 여 Entity Framework에 대 한 시작 시간을 더 느리게 하는 복잡성이 추가 됩니다. 비용은 빌드 중인 모델의 크기 및 복잡성에 따라 완전히 달라 집니다.

EDMX와 Code First를 사용 하도록 선택 하는 경우 Code First에서 도입 된 유연성으로 인해 처음으로 모델을 작성 하는 비용이 늘어납니다. 응용 프로그램에서이 최초 로드의 비용을 견딜 수 있는 경우 일반적으로 Code First 기본 설정 된 방법이 됩니다.

## <a name="10-investigating-performance"></a>10 성능 조사

### <a name="101-using-the-visual-studio-profiler"></a>10.1 Visual Studio Profiler 사용

Entity Framework에서 성능 문제가 발생 하는 경우 Visual Studio에 기본 제공 된 것과 같은 프로파일러를 사용 하 여 응용 프로그램이 시간을 소비 하는 위치를 확인할 수 있습니다. "ADO.NET Entity Framework-1 부의 성능 알아보기" 블로그 게시물에서 원형 차트를 생성 하는 데에서는 도구입니다 ( \<http://blogs.msdn.com/b/adonet/archive/2008/02/04/exploring-the-performance-of-the-ado-net-entity-framework-part-1.aspx>) Entity Framework 동작 콜드 및 웜 쿼리 중의 시간을 소모 하는 위치를 보여 주는 합니다.

데이터 및 모델링 고객 자문 팀에서 작성 한 "Visual Studio 2010 Profiler를 사용 하 여 프로 파일링 Entity Framework" 블로그 게시물에서는 프로파일러를 사용 하 여 성능 문제를 조사 하는 방법에 대 한 실제 예제를 보여 줍니다.  http://blogs.msdn.com/b/dmcat/archive/2010/04/30/profiling-entity-framework-using-the-visual-studio-2010-profiler.aspx>를 \<합니다. 이 게시물은 windows 응용 프로그램용으로 작성 되었습니다. 웹 응용 프로그램을 프로 파일링 해야 하는 경우 Visual Studio에서 작업 하는 것 보다 Windows 성능 레코더 (WPR) 및 Windows 성능 분석기 (WPA) 도구를 사용 하는 것이 좋습니다. WPR 및 WPA는 Windows 성능 도구 키트는 Windows 평가 및 배포 키트에 포함 된 부분 ( [http://www.microsoft.com/download/details.aspx?id=39982](https://www.microsoft.com/download/details.aspx?id=39982)).

### <a name="102-applicationdatabase-profiling"></a>10.2 응용 프로그램/데이터베이스 프로 파일링

Visual Studio에 기본 제공 되는 프로파일러와 같은 도구를 통해 응용 프로그램에서 시간이 소요 되는 위치를 알 수 있습니다.  실행 중인 응용 프로그램에 대 한 동적 분석을 수행 하는 또 다른 유형의 프로파일러를 사용할 수 있으며,이는 요구 사항에 따라 프로덕션 또는 사전 프로덕션 환경에서 실행 되는 응용 프로그램을 확인 하 고 일반적인 문제 및 데이터베이스 액세스의 패턴

상업적으로 사용할 수 있는 두 가지 프로파일러는 Entity Framework Profiler ( \<http://efprof.com>) ORMProfiler 및 ( \<http://ormprofiler.com>)합니다.

응용 프로그램이 Code First를 사용 하는 MVC 응용 프로그램 인 경우 StackExchange의 미니 프로파일러를 사용할 수 있습니다. Scott Hanselman 블로그가이 도구 설명: \<http://www.hanselman.com/blog/NuGetPackageOfTheWeek9ASPNETMiniProfilerFromStackExchangeRocksYourWorld.aspx>합니다.

응용 프로그램의 데이터베이스 작업을 프로 파일링 하는 방법에 대 한 자세한 내용은 [Entity Framework의 데이터베이스 작업 프로 파일링](https://msdn.microsoft.com/magazine/gg490349.aspx)이라는 Julie LERMAN의 MSDN Magazine 문서를 참조 하세요.

### <a name="103-database-logger"></a>10.3 데이터베이스로 거

Entity Framework 6을 사용 하는 경우 기본 제공 로깅 기능을 사용 하는 것도 좋습니다. 간단한 한 줄 구성을 통해 해당 작업을 기록 하도록 컨텍스트의 데이터베이스 속성에 지시할 수 있습니다.

``` csharp
    using (var context = newQueryComparison.DbC.NorthwindEntities())
    {
        context.Database.Log = Console.WriteLine;
        var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
        q.ToList();
    }
```

이 예제에서 데이터베이스 작업은 콘솔에 기록 되지만 Log 속성은 Action&lt;문자열&gt; delegate를 호출 하도록 구성할 수 있습니다.

다시 컴파일하지 않고 데이터베이스 로깅을 사용 하도록 설정 하 고 Entity Framework 6.1 이상을 사용 하려는 경우 응용 프로그램의 web.config 또는 app.config 파일에 인터셉터를 추가 하 여이 작업을 수행할 수 있습니다.

``` xml
  <interceptors>
    <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
      <parameters>
        <parameter value="C:\Path\To\My\LogOutput.txt"/>
      </parameters>
    </interceptor>
  </interceptors>
```

이동 다시 컴파일하지 않고도 로깅을 추가 하는 방법에 대 한 자세한 내용은 \<http://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/>합니다.

## <a name="11-appendix"></a>11 부록

### <a name="111-a-test-environment"></a>11.1 A. 테스트 환경

이 환경에서는 클라이언트 응용 프로그램을 사용 하 여 별도의 컴퓨터에 있는 2 컴퓨터 설치를 데이터베이스에 사용 합니다. 컴퓨터가 동일한 랙에 있으므로 네트워크 대기 시간은 상대적으로 낮지만 단일 컴퓨터 환경 보다 더 현실적인 것입니다.

#### <a name="1111-app-server"></a>11.1.1 App 서버

##### <a name="11111-software-environment"></a>11.1.1.1 소프트웨어 환경

-   Entity Framework 4 소프트웨어 환경
    -   OS 이름: Windows Server 2008 R2 Enterprise SP1.
    -   Visual Studio 2010 – Ultimate.
    -   Visual Studio 2010 SP1 (일부 비교에만 해당)
-   Entity Framework 5 및 6 소프트웨어 환경
    -   OS 이름: Windows 8.1 Enterprise
    -   Visual Studio 2013 – Ultimate.

##### <a name="11112-hardware-environment"></a>11.1.1.2 하드웨어 환경

-   이중 프로세서: Intel (R) Xeon (R) CPU L5520 W3530 @ 2.27 GHz, 2261 Mhz8 GHz, 4 개 코어, 84 개의 논리적 프로세서.
-   2412 GB의 Ram
-   136 GB SCSI250GB SATA 7200 rpm/s 드라이브를 4 개의 파티션으로 분할 합니다.

#### <a name="1112-db-server"></a>11.1.2 DB 서버

##### <a name="11121-software-environment"></a>11.1.2.1 소프트웨어 환경

-   OS 이름: Windows Server 2008 R 28.1 Enterprise SP1.
-   SQL Server 2008 R22012.

##### <a name="11122-hardware-environment"></a>11.1.2.2 하드웨어 환경

-   단일 프로세서: Intel (R) Xeon (R) CPU L5520 @ 2.27 GHz, 2261 MhzES-1620 0 @ 3.60 GHz, 4 개 코어, 8 개의 논리적 프로세서.
-   824 GB의 Ram
-   465 GB ATA500GB SATA 7200 rpm 6GB/s 드라이브를 4 개의 파티션으로 분할 합니다.

### <a name="112-b-query-performance-comparison-tests"></a>11.2 B. 쿼리 성능 비교 테스트

Northwind 모델은 이러한 테스트를 실행 하는 데 사용 되었습니다. Entity Framework 디자이너를 사용 하 여 데이터베이스에서 생성 되었습니다. 그런 다음, 다음 코드를 사용 하 여 쿼리 실행 옵션의 성능을 비교 합니다.

``` csharp
using System;
using System.Collections.Generic;
using System.Data;
using System.Data.Common;
using System.Data.Entity.Infrastructure;
using System.Data.EntityClient;
using System.Data.Objects;
using System.Linq;

namespace QueryComparison
{
    public partial class NorthwindEntities : ObjectContext
    {
        private static readonly Func<NorthwindEntities, string, IQueryable<Product>> productsForCategoryCQ = CompiledQuery.Compile(
            (NorthwindEntities context, string categoryName) =>
                context.Products.Where(p => p.Category.CategoryName == categoryName)
                );

        public IQueryable<Product> InvokeProductsForCategoryCQ(string categoryName)
        {
            return productsForCategoryCQ(this, categoryName);
        }
    }

    public class QueryTypePerfComparison
    {
        private static string entityConnectionStr = @"metadata=res://*/Northwind.csdl|res://*/Northwind.ssdl|res://*/Northwind.msl;provider=System.Data.SqlClient;provider connection string='data source=.;initial catalog=Northwind;integrated security=True;multipleactiveresultsets=True;App=EntityFramework'";

        public void LINQIncludingContextCreation()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTracking()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                context.Products.MergeOption = MergeOption.NoTracking;

                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void CompiledQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                var q = context.InvokeProductsForCategoryCQ("Beverages");
                q.ToList();
            }
        }

        public void ObjectQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectQuery<Product> products = context.Products.Where("it.Category.CategoryName = 'Beverages'");
                products.ToList();
            }
        }

        public void EntityCommand()
        {
            using (EntityConnection eConn = new EntityConnection(entityConnectionStr))
            {
                eConn.Open();
                EntityCommand cmd = eConn.CreateCommand();
                cmd.CommandText = "Select p From NorthwindEntities.Products As p Where p.Category.CategoryName = 'Beverages'";

                using (EntityDataReader reader = cmd.ExecuteReader(CommandBehavior.SequentialAccess))
                {
                    List<Product> productsList = new List<Product>();
                    while (reader.Read())
                    {
                        DbDataRecord record = (DbDataRecord)reader.GetValue(0);

                        // 'materialize' the product by accessing each field and value. Because we are materializing products, we won't have any nested data readers or records.
                        int fieldCount = record.FieldCount;

                        // Treat all products as Product, even if they are the subtype DiscontinuedProduct.
                        Product product = new Product();  

                        product.ProductID = record.GetInt32(0);
                        product.ProductName = record.GetString(1);
                        product.SupplierID = record.GetInt32(2);
                        product.CategoryID = record.GetInt32(3);
                        product.QuantityPerUnit = record.GetString(4);
                        product.UnitPrice = record.GetDecimal(5);
                        product.UnitsInStock = record.GetInt16(6);
                        product.UnitsOnOrder = record.GetInt16(7);
                        product.ReorderLevel = record.GetInt16(8);
                        product.Discontinued = record.GetBoolean(9);

                        productsList.Add(product);
                    }
                }
            }
        }

        public void ExecuteStoreQuery()
        {
            using (NorthwindEntities context = new NorthwindEntities())
            {
                ObjectResult<Product> beverages = context.ExecuteStoreQuery<Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Database.SqlQuery\<QueryComparison.DbC.Product>(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void ExecuteStoreQueryDbSet()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var beverages = context.Products.SqlQuery(
@"    SELECT        P.ProductID, P.ProductName, P.SupplierID, P.CategoryID, P.QuantityPerUnit, P.UnitPrice, P.UnitsInStock, P.UnitsOnOrder, P.ReorderLevel, P.Discontinued
    FROM            Products AS P INNER JOIN Categories AS C ON P.CategoryID = C.CategoryID
    WHERE        (C.CategoryName = 'Beverages')"
);
                beverages.ToList();
            }
        }

        public void LINQIncludingContextCreationDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {                 
                var q = context.Products.Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }

        public void LINQNoTrackingDbContext()
        {
            using (var context = new QueryComparison.DbC.NorthwindEntities())
            {
                var q = context.Products.AsNoTracking().Where(p => p.Category.CategoryName == "Beverages");
                q.ToList();
            }
        }
    }
}
```

### <a name="113-c-navision-model"></a>11.3 Navision 모델

Navision 데이터베이스는 Microsoft Dynamics – NAV를 시연 하는 데 사용 되는 많은 데이터베이스입니다. 생성 된 개념적 모델에는 1005 엔터티 집합과 4227 연결 집합이 포함 되어 있습니다. 테스트에 사용 되는 모델은 "flat" 이며 상속을 추가 하지 않습니다.

#### <a name="1131-queries-used-for-navision-tests"></a>Navision 테스트에 사용 되는 11.3.1 쿼리

Navision 모델에 사용 되는 쿼리 목록에는 다음과 같은 3 가지 범주의 Entity SQL 쿼리가 포함 되어 있습니다.

##### <a name="11311-lookup"></a>11.3.1.1 조회

집계가 없는 간단한 조회 쿼리

-   개수: 16232
-   예:

``` xml
  <Query complexity="Lookup">
    <CommandText>Select value distinct top(4) e.Idle_Time From NavisionFKContext.Session as e</CommandText>
  </Query>
```

##### <a name="11312singleaggregating"></a>11.3.1.2 SingleAggregating

여러 집계가 있지만 부분합이 없는 일반 BI 쿼리 (단일 쿼리)

-   개수: 2313
-   예:

``` xml
  <Query complexity="SingleAggregating">
    <CommandText>NavisionFK.MDF_SessionLogin_Time_Max()</CommandText>
  </Query>
```

MDF\_SessionLogin\_Time\_Max ()는 모델에서 다음과 같이 정의 됩니다.

``` xml
  <Function Name="MDF_SessionLogin_Time_Max" ReturnType="Collection(DateTime)">
    <DefiningExpression>SELECT VALUE Edm.Min(E.Login_Time) FROM NavisionFKContext.Session as E</DefiningExpression>
  </Function>
```

##### <a name="11313aggregatingsubtotals"></a>11.3.1.3 AggregatingSubtotals

집계와 부분합이 있는 BI 쿼리 (union all을 통해)

-   개수: 178
-   예:

``` xml
  <Query complexity="AggregatingSubtotals">
    <CommandText>
using NavisionFK;
function AmountConsumed(entities Collection([CRONUS_International_Ltd__Zone])) as
(
    Edm.Sum(select value N.Block_Movement FROM entities as E, E.CRONUS_International_Ltd__Bin as N)
)
function AmountConsumed(P1 Edm.Int32) as
(
    AmountConsumed(select value e from NavisionFKContext.CRONUS_International_Ltd__Zone as e where e.Zone_Ranking = P1)
)
----------------------------------------------------------------------------------------------------------------------
(
    select top(10) Zone_Ranking, Cross_Dock_Bin_Zone, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking, E.Cross_Dock_Bin_Zone
)
union all
(
    select top(10) Zone_Ranking, Cast(null as Edm.Byte) as P2, AmountConsumed(GroupPartition(E))
    from NavisionFKContext.CRONUS_International_Ltd__Zone as E
    where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed
    group by E.Zone_Ranking
)
union all
{
    Row(Cast(null as Edm.Int32) as P1, Cast(null as Edm.Byte) as P2, AmountConsumed(select value E
                                                                         from NavisionFKContext.CRONUS_International_Ltd__Zone as E
                                                                         where AmountConsumed(E.Zone_Ranking) > @MinAmountConsumed))
}</CommandText>
    <Parameters>
      <Parameter Name="MinAmountConsumed" DbType="Int32" Value="10000" />
    </Parameters>
  </Query>
```
