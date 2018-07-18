---
title: 코드 기반 구성-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13886d24-2c74-4a00-89eb-aa0dee328d83
caps.latest.revision: 3
ms.openlocfilehash: d6a33434e582fcd7ce756b447d7f2cbab4ca43ec
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122743"
---
# <a name="code-based-configuration"></a>코드 기반 구성
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.  

Entity Framework 응용 프로그램에 대 한 구성은 구성 파일 (app.config/web.config) 또는 코드를 통해 지정할 수 있습니다. 후자는 라고 코드 기반 구성 합니다.  

구성 파일에서 구성에 설명 되어는 [별도 문서](config-file.md)합니다. 구성 파일 코드 기반 구성 보다 우선합니다. 즉, 구성 옵션 모두 코드 및 config 파일을 설정 하는 경우 구성 파일의 설정이 사용 됩니다.  

## <a name="using-dbconfiguration"></a>DbConfiguration를 사용 하 여  

코드 기반 구성 이상 EF6에서 System.Data.Entity.Config.DbConfiguration의 서브 클래스를 만들어서 이루어집니다. DbConfiguration 서브클래싱 할 경우 다음 지침을 따라야 합니다.  

- 응용 프로그램에 대 한 하나의 DbConfiguration 클래스를 만듭니다. 이 클래스는 응용 프로그램 도메인 전체 설정을 지정합니다.  
- DbConfiguration 클래스 DbContext 클래스와 동일한 어셈블리에 배치 합니다. (참조를 *DbConfiguration 이동* 변경 하려는 경우 섹션입니다.)  
- DbConfiguration 클래스에 매개 변수가 없는 public 생성자를 지정 합니다.  
- 이 생성자 내에서 보호 된 DbConfiguration 메서드를 호출 하 여 구성 옵션을 설정 합니다.  

다음 지침에 따라 EF를 검색 하 고 모델에 액세스 해야 하 고 응용 프로그램이 실행 될 때 도구 모두에서 구성을 자동으로 사용할 수 있습니다.  

## <a name="example"></a>예  

DbConfiguration에서 파생 된 클래스는 다음과 같습니다.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.SqlServer;

namespace MyNamespace
{
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            SetDefaultConnectionFactory(new LocalDBConnectionFactory("mssqllocaldb"));
        }
    }
}
```  

이 클래스 EF를 자동으로 실패 한 데이터베이스 작업-다시 시도 하 고 Code First에서 규칙에 따라 생성 된 데이터베이스에 대 한 로컬 DB를 사용 하려면 SQL Azure 실행 전략-을 사용 하도록 설정 합니다.  

## <a name="moving-dbconfiguration"></a>DbConfiguration 이동  

DbConfiguration 클래스 DbContext 클래스와 동일한 어셈블리에 적용할 수 없는 경우가 있습니다. 예를 들어 두 DbContext 클래스는 각각 서로 다른 어셈블리에서 할 수 있습니다. 이 처리 하기 위한 두 가지 옵션이 있습니다.  

첫 번째 옵션 DbConfiguration 사용할 인스턴스를 지정 하려면 구성 파일을 사용 하는 것입니다. 이 작업을 수행 하려면 entityFramework 구역의 codeConfigurationType 특성을 설정 합니다. 예를 들어:  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyDbConfiguration, MyAssembly">
    ...Your EF config...
</entityFramework>
```  

어셈블리 및 네임 스페이스 DbConfiguration 클래스의 정규화 된 이름을 codeConfigurationType 값 이어야 합니다.  

두 번째 방법은 DbConfigurationTypeAttribute 컨텍스트 클래스에 배치 하는 것입니다. 예를 들어:  

``` csharp  
[DbConfigurationType(typeof(MyDbConfiguration))]
public class MyContextContext : DbContext
{
}
```  

수 DbConfiguration 형식--위에 표시 된 대로 특성에 전달 된 값 또는 어셈블리와 네임 스페이스 정규화 된 형식 이름 문자열입니다. 예를 들어:  

