---
title: 사용자 지정 Code First 규칙-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415894"
---
# <a name="custom-code-first-conventions"></a>사용자 지정 Code First 규칙
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

Code First 사용 하는 경우 모델은 규칙 집합을 사용 하 여 클래스에서 계산 됩니다. 기본 [Code First 규칙](~/ef6/modeling/code-first/conventions/built-in.md) 은 엔터티의 기본 키가 되는 속성, 엔터티가 매핑되는 테이블의 이름 및 기본적으로 10 진수 열의 전체 자릿수와 소수 자릿수를 결정 합니다.

경우에 따라 이러한 기본 규칙은 모델에 적합 하지 않으며 데이터 주석 또는 흐름 API를 사용 하 여 여러 개별 엔터티를 구성 하 여 해결 해야 합니다. 사용자 지정 Code First 규칙을 사용 하 여 모델에 대 한 구성 기본값을 제공 하는 고유한 규칙을 정의할 수 있습니다. 이 연습에서는 다양 한 유형의 사용자 지정 규칙과 각 규칙을 만드는 방법을 살펴봅니다.


## <a name="model-based-conventions"></a>모델 기반 규칙

이 페이지에서는 사용자 지정 규칙에 대 한 DbModelBuilder API를 다룹니다. 이 API는 대부분의 사용자 지정 규칙을 작성 하는 데 충분 합니다. 그러나 모델 기반 규칙을 작성 하는 기능도 있습니다. 이러한 규칙은 최종 모델을 만든 후이를 조작 하 여 고급 시나리오를 처리 합니다. 자세한 내용은 [모델 기반 규칙](~/ef6/modeling/code-first/conventions/model.md)을 참조 하세요.

 

## <a name="our-model"></a>모델

규칙에 사용할 수 있는 간단한 모델을 정의 하는 것부터 시작 해 보겠습니다. 프로젝트에 다음 클래스를 추가 합니다.

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a>사용자 지정 규칙 소개

키 라는 속성을 엔터티 형식에 대 한 기본 키로 구성 하는 규칙을 작성 하겠습니다.

모델 작성기에서 규칙을 사용할 수 있으며,이는 컨텍스트에서 OnModelCreating을 재정의 하 여 액세스할 수 있습니다. 다음과 같이 제품 컨텍스트 클래스를 업데이트 합니다.

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

이제 모델 이름이 Key 인 모든 속성은 해당 부분이 속한 엔터티의 기본 키로 구성 됩니다.

구성할 속성의 형식을 필터링 하 여 규칙을 더 구체적으로 만들 수도 있습니다.

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

그러면 Key 라는 모든 속성이 해당 엔터티의 기본 키가 되도록 구성 됩니다. 단, 정수인 경우에만 해당 됩니다.

IsKey 메서드의 흥미로운 기능은 추가 된 것입니다. 즉, 여러 속성에 대해 IsKey를 호출 하는 경우 모두 복합 키의 일부가 됩니다. 한 가지 주의할 점은 키에 대해 여러 속성을 지정 하는 경우 해당 속성의 순서만 지정 해야 한다는 것입니다. 다음과 같이 HasColumnOrder 메서드를 호출 하 여이 작업을 수행할 수 있습니다.

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

이 코드에서는 int 키 열과 문자열 이름 열로 구성 된 복합 키를 갖도록 모델의 형식을 구성 합니다. 디자이너에서 모델을 보면 다음과 같습니다.

![복합 키](~/ef6/media/compositekey.png)

속성 규칙의 또 다른 예는 datetime이 아닌 SQL Server의 datetime2 형식에 매핑되도록 내 모델의 모든 DateTime 속성을 구성 하는 것입니다. 다음을 사용 하 여이 작업을 수행할 수 있습니다.

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>규칙 클래스

규칙을 정의 하는 또 다른 방법은 규칙 클래스를 사용 하 여 규칙을 캡슐화 하는 것입니다. 규칙 클래스를 사용 하는 경우에는 System.object 네임 스페이스의 규칙 클래스에서 상속 되는 형식을 만듭니다.

다음을 수행 하 여 앞에서 설명한 datetime2 규칙을 사용 하 여 규칙 클래스를 만들 수 있습니다.

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

EF에이 규칙을 사용 하도록 지시 하려면이 규칙을 OnModelCreating의 규칙 컬렉션에 추가 합니다 .이는 연습을 통해 다음과 같이 표시 됩니다.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

여기에서 볼 수 있듯이 규칙의 인스턴스를 규칙 컬렉션에 추가 합니다. 규칙에서 상속을 통해 팀 또는 프로젝트에서 규칙을 그룹화 하 고 공유 하는 편리한 방법을 제공 합니다. 예를 들어 모든 조직 프로젝트에서 사용 하는 일반적인 규칙 집합을 포함 하는 클래스 라이브러리가 있을 수 있습니다.

 

