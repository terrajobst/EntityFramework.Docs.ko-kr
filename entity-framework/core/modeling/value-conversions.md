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
# <a name="value-conversions"></a><span data-ttu-id="e8319-102">값 변환</span><span class="sxs-lookup"><span data-stu-id="e8319-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="e8319-103">이 기능은 EF Core 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e8319-104">값 변환기에서 읽거나 데이터베이스에 쓸 때 변환할 속성 값을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="e8319-105">이 변환은 한 형식의 값 (예를 들어 변환 열거형 값을 데이터베이스에 대 한 문자열에서) 다른 형식의 값 또는 값 (예를 들어 암호화 문자열) 같은 형식의 다른에서 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="e8319-106">기본 사항</span><span class="sxs-lookup"><span data-stu-id="e8319-106">Fundamentals</span></span>

<span data-ttu-id="e8319-107">값 변환기의 측면에서 지정 된 된 `ModelClrType` 및 `ProviderClrType`합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="e8319-108">모델 형식은 엔터티 형식에서 속성의.NET 형식이입니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="e8319-109">공급자 형식은 데이터베이스 공급자가 이해 하는.NET 형식이입니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="e8319-110">예를 들어 데이터베이스에서 문자열로 열거형 저장, 모델 형식 열거형 형식의 이며 공급자 형식을 `String`합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="e8319-111">이러한 두 형식이 같을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-111">These two types can be the same.</span></span>

<span data-ttu-id="e8319-112">변환이 두 개를 사용 하 여 정의 됩니다 `Func` 식 트리: 중 하 나와 `ModelClrType` 에 `ProviderClrType` 에서 다른 `ProviderClrType` 에 `ModelClrType`.</span><span class="sxs-lookup"><span data-stu-id="e8319-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="e8319-113">식 트리는 효율적인 변환에 대 한 데이터베이스 액세스 코드로 컴파일될 수 있도록 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="e8319-114">복잡 한 변환, 식 트리 변환을 수행 하는 메서드에 대 한 단순 호출 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="e8319-115">값 변환기를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-115">Configuring a value converter</span></span>

<span data-ttu-id="e8319-116">값 변환의 속성에 정의 된 합니다 `OnModelCreating` 의 프로그램 `DbContext`합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-116">Value conversions are defined on properties in the `OnModelCreating` of your `DbContext`.</span></span> <span data-ttu-id="e8319-117">예를 들어으로 정의 하는 열거형 및 엔터티 형식:</span><span class="sxs-lookup"><span data-stu-id="e8319-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="e8319-118">변환을 정의할 수 있습니다 다음 `OnModelCreating` 열거형 값 문자열 (예: "당나귀", "노새",...) 데이터베이스에 저장 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-118">Then conversions can be defined in `OnModelCreating` to store the enum values as strings (for example, "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="e8319-119">`null` 값 값 변환기에 전달 되지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="e8319-120">이 변환의 구현을 쉽게 및 nullable과 비 nullable 속성 간에 공유 될 수 있도록 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="e8319-121">ValueConverter 클래스</span><span class="sxs-lookup"><span data-stu-id="e8319-121">The ValueConverter class</span></span>

<span data-ttu-id="e8319-122">호출 `HasConversion` 만들어집니다 위에 표시 된 대로 `ValueConverter` 인스턴스 및 속성에 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="e8319-123">`ValueConverter` 대신 명시적으로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="e8319-124">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="e8319-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="e8319-125">이 기능은 여러 속성이 같은 변환을 사용 하는 경우에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="e8319-126">지정 된 형식의 모든 속성에서 동일한 값 변환기를 사용 해야 함을 한곳에서 지정할 수 없으므로 현재 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="e8319-127">이 기능은 이후 릴리스에서 간주 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="e8319-128">기본 제공 변환기</span><span class="sxs-lookup"><span data-stu-id="e8319-128">Built-in converters</span></span>

