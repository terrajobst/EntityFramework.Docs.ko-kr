---
title: 값 변환-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654789"
---
# <a name="value-conversions"></a>값 변환

> [!NOTE]  
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

값 변환기를 사용 하면 데이터베이스에서 읽거나 데이터베이스에 쓸 때 속성 값을 변환할 수 있습니다. 이러한 변환은 한 값에서 같은 형식의 다른 값 (예: 문자열 암호화) 또는 한 형식의 값에서 다른 형식의 값으로 변환 될 수 있습니다. 예를 들어 데이터베이스에 있는 문자열에서 열거형 값으로 변환 하는 것이 가능 합니다.

## <a name="fundamentals"></a>기본 사항

값 변환기는 `ModelClrType` 및 `ProviderClrType`를 기준으로 지정 됩니다. 모델 형식은 엔터티 형식의 속성에 대 한 .NET 형식입니다. 공급자 형식은 데이터베이스 공급자가 인식 하는 .NET 형식입니다. 예를 들어 데이터베이스에 열거형을 문자열로 저장 하는 경우 모델 형식은 열거형의 형식이 고 공급자 형식은 `String`입니다. 이러한 두 유형은 동일할 수 있습니다.

변환은 두 개의 `Func` 식 트리를 사용 하 여 정의 됩니다. 하나는 `ModelClrType`에서 `ProviderClrType`, 다른 하나는 `ProviderClrType`에서 `ModelClrType`로 정의 됩니다. 식 트리를 사용 하 여 효율적인 변환을 위해 데이터베이스 액세스 코드로 컴파일할 수 있습니다. 복합 변환의 경우 식 트리는 변환을 수행 하는 메서드를 간단 하 게 호출할 수 있습니다.

## <a name="configuring-a-value-converter"></a>값 변환기 구성

값 변환은 `DbContext``OnModelCreating` 속성에 대해 정의 됩니다. 예를 들어 열거형 및 엔터티 형식이 다음과 같이 정의 되어 있다고 가정 합니다.

``` csharp
public class Rider
{
    public int Id { get; set; }
    public EquineBeast Mount { get; set; }
}

public enum EquineBeast
{
    Donkey,
    Mule,
    Horse,
    Unicorn
}
```

그런 다음 `OnModelCreating`에서 변환을 정의 하 여 데이터베이스에 열거형 값을 문자열 (예: "Donkey", "Mule", ...)로 저장할 수 있습니다.

``` csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder
        .Entity<Rider>()
        .Property(e => e.Mount)
        .HasConversion(
            v => v.ToString(),
            v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));
}
```

> [!NOTE]  
> `null` 값은 값 변환기에 전달 되지 않습니다. 이렇게 하면 변환의 구현이 더 쉬워질 뿐만 아니라 nullable 및 null을 허용 하지 않는 속성 간에 공유 될 수 있습니다.

## <a name="the-valueconverter-class"></a>ValueConverter 클래스

위에 표시 된 대로 `HasConversion`를 호출 하면 `ValueConverter` 인스턴스가 생성 되 고 속성에 설정 됩니다. 대신 `ValueConverter`을 명시적으로 만들 수 있습니다. 예를 들면,

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

이는 여러 속성이 같은 변환을 사용 하는 경우에 유용할 수 있습니다.

> [!NOTE]  
> 현재는 지정 된 형식의 모든 속성에 동일한 값 변환기를 사용 해야 하는 한 곳에서 지정할 수 있는 방법이 없습니다. 이 기능은 이후 릴리스에서 고려 될 예정입니다.

## <a name="built-in-converters"></a>기본 제공 변환기

EF Core는 `Microsoft.EntityFrameworkCore.Storage.ValueConversion` 네임 스페이스에 있는 미리 정의 된 `ValueConverter` 클래스 집합과 함께 제공 됩니다. 이러한 방법은 다음과 같습니다.

* `BoolToZeroOneConverter`-Bool to 0 및 one
* `BoolToStringConverter`-Bool에서 "Y" 및 "N"과 같은 문자열
* `BoolToTwoValuesConverter`-Bool을 임의의 두 값으로
* `BytesToStringConverter` 바이트 배열을 b a s e 64로 인코딩된 문자열로
* 형식 캐스트만 필요한 `CastingConverter` 변환
* `CharToStringConverter`-문자를 단일 문자열로
* `DateTimeOffsetToBinaryConverter`-DateTimeOffset에서 이진 인코딩 64 비트 값
* `DateTimeOffsetToBytesConverter`-DateTimeOffset에서 바이트 배열로
* `DateTimeOffsetToStringConverter`-DateTimeOffset to string
* Datetimekind.utc를 포함 하는 `DateTimeToBinaryConverter`-DateTime-64 비트 값
* `DateTimeToStringConverter`-DateTime to string
* `DateTimeToTicksConverter`-DateTime에서 틱으로
* `EnumToNumberConverter` 열거형 (기본 숫자)
* `EnumToStringConverter`-열거형
* `GuidToBytesConverter`-바이트 배열에 대 한 Guid
* `GuidToStringConverter`-Guid를 문자열로
* `NumberToBytesConverter`-바이트 배열에 대 한 숫자 값
* `NumberToStringConverter`-문자열에 대 한 숫자 값
* `StringToBytesConverter`-문자열-UTF8 바이트
* `TimeSpanToStringConverter`-TimeSpan to string
* `TimeSpanToTicksConverter`-TimeSpan to 틱

이 목록에 `EnumToStringConverter` 포함 되어 있습니다. 즉, 위와 같이 변환을 명시적으로 지정할 필요가 없습니다. 대신 기본 제공 변환기를 사용 하면 됩니다.

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

모든 기본 제공 변환기는 상태 비저장 이므로 단일 인스턴스를 여러 속성에서 안전 하 게 공유할 수 있습니다.

## <a name="pre-defined-conversions"></a>미리 정의 된 변환

기본 변환기가 존재 하는 일반적인 변환의 경우 변환기를 명시적으로 지정할 필요가 없습니다. 대신 사용 해야 하는 공급자 유형을 구성 하기만 하면 EF에서 적절 한 기본 제공 변환기를 자동으로 사용 합니다. 문자열 변환의 Enum은 위의 예제로 사용 되지만, EF는 공급자 형식이 구성 된 경우 실제로이 작업을 자동으로 수행 합니다.

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

열 유형을 명시적으로 지정 하 여 동일한 작업을 수행할 수 있습니다. 예를 들어 엔터티 형식이 다음과 같이 정의 된 경우

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

그런 다음 `OnModelCreating`의 추가 구성 없이 열거 값이 데이터베이스에 문자열로 저장 됩니다.

## <a name="limitations"></a>제한 사항

값 변환 시스템에 대 한 몇 가지 알려진 현재 제한 사항이 있습니다.

* 위에서 설명한 것 처럼 `null`는 변환할 수 없습니다.
* 현재 하나의 속성을 여러 열로 변환 하거나 그 반대로 분배할 수 있는 방법은 없습니다.
* 값 변환의 사용은 식을 SQL로 변환 하는 EF Core의 기능에 영향을 줄 수 있습니다. 이러한 경우에는 경고가 기록 됩니다.
이러한 제한 제거는 이후 버전에서 고려 되 고 있습니다.