## <a name="custom-attributes"></a>사용자 지정 특성

규칙을 사용 하는 또 다른 방법은 모델을 구성할 때 새 특성을 사용 하도록 설정 하는 것입니다. 이를 설명 하기 위해 문자열 속성을 비유니코드 것으로 표시 하는 데 사용할 수 있는 특성을 만들어 보겠습니다.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

이제 모델에이 특성을 적용 하는 규칙을 만들어 보겠습니다.

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

이 규칙을 사용 하 여 문자열이 아닌 특성을 문자열 속성에 추가할 수 있습니다. 즉, 데이터베이스의 열은 nvarchar 대신 varchar로 저장 됩니다.

이 규칙에 대해 기억해 야 할 한 가지 사항은 유니코드가 아닌 특성을 문자열 속성 이외의 값에 넣으면 예외가 throw 된다는 것입니다. 이는 문자열 이외의 형식에 대해 IsUnicode를 구성할 수 없기 때문입니다. 이런 경우에는 더 구체적인 규칙을 만들어 문자열이 아닌 모든 항목을 필터링 할 수 있습니다.

위의 규칙은 사용자 지정 특성을 정의 하는 데 사용할 수 있지만 특히 특성 클래스의 속성을 사용 하려는 경우에는 더 쉽게 사용할 수 있는 다른 API가 있습니다.

이 예에서는 특성을 업데이트 하 고 IsUnicode 특성으로 변경 하 여 다음과 같이 표시 됩니다.

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

이를 완료 한 후에는 특성에 부울을 설정 하 여 속성이 유니코드 여야 하는지 여부를 규칙에 지시할 수 있습니다. 다음과 같이 구성 클래스의 ClrProperty에 액세스 하 여 이미 제공 된 규칙에 따라이 작업을 수행할 수 있습니다.

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

이 방법은 충분 하지만 규칙 API의 Having 메서드를 사용 하 여이를 달성할 수 있는 보다 간결한 방법이 있습니다. Having 메서드에는 Where 메서드와 동일한 PropertyInfo를 수락 하지만 개체를 반환할 것으로 예상 되는 Func&lt;PropertyInfo, T&gt; 형식의 매개 변수가 있습니다. 반환 된 개체가 null 이면 속성이 구성 되지 않습니다. 즉, Where와 마찬가지로 속성을 필터링 할 수 있지만 반환 된 개체를 캡처하여 Configure 메서드로 전달 한다는 것을 의미 합니다. 다음과 같이 작동 합니다.

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

사용자 지정 특성은 Having 메서드를 사용 하는 유일한 이유가 아니라 형식이 나 속성을 구성할 때 필터링 하는 항목에 대 한 이유를 알아야 하는 모든 경우에 유용 합니다.

 

## <a name="configuring-types"></a>유형 구성

지금 까지는 속성에 대 한 모든 규칙이 있지만 모델에서 형식을 구성 하기 위한 규칙 API의 또 다른 영역이 있습니다. 이 환경은 지금까지 확인 한 규칙과 유사 하지만 구성 내의 옵션은 속성 수준이 아니라 엔터티에 있습니다.

유형 수준 규칙을 사용 하는 경우에는 EF 기본값과 다른 기존 스키마에 매핑하거나 다른 명명 규칙을 사용 하 여 새 데이터베이스를 만들기 위해 테이블 명명 규칙을 변경 하는 것이 매우 유용할 수 있습니다. 이렇게 하려면 먼저 모델의 형식에 대 한 TypeInfo를 수락 하 고 해당 형식에 대 한 테이블 이름을 반환할 수 있는 메서드가 필요 합니다.

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

이 메서드는 형식을 사용 하 고 CamelCase 대신 밑줄을 사용 하 여 소문자를 사용 하는 문자열을 반환 합니다. 이 모델에서이는 제품 범주 클래스가 제품 범주가 아닌 product\_category 라는 테이블에 매핑되는지를 의미 합니다.

이 메서드가 있으면 다음과 같은 규칙에 따라 호출할 수 있습니다.

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

이 규칙은 GetTableName 메서드에서 반환 되는 테이블 이름에 매핑되도록 모델의 모든 형식을 구성 합니다. 이 규칙은 흐름 API를 사용 하 여 모델의 각 엔터티에 대해 ToTable 메서드를 호출 하는 것과 동일 합니다.

이에 대 한 한 가지 주의할 점은 ToTable EF를 호출할 때 사용자가 테이블 이름을 확인할 때 일반적으로 수행 하는 복수화 없이 정확한 테이블 이름으로 제공 하는 문자열을 사용 한다는 것입니다. 그 이유는 규칙의 테이블 이름이 제품\_범주 대신 제품\_범주입니다. 복수화 서비스를 호출 하 여 규칙에서이 문제를 해결할 수 있습니다.

