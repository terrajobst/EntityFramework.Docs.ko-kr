---
title: 값 변환-EF Core
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: d5189cef6d44fdf3fd6116a2952ce07ff3a389d4
ms.sourcegitcommit: 902257be9c63c427dc793750a2b827d6feb8e38c
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/07/2018
ms.locfileid: "39614401"
---
# <a name="value-conversions"></a>값 변환

> [!NOTE]  
> 이 기능은 EF Core 2.1의 새로운 기능입니다.

값 변환기에서 읽거나 데이터베이스에 쓸 때 변환할 속성 값을 허용 합니다. 이 변환은 한 형식의 값 (예를 들어 변환 열거형 값을 데이터베이스에 대 한 문자열에서) 다른 형식의 값 또는 값 (예를 들어 암호화 문자열) 같은 형식의 다른에서 수 있습니다.

## <a name="fundamentals"></a>기본 사항

값 변환기의 측면에서 지정 된 된 `ModelClrType` 및 `ProviderClrType`합니다. 모델 형식은 엔터티 형식에서 속성의.NET 형식이입니다. 공급자 형식은 데이터베이스 공급자가 이해 하는.NET 형식이입니다. 예를 들어 데이터베이스에서 문자열로 열거형 저장, 모델 형식 열거형 형식의 이며 공급자 형식을 `String`합니다. 이러한 두 형식이 같을 수 있습니다.

변환이 두 개를 사용 하 여 정의 됩니다 `Func` 식 트리: 중 하 나와 `ModelClrType` 에 `ProviderClrType` 에서 다른 `ProviderClrType` 에 `ModelClrType`. 식 트리는 효율적인 변환에 대 한 데이터베이스 액세스 코드로 컴파일될 수 있도록 사용 됩니다. 복잡 한 변환, 식 트리 변환을 수행 하는 메서드에 대 한 단순 호출 될 수 있습니다.

## <a name="configuring-a-value-converter"></a>값 변환기를 구성합니다.

값 변환의 속성에 정의 된 합니다 `OnModelCreating` 의 프로그램 `DbContext`합니다. 예를 들어으로 정의 하는 열거형 및 엔터티 형식:
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
변환을 정의할 수 있습니다 다음 `OnModelCreating` 열거형 값 문자열 (예: "당나귀", "노새",...) 데이터베이스에 저장 합니다.
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
> `null` 값 값 변환기에 전달 되지 것입니다. 이 변환의 구현을 쉽게 및 nullable과 비 nullable 속성 간에 공유 될 수 있도록 합니다.

## <a name="the-valueconverter-class"></a>ValueConverter 클래스

호출 `HasConversion` 만들어집니다 위에 표시 된 대로 `ValueConverter` 인스턴스 및 속성에 설정 합니다. `ValueConverter` 대신 명시적으로 만들 수 있습니다. 예를 들어:
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
이 기능은 여러 속성이 같은 변환을 사용 하는 경우에 유용할 수 있습니다.

> [!NOTE]  
> 지정 된 형식의 모든 속성에서 동일한 값 변환기를 사용 해야 함을 한곳에서 지정할 수 없으므로 현재 됩니다. 이 기능은 이후 릴리스에서 간주 됩니다.

## <a name="built-in-converters"></a>기본 제공 변환기

집합을 사용 하 여 EF Core는 미리 정의 된 `ValueConverter` 클래스에는 `Microsoft.EntityFrameworkCore.Storage.ValueConversion` 네임 스페이스입니다. 이러한 항목은 다음과 같습니다.
* `BoolToZeroOneConverter` -0과 1 Bool
* `BoolToStringConverter` -"Y"와 "N"와 같은 문자열에 Bool
* `BoolToTwoValuesConverter` -두 값을 Bool
* `BytesToStringConverter` 바이트 배열을 Base64로 인코딩된 문자열로
* `CastingConverter` -Csharp 캐스팅만 필요로 하는 변환
* `CharToStringConverter` -단일 문자 문자열에 문자
* `DateTimeOffsetToBinaryConverter` -64 비트 값 이진 인코딩된 DateTimeOffset
* `DateTimeOffsetToBytesConverter` -DateTimeOffset 바이트 배열
* `DateTimeOffsetToStringConverter` -DateTimeOffset 문자열
* `DateTimeToBinaryConverter` -DateTimeKind를 포함 하 여 64 비트 값으로 날짜/시간
* `DateTimeToStringConverter` -문자열을 날짜/시간
* `DateTimeToTicksConverter` -날짜/시간을 틱
* `EnumToNumberConverter` -열거형 기본 수
* `EnumToStringConverter` -문자열 열거형
* `GuidToBytesConverter` 바이트 배열에 Guid
* `GuidToStringConverter` Guid 문자열을
* `NumberToBytesConverter` 숫자 값-바이트 배열
* `NumberToStringConverter` -문자열을 숫자 값
* `StringToBytesConverter` -UTF8 바이트 문자열
* `TimeSpanToStringConverter` -문자열을 TimeSpan
* `TimeSpanToTicksConverter` -틱 TimeSpan

있음을 `EnumToStringConverter` 이 목록에 포함 됩니다. 이 변환 위에 표시 된 대로 명시적으로 지정할 필요가 없습니다 임을 의미 합니다. 기본 제공 변환기를 사용 합니다.
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
모든 기본 제공 변환기는 상태 비저장 있으며 따라서 단일 인스턴스를 공유할 수 있는 안전 하 게 여러 속성으로 note 합니다.

## <a name="pre-defined-conversions"></a>미리 정의 된 변환

기본 제공 변환기 존재 하는 일반적인 변환에 대 한 변환기를 명시적으로 지정할 필요가 없습니다 있습니다. 대신 공급자 형식을 사용 해야만 구성 하 고 EF는 자동으로 적절 한 기본 제공 변환기를 사용 합니다. 문자열 변환 하는 열거형은 위의 예제로 사용 하지만 EF에서는 실제로이 자동으로 수행 된 공급자 형식을 구성 된 경우:
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
명시적으로 열 형식을 지정 하 여 동일한 작업을 수행할 수 있습니다. 엔터티 형식이 정의 되어 있는 경우와 같은 예를 들어 있으므로:
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
열거형 값의 추가 구성 없이 데이터베이스에서 문자열로 저장한 다음 `OnModelCreating`합니다.

## <a name="limitations"></a>제한 사항

값 변환 시스템 몇 가지 알려진된 현재 제한이 있습니다.
* 위에서 설명 했 듯이 `null` 로 변환할 수 없습니다.
* 방법이 현재 없습니다 분산 하나의 속성이 여러 열 또는 그 반대로 변환 합니다.
* 값 변환을 사용 하 여 SQL 식을 변환 하는 EF Core의 기능에 영향을 줄 수 있습니다. 이러한 경우에 대 한 경고가 기록 됩니다.
이러한 제한 사항 제거 이후 버전으로 간주 되 고 됩니다.
