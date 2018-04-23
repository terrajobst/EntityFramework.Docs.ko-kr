---
title: 값 변환-EF 코어
author: ajcvickers
ms.author: divega
ms.date: 02/19/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/value-conversions
ms.openlocfilehash: 329d2757059462468ca30772d37789343c03ba7b
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/16/2018
---
# <a name="value-conversions"></a><span data-ttu-id="ea729-102">값 변환</span><span class="sxs-lookup"><span data-stu-id="ea729-102">Value Conversions</span></span>

> [!NOTE]  
> <span data-ttu-id="ea729-103">이 기능은 EF 코어 2.1의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="ea729-104">값 변환기에서 읽거나 데이터베이스에 쓸 때 변환할 속성 값을 허용 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-104">Value converters allow property values to be converted when reading from or writing to the database.</span></span> <span data-ttu-id="ea729-105">이 변환은 할 수 있습니다 (예: 암호화 문자열) 동일한 형식의 다른 값에서 다른 형식 (예를 들어 변환 하는 동안 열거형에서에서 값을 데이터베이스에는 문자열입니다.)의 값으로 한 형식의 값</span><span class="sxs-lookup"><span data-stu-id="ea729-105">This conversion can be from one value to another of the same type (for example, encrypting strings) or from a value of one type to a value of another type (for example, converting enum values to and from strings in the database.)</span></span>

## <a name="fundamentals"></a><span data-ttu-id="ea729-106">기본 사항</span><span class="sxs-lookup"><span data-stu-id="ea729-106">Fundamentals</span></span>

<span data-ttu-id="ea729-107">측면에서 지정 된 값 변환기는 `ModelClrType` 및 `ProviderClrType`합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-107">Value converters are specified in terms of a `ModelClrType` and a `ProviderClrType`.</span></span> <span data-ttu-id="ea729-108">모델 형식은 엔터티 형식에서 속성의.NET 형식이입니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-108">The model type is the .NET type of the property in the entity type.</span></span> <span data-ttu-id="ea729-109">공급자 형식은 데이터베이스 공급자가 이해 하는.NET 형식이입니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-109">The provider type is the .NET type understood by the database provider.</span></span> <span data-ttu-id="ea729-110">예를 들어 열거형은 데이터베이스에서 문자열을 저장 하려면 모델 유형에 열거형 형식 이므로 공급자 유형 `String`합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-110">For example, to save enums as strings in the database, the model type is the type of the enum, and the provider type is `String`.</span></span> <span data-ttu-id="ea729-111">이 두 가지 종류로 같을 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-111">These two types can be the same.</span></span>

<span data-ttu-id="ea729-112">변환이 두 개를 사용 하 여 정의 됩니다 `Func` 식 트리:에서 `ModelClrType` 를 `ProviderClrType` 에서 다른 `ProviderClrType` 를 `ModelClrType`합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-112">Conversions are defined using two `Func` expression trees: one from `ModelClrType` to `ProviderClrType` and the other from `ProviderClrType` to `ModelClrType`.</span></span> <span data-ttu-id="ea729-113">식 트리는 효율적인 변환에 대 한 데이터베이스 액세스 코드로 컴파일될 수 있도록 사용 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-113">Expression trees are used so that they can be compiled into the database access code for efficient conversions.</span></span> <span data-ttu-id="ea729-114">복잡 한 변환, 식 트리는 변환을 수행 하는 메서드에 대 한 단순 호출 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-114">For complex conversions, the expression tree may be a simple call to a method that performs the conversion.</span></span>

## <a name="configuring-a-value-converter"></a><span data-ttu-id="ea729-115">값 변환기를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-115">Configuring a value converter</span></span>