다음 코드에서는 EF6에 추가 된 [종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md) 기능을 사용 하 여 EF가 사용한 복수화 서비스를 검색 하 고 테이블 이름을 복수화 합니다.

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> 제네릭 버전의 GetService는 DependencyResolution 네임 스페이스에 있는 확장 메서드 이므로 사용 하기 위해 컨텍스트에 using 문을 추가 해야 할 수 있습니다.

### <a name="totable-and-inheritance"></a>ToTable 및 상속

ToTable의 또 다른 중요 한 측면은 명시적으로 유형을 지정 된 테이블에 매핑하는 경우 EF에서 사용할 매핑 전략을 변경할 수 있다는 것입니다. 상속 계층 구조의 모든 형식에 대해 ToTable를 호출 하 여 형식 이름을 위에서 설명한 것과 같은 테이블의 이름으로 전달 하는 경우 기본 계층당 하나의 계층당 단일 테이블 매핑 전략을 TPT (형식당 하나의 테이블)로 변경 합니다. 이를 설명 하는 가장 좋은 방법은 구체적인 예입니다.

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

기본적으로 employee와 manager는 모두 데이터베이스의 동일한 테이블 (직원)에 매핑됩니다. 테이블에는 각 행에 저장 되는 인스턴스 유형을 알려주는 판별자 열이 있는 직원과 관리자가 모두 포함 됩니다. 이는 계층 구조에 대 한 단일 테이블이 있으므로 TPH 매핑입니다. 그러나 두 classe 모두에서 ToTable를 호출 하는 경우 각 형식에 자체 테이블이 있으므로 각 형식이 TPT 라고도 하는 자체 테이블에 매핑됩니다.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

위의 코드는 다음과 같은 테이블 구조에 매핑됩니다.

![tpt 예제](~/ef6/media/tptexample.jpg)

이를 방지 하 고 기본 TPH 매핑을 다음 두 가지 방법으로 유지 관리할 수 있습니다.

1.  계층의 각 형식에 대해 동일한 테이블 이름을 사용 하 여 ToTable를 호출 합니다.
2.  계층 구조의 기본 클래스 에서만 ToTable를 호출 합니다 .이 예제에서는 employee입니다.

 

## <a name="execution-order"></a>실행 순서

규칙은 마지막 wins 방식으로 작동 하며 흐름 API와 동일 합니다. 즉, 동일한 속성의 동일한 옵션을 구성 하는 두 가지 규칙을 작성 하는 경우 마지막으로 wins를 실행 합니다. 예를 들어 아래 코드에서 모든 문자열의 최대 길이는 500로 설정 되지만 모델에서 Name 이라는 모든 속성을 구성 하 여 최대 길이가 250 되도록 구성 합니다.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

최대 길이를 250로 설정 하는 규칙은 모든 문자열을 500로 설정 하는 규칙을 가지 므로 모델에서 Name 이라는 모든 속성의 MaxLength는 250 이지만 설명과 같은 다른 문자열은 500입니다. 이러한 방식으로 규칙을 사용 하면 모델의 형식 또는 속성에 대 한 일반 규칙을 제공 하 고 다른 하위 집합에 대해 재정의 수 있습니다.

특정 한 경우에는 흐름 API 및 데이터 주석을 사용 하 여 규칙을 재정의할 수도 있습니다. 위의 예제에서 흐름 API를 사용 하 여 속성의 최대 길이를 설정 하는 경우 보다 구체적인 흐름 API가 보다 일반적인 구성 규칙을 사용 하기 때문에 규칙 앞 이나 뒤에 배치 했을 수 있습니다.

 

## <a name="built-in-conventions"></a>기본 제공 규칙

사용자 지정 규칙은 기본 Code First 규칙의 영향을 받을 수 있으므로 다른 규칙 전후에 실행 되는 규칙을 추가 하는 것이 유용할 수 있습니다. 이렇게 하려면 파생 DbContext에서 규칙 컬렉션의 AddBefore 및 Addbefore 메서드를 사용할 수 있습니다. 다음 코드는 기본 제공 키 검색 규칙 보다 먼저 실행 되도록 이전에 만든 규칙 클래스를 추가 합니다.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

이는 기본 제공 규칙 전후에 실행 해야 하는 규칙을 추가할 때 가장 많이 사용 [됩니다. 기본](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx)제공 규칙 목록은 여기에서 찾을 수 있습니다...

모델에 적용 하지 않으려는 규칙도 제거할 수 있습니다. 규칙을 제거 하려면 Remove 메서드를 사용 합니다. PluralizingTableNameConvention를 제거 하는 예제는 다음과 같습니다.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
