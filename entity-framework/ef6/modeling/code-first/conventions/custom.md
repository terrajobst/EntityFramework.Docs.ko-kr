---
title: 사용자 지정 코드 첫 번째 규칙-EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
caps.latest.revision: 3
ms.openlocfilehash: 24d6f1bd5eb2ff8be59b9eedd1c4156709fa42fb
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122610"
---
# <a name="custom-code-first-conventions"></a>사용자 지정 코드의 첫 번째 규칙
> [!NOTE]
> **EF6 이상만** - 이 페이지에서 다루는 기능, API 등은 Entity Framework 6에 도입되었습니다. 이전 버전을 사용하는 경우 이 정보의 일부 또는 전체가 적용되지 않습니다.

Code First를 사용 하는 경우 모델은 규칙 집합을 사용 하 여 수업에서 계산 됩니다. 기본값 [코드의 첫 번째 규칙](~/ef6/modeling/code-first/conventions/built-in.md) 는 같은 속성은 엔터티의 기본 키, 어떤 전체 자릿수 및 소수 10 진수 열에는 기본적으로 엔터티의 매핑될 테이블의 이름을 가지를 확인 합니다.

경우에 따라 이러한 기본 규칙 모델에 대 한 적합 하지 않습니다 하 고 데이터 주석 또는 Fluent API를 사용 하 여 여러 개별 엔터티를 구성 하 여 해결 해야 합니다. 사용자 지정 코드의 첫 번째 규칙을 통해 모델에 대 한 구성 기본값을 제공 하는 사용자 고유의 규칙을 정의할 수 있습니다. 이 연습에서는 다양 한 유형의 사용자 지정 규칙 및 각각을 만드는 방법에 살펴봅니다.


## <a name="model-based-conventions"></a>모델 기반 규칙

이 페이지에서는 사용자 지정 규칙에 대 한 DbModelBuilder API를 설명합니다. 이 API는 대부분의 사용자 지정 규칙을 작성 하기 위한 충분 해야 합니다. 그러나 모델 기반 규칙-생성 된 후 최종 모델을 조작 하는 규칙을 작성 하려면-고급 시나리오를 처리 하는 기능 또한 됩니다. 자세한 내용은 [모델 기반 규칙](~/ef6/modeling/code-first/conventions/model.md)합니다.

 

## <a name="our-model"></a>모델

규칙을 사용 하 여 사용할 수 있는 간단한 모델을 정의 하 여 시작 해 보겠습니다. 다음 클래스를 프로젝트에 추가 합니다.

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

 

## <a name="introducing-custom-conventions"></a>사용자 지정 규칙을 소개

해당 엔터티 형식에 대 한 기본 키로 키 속성을 구성 하는 규칙을 작성해 보겠습니다.

규칙 컨텍스트에서 OnModelCreating을 재정의 하 여 액세스할 수 있는 모델 작성기를 사용할 수 있습니다. ProductContext 클래스를 다음과 같이 업데이트 합니다.

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

키 이름이 모델의 속성 수는 이제 모든 엔터티의 기본 키로 해당 부분을 구성 합니다.

구성 하려는 속성의 형식을 필터링 하 여 규칙을 구체적인 만들 수도 했습니다.

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

이렇게 하면 해당 엔터티의 키 이지만 정수 있는 경우에 기본 키라고 하는 모든 속성이 구성 됩니다.

IsKey 메서드는 흥미로운 기능은 것은 가산적입니다. 즉, 여러 속성에 대해 IsKey를 호출 하 고 이러한 모든 복합 키의 일부가 될 경우. 이 대 한 중요 한 점은 키에 대 한 여러 속성을 지정 하는 경우 지정 해야 합니다도 해당 속성에 대 한 주문을 합니다. 아래와 같은 메서드 HasColumnOrder를 호출 하 여이 수행할 수 있습니다.

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

이 코드에서 int 키 열과 문자열 이름 열으로 구성 된 복합 키를 갖는 모델 유형을 구성 합니다. 디자이너에서 모델을 보면 다음과 같이 표시 됩니다.

![compositeKey](~/ef6/media/compositekey.png)

속성 규칙의 또 다른 예로 날짜/시간 대신 SQL server에서 datetime2 형식에 매핑할 모델 내에서 모든 날짜/시간 속성을 구성 하는 것입니다. 다음을 사용 하 여이 얻을 수 있습니다.

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a>규칙 클래스

규칙을 정의 하는 또 다른 방법은 규칙을 캡슐화 하는 규칙 클래스를 사용 하는 것입니다. 규칙 클래스를 사용 하는 경우 System.Data.Entity.ModelConfiguration.Conventions 네임 스페이스의 규칙 클래스에서 상속 하는 형식을 만들 있습니다.

규칙 클래스는 다음을 수행 하 여 앞서 살펴본 datetime2 규칙을 사용 하 여 만들 수 있습니다.

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