<span data-ttu-id="ea729-116">값 변환에 DbContext OnModelCreating에서 속성에 정의 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-116">Value conversions are defined on properties in the OnModelCreating of your DbContext.</span></span> <span data-ttu-id="ea729-117">예를 들어로 정의 하는 열거형 및 엔터티 형식:</span><span class="sxs-lookup"><span data-stu-id="ea729-117">For example, consider an enum and entity type defined as:</span></span>
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
<span data-ttu-id="ea729-118">다음 변환 정의할 수 있습니다 (예: 문자열 형식으로 열거형 값을 저장 하는 OnModelCreating "당나귀", "노새",...) 데이터베이스:</span><span class="sxs-lookup"><span data-stu-id="ea729-118">Then conversions can be defined in OnModelCreating to store the enum values as strings (e.g. "Donkey", "Mule", ...) in the database:</span></span>
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
> <span data-ttu-id="ea729-119">A `null` 값 값 변환기에 전달 되지 것입니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-119">A `null` value will never be passed to a value converter.</span></span> <span data-ttu-id="ea729-120">이 변환의 구현은 보다 쉽게 있으며 null을 허용 하 고 nullable이 아닌 속성 간에 공유 될 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-120">This makes the implementation of conversions easier and allows them to be shared amongst nullable and non-nullable properties.</span></span>

## <a name="the-valueconverter-class"></a><span data-ttu-id="ea729-121">ValueConverter 클래스</span><span class="sxs-lookup"><span data-stu-id="ea729-121">The ValueConverter class</span></span>