``` csharp
[DbConfigurationType("MyNamespace.MyDbConfiguration, MyAssembly")]
public class MyContextContext : DbContext
{
}
```  

## <a name="setting-dbconfiguration-explicitly"></a>DbConfiguration를 명시적으로 설정  

DbContext 형식일이 사용 하기 전에 구성 필요할 수 있는 몇 가지 경우가 있습니다. 이 예제:  

- DbModelBuilder를 사용 하 여 컨텍스트 없이 모델 작성  
- 응용 프로그램 컨텍스트를 사용 하기 전에 해당 컨텍스트를 사용 하는 위치 DbContext를 사용 하는 다른 프레임 워크/유틸리티 코드를 사용 하 여  

이러한 상황에서는 EF 구성을 자동으로 검색할 수 있도록 아니며 다음 중 하나를 대신 수행 해야 합니다.  

- 에 설명 된 대로 구성 파일에서 DbConfiguration 형식을 설정 합니다 *DbConfiguration 이동* 위의 섹션
- 응용 프로그램 시작 하는 동안 정적 DbConfiguration.SetConfiguration 메서드를 호출 합니다.  

## <a name="overriding-dbconfiguration"></a>DbConfiguration 재정의  

DbConfiguration에서 설정 된 구성을 재정의 해야 하는 경우도 있습니다. 이렇게 하지 않으면 일반적으로 응용 프로그램 개발자가 아니라 대신 타사 공급자 및 플러그 인 파생된 DbConfiguration 클래스를 사용할 수 없습니다.  

EntityFramework는이 위해 잠기기 직전 기존 구성을 수정할 수 있는 이벤트 처리기를를 등록할 수 있습니다.  또한 대체 EF 서비스 로케이터를 반환한 모든 서비스에 맞게 sugar 메서드를 제공 합니다. 다음은 방법을 사용할 것입니다.  

- (EF가 사용 됨) 전에 앱 시작 시 플러그 인 또는 공급자는이 이벤트에 대 한 이벤트 처리기 메서드를 등록 해야 합니다. (응용 프로그램 EF를 사용 하기 전에 발생 해야이 note 합니다.)  
- 이벤트 처리기를 호출 ReplaceService 교체 해야 하는 모든 서비스에 대 한 합니다.  

예를 들어 repalce IDbConnectionFactory DbProviderService를 다음과 같이 결과 처리기 등록:  

``` csharp
DbConfiguration.Loaded += (_, a) =>
   {
       a.ReplaceService<DbProviderServices>((s, k) => new MyProviderServices(s));
       a.ReplaceService<IDbConnectionFactory>((s, k) => new MyConnectionFactory(s));
   };
```  

MyProviderServices 및 MyConnectionFactory 위의 코드에서 서비스의 구현을 나타냅니다.  

또한 동일한 효과를 얻을 때 추가 종속성 처리기를 추가할 수 있습니다.  

따라서에서 DbProviderFactory를 래핑할 수도 있습니다 하지만 수행 하므로 영향을 받습니다 EF 및 EF 외부 DbProviderFactory 사용 하지는 note 합니다. 이러한 이유로 원할 것 계속 하기 전에 동일 DbProviderFactory를 래핑합니다.  

또한 유지 해야 염두에서 실행 하는 응용 프로그램 외부에 예를 들어, 패키지 관리자 콘솔에서 마이그레이션을 실행할 때 서비스입니다. 실행 하는 경우에 DbConfiguration을 찾으려고 시도 하는 콘솔에서 마이그레이션하십시오. 그러나 래핑된를 가져오게 됩니다 여부에 따라 달라 집니다 where 이벤트 처리기를 등록 합니다. 프로그램 DbConfiguration 생성의 일부로 등록 된 경우 그런 다음 코드를 실행 해야 하 고 서비스 래핑해야 가져옵니다. 일반적으로이 그렇지 않을 고 즉, 도구가 래핑된 서비스를 가져오지 않습니다.  