OnModelCreating 연습을 따라 수행 하는 경우 다음과 같이 표시 됩니다는 규칙 컬렉션에 추가한이 규칙을 사용 하는 EF를 알려야 합니다.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

알 수 있듯이 규칙 컬렉션에이 규칙의 인스턴스를 추가 했습니다. 규칙에서 상속 하는 그룹화 하 고 팀 또는 프로젝트에서 규칙을 공유 하는 편리한 방법을 제공 합니다. 예를 들어, 공통 집합을 사용 하 여를 프로젝트 조직의의 모든 규칙을 사용 하 여 클래스 라이브러리를 있을 수 있습니다.

 

## <a name="custom-attributes"></a>사용자 지정 특성

규칙의 다른 좋은 사용 모델을 구성할 때 사용할 새 특성을 사용 하도록 설정 하는 것입니다. 예를 들어 문자열 속성을 비유니코드 됨으로 표시 하려면 사용할 수 있는 특성을 만들어 보겠습니다.

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

이 규칙을 사용 하 여 nvarchar 대신 varchar로 저장할 유니코드가 아닌 특성 즉, 데이터베이스의 열이 문자열 속성을 추가할 수 있습니다.

이 규칙에 대해 한 가지는 경우 예외를 throw 하는 다음 문자열 속성 이외에 유니코드가 아닌 특성을 삽입 하면입니다. 문자열이 아닌 형식에서 IsUnicode를 구성할 수 없으므로이 수행 합니다. 이 경우 만들 수 있습니다 규칙에 따라 더 구체적으로 문자열이 있는 모든 항목이 필터링 되도록 합니다.

위의 규칙을 사용자 지정 특성을 정의 하기 위한 작동 하는 동안에 사용, 특히 하려는 경우 특성 클래스에서 속성을 사용 하기 쉽게 될 수 있는 다른 API입니다.

이 예제에서는 것이 특성을 업데이트 하 고 다음과 같이 표시 됩니다 IsUnicode 특성을 변경 하려면:

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

이 있으면, bool 속성 유니코드 해야 할지 여부는 규칙을 구별 하는 특성에 설정할 수 있습니다. 에서는 다음과 같은 구성 클래스의 ClrProperty에 액세스 하 여 이미 규칙에서 수행할 수 없습니다.

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

이 쉽지만 이지만 Having을 사용 하 여이 달성 하는 보다 간결한 방법을 규칙 API의 메서드. 메서드 매개 변수가 있는 Func를 입력&lt;PropertyInfo, T&gt; 허용 하는 PropertyInfo Where와 동일 메서드를 이지만 개체를 반환 합니다. 속성을 구성할 수는 반환 된 개체가 null 이면 Where, 마찬가지로 속성으로 필터링 할 수 있다는 의미입니다 이지만 다른는 또한으로 반환 되는 개체를 캡처 및 구성 메서드에 전달 됩니다. 이 다음과 같이 작동합니다.

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

사용자 지정 특성은 Having을 사용 하는 유일한 이유 메서드를 유용 프로그램 형식 또는 속성을 구성 하는 경우에 필터링 하는 것에 대해 추론 해야 하는 원격입니다.

 

## <a name="configuring-types"></a>구성 형식

속성에 대 한 된 규칙을 모두 지금 이지만 규칙 API의 다른 영역 모델에서 형식을 구성 합니다. 환경을 지금까지 살펴본 규칙 유사 하지만 내부 구성에서 옵션 수준에서 속성 대신 엔터티 수 있습니다.

형식 수준 규칙에 매우 유용한 수 있는 작업 중 하나는 테이블 명명 규칙을 EF 기본값과에서 다른 기존 스키마에 매핑할 또는 다른 명명 규칙을 사용 하 여 새 데이터베이스를 만들려는 변경입니다. 이렇게 하려면 먼저 모델의 형식에 대 한 TypeInfo를 수락 하 고 있어야 하는 형식에 대 한 테이블 이름을 반환할 수 있는 메서드를 해야 합니다.

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

이 메서드는 형식을 사용 하 고 CamelCase 대신 밑줄을 사용 하 여 소문자를 사용 하는 문자열을 반환 합니다. 모델 즉 ProductCategory 클래스 제품 이라고 하는 테이블에 매핑될는\_ProductCategories 대신 범주입니다.

해당 메서드가 있으면 다음과 같은 규칙에서 호출할 수 것:

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

GetTableName 메서드에서 반환 되는 테이블 이름에 매핑할 모델에서 모든 형식을 구성 하는이 규칙. 이 규칙은 Fluent API를 사용 하 여 모델의 각 엔터티에 대해 ToTable 메서드를 호출 하는 것과 같습니다.

하나에 대해 알아두어야 할이 되는 호출 하는 경우 ToTable EF는 걸릴 정확한 테이블 이름을 복수화 테이블 이름을 확인할 때 수행 일반적으로 사용 하지 않고으로 제공 하는 문자열입니다. 이 때문에 규칙에서 테이블 이름은 제품\_제품 대신 범주\_범주입니다. 에서는 해결할 수는 규칙에서 직접 복수 적용 서비스를 호출 하 여.