<span data-ttu-id="ea729-122">호출 `HasConversion` 위와 같이 만들어집니다는 `ValueConverter` 인스턴스 및 속성에 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-122">Calling `HasConversion` as shown above will create a `ValueConverter` instance and set it on the property.</span></span> <span data-ttu-id="ea729-123">`ValueConverter` 대신 명시적으로 만들 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-123">The `ValueConverter` can instead be created explicitly.</span></span> <span data-ttu-id="ea729-124">예를 들어:</span><span class="sxs-lookup"><span data-stu-id="ea729-124">For example:</span></span>
```Csharp
var converter = new ValueConverter<EquineBeast, string>(
    v => v.ToString(),
    v => (EquineBeast)Enum.Parse(typeof(EquineBeast), v));

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="ea729-125">여러 속성에 동일한 변환을 사용 하는 경우에 유용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-125">This can be useful when multiple properties use the same conversion.</span></span>

> [!NOTE]  
> <span data-ttu-id="ea729-126">지정 된 형식의 모든 속성은 동일한 값 변환기를 사용 해야 함을 한 곳에서 지정할 수 있는 방법은 현재 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-126">There is currently no way to specify in one place that every property of a given type must use the same value converter.</span></span> <span data-ttu-id="ea729-127">이 기능은 이후 버전에 대 한 고려 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-127">This feature will be considered for a future release.</span></span>

## <a name="built-in-converters"></a><span data-ttu-id="ea729-128">기본 제공 변환기</span><span class="sxs-lookup"><span data-stu-id="ea729-128">Built-in converters</span></span>

<span data-ttu-id="ea729-129">EF 코어 배 집합이 있는 미리 정의 된 `ValueConverter` 에서 찾은 클래스는 `Microsoft.EntityFrameworkCore.Storage.Converters` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-129">EF Core ships with a set of pre-defined `ValueConverter` classes, found in the `Microsoft.EntityFrameworkCore.Storage.Converters` namespace.</span></span> <span data-ttu-id="ea729-130">이러한 항목은 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-130">These are:</span></span>
* <span data-ttu-id="ea729-131">`BoolToZeroOneConverter` -0과 1에 Bool</span><span class="sxs-lookup"><span data-stu-id="ea729-131">`BoolToZeroOneConverter` - Bool to zero and one</span></span>
* <span data-ttu-id="ea729-132">`BoolToStringConverter` -예: "Y" 및 "N" 문자열로 Bool</span><span class="sxs-lookup"><span data-stu-id="ea729-132">`BoolToStringConverter` - Bool to strings such as "Y" and "N"</span></span>
* <span data-ttu-id="ea729-133">`BoolToTwoValuesConverter` -Bool 하는 두 값 수</span><span class="sxs-lookup"><span data-stu-id="ea729-133">`BoolToTwoValuesConverter` - Bool to any two values</span></span>
* <span data-ttu-id="ea729-134">`BytesToStringConverter` Base64 인코딩된 문자열을 바이트 배열</span><span class="sxs-lookup"><span data-stu-id="ea729-134">`BytesToStringConverter` - Byte array to Base64-encoded string</span></span>
* <span data-ttu-id="ea729-135">`CastingConverter` -Csharp 캐스트만 필요로 하는 변환</span><span class="sxs-lookup"><span data-stu-id="ea729-135">`CastingConverter` - Conversions that require only a Csharp cast</span></span>
* <span data-ttu-id="ea729-136">`CharToStringConverter` -단일 문자열에 문자</span><span class="sxs-lookup"><span data-stu-id="ea729-136">`CharToStringConverter` - Char to single character string</span></span>
* <span data-ttu-id="ea729-137">`DateTimeOffsetToBinaryConverter` -64 비트 이진 인코딩 값으로 DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ea729-137">`DateTimeOffsetToBinaryConverter` - DateTimeOffset to binary-encoded 64-bit value</span></span>
* <span data-ttu-id="ea729-138">`DateTimeOffsetToBytesConverter` -바이트 배열에 DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="ea729-138">`DateTimeOffsetToBytesConverter` - DateTimeOffset to byte array</span></span>
* <span data-ttu-id="ea729-139">`DateTimeOffsetToStringConverter` -DateTimeOffset 문자열로</span><span class="sxs-lookup"><span data-stu-id="ea729-139">`DateTimeOffsetToStringConverter` - DateTimeOffset to string</span></span>
* <span data-ttu-id="ea729-140">`DateTimeToBinaryConverter` -DateTime DateTimeKind를 포함 하는 64 비트 값</span><span class="sxs-lookup"><span data-stu-id="ea729-140">`DateTimeToBinaryConverter` - DateTime to 64-bit value including DateTimeKind</span></span>
* <span data-ttu-id="ea729-141">`DateTimeToStringConverter` -문자열로 날짜/시간</span><span class="sxs-lookup"><span data-stu-id="ea729-141">`DateTimeToStringConverter` - DateTime to string</span></span>
* <span data-ttu-id="ea729-142">`DateTimeToTicksConverter` -틱으로 날짜/시간</span><span class="sxs-lookup"><span data-stu-id="ea729-142">`DateTimeToTicksConverter` - DateTime to ticks</span></span>
* <span data-ttu-id="ea729-143">`EnumToNumberConverter` -열거형 기본 수</span><span class="sxs-lookup"><span data-stu-id="ea729-143">`EnumToNumberConverter` - Enum to underlying number</span></span>
* <span data-ttu-id="ea729-144">`EnumToStringConverter` -문자열로 열거형</span><span class="sxs-lookup"><span data-stu-id="ea729-144">`EnumToStringConverter` - Enum to string</span></span>
* <span data-ttu-id="ea729-145">`GuidToBytesConverter` -바이트 배열에 Guid</span><span class="sxs-lookup"><span data-stu-id="ea729-145">`GuidToBytesConverter` - Guid to byte array</span></span>
* <span data-ttu-id="ea729-146">`GuidToStringConverter` -Guid를 문자열로</span><span class="sxs-lookup"><span data-stu-id="ea729-146">`GuidToStringConverter` - Guid to string</span></span>
* <span data-ttu-id="ea729-147">`NumberToBytesConverter` -바이트 배열에 모든 숫자 값</span><span class="sxs-lookup"><span data-stu-id="ea729-147">`NumberToBytesConverter` - Any numerical value to byte array</span></span>
* <span data-ttu-id="ea729-148">`NumberToStringConverter` -모든 숫자 값을 문자열로</span><span class="sxs-lookup"><span data-stu-id="ea729-148">`NumberToStringConverter` - Any numerical value to string</span></span>
* <span data-ttu-id="ea729-149">`StringToBytesConverter` -U t f 8 바이트 문자열</span><span class="sxs-lookup"><span data-stu-id="ea729-149">`StringToBytesConverter` - String to UTF8 bytes</span></span>
* <span data-ttu-id="ea729-150">`TimeSpanToStringConverter` -문자열을 TimeSpan</span><span class="sxs-lookup"><span data-stu-id="ea729-150">`TimeSpanToStringConverter` - TimeSpan to string</span></span>
* <span data-ttu-id="ea729-151">`TimeSpanToTicksConverter` -TimeSpan 틱</span><span class="sxs-lookup"><span data-stu-id="ea729-151">`TimeSpanToTicksConverter` - TimeSpan to ticks</span></span>

<span data-ttu-id="ea729-152">다음에 유의 `EnumToStringConverter` 이 목록에 포함 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-152">Notice that `EnumToStringConverter` is included in this list.</span></span> <span data-ttu-id="ea729-153">변환 위에 표시 된 대로 명시적으로 지정할 필요가 없습니다 있다는 것을 의미 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-153">This means that there is no need to specify the conversion explicitly, as shown above.</span></span> <span data-ttu-id="ea729-154">대신, 기본 제공 된 변환기를 사용 하기만 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-154">Instead, just use the built-in converter:</span></span>
```Csharp
var converter = new EnumToStringConverter<EquineBeast>();

modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion(converter);
```
<span data-ttu-id="ea729-155">모든 기본 제공 변환기는 상태 비저장 및 하므로 단일 인스턴스를 안전 하 게 공유할 수 여러 속성에 의해 note 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-155">Note that all the built-in converters are stateless and so a single instance can be safely shared by multiple properties.</span></span>

## <a name="pre-defined-conversions"></a><span data-ttu-id="ea729-156">미리 정의 된 변환</span><span class="sxs-lookup"><span data-stu-id="ea729-156">Pre-defined conversions</span></span>

<span data-ttu-id="ea729-157">기본 제공 된 변환기 존재 하는 일반적인 변환에 대 한 변환기를 명시적으로 지정 하려면 않아도가 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-157">For common conversions for which a built-in converter exists there is no need to specify the converter explicitly.</span></span> <span data-ttu-id="ea729-158">대신, 어떤 공급자 형식을 사용 해야만 구성 하 고 EF 자동으로 적절 한 빌드-변환기를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-158">Instead, just configure which provider type should be used and EF will automatically use the appropriate build-in converter.</span></span> <span data-ttu-id="ea729-159">위의 예제로 사용 되는 문자열 변환 하는 열거형 이지만 EF에서는 실제로이 자동으로 수행 된 공급자 형식을 구성 된 경우:</span><span class="sxs-lookup"><span data-stu-id="ea729-159">Enum to string conversions are used as an example above, but EF will actually do this automatically if the provider type is configured:</span></span>
```Csharp
modelBuilder
    .Entity<Rider>()
    .Property(e => e.Mount)
    .HasConversion<string>();
```
<span data-ttu-id="ea729-160">명시적으로 열 형식을 지정 하 여 동일한 작업을 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-160">The same thing can be achieved by explicitly specifying the column type.</span></span> <span data-ttu-id="ea729-161">엔터티 형식이 정의 되어 있으면 같은 예를 들어 있으므로:</span><span class="sxs-lookup"><span data-stu-id="ea729-161">For example, if the entity type is defined like so:</span></span>
```Csharp
public class Rider
{
    public int Id { get; set; }

    [Column(TypeName = "nvarchar(24)")]
    public EquineBeast Mount { get; set; }
}
```
<span data-ttu-id="ea729-162">다음 열거형 값 문자열로 OnModelCreating에서 추가 구성 없이 데이터베이스에 저장 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-162">Then the enum values will be saved as strings in the database without any further configuration in OnModelCreating.</span></span>

## <a name="limitations"></a><span data-ttu-id="ea729-163">제한 사항</span><span class="sxs-lookup"><span data-stu-id="ea729-163">Limitations</span></span>

<span data-ttu-id="ea729-164">값 변환 시스템의 몇 가지 알려진된 현재 제한 되어 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-164">There are a few known current limitations of the value convertion system:</span></span>
* <span data-ttu-id="ea729-165">위에서 언급 했 듯이 `null` 로 변환할 수 없습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-165">As noted above, `null` cannot be converted.</span></span>
* <span data-ttu-id="ea729-166">현재 방법이 있으면 확산 한 속성의 여러 열 이나 그 반대로으로 변환 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-166">There is currently no way to spread a conversion of one property to multiple columns or vice-versa.</span></span>
* <span data-ttu-id="ea729-167">값 변환에 대 한 사용 하 여 SQL 식을 변환 하는 기능이 EF 코어의 떨어질 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-167">Use of value conversions may impact the ability of EF Core to translate expressions to SQL.</span></span> <span data-ttu-id="ea729-168">이 경우에는 경고가 기록 됩니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-168">A warning will be logged for such cases.</span></span>
<span data-ttu-id="ea729-169">향후 릴리스에 고려할 이러한 제한 제거 합니다.</span><span class="sxs-lookup"><span data-stu-id="ea729-169">Removal of these limitations is being considered for a future release.</span></span>
