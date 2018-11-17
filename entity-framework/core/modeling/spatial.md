---
title: 공간 데이터-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817912"
---
# <a name="spatial-data"></a><span data-ttu-id="4460b-102">공간 데이터</span><span class="sxs-lookup"><span data-stu-id="4460b-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="4460b-103">이 기능은 EF Core 2.2의 새로운 기능입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="4460b-104">공간 데이터는 물리적 위치와 개체의 모양을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="4460b-105">많은 데이터베이스는 인덱싱 및 다른 데이터와 함께 쿼리할 수 있도록 이러한 종류의 데이터를 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="4460b-106">위치에서 지정 된 거리 내에 있는 개체에 대 한 쿼리 또는 테두리가 지정된 된 위치를 포함 하는 개체를 선택 하면 일반적인 시나리오에 포함 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="4460b-107">EF Core를 사용 하 여 공간 데이터 형식 매핑을 지원 합니다 [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) 공간 라이브러리입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="4460b-108">설치</span><span class="sxs-lookup"><span data-stu-id="4460b-108">Installing</span></span>

<span data-ttu-id="4460b-109">공간 데이터 및 EF Core를 사용 하려면 적절 한 지원 NuGet 패키지를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="4460b-110">설치 해야 하는 패키지는 사용 중인 공급자에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="4460b-111">EF Core 공급자</span><span class="sxs-lookup"><span data-stu-id="4460b-111">EF Core Provider</span></span>                        | <span data-ttu-id="4460b-112">공간 NuGet 패키지</span><span class="sxs-lookup"><span data-stu-id="4460b-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="4460b-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="4460b-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="4460b-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4460b-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="4460b-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="4460b-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="4460b-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4460b-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="4460b-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="4460b-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="4460b-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4460b-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="4460b-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="4460b-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="4460b-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4460b-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="4460b-121">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="4460b-121">Reverse engineering</span></span>