다음 코드에서는 사용 하 여는 [종속성 확인](~/ef6/fundamentals/configuring/dependency-resolution.md) EF 사용 있는 복수 적용 서비스를 검색 하는 EF6에 추가 된 기능 및이 테이블 이름을 복수화 합니다.

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
> GetService의 제네릭 버전 System.Data.Entity.Infrastructure.DependencyResolution 네임 스페이스의 확장 메서드를 추가 하 여 해야 사용 하기 위해 컨텍스트를 문의 합니다.

### <a name="totable-and-inheritance"></a>ToTable 및 상속

ToTable의 또 다른 중요 한 측면에 있으면는 명시적으로 형식이 지정된 된 테이블에 매핑하는 경우 EF를 사용 하는 매핑 전략을 변경할 수 있습니다. ToTable 상속 계층 구조의 모든 형식에 대해를 호출 하면 위에서 수행한 것과 같이 형식 이름을 테이블 이름으로 전달 됩니다 변경한 다음 기본 (TPH) 계층당 하나의 테이블 매핑 전략에 형식당 하나의 테이블 TPT (). 이 설명 하는 가장 좋은 방법은 whith 구체적인 예제를 사용 하는 것:

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

기본적으로 employee 및 manager 둘 다 동일한 테이블 (직원) 데이터베이스에 매핑됩니다. 테이블에는 직원 및 관리자는 사실을 알려 주는 어떤 유형의 인스턴스는 각 행에 저장 되어 판별자 열을 사용 하 여 모두 포함 됩니다. 이 계층에 대 한 단일 테이블이 TPH 매핑이입니다. 그러나 두 classe의 ToTable을 호출 하면 다음 각 형식 대신 매핑될 자체 테이블에 각 형식에는 자체 테이블 이므로 TPT 라고도 합니다.

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

위의 코드는 다음과 같이 표시 된 테이블 구조에 매핑합니다.

![tptExample](~/ef6/media/tptexample.jpg)

이 문제를 방지 하 고 기본 TPH 매핑이 두 가지 방법으로 유지할 수 있습니다.

1.  계층의 각 형식에 대 한 테이블 이름이 같은 ToTable을 호출 합니다.
2.  예에서 직원 수는 해당 계층의 기본 클래스 에서만 ToTable을 호출 합니다.

 

## <a name="execution-order"></a>실행 순서

규칙 마지막 wins 방식으로 Fluent API와 동일 하 게 작동합니다. 이 의미 하는 동일한 속성의 동일한 옵션을 구성 하는 두 개의 규칙을 마지막 wins 실행 하나를 작성 하는 경우. 예를 들어 아래 코드에서 모든 문자열의 최대 길이 500 개로 설정 되어 있지만 다음 호출 이름 최대 길이 250 모델에서 모든 속성을 구성 했습니다.

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

모든 문자열을 500으로 설정 된 후 최대 길이 250으로 설정 하는 규칙 이기 때문에 500 것를 모델의 Name 이라는 속성을 모두 설명 등의 다른 문자열 하는 동안 250의 MaxLength를 갖게 됩니다. 이 방식으로 규칙을 사용 하 여 형식 또는 모델 및 다음 오른쪽에 속성에 대 한 일반 규칙을 제공할 수 있음을 의미 하는 다른 하위 집합에 대 한 합니다.

Fluent API 및 데이터 주석 특정 사례에 규칙을 재정의 하려면 사용할 수 있습니다. 위의 예에서 Fluent API를 사용 하는 속성의 최대 길이 설정 하는 경우 다음 수 있는 배치 했으므로 전이나 후 규칙에 보다 구체적인 Fluent API는 보다 일반적인 구성 규칙을 통해 win 때문에 합니다.

 

## <a name="built-in-conventions"></a>기본 제공 규칙

사용자 지정 규칙은 기본 Code First 규칙에 의해 저하 될 수 있습니다, 되므로 전이나 후 다른 규칙을 실행 하는 규칙을 추가 하려면 유용할 수 있습니다. 이렇게 하려면 파생 된 DbContext에서 규칙 컬렉션의 AddBefore 및 addafter입니다 메서드를 사용할 수 있습니다. 다음 코드를 작성 하기 전에 실행 됩니다 있도록 앞에서 만든 규칙 클래스를 추가할 키 검색 규칙에서입니다.

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

이 것 가장 사용 하기 전에 또는 기본 제공된 규칙 뒤에 실행 해야 하는 규칙을 추가 하는 경우, 기본 제공된 규칙의 목록을 여기: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .

또한 모델에 적용 하지 않을 규칙을 제거할 수 있습니다. 규칙을 제거 하려면 Remove 메서드를 사용 합니다. 제거 된 PluralizingTableNameConvention의 예는 다음과 같습니다.

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
