---
title: 값 변환-EF Core
author: ajcvickers
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/value-conversions
ms.openlocfilehash: 93774bc1bc3887f982faeac151825a6643c1107c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414556"
---
# <a name="value-conversions"></a><span data-ttu-id="451cc-102">값 변환</span><span class="sxs-lookup"><span data-stu-id="451cc-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="451cc-103">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="451cc-104">값 변환기를 사용 하면 데이터베이스에서 읽거나 데이터베이스에 쓸 때 속성 값을 변환할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="451cc-105">이러한 변환은 한 값에서 같은 형식의 다른 값 (예: 문자열 암호화) 또는 한 형식의 값에서 다른 형식의 값으로 변환 될 수 있습니다. 예를 들어 데이터베이스에 있는 문자열에서 열거형 값으로 변환 하는 것이 가능 합니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="451cc-106">기본 사항</span><span class="sxs-lookup"><span data-stu-id="451cc-106">Fundamentals</span></span>

<span data-ttu-id="451cc-107">값 변환기는 `ModelClrType` 및 `ProviderClrType`를 기준으로 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="451cc-108">모델 형식은 엔터티 형식의 속성에 대 한 .NET 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="451cc-109">공급자 형식은 데이터베이스 공급자가 인식 하는 .NET 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="451cc-110">예를 들어 데이터베이스에 열거형을 문자열로 저장 하는 경우 모델 형식은 열거형의 형식이 고 공급자 형식은 `String`입니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="451cc-111">이러한 두 유형은 동일할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-111">These two types can be the same.</span></span>

<span data-ttu-id="451cc-112">변환은 두 개의 `Func` 식 트리를 사용 하 여 정의 됩니다. 하나는 `ModelClrType`에서 `ProviderClrType`, 다른 하나는 `ProviderClrType`에서 `ModelClrType`로 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="451cc-113">식 트리를 사용 하 여 효율적인 변환을 위해 데이터베이스 액세스 코드로 컴파일할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="451cc-114">복합 변환의 경우 식 트리는 변환을 수행 하는 메서드를 간단 하 게 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="451cc-115">값 변환기 구성</span><span class="sxs-lookup"><span data-stu-id="451cc-115">Configuring a value converter</span></span>

<span data-ttu-id="451cc-116">값 변환은 `DbContext``OnModelCreating` 속성에 대해 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="451cc-117">예를 들어 열거형 및 엔터티 형식이 다음과 같이 정의 되어 있다고 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-117">For example, consider an enum and entity type defined as:</span></span>

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

<span data-ttu-id="451cc-118">그런 다음 `OnModelCreating`에서 변환을 정의 하 여 데이터베이스에 열거형 값을 문자열 (예: "Donkey", "Mule", ...)로 저장할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>

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
> <span data-ttu-id="451cc-119">`null` 값은 값 변환기에 전달 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="451cc-120">이렇게 하면 변환의 구현이 더 쉬워질 뿐만 아니라 nullable 및 null을 허용 하지 않는 속성 간에 공유 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="451cc-121">ValueConverter 클래스</span><span class="sxs-lookup"><span data-stu-id="451cc-121">The ValueConverter class</span></span>

<span data-ttu-id="451cc-122">위에 표시 된 대로 `HasConversion`를 호출 하면 `ValueConverter` 인스턴스가 생성 되 고 속성에 설정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="451cc-123">대신 `ValueConverter`을 명시적으로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="451cc-124">예를 들면 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-124">For example:</span></span>

``` csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="451cc-125">이는 여러 속성이 같은 변환을 사용 하는 경우에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="451cc-126">현재는 지정 된 형식의 모든 속성에 동일한 값 변환기를 사용 해야 하는 한 곳에서 지정할 수 있는 방법이 없습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="451cc-127">이 기능은 이후 릴리스에서 고려 될 예정입니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="451cc-128">기본 제공 변환기</span><span class="sxs-lookup"><span data-stu-id="451cc-128">Built-in converters</span></span>

<span data-ttu-id="451cc-129">EF Core는 `Microsoft.EntityFrameworkCore.Storage.ValueConversion` 네임 스페이스에 있는 미리 정의 된 `ValueConverter` 클래스 집합과 함께 제공 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="451cc-130">이러한 설정은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-130">These are:</span></span>

* <span data-ttu-id="451cc-131">`BoolToZeroOneConverter`-Bool to 0 및 one</span><span class="sxs-lookup"><span data-stu-id="451cc-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="451cc-132">`BoolToStringConverter`-Bool에서 "Y" 및 "N"과 같은 문자열</span><span class="sxs-lookup"><span data-stu-id="451cc-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="451cc-133">`BoolToTwoValuesConverter`-Bool을 임의의 두 값으로</span><span class="sxs-lookup"><span data-stu-id="451cc-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="451cc-134">`BytesToStringConverter` 바이트 배열을 b a s e 64로 인코딩된 문자열로</span><span class="sxs-lookup"><span data-stu-id="451cc-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="451cc-135">형식 캐스트만 필요한 `CastingConverter` 변환</span><span class="sxs-lookup"><span data-stu-id="451cc-135">`CastingConverter` - Conversions that require only a type cast</span></span>
* <span data-ttu-id="451cc-136">`CharToStringConverter`-문자를 단일 문자열로</span><span class="sxs-lookup"><span data-stu-id="451cc-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="451cc-137">`DateTimeOffsetToBinaryConverter`-DateTimeOffset에서 이진 인코딩 64 비트 값</span><span class="sxs-lookup"><span data-stu-id="451cc-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="451cc-138">`DateTimeOffsetToBytesConverter`-DateTimeOffset에서 바이트 배열로</span><span class="sxs-lookup"><span data-stu-id="451cc-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="451cc-139">`DateTimeOffsetToStringConverter`-DateTimeOffset to string</span><span class="sxs-lookup"><span data-stu-id="451cc-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="451cc-140">Datetimekind.utc를 포함 하는 `DateTimeToBinaryConverter`-DateTime-64 비트 값</span><span class="sxs-lookup"><span data-stu-id="451cc-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="451cc-141">`DateTimeToStringConverter`-DateTime to string</span><span class="sxs-lookup"><span data-stu-id="451cc-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="451cc-142">`DateTimeToTicksConverter`-DateTime에서 틱으로</span><span class="sxs-lookup"><span data-stu-id="451cc-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="451cc-143">`EnumToNumberConverter` 열거형 (기본 숫자)</span><span class="sxs-lookup"><span data-stu-id="451cc-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="451cc-144">`EnumToStringConverter`-열거형</span><span class="sxs-lookup"><span data-stu-id="451cc-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="451cc-145">`GuidToBytesConverter`-바이트 배열에 대 한 Guid</span><span class="sxs-lookup"><span data-stu-id="451cc-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="451cc-146">`GuidToStringConverter`-Guid를 문자열로</span><span class="sxs-lookup"><span data-stu-id="451cc-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="451cc-147">`NumberToBytesConverter`-바이트 배열에 대 한 숫자 값</span><span class="sxs-lookup"><span data-stu-id="451cc-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="451cc-148">`NumberToStringConverter`-문자열에 대 한 숫자 값</span><span class="sxs-lookup"><span data-stu-id="451cc-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="451cc-149">`StringToBytesConverter`-문자열-UTF8 바이트</span><span class="sxs-lookup"><span data-stu-id="451cc-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="451cc-150">`TimeSpanToStringConverter`-TimeSpan to string</span><span class="sxs-lookup"><span data-stu-id="451cc-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="451cc-151">`TimeSpanToTicksConverter`-TimeSpan to 틱</span><span class="sxs-lookup"><span data-stu-id="451cc-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="451cc-152">이 목록에 `EnumToStringConverter` 포함 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="451cc-153">즉, 위와 같이 변환을 명시적으로 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="451cc-154">대신 기본 제공 변환기를 사용 하면 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-154">Instead, just use the built-in converter:</span></span>

``` csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```

<span data-ttu-id="451cc-155">모든 기본 제공 변환기는 상태 비저장 이므로 단일 인스턴스를 여러 속성에서 안전 하 게 공유할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="451cc-156">미리 정의 된 변환</span><span class="sxs-lookup"><span data-stu-id="451cc-156">Pre-defined conversions</span></span>

<span data-ttu-id="451cc-157">기본 변환기가 존재 하는 일반적인 변환의 경우 변환기를 명시적으로 지정할 필요가 없습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="451cc-158">대신 사용 해야 하는 공급자 유형을 구성 하기만 하면 EF에서 적절 한 기본 제공 변환기를 자동으로 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="451cc-159">문자열 변환의 Enum은 위의 예제로 사용 되지만, EF는 공급자 형식이 구성 된 경우 실제로이 작업을 자동으로 수행 합니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>

``` csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```

<span data-ttu-id="451cc-160">열 유형을 명시적으로 지정 하 여 동일한 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="451cc-161">예를 들어 엔터티 형식이 다음과 같이 정의 된 경우</span><span class="sxs-lookup"><span data-stu-id="451cc-161">For example, if the entity type is defined like so:</span></span>

``` csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```

<span data-ttu-id="451cc-162">그런 다음 `OnModelCreating`의 추가 구성 없이 열거 값이 데이터베이스에 문자열로 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="451cc-163">제한 사항</span><span class="sxs-lookup"><span data-stu-id="451cc-163">Limitations</span></span>

<span data-ttu-id="451cc-164">값 변환 시스템에 대 한 몇 가지 알려진 현재 제한 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-164">There are a few known current limitations of the value conversion system:</span></span>

* <span data-ttu-id="451cc-165">위에서 설명한 것 처럼 `null`는 변환할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="451cc-166">현재 하나의 속성을 여러 열로 변환 하거나 그 반대로 분배할 수 있는 방법은 없습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="451cc-167">값 변환의 사용은 식을 SQL로 변환 하는 EF Core의 기능에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="451cc-168">이러한 경우에는 경고가 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="451cc-169">이러한 제한 제거는 이후 버전에서 고려 되 고 있습니다.</span><span class="sxs-lookup"><span data-stu-id="451cc-169">Removal of these limitations is being considered for a future release.</span></span>