<span data-ttu-id="4460b-122">공간 NuGet 패키지 사용도 [리버스 엔지니어링](../managing-schemas/scaffolding.md) 패키지를 설치 해야 할 수 있지만 공간 속성을 사용 하 여 모델 ***하기 전에*** 실행 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold`합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="4460b-123">이렇게 하지 않으면 열에 대 한 형식 매핑을 찾지에 대 한 경고를 받게 됩니다 및 열을 건너뜁니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="4460b-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="4460b-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="4460b-125">NetTopologySuite에.NET에 대 한 공간 라이브러리입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="4460b-126">EF Core 모델에서 NTS 형식을 사용 하 여 데이터베이스의 형식은 공간 데이터 매핑 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="4460b-127">NTS 통해 공간 형식에 대 한 매핑을 사용 하려면 공급자의 DbContext 옵션 작성기 UseNetTopologySuite 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="4460b-128">예를 들어, SQL Server를 사용 하 여 다음과 같이 호출할는 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="4460b-129">공간 데이터 유형은 여러 가지가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-129">There are several spatial data types.</span></span> <span data-ttu-id="4460b-130">사용 하는 형식을 허용 하려는 모양의 종류에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="4460b-131">속성에 대 한 모델에 사용할 수 있는 NTS 형식의 계층 구조는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="4460b-132">내는 `NetTopologySuite.Geometries` 네임 스페이스입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="4460b-133">GeoAPI 패키지에 해당 하는 인터페이스가 (`GeoAPI.Geometries` 네임 스페이스)도 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="4460b-134">기 하 도형</span><span class="sxs-lookup"><span data-stu-id="4460b-134">Geometry</span></span>
  * <span data-ttu-id="4460b-135">요소</span><span class="sxs-lookup"><span data-stu-id="4460b-135">Point</span></span>
  * <span data-ttu-id="4460b-136">LineString</span><span class="sxs-lookup"><span data-stu-id="4460b-136">LineString</span></span>
  * <span data-ttu-id="4460b-137">다각형</span><span class="sxs-lookup"><span data-stu-id="4460b-137">Polygon</span></span>
  * <span data-ttu-id="4460b-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="4460b-138">GeometryCollection</span></span>
    * <span data-ttu-id="4460b-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="4460b-139">MultiPoint</span></span>
    * <span data-ttu-id="4460b-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="4460b-140">MultiLineString</span></span>
    * <span data-ttu-id="4460b-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="4460b-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="4460b-142">CircularString, CompoundCurve, 및 CurePolygon NTS에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="4460b-143">기본 기 하 도형 형식을 사용 하 여 모든 종류를의 셰이프를 속성으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="4460b-144">다음 엔터티 클래스를 사용 하 여 테이블에 매핑할 수 없습니다는 [Wide World Importers 샘플 데이터베이스](http://go.microsoft.com/fwlink/?LinkID=800630)합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a><span data-ttu-id="4460b-145">값 만들기</span><span class="sxs-lookup"><span data-stu-id="4460b-145">Creating values</span></span>

<span data-ttu-id="4460b-146">생성자를 사용 하 여 기 하 도형 개체를 만들려면 그러나 NTS 대신 geometry 팩터리를 사용 하 여 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="4460b-147">이 기본 SRID (공간 참조 시스템 좌표로 사용)을 지정할 수 있습니다 및을 사용 하면 고급 등 (계산 하는 동안 사용 됨)는 전체 자릿수 모델 및 좌표 (좌표는-크기를 결정 합니다. 시퀀스에 대 한 제어 측정값 제공 되며).</span><span class="sxs-lookup"><span data-stu-id="4460b-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="4460b-148">4326은 GPS와 다른 지리적 시스템에 사용 되는 표준 WGS 84을 가리킵니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="4460b-149">경도 및 위도</span><span class="sxs-lookup"><span data-stu-id="4460b-149">Longitude and Latitude</span></span>

<span data-ttu-id="4460b-150">NTS에 좌표가의 측면에서 X 및 Y 값입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="4460b-151">경도 및 위도 나타내는, 경도 및 Y에 대 한 X를 사용 하 여 위도입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="4460b-152">이 **이전 버전과** 에서 `latitude, longitude` 는 일반적으로 표시 되는이 값 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="4460b-153">SRID는 클라이언트 작업 중 무시</span><span class="sxs-lookup"><span data-stu-id="4460b-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="4460b-154">NTS 작업 중 SRID 값을 무시합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="4460b-155">평면 좌표계를 가정합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="4460b-156">즉, 경도 및 위도, 거리, 길이 및 영역도 하지 미터의에서 수와 같은 일부 클라이언트 평가 값 기준으로 좌표를 지정 하는 경우.</span><span class="sxs-lookup"><span data-stu-id="4460b-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="4460b-157">보다 의미 있는 값에 대 한 해야 프로젝트와 같은 라이브러리를 사용 하 여 다른 좌표 시스템 좌표 [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) 이러한 값을 계산 하기 전에 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="4460b-158">SQL 통해 EF Core에서 서버 평가 작업 인 경우 결과 단위 데이터베이스에 의해 결정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="4460b-159">ProjNet4GeoAPI를 사용 하 여 두 도시 간의 거리를 계산 하는 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a><span data-ttu-id="4460b-160">데이터 쿼리</span><span class="sxs-lookup"><span data-stu-id="4460b-160">Querying Data</span></span>

<span data-ttu-id="4460b-161">Linq에서 NTS 메서드와 데이터베이스 함수를 사용할 수 있는 속성 SQL 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="4460b-162">예를 들어, 거리 및 Contains 메서드를 다음 쿼리와에서 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="4460b-163">이 문서의 끝에 있는 표에서 다양 한 EF Core 공급자가 지원 되는 멤버를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="4460b-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4460b-164">SQL Server</span></span>

<span data-ttu-id="4460b-165">SQL Server를 사용 하는 경우 몇 가지 추가 사항을 고려해 야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="4460b-166">지리 또는 기 하 도형</span><span class="sxs-lookup"><span data-stu-id="4460b-166">Geography or geometry</span></span>

<span data-ttu-id="4460b-167">기본적으로 공간 속성에 매핑됩니다 `geography` SQL Server의 열입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="4460b-168">사용 하도록 `geometry`, [열 유형을 구성](xref:core/modeling/relational/data-types) 모델에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="4460b-169">지리 다각형 링</span><span class="sxs-lookup"><span data-stu-id="4460b-169">Geography polygon rings</span></span>

<span data-ttu-id="4460b-170">사용 하는 경우는 `geography` 열 형식, SQL Server는 외부 링 (또는 셸)에서 추가 요구 사항 및 내부 링 (또는 빈 영역이).</span><span class="sxs-lookup"><span data-stu-id="4460b-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="4460b-171">외부 링 시계 반대 방향으로 방향 이어야 하며 내부 링이 시계 방향으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="4460b-172">NTS 값 데이터베이스로 보내기 전에이 확인 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="4460b-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="4460b-173">FullGlobe</span></span>

<span data-ttu-id="4460b-174">SQL Server를 사용 하는 경우 전체 구형을 나타내는 geometry 비표준 형식에는 `geography` 열 형식입니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="4460b-175">또한 전체 구형 (없이 외부 링)에 따라 다각형을 나타낼 수가 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="4460b-176">이러한 작업은 NTS에서 지원 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="4460b-177">FullGlobe 및 다각형에 따라 NTS에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="4460b-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="4460b-178">SQLite</span></span>

<span data-ttu-id="4460b-179">SQLite를 사용 하는 몇 가지 추가 정보는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="4460b-180">SpatiaLite 설치</span><span class="sxs-lookup"><span data-stu-id="4460b-180">Installing SpatiaLite</span></span>

<span data-ttu-id="4460b-181">Windows에서 네이티브 mod_spatialite 라이브러리는 NuGet 패키지 종속성으로 배포 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="4460b-182">다른 플랫폼을 별도로 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="4460b-183">일반적으로 이렇게 소프트웨어 패키지 관리자를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="4460b-184">예를 들어, Ubuntu 및 MacOS에서 Homebrew에서 APT를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="4460b-185">SRID를 구성합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-185">Configuring SRID</span></span>

<span data-ttu-id="4460b-186">SpatiaLite, 열 열당 SRID를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="4460b-187">기본 SRID는 `0`합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-187">The default SRID is `0`.</span></span> <span data-ttu-id="4460b-188">ForSqliteHasSrid 메서드를 사용 하 여 다른 SRID를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="4460b-189">크기</span><span class="sxs-lookup"><span data-stu-id="4460b-189">Dimension</span></span>

<span data-ttu-id="4460b-190">SRID와 마찬가지로, 열의 차원 (또는 부분)도 지정 됩니다 열의 일부로.</span><span class="sxs-lookup"><span data-stu-id="4460b-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="4460b-191">기본 좌표는 X 및 Z와 M Y. 사용 추가 부분 ForSqliteHasDimension 메서드를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="4460b-192">번역된 작업</span><span class="sxs-lookup"><span data-stu-id="4460b-192">Translated Operations</span></span>

<span data-ttu-id="4460b-193">이 표에서 NTS 멤버 각 EF Core 공급자에서 SQL로 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="4460b-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="4460b-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="4460b-194">NetTopologySuite</span></span> | <span data-ttu-id="4460b-195">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-195">SQL Server (geometry)</span></span> | <span data-ttu-id="4460b-196">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="4460b-196">SQL Server (geography)</span></span> | <span data-ttu-id="4460b-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="4460b-197">SQLite</span></span> | <span data-ttu-id="4460b-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="4460b-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="4460b-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="4460b-199">Geometry.Area</span></span> | <span data-ttu-id="4460b-200">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-200">✔</span></span> | <span data-ttu-id="4460b-201">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-201">✔</span></span> | <span data-ttu-id="4460b-202">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-202">✔</span></span> | <span data-ttu-id="4460b-203">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-203">✔</span></span>
<span data-ttu-id="4460b-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="4460b-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="4460b-205">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-205">✔</span></span> | <span data-ttu-id="4460b-206">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-206">✔</span></span> | <span data-ttu-id="4460b-207">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-207">✔</span></span> | <span data-ttu-id="4460b-208">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-208">✔</span></span>
<span data-ttu-id="4460b-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="4460b-209">Geometry.AsText()</span></span> | <span data-ttu-id="4460b-210">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-210">✔</span></span> | <span data-ttu-id="4460b-211">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-211">✔</span></span> | <span data-ttu-id="4460b-212">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-212">✔</span></span> | <span data-ttu-id="4460b-213">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-213">✔</span></span>
<span data-ttu-id="4460b-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="4460b-214">Geometry.Boundary</span></span> | <span data-ttu-id="4460b-215">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-215">✔</span></span> | | <span data-ttu-id="4460b-216">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-216">✔</span></span> | <span data-ttu-id="4460b-217">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-217">✔</span></span>
<span data-ttu-id="4460b-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="4460b-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="4460b-219">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-219">✔</span></span> | <span data-ttu-id="4460b-220">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-220">✔</span></span> | <span data-ttu-id="4460b-221">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-221">✔</span></span> | <span data-ttu-id="4460b-222">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-222">✔</span></span>
<span data-ttu-id="4460b-223">Geometry.Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="4460b-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="4460b-224">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-224">✔</span></span>
<span data-ttu-id="4460b-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="4460b-225">Geometry.Centroid</span></span> | <span data-ttu-id="4460b-226">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-226">✔</span></span> | | <span data-ttu-id="4460b-227">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-227">✔</span></span> | <span data-ttu-id="4460b-228">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-228">✔</span></span>
<span data-ttu-id="4460b-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="4460b-230">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-230">✔</span></span> | <span data-ttu-id="4460b-231">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-231">✔</span></span> | <span data-ttu-id="4460b-232">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-232">✔</span></span> | <span data-ttu-id="4460b-233">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-233">✔</span></span>
<span data-ttu-id="4460b-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="4460b-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="4460b-235">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-235">✔</span></span> | <span data-ttu-id="4460b-236">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-236">✔</span></span> | <span data-ttu-id="4460b-237">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-237">✔</span></span> | <span data-ttu-id="4460b-238">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-238">✔</span></span>
<span data-ttu-id="4460b-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="4460b-240">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-240">✔</span></span> | <span data-ttu-id="4460b-241">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-241">✔</span></span>
<span data-ttu-id="4460b-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="4460b-243">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-243">✔</span></span> | <span data-ttu-id="4460b-244">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-244">✔</span></span>
<span data-ttu-id="4460b-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="4460b-246">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-246">✔</span></span> | | <span data-ttu-id="4460b-247">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-247">✔</span></span> | <span data-ttu-id="4460b-248">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-248">✔</span></span>
<span data-ttu-id="4460b-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="4460b-250">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-250">✔</span></span> | <span data-ttu-id="4460b-251">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-251">✔</span></span> | <span data-ttu-id="4460b-252">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-252">✔</span></span> | <span data-ttu-id="4460b-253">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-253">✔</span></span>
<span data-ttu-id="4460b-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="4460b-254">Geometry.Dimension</span></span> | <span data-ttu-id="4460b-255">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-255">✔</span></span> | <span data-ttu-id="4460b-256">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-256">✔</span></span> | <span data-ttu-id="4460b-257">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-257">✔</span></span> | <span data-ttu-id="4460b-258">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-258">✔</span></span>
<span data-ttu-id="4460b-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="4460b-260">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-260">✔</span></span> | <span data-ttu-id="4460b-261">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-261">✔</span></span> | <span data-ttu-id="4460b-262">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-262">✔</span></span> | <span data-ttu-id="4460b-263">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-263">✔</span></span>
<span data-ttu-id="4460b-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="4460b-265">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-265">✔</span></span> | <span data-ttu-id="4460b-266">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-266">✔</span></span> | <span data-ttu-id="4460b-267">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-267">✔</span></span> | <span data-ttu-id="4460b-268">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-268">✔</span></span>
<span data-ttu-id="4460b-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="4460b-269">Geometry.Envelope</span></span> | <span data-ttu-id="4460b-270">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-270">✔</span></span> | | <span data-ttu-id="4460b-271">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-271">✔</span></span> | <span data-ttu-id="4460b-272">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-272">✔</span></span>
<span data-ttu-id="4460b-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="4460b-274">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-274">✔</span></span>
<span data-ttu-id="4460b-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="4460b-276">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-276">✔</span></span> | <span data-ttu-id="4460b-277">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-277">✔</span></span> | <span data-ttu-id="4460b-278">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-278">✔</span></span> | <span data-ttu-id="4460b-279">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-279">✔</span></span>
<span data-ttu-id="4460b-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="4460b-280">Geometry.GeometryType</span></span> | <span data-ttu-id="4460b-281">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-281">✔</span></span> | <span data-ttu-id="4460b-282">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-282">✔</span></span> | <span data-ttu-id="4460b-283">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-283">✔</span></span> | <span data-ttu-id="4460b-284">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-284">✔</span></span>
<span data-ttu-id="4460b-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="4460b-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="4460b-286">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-286">✔</span></span> | | <span data-ttu-id="4460b-287">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-287">✔</span></span> | <span data-ttu-id="4460b-288">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-288">✔</span></span>
<span data-ttu-id="4460b-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="4460b-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="4460b-290">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-290">✔</span></span> | | <span data-ttu-id="4460b-291">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-291">✔</span></span>
<span data-ttu-id="4460b-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="4460b-293">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-293">✔</span></span> | <span data-ttu-id="4460b-294">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-294">✔</span></span> | <span data-ttu-id="4460b-295">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-295">✔</span></span> | <span data-ttu-id="4460b-296">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-296">✔</span></span>
<span data-ttu-id="4460b-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="4460b-298">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-298">✔</span></span> | <span data-ttu-id="4460b-299">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-299">✔</span></span> | <span data-ttu-id="4460b-300">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-300">✔</span></span> | <span data-ttu-id="4460b-301">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-301">✔</span></span>
<span data-ttu-id="4460b-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="4460b-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="4460b-303">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-303">✔</span></span> | <span data-ttu-id="4460b-304">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-304">✔</span></span> | <span data-ttu-id="4460b-305">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-305">✔</span></span> | <span data-ttu-id="4460b-306">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-306">✔</span></span>
<span data-ttu-id="4460b-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="4460b-307">Geometry.IsSimple</span></span> | <span data-ttu-id="4460b-308">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-308">✔</span></span> | | <span data-ttu-id="4460b-309">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-309">✔</span></span> | <span data-ttu-id="4460b-310">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-310">✔</span></span>
<span data-ttu-id="4460b-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="4460b-311">Geometry.IsValid</span></span> | <span data-ttu-id="4460b-312">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-312">✔</span></span> | <span data-ttu-id="4460b-313">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-313">✔</span></span> | <span data-ttu-id="4460b-314">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-314">✔</span></span> | <span data-ttu-id="4460b-315">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-315">✔</span></span>
<span data-ttu-id="4460b-316">Geometry.IsWithinDistance (Geometry, double)</span><span class="sxs-lookup"><span data-stu-id="4460b-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="4460b-317">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-317">✔</span></span> | | <span data-ttu-id="4460b-318">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-318">✔</span></span>
<span data-ttu-id="4460b-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="4460b-319">Geometry.Length</span></span> | <span data-ttu-id="4460b-320">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-320">✔</span></span> | <span data-ttu-id="4460b-321">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-321">✔</span></span> | <span data-ttu-id="4460b-322">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-322">✔</span></span> | <span data-ttu-id="4460b-323">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-323">✔</span></span>
<span data-ttu-id="4460b-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="4460b-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="4460b-325">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-325">✔</span></span> | <span data-ttu-id="4460b-326">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-326">✔</span></span> | <span data-ttu-id="4460b-327">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-327">✔</span></span> | <span data-ttu-id="4460b-328">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-328">✔</span></span>
<span data-ttu-id="4460b-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="4460b-329">Geometry.NumPoints</span></span> | <span data-ttu-id="4460b-330">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-330">✔</span></span> | <span data-ttu-id="4460b-331">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-331">✔</span></span> | <span data-ttu-id="4460b-332">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-332">✔</span></span> | <span data-ttu-id="4460b-333">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-333">✔</span></span>
<span data-ttu-id="4460b-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="4460b-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="4460b-335">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-335">✔</span></span> | <span data-ttu-id="4460b-336">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-336">✔</span></span> | <span data-ttu-id="4460b-337">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-337">✔</span></span>
<span data-ttu-id="4460b-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="4460b-339">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-339">✔</span></span> | <span data-ttu-id="4460b-340">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-340">✔</span></span> | <span data-ttu-id="4460b-341">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-341">✔</span></span> | <span data-ttu-id="4460b-342">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-342">✔</span></span>
<span data-ttu-id="4460b-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="4460b-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="4460b-344">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-344">✔</span></span> | | <span data-ttu-id="4460b-345">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-345">✔</span></span> | <span data-ttu-id="4460b-346">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-346">✔</span></span>
<span data-ttu-id="4460b-347">Geometry.Relate (Geometry, 문자열)</span><span class="sxs-lookup"><span data-stu-id="4460b-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="4460b-348">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-348">✔</span></span> | | <span data-ttu-id="4460b-349">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-349">✔</span></span> | <span data-ttu-id="4460b-350">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-350">✔</span></span>
<span data-ttu-id="4460b-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="4460b-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="4460b-352">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-352">✔</span></span> | <span data-ttu-id="4460b-353">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-353">✔</span></span>
<span data-ttu-id="4460b-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="4460b-354">Geometry.SRID</span></span> | <span data-ttu-id="4460b-355">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-355">✔</span></span> | <span data-ttu-id="4460b-356">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-356">✔</span></span> | <span data-ttu-id="4460b-357">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-357">✔</span></span> | <span data-ttu-id="4460b-358">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-358">✔</span></span>
<span data-ttu-id="4460b-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="4460b-360">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-360">✔</span></span> | <span data-ttu-id="4460b-361">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-361">✔</span></span> | <span data-ttu-id="4460b-362">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-362">✔</span></span> | <span data-ttu-id="4460b-363">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-363">✔</span></span>
<span data-ttu-id="4460b-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="4460b-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="4460b-365">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-365">✔</span></span> | <span data-ttu-id="4460b-366">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-366">✔</span></span> | <span data-ttu-id="4460b-367">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-367">✔</span></span> | <span data-ttu-id="4460b-368">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-368">✔</span></span>
<span data-ttu-id="4460b-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="4460b-369">Geometry.ToText()</span></span> | <span data-ttu-id="4460b-370">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-370">✔</span></span> | <span data-ttu-id="4460b-371">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-371">✔</span></span> | <span data-ttu-id="4460b-372">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-372">✔</span></span> | <span data-ttu-id="4460b-373">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-373">✔</span></span>
<span data-ttu-id="4460b-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="4460b-375">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-375">✔</span></span> | | <span data-ttu-id="4460b-376">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-376">✔</span></span> | <span data-ttu-id="4460b-377">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-377">✔</span></span>
<span data-ttu-id="4460b-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="4460b-378">Geometry.Union()</span></span> | | | <span data-ttu-id="4460b-379">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-379">✔</span></span>
<span data-ttu-id="4460b-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="4460b-381">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-381">✔</span></span> | <span data-ttu-id="4460b-382">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-382">✔</span></span> | <span data-ttu-id="4460b-383">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-383">✔</span></span> | <span data-ttu-id="4460b-384">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-384">✔</span></span>
<span data-ttu-id="4460b-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="4460b-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="4460b-386">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-386">✔</span></span> | <span data-ttu-id="4460b-387">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-387">✔</span></span> | <span data-ttu-id="4460b-388">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-388">✔</span></span> | <span data-ttu-id="4460b-389">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-389">✔</span></span>
<span data-ttu-id="4460b-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="4460b-390">GeometryCollection.Count</span></span> | <span data-ttu-id="4460b-391">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-391">✔</span></span> | <span data-ttu-id="4460b-392">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-392">✔</span></span> | <span data-ttu-id="4460b-393">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-393">✔</span></span> | <span data-ttu-id="4460b-394">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-394">✔</span></span>
<span data-ttu-id="4460b-395">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="4460b-395">GeometryCollection[int]</span></span> | <span data-ttu-id="4460b-396">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-396">✔</span></span> | <span data-ttu-id="4460b-397">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-397">✔</span></span> | <span data-ttu-id="4460b-398">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-398">✔</span></span> | <span data-ttu-id="4460b-399">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-399">✔</span></span>
<span data-ttu-id="4460b-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="4460b-400">LineString.Count</span></span> | <span data-ttu-id="4460b-401">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-401">✔</span></span> | <span data-ttu-id="4460b-402">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-402">✔</span></span> | <span data-ttu-id="4460b-403">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-403">✔</span></span> | <span data-ttu-id="4460b-404">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-404">✔</span></span>
<span data-ttu-id="4460b-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="4460b-405">LineString.EndPoint</span></span> | <span data-ttu-id="4460b-406">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-406">✔</span></span> | <span data-ttu-id="4460b-407">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-407">✔</span></span> | <span data-ttu-id="4460b-408">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-408">✔</span></span> | <span data-ttu-id="4460b-409">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-409">✔</span></span>
<span data-ttu-id="4460b-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="4460b-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="4460b-411">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-411">✔</span></span> | <span data-ttu-id="4460b-412">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-412">✔</span></span> | <span data-ttu-id="4460b-413">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-413">✔</span></span> | <span data-ttu-id="4460b-414">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-414">✔</span></span>
<span data-ttu-id="4460b-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="4460b-415">LineString.IsClosed</span></span> | <span data-ttu-id="4460b-416">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-416">✔</span></span> | <span data-ttu-id="4460b-417">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-417">✔</span></span> | <span data-ttu-id="4460b-418">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-418">✔</span></span> | <span data-ttu-id="4460b-419">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-419">✔</span></span>
<span data-ttu-id="4460b-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="4460b-420">LineString.IsRing</span></span> | <span data-ttu-id="4460b-421">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-421">✔</span></span> | | <span data-ttu-id="4460b-422">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-422">✔</span></span> | <span data-ttu-id="4460b-423">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-423">✔</span></span>
<span data-ttu-id="4460b-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="4460b-424">LineString.StartPoint</span></span> | <span data-ttu-id="4460b-425">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-425">✔</span></span> | <span data-ttu-id="4460b-426">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-426">✔</span></span> | <span data-ttu-id="4460b-427">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-427">✔</span></span> | <span data-ttu-id="4460b-428">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-428">✔</span></span>
<span data-ttu-id="4460b-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="4460b-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="4460b-430">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-430">✔</span></span> | <span data-ttu-id="4460b-431">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-431">✔</span></span> | <span data-ttu-id="4460b-432">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-432">✔</span></span> | <span data-ttu-id="4460b-433">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-433">✔</span></span>
<span data-ttu-id="4460b-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="4460b-434">Point.M</span></span> | <span data-ttu-id="4460b-435">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-435">✔</span></span> | <span data-ttu-id="4460b-436">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-436">✔</span></span> | <span data-ttu-id="4460b-437">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-437">✔</span></span> | <span data-ttu-id="4460b-438">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-438">✔</span></span>
<span data-ttu-id="4460b-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="4460b-439">Point.X</span></span> | <span data-ttu-id="4460b-440">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-440">✔</span></span> | <span data-ttu-id="4460b-441">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-441">✔</span></span> | <span data-ttu-id="4460b-442">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-442">✔</span></span> | <span data-ttu-id="4460b-443">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-443">✔</span></span>
<span data-ttu-id="4460b-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="4460b-444">Point.Y</span></span> | <span data-ttu-id="4460b-445">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-445">✔</span></span> | <span data-ttu-id="4460b-446">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-446">✔</span></span> | <span data-ttu-id="4460b-447">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-447">✔</span></span> | <span data-ttu-id="4460b-448">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-448">✔</span></span>
<span data-ttu-id="4460b-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="4460b-449">Point.Z</span></span> | <span data-ttu-id="4460b-450">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-450">✔</span></span> | <span data-ttu-id="4460b-451">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-451">✔</span></span> | <span data-ttu-id="4460b-452">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-452">✔</span></span> | <span data-ttu-id="4460b-453">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-453">✔</span></span>
<span data-ttu-id="4460b-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="4460b-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="4460b-455">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-455">✔</span></span> | <span data-ttu-id="4460b-456">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-456">✔</span></span> | <span data-ttu-id="4460b-457">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-457">✔</span></span> | <span data-ttu-id="4460b-458">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-458">✔</span></span>
<span data-ttu-id="4460b-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="4460b-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="4460b-460">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-460">✔</span></span> | <span data-ttu-id="4460b-461">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-461">✔</span></span> | <span data-ttu-id="4460b-462">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-462">✔</span></span> | <span data-ttu-id="4460b-463">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-463">✔</span></span>
<span data-ttu-id="4460b-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="4460b-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="4460b-465">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-465">✔</span></span> | <span data-ttu-id="4460b-466">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-466">✔</span></span> | <span data-ttu-id="4460b-467">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-467">✔</span></span> | <span data-ttu-id="4460b-468">✔</span><span class="sxs-lookup"><span data-stu-id="4460b-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4460b-469">추가 자료</span><span class="sxs-lookup"><span data-stu-id="4460b-469">Additional resources</span></span>

* [<span data-ttu-id="4460b-470">SQL server에서 공간 데이터</span><span class="sxs-lookup"><span data-stu-id="4460b-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="4460b-471">SpatiaLite 홈 페이지</span><span class="sxs-lookup"><span data-stu-id="4460b-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="4460b-472">Npgsql 공간 설명서</span><span class="sxs-lookup"><span data-stu-id="4460b-472">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="4460b-473">PostGIS 설명서</span><span class="sxs-lookup"><span data-stu-id="4460b-473">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
