---
title: 값 변환-EF 코어
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 3e97c05a87ad9b4817c03f446031ea6c74704f5b
ms.sourcegitcommit: 605e42232854ce44bae09624a6eebc35b8e2473b
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/16/2018
---
# <a name="value-conversions"></a>값 변환

> [!NOTE]  
> 이 기능은 EF 코어 2.1의 새로운 기능입니다.

값 변환기에서 읽거나 데이터베이스에 쓸 때 변환할 속성 값을 허용 합니다. 이 변환은 할 수 있습니다 (예: 암호화 문자열) 동일한 형식의 다른 값에서 다른 형식 (예를 들어 변환 하는 동안 열거형에서에서 값을 데이터베이스에는 문자열입니다.)의 값으로 한 형식의 값

## <a name="fundamentals"></a>기본 사항

측면에서 지정 된 값 변환기는 `ModelClrType` 및 `ProviderClrType`합니다. 모델 형식은 엔터티 형식에서 속성의.NET 형식이입니다. 공급자 형식은 데이터베이스 공급자가 이해 하는.NET 형식이입니다. 예를 들어 열거형은 데이터베이스에서 문자열을 저장 하려면 모델 유형에 열거형 형식 이므로 공급자 유형 `String`합니다. 이 두 가지 종류로 같을 수 있습니다.

변환이 두 개를 사용 하 여 정의 됩니다 `Func` 식 트리:에서 `ModelClrType` 를 `ProviderClrType` 에서 다른 `ProviderClrType` 를 `ModelClrType`합니다. 식 트리는 효율적인 변환에 대 한 데이터베이스 액세스 코드로 컴파일될 수 있도록 사용 됩니다. 복잡 한 변환, 식 트리는 변환을 수행 하는 메서드에 대 한 단순 호출 될 수 있습니다.

## <a name="configuring-a-value-converter"></a>값 변환기를 구성합니다.

값 변환에 DbContext OnModelCreating에서 속성에 정의 됩니다. 예를 들어로 정의 하는 열거형 및 엔터티 형식:
```Csharp
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
다음 변환 정의할 수 있습니다 (예: 문자열 형식으로 열거형 값을 저장 하는 OnModelCreating "당나귀", "노새",...) 데이터베이스:
```Csharp
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
> A `null` 값 값 변환기에 전달 되지 것입니다. 이 변환의 구현은 보다 쉽게 있으며 null을 허용 하 고 nullable이 아닌 속성 간에 공유 될 수 있습니다.

## <a name="the-valueconverter-class"></a>ValueConverter 클래스

호출 `HasConversion` 위와 같이 만들어집니다는 `ValueConverter` 인스턴스 및 속성에 설정 합니다. `ValueConverter` 대신 명시적으로 만들 수 있습니다. 예:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
여러 속성에 동일한 변환을 사용 하는 경우에 유용할 수 있습니다.

> [!NOTE]  
> 지정 된 형식의 모든 속성은 동일한 값 변환기를 사용 해야 함을 한 곳에서 지정할 수 있는 방법은 현재 됩니다. 이 기능은 이후 버전에 대 한 고려 됩니다.

## <a name="built-in-converters"></a>기본 제공 변환기

EF 코어 배 집합이 있는 미리 정의 된 `ValueConverter` 에서 찾은 클래스는 `Microsoft.EntityFrameworkCore.Storage.ValueConversion` 네임 스페이스입니다. 이러한 항목은 다음과 같습니다.
* `BoolToZeroOneConverter` -0과 1에 Bool
* `BoolToStringConverter` -예: "Y" 및 "N" 문자열로 Bool
* `BoolToTwoValuesConverter` -Bool 하는 두 값 수
* `BytesToStringConverter` Base64 인코딩된 문자열을 바이트 배열
* `CastingConverter` -Csharp 캐스트만 필요로 하는 변환
* `CharToStringConverter` -단일 문자열에 문자
* `DateTimeOffsetToBinaryConverter` -64 비트 이진 인코딩 값으로 DateTimeOffset
* `DateTimeOffsetToBytesConverter` -바이트 배열에 DateTimeOffset
* `DateTimeOffsetToStringConverter` -DateTimeOffset 문자열로
* `DateTimeToBinaryConverter` -DateTime DateTimeKind를 포함 하는 64 비트 값
* `DateTimeToStringConverter` -문자열로 날짜/시간
* `DateTimeToTicksConverter` -틱으로 날짜/시간
* `EnumToNumberConverter` -열거형 기본 수
* `EnumToStringConverter` -문자열로 열거형
* `GuidToBytesConverter` -바이트 배열에 Guid
* `GuidToStringConverter` -Guid를 문자열로
* `NumberToBytesConverter` -바이트 배열에 모든 숫자 값
* `NumberToStringConverter` -모든 숫자 값을 문자열로
* `StringToBytesConverter` -U t f 8 바이트 문자열
* `TimeSpanToStringConverter` -문자열을 TimeSpan
* `TimeSpanToTicksConverter` -TimeSpan 틱

다음에 유의 `EnumToStringConverter` 이 목록에 포함 합니다. 변환 위에 표시 된 대로 명시적으로 지정할 필요가 없습니다 있다는 것을 의미 합니다. 대신, 기본 제공 된 변환기를 사용 하기만 됩니다.
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
모든 기본 제공 변환기는 상태 비저장 및 하므로 단일 인스턴스를 안전 하 게 공유할 수 여러 속성에 의해 note 합니다.

## <a name="pre-defined-conversions"></a>미리 정의 된 변환

기본 제공 된 변환기 존재 하는 일반적인 변환에 대 한 변환기를 명시적으로 지정 하려면 않아도가 됩니다. 대신, 어떤 공급자 형식을 사용 해야만 구성 하 고 EF 자동으로 적절 한 빌드-변환기를 사용 합니다. 위의 예제로 사용 되는 문자열 변환 하는 열거형 이지만 EF에서는 실제로이 자동으로 수행 된 공급자 형식을 구성 된 경우:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
명시적으로 열 형식을 지정 하 여 동일한 작업을 수행할 수 있습니다. 엔터티 형식이 정의 되어 있으면 같은 예를 들어 있으므로:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
다음 열거형 값 문자열로 OnModelCreating에서 추가 구성 없이 데이터베이스에 저장 됩니다.

## <a name="limitations"></a>제한 사항

값 변환 시스템의 몇 가지 알려진된 현재 제한 되어 있습니다.
* 위에서 언급 했 듯이 `null` 로 변환할 수 없습니다.
* 현재 방법이 있으면 확산 한 속성의 여러 열 이나 그 반대로으로 변환 합니다.
* 값 변환에 대 한 사용 하 여 SQL 식을 변환 하는 기능이 EF 코어의 떨어질 수 있습니다. 이 경우에는 경고가 기록 됩니다.
향후 릴리스에 고려할 이러한 제한 제거 합니다.