<span data-ttu-id="e8319-129">집합을 사용 하 여 EF Core는 미리 정의 된 `ValueConverter` 클래스에는 `Microsoft.EntityFrameworkCore.Storage.ValueConversion` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.ValueConversion` namespace.</span></span> <span data-ttu-id="e8319-130">이러한 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-130">These are:</span></span>
* <span data-ttu-id="e8319-131">`BoolToZeroOneConverter` -0과 1 Bool</span><span class="sxs-lookup"><span data-stu-id="e8319-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="e8319-132">`BoolToStringConverter` -"Y"와 "N"와 같은 문자열에 Bool</span><span class="sxs-lookup"><span data-stu-id="e8319-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="e8319-133">`BoolToTwoValuesConverter` -두 값을 Bool</span><span class="sxs-lookup"><span data-stu-id="e8319-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="e8319-134">`BytesToStringConverter` 바이트 배열을 Base64로 인코딩된 문자열로</span><span class="sxs-lookup"><span data-stu-id="e8319-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="e8319-135">`CastingConverter` -Csharp 캐스팅만 필요로 하는 변환</span><span class="sxs-lookup"><span data-stu-id="e8319-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="e8319-136">`CharToStringConverter` -단일 문자 문자열에 문자</span><span class="sxs-lookup"><span data-stu-id="e8319-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="e8319-137">`DateTimeOffsetToBinaryConverter` -64 비트 값 이진 인코딩된 DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="e8319-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="e8319-138">`DateTimeOffsetToBytesConverter` -DateTimeOffset 바이트 배열</span><span class="sxs-lookup"><span data-stu-id="e8319-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="e8319-139">`DateTimeOffsetToStringConverter` -DateTimeOffset 문자열</span><span class="sxs-lookup"><span data-stu-id="e8319-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="e8319-140">`DateTimeToBinaryConverter` -DateTimeKind를 포함 하 여 64 비트 값으로 날짜/시간</span><span class="sxs-lookup"><span data-stu-id="e8319-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="e8319-141">`DateTimeToStringConverter` -문자열을 날짜/시간</span><span class="sxs-lookup"><span data-stu-id="e8319-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="e8319-142">`DateTimeToTicksConverter` -날짜/시간을 틱</span><span class="sxs-lookup"><span data-stu-id="e8319-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="e8319-143">`EnumToNumberConverter` -열거형 기본 수</span><span class="sxs-lookup"><span data-stu-id="e8319-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="e8319-144">`EnumToStringConverter` -문자열 열거형</span><span class="sxs-lookup"><span data-stu-id="e8319-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="e8319-145">`GuidToBytesConverter` 바이트 배열에 Guid</span><span class="sxs-lookup"><span data-stu-id="e8319-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="e8319-146">`GuidToStringConverter` Guid 문자열을</span><span class="sxs-lookup"><span data-stu-id="e8319-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="e8319-147">`NumberToBytesConverter` 숫자 값-바이트 배열</span><span class="sxs-lookup"><span data-stu-id="e8319-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="e8319-148">`NumberToStringConverter` -문자열을 숫자 값</span><span class="sxs-lookup"><span data-stu-id="e8319-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="e8319-149">`StringToBytesConverter` -UTF8 바이트 문자열</span><span class="sxs-lookup"><span data-stu-id="e8319-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="e8319-150">`TimeSpanToStringConverter` -문자열을 TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e8319-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="e8319-151">`TimeSpanToTicksConverter` -틱 TimeSpan</span><span class="sxs-lookup"><span data-stu-id="e8319-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="e8319-152">있음을 `EnumToStringConverter` 이 목록에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="e8319-153">이 변환 위에 표시 된 대로 명시적으로 지정할 필요가 없습니다 임을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="e8319-154">기본 제공 변환기를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="e8319-155">모든 기본 제공 변환기는 상태 비저장 있으며 따라서 단일 인스턴스를 공유할 수 있는 안전 하 게 여러 속성으로 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="e8319-156">미리 정의 된 변환</span><span class="sxs-lookup"><span data-stu-id="e8319-156">Pre-defined conversions</span></span>

<span data-ttu-id="e8319-157">기본 제공 변환기 존재 하는 일반적인 변환에 대 한 변환기를 명시적으로 지정할 필요가 없습니다 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="e8319-158">대신 공급자 형식을 사용 해야만 구성 하 고 EF는 자동으로 적절 한 기본 제공 변환기를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate built-in converter.</span></span> <span data-ttu-id="e8319-159">문자열 변환 하는 열거형은 위의 예제로 사용 하지만 EF에서는 실제로이 자동으로 수행 된 공급자 형식을 구성 된 경우:</span><span class="sxs-lookup"><span data-stu-id="e8319-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="e8319-160">명시적으로 열 형식을 지정 하 여 동일한 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="e8319-161">엔터티 형식이 정의 되어 있는 경우와 같은 예를 들어 있으므로:</span><span class="sxs-lookup"><span data-stu-id="e8319-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="e8319-162">열거형 값의 추가 구성 없이 데이터베이스에서 문자열로 저장한 다음 `OnModelCreating`합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-162">Then the enum values will be saved as strings in the database without any further configuration in `OnModelCreating`.</span></span>

## <a name="limitations"></a><span data-ttu-id="e8319-163">제한 사항</span><span class="sxs-lookup"><span data-stu-id="e8319-163">Limitations</span></span>

<span data-ttu-id="e8319-164">값 변환 시스템 몇 가지 알려진된 현재 제한이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-164">There are a few known current limitations of the value conversion system:</span></span>
* <span data-ttu-id="e8319-165">위에서 설명 했 듯이 `null` 로 변환할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="e8319-166">방법이 현재 없습니다 분산 하나의 속성이 여러 열 또는 그 반대로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="e8319-167">값 변환을 사용 하 여 SQL 식을 변환 하는 EF Core의 기능에 영향을 줄 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="e8319-168">이러한 경우에 대 한 경고가 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="e8319-169">이러한 제한 사항 제거 이후 버전으로 간주 되 고 됩니다.</span><span class="sxs-lookup"><span data-stu-id="e8319-169">Removal of these limitations is being considered for a future release.</span></span>
