---
title: 공간 데이터-EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 8dae1ab949c77ffa08904b12a5716b729e6913a1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502242"
---
# <a name="spatial-data"></a><span data-ttu-id="932db-102">공간 데이터</span><span class="sxs-lookup"><span data-stu-id="932db-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="932db-103">이 기능은 EF Core 2.2에서 추가 되었습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="932db-104">공간 데이터는 물리적 위치와 개체의 모양을 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="932db-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="932db-105">많은 데이터베이스는 다른 데이터와 함께 인덱싱 및 쿼리할 수 있도록 이러한 유형의 데이터에 대 한 지원을 제공 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="932db-106">일반적인 시나리오는 위치에서 지정 된 거리 내에 있는 개체에 대 한 쿼리를 포함 하거나 테두리가 지정 된 위치를 포함 하는 개체를 선택 하는 것입니다.</span><span class="sxs-lookup"><span data-stu-id="932db-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="932db-107">EF Core [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) 공간 라이브러리를 사용 하 여 공간 데이터 형식에 대 한 매핑을 지원 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="932db-108">설치</span><span class="sxs-lookup"><span data-stu-id="932db-108">Installing</span></span>

<span data-ttu-id="932db-109">EF Core에서 공간 데이터를 사용 하려면 적절 한 지원 NuGet 패키지를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="932db-110">설치 해야 하는 패키지는 사용 중인 공급자에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="932db-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="932db-111">EF Core 공급자</span><span class="sxs-lookup"><span data-stu-id="932db-111">EF Core Provider</span></span>                        | <span data-ttu-id="932db-112">공간 NuGet 패키지</span><span class="sxs-lookup"><span data-stu-id="932db-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="932db-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="932db-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="932db-114">Microsoft.entityframeworkcore.tools.dotnet. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="932db-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="932db-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="932db-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="932db-116">Microsoft.entityframeworkcore.tools.dotnet NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="932db-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="932db-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="932db-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="932db-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="932db-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="932db-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="932db-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="932db-120">Npgsql. PostgreSQL. NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="932db-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="932db-121">리버스 엔지니어링</span><span class="sxs-lookup"><span data-stu-id="932db-121">Reverse engineering</span></span>

<span data-ttu-id="932db-122">공간 NuGet 패키지는 공간 속성을 사용 하 여 [리버스 엔지니어링](../managing-schemas/scaffolding.md) 모델을 사용할 수도 있지만 `Scaffold-DbContext` 또는 `dotnet ef dbcontext scaffold`을 실행 ***하기 전에*** 패키지를 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="932db-123">그렇지 않으면 열에 대 한 형식 매핑을 찾을 수 없다는 경고가 표시 되 고 열이 생략 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="932db-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="932db-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="932db-125">NetTopologySuite는 .NET의 공간 라이브러리입니다.</span><span class="sxs-lookup"><span data-stu-id="932db-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="932db-126">EF Core를 사용 하면 모델에서 NTS 유형을 사용 하 여 데이터베이스의 공간 데이터 형식에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="932db-127">NTS를 통해 공간 형식에 매핑할 수 있도록 하려면 공급자의 DbContext options builder에서 UseNetTopologySuite 메서드를 호출 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="932db-128">예를 들어 SQL Server를 사용 하 여 다음과 같이 호출할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="932db-129">여러 공간 데이터 형식이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-129">There are several spatial data types.</span></span> <span data-ttu-id="932db-130">사용할 형식은 허용할 셰이프 유형에 따라 달라 집니다.</span><span class="sxs-lookup"><span data-stu-id="932db-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="932db-131">모델의 속성에 사용할 수 있는 NTS 형식의 계층 구조는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="932db-132">`NetTopologySuite.Geometries` 네임 스페이스 내에 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="932db-133">geometry</span><span class="sxs-lookup"><span data-stu-id="932db-133">Geometry</span></span>
  * <span data-ttu-id="932db-134">요소</span><span class="sxs-lookup"><span data-stu-id="932db-134">Point</span></span>
  * <span data-ttu-id="932db-135">LineString</span><span class="sxs-lookup"><span data-stu-id="932db-135">LineString</span></span>
  * <span data-ttu-id="932db-136">다각형</span><span class="sxs-lookup"><span data-stu-id="932db-136">Polygon</span></span>
  * <span data-ttu-id="932db-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="932db-137">GeometryCollection</span></span>
    * <span data-ttu-id="932db-138">다중 포인트</span><span class="sxs-lookup"><span data-stu-id="932db-138">MultiPoint</span></span>
    * <span data-ttu-id="932db-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="932db-139">MultiLineString</span></span>
    * <span data-ttu-id="932db-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="932db-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="932db-141">CircularString, CompoundCurve 및 NTS는에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="932db-142">기본 Geometry 형식을 사용 하면 모든 형식의 모양을 속성으로 지정할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="932db-143">다음 엔터티 클래스를 사용 하 여 [와이드 세계의 가져오기 샘플 데이터베이스](https://go.microsoft.com/fwlink/?LinkID=800630)의 테이블에 매핑할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a><span data-ttu-id="932db-144">값 만들기</span><span class="sxs-lookup"><span data-stu-id="932db-144">Creating values</span></span>

<span data-ttu-id="932db-145">생성자를 사용 하 여 geometry 개체를 만들 수 있습니다. 그러나 NTS는 기 하 도형 팩터리를 대신 사용 하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="932db-146">이를 통해 기본 SRID (좌표에서 사용 되는 공간 참조 시스템)를 지정할 수 있으며 전체 자릿수 모델 (계산 중 사용) 및 좌표 시퀀스 (좌표를 결정)와 같은 고급 작업을 제어할 수 있습니다. 및 측정값을 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="932db-147">4326는 GPS 및 기타 지리적 시스템에서 사용 되는 표준인 WGS 84를 나타냅니다.</span><span class="sxs-lookup"><span data-stu-id="932db-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="932db-148">경도 및 위도</span><span class="sxs-lookup"><span data-stu-id="932db-148">Longitude and Latitude</span></span>

<span data-ttu-id="932db-149">NTS의 좌표는 X 및 Y 값을 기준으로 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="932db-150">경도 및 위도를 나타내려면 경도에 X를, 위도에 Y를 사용 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="932db-151">이 값은 일반적으로 이러한 값이 표시 되는 `latitude, longitude` 형식에서 **이전** 입니다.</span><span class="sxs-lookup"><span data-stu-id="932db-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="932db-152">클라이언트 작업 중에 무시 되는 SRID</span><span class="sxs-lookup"><span data-stu-id="932db-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="932db-153">NTS는 작업 중에 SRID 값을 무시 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="932db-154">평면 좌표계를 가정 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="932db-155">즉, 경도 및 위도 면에서 좌표를 지정 하는 경우 거리, 길이 및 영역과 같은 일부 클라이언트 계산 값은 단위가 아니라도 단위로 표시 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="932db-156">보다 의미 있는 값을 사용 하려면 먼저 이러한 값을 계산 하기 전에 [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) 와 같은 라이브러리를 사용 하 여 좌표를 다른 좌표계에 프로젝션 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="932db-157">작업이 SQL을 통해 EF Core 의해 서버에서 계산 되는 경우 결과의 단위는 데이터베이스에 의해 결정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="932db-158">다음은 ProjNet4GeoAPI를 사용 하 여 두 도시 간의 거리를 계산 하는 예입니다.</span><span class="sxs-lookup"><span data-stu-id="932db-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a><span data-ttu-id="932db-159">데이터 쿼리</span><span class="sxs-lookup"><span data-stu-id="932db-159">Querying Data</span></span>

<span data-ttu-id="932db-160">LINQ에서 데이터베이스 함수로 사용할 수 있는 NTS 메서드와 속성은 SQL로 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="932db-161">예를 들어 Distance 및 Contains 메서드는 다음 쿼리에서 변환 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="932db-162">이 문서의 끝에 있는 표에는 다양 한 EF Core 공급자가 지 원하는 멤버가 나와 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="932db-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="932db-163">SQL Server</span></span>

<span data-ttu-id="932db-164">SQL Server를 사용 하는 경우 알아야 할 몇 가지 추가 사항이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="932db-165">지리 또는 기 하 도형</span><span class="sxs-lookup"><span data-stu-id="932db-165">Geography or geometry</span></span>

<span data-ttu-id="932db-166">기본적으로 공간 속성은 SQL Server의 `geography` 열에 매핑됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="932db-167">`geometry`를 사용 하려면 모델에서 [열 유형을 구성](xref:core/modeling/entity-properties#column-data-types) 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="932db-168">지리 다각형 링</span><span class="sxs-lookup"><span data-stu-id="932db-168">Geography polygon rings</span></span>

<span data-ttu-id="932db-169">`geography` 열 유형을 사용 하는 경우 SQL Server는 외부 링 (또는 셸) 및 내부 링 (또는 구멍)에 추가 요구 사항을 적용 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="932db-170">외부 링은 시계 반대 방향으로, 내부는 시계 방향으로 링 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="932db-171">NTS는 데이터베이스에 값을 보내기 전에 유효성을 검사 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="932db-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="932db-172">FullGlobe</span></span>

<span data-ttu-id="932db-173">SQL Server에는 `geography` 열 형식을 사용할 때 전체 구형을 나타내는 비표준 geometry 형식이 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="932db-174">또한 전체 구형 (외부 링 제외)을 기반으로 하는 다각형을 나타내는 방법도 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="932db-175">이러한 두 가지 모두 NTS에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="932db-176">이를 기반으로 하는 FullGlobe 및 다각형은 NTS에서 지원 되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="932db-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="932db-177">SQLite</span></span>

<span data-ttu-id="932db-178">SQLite를 사용 하는 몇 가지 추가 정보는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="932db-179">SpatiaLite 설치</span><span class="sxs-lookup"><span data-stu-id="932db-179">Installing SpatiaLite</span></span>

<span data-ttu-id="932db-180">Windows에서 네이티브 mod_spatialite 라이브러리는 NuGet 패키지 종속성으로 배포 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="932db-181">다른 플랫폼은 개별적으로 설치 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="932db-182">일반적으로 소프트웨어 패키지 관리자를 사용 하 여 수행 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="932db-183">예를 들어 MacOS에서 Ubuntu 및 Homebrew의 APT를 사용할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="932db-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="932db-184">SRID 구성</span><span class="sxs-lookup"><span data-stu-id="932db-184">Configuring SRID</span></span>

<span data-ttu-id="932db-185">SpatiaLite에서 열은 열 당 SRID를 지정 해야 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="932db-186">기본 SRID `0`입니다.</span><span class="sxs-lookup"><span data-stu-id="932db-186">The default SRID is `0`.</span></span> <span data-ttu-id="932db-187">ForSqliteHasSrid 메서드를 사용 하 여 다른 SRID를 지정 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="932db-188">Dimension</span><span class="sxs-lookup"><span data-stu-id="932db-188">Dimension</span></span>

<span data-ttu-id="932db-189">SRID와 마찬가지로 열의 차원 (또는 좌표)도 열의 일부로 지정 됩니다.</span><span class="sxs-lookup"><span data-stu-id="932db-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="932db-190">기본 좌표는 X 및 Y입니다. ForSqliteHasDimension 메서드를 사용 하 여 추가 좌표 (Z 및 M)를 사용 하도록 설정 합니다.</span><span class="sxs-lookup"><span data-stu-id="932db-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="932db-191">번역 된 작업</span><span class="sxs-lookup"><span data-stu-id="932db-191">Translated Operations</span></span>

<span data-ttu-id="932db-192">이 표에서는 각 EF Core 공급자가 SQL로 변환 하는 NTS 멤버를 보여 줍니다.</span><span class="sxs-lookup"><span data-stu-id="932db-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="932db-193">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="932db-193">NetTopologySuite</span></span> | <span data-ttu-id="932db-194">SQL Server (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-194">SQL Server (geometry)</span></span> | <span data-ttu-id="932db-195">SQL Server (지리)</span><span class="sxs-lookup"><span data-stu-id="932db-195">SQL Server (geography)</span></span> | <span data-ttu-id="932db-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="932db-196">SQLite</span></span> | <span data-ttu-id="932db-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="932db-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="932db-198">기 하 도형. 영역</span><span class="sxs-lookup"><span data-stu-id="932db-198">Geometry.Area</span></span> | <span data-ttu-id="932db-199">✔</span><span class="sxs-lookup"><span data-stu-id="932db-199">✔</span></span> | <span data-ttu-id="932db-200">✔</span><span class="sxs-lookup"><span data-stu-id="932db-200">✔</span></span> | <span data-ttu-id="932db-201">✔</span><span class="sxs-lookup"><span data-stu-id="932db-201">✔</span></span> | <span data-ttu-id="932db-202">✔</span><span class="sxs-lookup"><span data-stu-id="932db-202">✔</span></span>
<span data-ttu-id="932db-203">Geometry. AsBinary ()</span><span class="sxs-lookup"><span data-stu-id="932db-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="932db-204">✔</span><span class="sxs-lookup"><span data-stu-id="932db-204">✔</span></span> | <span data-ttu-id="932db-205">✔</span><span class="sxs-lookup"><span data-stu-id="932db-205">✔</span></span> | <span data-ttu-id="932db-206">✔</span><span class="sxs-lookup"><span data-stu-id="932db-206">✔</span></span> | <span data-ttu-id="932db-207">✔</span><span class="sxs-lookup"><span data-stu-id="932db-207">✔</span></span>
<span data-ttu-id="932db-208">기 하 도형. 고가 ()</span><span class="sxs-lookup"><span data-stu-id="932db-208">Geometry.AsText()</span></span> | <span data-ttu-id="932db-209">✔</span><span class="sxs-lookup"><span data-stu-id="932db-209">✔</span></span> | <span data-ttu-id="932db-210">✔</span><span class="sxs-lookup"><span data-stu-id="932db-210">✔</span></span> | <span data-ttu-id="932db-211">✔</span><span class="sxs-lookup"><span data-stu-id="932db-211">✔</span></span> | <span data-ttu-id="932db-212">✔</span><span class="sxs-lookup"><span data-stu-id="932db-212">✔</span></span>
<span data-ttu-id="932db-213">Geometry. 경계</span><span class="sxs-lookup"><span data-stu-id="932db-213">Geometry.Boundary</span></span> | <span data-ttu-id="932db-214">✔</span><span class="sxs-lookup"><span data-stu-id="932db-214">✔</span></span> | | <span data-ttu-id="932db-215">✔</span><span class="sxs-lookup"><span data-stu-id="932db-215">✔</span></span> | <span data-ttu-id="932db-216">✔</span><span class="sxs-lookup"><span data-stu-id="932db-216">✔</span></span>
<span data-ttu-id="932db-217">Geometry. Buffer (double)</span><span class="sxs-lookup"><span data-stu-id="932db-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="932db-218">✔</span><span class="sxs-lookup"><span data-stu-id="932db-218">✔</span></span> | <span data-ttu-id="932db-219">✔</span><span class="sxs-lookup"><span data-stu-id="932db-219">✔</span></span> | <span data-ttu-id="932db-220">✔</span><span class="sxs-lookup"><span data-stu-id="932db-220">✔</span></span> | <span data-ttu-id="932db-221">✔</span><span class="sxs-lookup"><span data-stu-id="932db-221">✔</span></span>
<span data-ttu-id="932db-222">Geometry. Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="932db-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="932db-223">✔</span><span class="sxs-lookup"><span data-stu-id="932db-223">✔</span></span> | <span data-ttu-id="932db-224">✔</span><span class="sxs-lookup"><span data-stu-id="932db-224">✔</span></span>
<span data-ttu-id="932db-225">중심</span><span class="sxs-lookup"><span data-stu-id="932db-225">Geometry.Centroid</span></span> | <span data-ttu-id="932db-226">✔</span><span class="sxs-lookup"><span data-stu-id="932db-226">✔</span></span> | | <span data-ttu-id="932db-227">✔</span><span class="sxs-lookup"><span data-stu-id="932db-227">✔</span></span> | <span data-ttu-id="932db-228">✔</span><span class="sxs-lookup"><span data-stu-id="932db-228">✔</span></span>
<span data-ttu-id="932db-229">Geometry. Contains (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="932db-230">✔</span><span class="sxs-lookup"><span data-stu-id="932db-230">✔</span></span> | <span data-ttu-id="932db-231">✔</span><span class="sxs-lookup"><span data-stu-id="932db-231">✔</span></span> | <span data-ttu-id="932db-232">✔</span><span class="sxs-lookup"><span data-stu-id="932db-232">✔</span></span> | <span data-ttu-id="932db-233">✔</span><span class="sxs-lookup"><span data-stu-id="932db-233">✔</span></span>
<span data-ttu-id="932db-234">ConvexHull ()</span><span class="sxs-lookup"><span data-stu-id="932db-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="932db-235">✔</span><span class="sxs-lookup"><span data-stu-id="932db-235">✔</span></span> | <span data-ttu-id="932db-236">✔</span><span class="sxs-lookup"><span data-stu-id="932db-236">✔</span></span> | <span data-ttu-id="932db-237">✔</span><span class="sxs-lookup"><span data-stu-id="932db-237">✔</span></span> | <span data-ttu-id="932db-238">✔</span><span class="sxs-lookup"><span data-stu-id="932db-238">✔</span></span>
<span data-ttu-id="932db-239">Geometry. CoveredBy (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="932db-240">✔</span><span class="sxs-lookup"><span data-stu-id="932db-240">✔</span></span> | <span data-ttu-id="932db-241">✔</span><span class="sxs-lookup"><span data-stu-id="932db-241">✔</span></span>
<span data-ttu-id="932db-242">기 하 도형. 커버 (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="932db-243">✔</span><span class="sxs-lookup"><span data-stu-id="932db-243">✔</span></span> | <span data-ttu-id="932db-244">✔</span><span class="sxs-lookup"><span data-stu-id="932db-244">✔</span></span>
<span data-ttu-id="932db-245">Geometry (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="932db-246">✔</span><span class="sxs-lookup"><span data-stu-id="932db-246">✔</span></span> | | <span data-ttu-id="932db-247">✔</span><span class="sxs-lookup"><span data-stu-id="932db-247">✔</span></span> | <span data-ttu-id="932db-248">✔</span><span class="sxs-lookup"><span data-stu-id="932db-248">✔</span></span>
<span data-ttu-id="932db-249">기 하 도형. 차 (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="932db-250">✔</span><span class="sxs-lookup"><span data-stu-id="932db-250">✔</span></span> | <span data-ttu-id="932db-251">✔</span><span class="sxs-lookup"><span data-stu-id="932db-251">✔</span></span> | <span data-ttu-id="932db-252">✔</span><span class="sxs-lookup"><span data-stu-id="932db-252">✔</span></span> | <span data-ttu-id="932db-253">✔</span><span class="sxs-lookup"><span data-stu-id="932db-253">✔</span></span>
<span data-ttu-id="932db-254">Geometry. 차원</span><span class="sxs-lookup"><span data-stu-id="932db-254">Geometry.Dimension</span></span> | <span data-ttu-id="932db-255">✔</span><span class="sxs-lookup"><span data-stu-id="932db-255">✔</span></span> | <span data-ttu-id="932db-256">✔</span><span class="sxs-lookup"><span data-stu-id="932db-256">✔</span></span> | <span data-ttu-id="932db-257">✔</span><span class="sxs-lookup"><span data-stu-id="932db-257">✔</span></span> | <span data-ttu-id="932db-258">✔</span><span class="sxs-lookup"><span data-stu-id="932db-258">✔</span></span>
<span data-ttu-id="932db-259">Geometry. 비연속 (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="932db-260">✔</span><span class="sxs-lookup"><span data-stu-id="932db-260">✔</span></span> | <span data-ttu-id="932db-261">✔</span><span class="sxs-lookup"><span data-stu-id="932db-261">✔</span></span> | <span data-ttu-id="932db-262">✔</span><span class="sxs-lookup"><span data-stu-id="932db-262">✔</span></span> | <span data-ttu-id="932db-263">✔</span><span class="sxs-lookup"><span data-stu-id="932db-263">✔</span></span>
<span data-ttu-id="932db-264">기 하 도형. 거리 (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="932db-265">✔</span><span class="sxs-lookup"><span data-stu-id="932db-265">✔</span></span> | <span data-ttu-id="932db-266">✔</span><span class="sxs-lookup"><span data-stu-id="932db-266">✔</span></span> | <span data-ttu-id="932db-267">✔</span><span class="sxs-lookup"><span data-stu-id="932db-267">✔</span></span> | <span data-ttu-id="932db-268">✔</span><span class="sxs-lookup"><span data-stu-id="932db-268">✔</span></span>
<span data-ttu-id="932db-269">기 하 도형. 봉투</span><span class="sxs-lookup"><span data-stu-id="932db-269">Geometry.Envelope</span></span> | <span data-ttu-id="932db-270">✔</span><span class="sxs-lookup"><span data-stu-id="932db-270">✔</span></span> | | <span data-ttu-id="932db-271">✔</span><span class="sxs-lookup"><span data-stu-id="932db-271">✔</span></span> | <span data-ttu-id="932db-272">✔</span><span class="sxs-lookup"><span data-stu-id="932db-272">✔</span></span>
<span data-ttu-id="932db-273">Geometry. EqualsExact (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="932db-274">✔</span><span class="sxs-lookup"><span data-stu-id="932db-274">✔</span></span>
<span data-ttu-id="932db-275">Geometry. EqualsTopologically (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="932db-276">✔</span><span class="sxs-lookup"><span data-stu-id="932db-276">✔</span></span> | <span data-ttu-id="932db-277">✔</span><span class="sxs-lookup"><span data-stu-id="932db-277">✔</span></span> | <span data-ttu-id="932db-278">✔</span><span class="sxs-lookup"><span data-stu-id="932db-278">✔</span></span> | <span data-ttu-id="932db-279">✔</span><span class="sxs-lookup"><span data-stu-id="932db-279">✔</span></span>
<span data-ttu-id="932db-280">GeometryType</span><span class="sxs-lookup"><span data-stu-id="932db-280">Geometry.GeometryType</span></span> | <span data-ttu-id="932db-281">✔</span><span class="sxs-lookup"><span data-stu-id="932db-281">✔</span></span> | <span data-ttu-id="932db-282">✔</span><span class="sxs-lookup"><span data-stu-id="932db-282">✔</span></span> | <span data-ttu-id="932db-283">✔</span><span class="sxs-lookup"><span data-stu-id="932db-283">✔</span></span> | <span data-ttu-id="932db-284">✔</span><span class="sxs-lookup"><span data-stu-id="932db-284">✔</span></span>
<span data-ttu-id="932db-285">GetGeometryN (int)</span><span class="sxs-lookup"><span data-stu-id="932db-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="932db-286">✔</span><span class="sxs-lookup"><span data-stu-id="932db-286">✔</span></span> | | <span data-ttu-id="932db-287">✔</span><span class="sxs-lookup"><span data-stu-id="932db-287">✔</span></span> | <span data-ttu-id="932db-288">✔</span><span class="sxs-lookup"><span data-stu-id="932db-288">✔</span></span>
<span data-ttu-id="932db-289">InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="932db-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="932db-290">✔</span><span class="sxs-lookup"><span data-stu-id="932db-290">✔</span></span> | | <span data-ttu-id="932db-291">✔</span><span class="sxs-lookup"><span data-stu-id="932db-291">✔</span></span> | <span data-ttu-id="932db-292">✔</span><span class="sxs-lookup"><span data-stu-id="932db-292">✔</span></span>
<span data-ttu-id="932db-293">기 하 도형. 교차로 (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="932db-294">✔</span><span class="sxs-lookup"><span data-stu-id="932db-294">✔</span></span> | <span data-ttu-id="932db-295">✔</span><span class="sxs-lookup"><span data-stu-id="932db-295">✔</span></span> | <span data-ttu-id="932db-296">✔</span><span class="sxs-lookup"><span data-stu-id="932db-296">✔</span></span> | <span data-ttu-id="932db-297">✔</span><span class="sxs-lookup"><span data-stu-id="932db-297">✔</span></span>
<span data-ttu-id="932db-298">기 하 도형. 교차 (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="932db-299">✔</span><span class="sxs-lookup"><span data-stu-id="932db-299">✔</span></span> | <span data-ttu-id="932db-300">✔</span><span class="sxs-lookup"><span data-stu-id="932db-300">✔</span></span> | <span data-ttu-id="932db-301">✔</span><span class="sxs-lookup"><span data-stu-id="932db-301">✔</span></span> | <span data-ttu-id="932db-302">✔</span><span class="sxs-lookup"><span data-stu-id="932db-302">✔</span></span>
<span data-ttu-id="932db-303">IsEmpty</span><span class="sxs-lookup"><span data-stu-id="932db-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="932db-304">✔</span><span class="sxs-lookup"><span data-stu-id="932db-304">✔</span></span> | <span data-ttu-id="932db-305">✔</span><span class="sxs-lookup"><span data-stu-id="932db-305">✔</span></span> | <span data-ttu-id="932db-306">✔</span><span class="sxs-lookup"><span data-stu-id="932db-306">✔</span></span> | <span data-ttu-id="932db-307">✔</span><span class="sxs-lookup"><span data-stu-id="932db-307">✔</span></span>
<span data-ttu-id="932db-308">Geometry. IsSimple</span><span class="sxs-lookup"><span data-stu-id="932db-308">Geometry.IsSimple</span></span> | <span data-ttu-id="932db-309">✔</span><span class="sxs-lookup"><span data-stu-id="932db-309">✔</span></span> | | <span data-ttu-id="932db-310">✔</span><span class="sxs-lookup"><span data-stu-id="932db-310">✔</span></span> | <span data-ttu-id="932db-311">✔</span><span class="sxs-lookup"><span data-stu-id="932db-311">✔</span></span>
<span data-ttu-id="932db-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="932db-312">Geometry.IsValid</span></span> | <span data-ttu-id="932db-313">✔</span><span class="sxs-lookup"><span data-stu-id="932db-313">✔</span></span> | <span data-ttu-id="932db-314">✔</span><span class="sxs-lookup"><span data-stu-id="932db-314">✔</span></span> | <span data-ttu-id="932db-315">✔</span><span class="sxs-lookup"><span data-stu-id="932db-315">✔</span></span> | <span data-ttu-id="932db-316">✔</span><span class="sxs-lookup"><span data-stu-id="932db-316">✔</span></span>
<span data-ttu-id="932db-317">Geometry. IsWithinDistance (Geometry, double)</span><span class="sxs-lookup"><span data-stu-id="932db-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="932db-318">✔</span><span class="sxs-lookup"><span data-stu-id="932db-318">✔</span></span> | | <span data-ttu-id="932db-319">✔</span><span class="sxs-lookup"><span data-stu-id="932db-319">✔</span></span> | <span data-ttu-id="932db-320">✔</span><span class="sxs-lookup"><span data-stu-id="932db-320">✔</span></span>
<span data-ttu-id="932db-321">기 하 도형. 길이</span><span class="sxs-lookup"><span data-stu-id="932db-321">Geometry.Length</span></span> | <span data-ttu-id="932db-322">✔</span><span class="sxs-lookup"><span data-stu-id="932db-322">✔</span></span> | <span data-ttu-id="932db-323">✔</span><span class="sxs-lookup"><span data-stu-id="932db-323">✔</span></span> | <span data-ttu-id="932db-324">✔</span><span class="sxs-lookup"><span data-stu-id="932db-324">✔</span></span> | <span data-ttu-id="932db-325">✔</span><span class="sxs-lookup"><span data-stu-id="932db-325">✔</span></span>
<span data-ttu-id="932db-326">기 하 도형. NumGeometries</span><span class="sxs-lookup"><span data-stu-id="932db-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="932db-327">✔</span><span class="sxs-lookup"><span data-stu-id="932db-327">✔</span></span> | <span data-ttu-id="932db-328">✔</span><span class="sxs-lookup"><span data-stu-id="932db-328">✔</span></span> | <span data-ttu-id="932db-329">✔</span><span class="sxs-lookup"><span data-stu-id="932db-329">✔</span></span> | <span data-ttu-id="932db-330">✔</span><span class="sxs-lookup"><span data-stu-id="932db-330">✔</span></span>
<span data-ttu-id="932db-331">기 하 도형. NumPoints</span><span class="sxs-lookup"><span data-stu-id="932db-331">Geometry.NumPoints</span></span> | <span data-ttu-id="932db-332">✔</span><span class="sxs-lookup"><span data-stu-id="932db-332">✔</span></span> | <span data-ttu-id="932db-333">✔</span><span class="sxs-lookup"><span data-stu-id="932db-333">✔</span></span> | <span data-ttu-id="932db-334">✔</span><span class="sxs-lookup"><span data-stu-id="932db-334">✔</span></span> | <span data-ttu-id="932db-335">✔</span><span class="sxs-lookup"><span data-stu-id="932db-335">✔</span></span>
<span data-ttu-id="932db-336">OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="932db-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="932db-337">✔</span><span class="sxs-lookup"><span data-stu-id="932db-337">✔</span></span> | <span data-ttu-id="932db-338">✔</span><span class="sxs-lookup"><span data-stu-id="932db-338">✔</span></span> | <span data-ttu-id="932db-339">✔</span><span class="sxs-lookup"><span data-stu-id="932db-339">✔</span></span> | <span data-ttu-id="932db-340">✔</span><span class="sxs-lookup"><span data-stu-id="932db-340">✔</span></span>
<span data-ttu-id="932db-341">기 하 도형. 겹치기 (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="932db-342">✔</span><span class="sxs-lookup"><span data-stu-id="932db-342">✔</span></span> | <span data-ttu-id="932db-343">✔</span><span class="sxs-lookup"><span data-stu-id="932db-343">✔</span></span> | <span data-ttu-id="932db-344">✔</span><span class="sxs-lookup"><span data-stu-id="932db-344">✔</span></span> | <span data-ttu-id="932db-345">✔</span><span class="sxs-lookup"><span data-stu-id="932db-345">✔</span></span>
<span data-ttu-id="932db-346">기 하 도형. PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="932db-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="932db-347">✔</span><span class="sxs-lookup"><span data-stu-id="932db-347">✔</span></span> | | <span data-ttu-id="932db-348">✔</span><span class="sxs-lookup"><span data-stu-id="932db-348">✔</span></span> | <span data-ttu-id="932db-349">✔</span><span class="sxs-lookup"><span data-stu-id="932db-349">✔</span></span>
<span data-ttu-id="932db-350">Geometry. Relate (Geometry, string)</span><span class="sxs-lookup"><span data-stu-id="932db-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="932db-351">✔</span><span class="sxs-lookup"><span data-stu-id="932db-351">✔</span></span> | | <span data-ttu-id="932db-352">✔</span><span class="sxs-lookup"><span data-stu-id="932db-352">✔</span></span> | <span data-ttu-id="932db-353">✔</span><span class="sxs-lookup"><span data-stu-id="932db-353">✔</span></span>
<span data-ttu-id="932db-354">Geometry. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="932db-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="932db-355">✔</span><span class="sxs-lookup"><span data-stu-id="932db-355">✔</span></span> | <span data-ttu-id="932db-356">✔</span><span class="sxs-lookup"><span data-stu-id="932db-356">✔</span></span>
<span data-ttu-id="932db-357">SRID</span><span class="sxs-lookup"><span data-stu-id="932db-357">Geometry.SRID</span></span> | <span data-ttu-id="932db-358">✔</span><span class="sxs-lookup"><span data-stu-id="932db-358">✔</span></span> | <span data-ttu-id="932db-359">✔</span><span class="sxs-lookup"><span data-stu-id="932db-359">✔</span></span> | <span data-ttu-id="932db-360">✔</span><span class="sxs-lookup"><span data-stu-id="932db-360">✔</span></span> | <span data-ttu-id="932db-361">✔</span><span class="sxs-lookup"><span data-stu-id="932db-361">✔</span></span>
<span data-ttu-id="932db-362">Geometry. SymmetricDifference (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="932db-363">✔</span><span class="sxs-lookup"><span data-stu-id="932db-363">✔</span></span> | <span data-ttu-id="932db-364">✔</span><span class="sxs-lookup"><span data-stu-id="932db-364">✔</span></span> | <span data-ttu-id="932db-365">✔</span><span class="sxs-lookup"><span data-stu-id="932db-365">✔</span></span> | <span data-ttu-id="932db-366">✔</span><span class="sxs-lookup"><span data-stu-id="932db-366">✔</span></span>
<span data-ttu-id="932db-367">Geometry. ToBinary ()</span><span class="sxs-lookup"><span data-stu-id="932db-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="932db-368">✔</span><span class="sxs-lookup"><span data-stu-id="932db-368">✔</span></span> | <span data-ttu-id="932db-369">✔</span><span class="sxs-lookup"><span data-stu-id="932db-369">✔</span></span> | <span data-ttu-id="932db-370">✔</span><span class="sxs-lookup"><span data-stu-id="932db-370">✔</span></span> | <span data-ttu-id="932db-371">✔</span><span class="sxs-lookup"><span data-stu-id="932db-371">✔</span></span>
<span data-ttu-id="932db-372">Geometry. ToText ()</span><span class="sxs-lookup"><span data-stu-id="932db-372">Geometry.ToText()</span></span> | <span data-ttu-id="932db-373">✔</span><span class="sxs-lookup"><span data-stu-id="932db-373">✔</span></span> | <span data-ttu-id="932db-374">✔</span><span class="sxs-lookup"><span data-stu-id="932db-374">✔</span></span> | <span data-ttu-id="932db-375">✔</span><span class="sxs-lookup"><span data-stu-id="932db-375">✔</span></span> | <span data-ttu-id="932db-376">✔</span><span class="sxs-lookup"><span data-stu-id="932db-376">✔</span></span>
<span data-ttu-id="932db-377">기 하 도형 (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="932db-378">✔</span><span class="sxs-lookup"><span data-stu-id="932db-378">✔</span></span> | | <span data-ttu-id="932db-379">✔</span><span class="sxs-lookup"><span data-stu-id="932db-379">✔</span></span> | <span data-ttu-id="932db-380">✔</span><span class="sxs-lookup"><span data-stu-id="932db-380">✔</span></span>
<span data-ttu-id="932db-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="932db-381">Geometry.Union()</span></span> | | | <span data-ttu-id="932db-382">✔</span><span class="sxs-lookup"><span data-stu-id="932db-382">✔</span></span> | <span data-ttu-id="932db-383">✔</span><span class="sxs-lookup"><span data-stu-id="932db-383">✔</span></span>
<span data-ttu-id="932db-384">Geometry. Union (Geometry)</span><span class="sxs-lookup"><span data-stu-id="932db-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="932db-385">✔</span><span class="sxs-lookup"><span data-stu-id="932db-385">✔</span></span> | <span data-ttu-id="932db-386">✔</span><span class="sxs-lookup"><span data-stu-id="932db-386">✔</span></span> | <span data-ttu-id="932db-387">✔</span><span class="sxs-lookup"><span data-stu-id="932db-387">✔</span></span> | <span data-ttu-id="932db-388">✔</span><span class="sxs-lookup"><span data-stu-id="932db-388">✔</span></span>
<span data-ttu-id="932db-389">Geometry. 내 (기 하 도형)</span><span class="sxs-lookup"><span data-stu-id="932db-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="932db-390">✔</span><span class="sxs-lookup"><span data-stu-id="932db-390">✔</span></span> | <span data-ttu-id="932db-391">✔</span><span class="sxs-lookup"><span data-stu-id="932db-391">✔</span></span> | <span data-ttu-id="932db-392">✔</span><span class="sxs-lookup"><span data-stu-id="932db-392">✔</span></span> | <span data-ttu-id="932db-393">✔</span><span class="sxs-lookup"><span data-stu-id="932db-393">✔</span></span>
<span data-ttu-id="932db-394">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="932db-394">GeometryCollection.Count</span></span> | <span data-ttu-id="932db-395">✔</span><span class="sxs-lookup"><span data-stu-id="932db-395">✔</span></span> | <span data-ttu-id="932db-396">✔</span><span class="sxs-lookup"><span data-stu-id="932db-396">✔</span></span> | <span data-ttu-id="932db-397">✔</span><span class="sxs-lookup"><span data-stu-id="932db-397">✔</span></span> | <span data-ttu-id="932db-398">✔</span><span class="sxs-lookup"><span data-stu-id="932db-398">✔</span></span>
<span data-ttu-id="932db-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="932db-399">GeometryCollection[int]</span></span> | <span data-ttu-id="932db-400">✔</span><span class="sxs-lookup"><span data-stu-id="932db-400">✔</span></span> | <span data-ttu-id="932db-401">✔</span><span class="sxs-lookup"><span data-stu-id="932db-401">✔</span></span> | <span data-ttu-id="932db-402">✔</span><span class="sxs-lookup"><span data-stu-id="932db-402">✔</span></span> | <span data-ttu-id="932db-403">✔</span><span class="sxs-lookup"><span data-stu-id="932db-403">✔</span></span>
<span data-ttu-id="932db-404">LineString</span><span class="sxs-lookup"><span data-stu-id="932db-404">LineString.Count</span></span> | <span data-ttu-id="932db-405">✔</span><span class="sxs-lookup"><span data-stu-id="932db-405">✔</span></span> | <span data-ttu-id="932db-406">✔</span><span class="sxs-lookup"><span data-stu-id="932db-406">✔</span></span> | <span data-ttu-id="932db-407">✔</span><span class="sxs-lookup"><span data-stu-id="932db-407">✔</span></span> | <span data-ttu-id="932db-408">✔</span><span class="sxs-lookup"><span data-stu-id="932db-408">✔</span></span>
<span data-ttu-id="932db-409">LineString</span><span class="sxs-lookup"><span data-stu-id="932db-409">LineString.EndPoint</span></span> | <span data-ttu-id="932db-410">✔</span><span class="sxs-lookup"><span data-stu-id="932db-410">✔</span></span> | <span data-ttu-id="932db-411">✔</span><span class="sxs-lookup"><span data-stu-id="932db-411">✔</span></span> | <span data-ttu-id="932db-412">✔</span><span class="sxs-lookup"><span data-stu-id="932db-412">✔</span></span> | <span data-ttu-id="932db-413">✔</span><span class="sxs-lookup"><span data-stu-id="932db-413">✔</span></span>
<span data-ttu-id="932db-414">LineString. GetPointN (int)</span><span class="sxs-lookup"><span data-stu-id="932db-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="932db-415">✔</span><span class="sxs-lookup"><span data-stu-id="932db-415">✔</span></span> | <span data-ttu-id="932db-416">✔</span><span class="sxs-lookup"><span data-stu-id="932db-416">✔</span></span> | <span data-ttu-id="932db-417">✔</span><span class="sxs-lookup"><span data-stu-id="932db-417">✔</span></span> | <span data-ttu-id="932db-418">✔</span><span class="sxs-lookup"><span data-stu-id="932db-418">✔</span></span>
<span data-ttu-id="932db-419">LineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="932db-419">LineString.IsClosed</span></span> | <span data-ttu-id="932db-420">✔</span><span class="sxs-lookup"><span data-stu-id="932db-420">✔</span></span> | <span data-ttu-id="932db-421">✔</span><span class="sxs-lookup"><span data-stu-id="932db-421">✔</span></span> | <span data-ttu-id="932db-422">✔</span><span class="sxs-lookup"><span data-stu-id="932db-422">✔</span></span> | <span data-ttu-id="932db-423">✔</span><span class="sxs-lookup"><span data-stu-id="932db-423">✔</span></span>
<span data-ttu-id="932db-424">LineString</span><span class="sxs-lookup"><span data-stu-id="932db-424">LineString.IsRing</span></span> | <span data-ttu-id="932db-425">✔</span><span class="sxs-lookup"><span data-stu-id="932db-425">✔</span></span> | | <span data-ttu-id="932db-426">✔</span><span class="sxs-lookup"><span data-stu-id="932db-426">✔</span></span> | <span data-ttu-id="932db-427">✔</span><span class="sxs-lookup"><span data-stu-id="932db-427">✔</span></span>
<span data-ttu-id="932db-428">LineString</span><span class="sxs-lookup"><span data-stu-id="932db-428">LineString.StartPoint</span></span> | <span data-ttu-id="932db-429">✔</span><span class="sxs-lookup"><span data-stu-id="932db-429">✔</span></span> | <span data-ttu-id="932db-430">✔</span><span class="sxs-lookup"><span data-stu-id="932db-430">✔</span></span> | <span data-ttu-id="932db-431">✔</span><span class="sxs-lookup"><span data-stu-id="932db-431">✔</span></span> | <span data-ttu-id="932db-432">✔</span><span class="sxs-lookup"><span data-stu-id="932db-432">✔</span></span>
<span data-ttu-id="932db-433">MultiLineString. IsClosed</span><span class="sxs-lookup"><span data-stu-id="932db-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="932db-434">✔</span><span class="sxs-lookup"><span data-stu-id="932db-434">✔</span></span> | <span data-ttu-id="932db-435">✔</span><span class="sxs-lookup"><span data-stu-id="932db-435">✔</span></span> | <span data-ttu-id="932db-436">✔</span><span class="sxs-lookup"><span data-stu-id="932db-436">✔</span></span> | <span data-ttu-id="932db-437">✔</span><span class="sxs-lookup"><span data-stu-id="932db-437">✔</span></span>
<span data-ttu-id="932db-438">Point. M</span><span class="sxs-lookup"><span data-stu-id="932db-438">Point.M</span></span> | <span data-ttu-id="932db-439">✔</span><span class="sxs-lookup"><span data-stu-id="932db-439">✔</span></span> | <span data-ttu-id="932db-440">✔</span><span class="sxs-lookup"><span data-stu-id="932db-440">✔</span></span> | <span data-ttu-id="932db-441">✔</span><span class="sxs-lookup"><span data-stu-id="932db-441">✔</span></span> | <span data-ttu-id="932db-442">✔</span><span class="sxs-lookup"><span data-stu-id="932db-442">✔</span></span>
<span data-ttu-id="932db-443">Point. X</span><span class="sxs-lookup"><span data-stu-id="932db-443">Point.X</span></span> | <span data-ttu-id="932db-444">✔</span><span class="sxs-lookup"><span data-stu-id="932db-444">✔</span></span> | <span data-ttu-id="932db-445">✔</span><span class="sxs-lookup"><span data-stu-id="932db-445">✔</span></span> | <span data-ttu-id="932db-446">✔</span><span class="sxs-lookup"><span data-stu-id="932db-446">✔</span></span> | <span data-ttu-id="932db-447">✔</span><span class="sxs-lookup"><span data-stu-id="932db-447">✔</span></span>
<span data-ttu-id="932db-448">Point. Y</span><span class="sxs-lookup"><span data-stu-id="932db-448">Point.Y</span></span> | <span data-ttu-id="932db-449">✔</span><span class="sxs-lookup"><span data-stu-id="932db-449">✔</span></span> | <span data-ttu-id="932db-450">✔</span><span class="sxs-lookup"><span data-stu-id="932db-450">✔</span></span> | <span data-ttu-id="932db-451">✔</span><span class="sxs-lookup"><span data-stu-id="932db-451">✔</span></span> | <span data-ttu-id="932db-452">✔</span><span class="sxs-lookup"><span data-stu-id="932db-452">✔</span></span>
<span data-ttu-id="932db-453">Point. Z</span><span class="sxs-lookup"><span data-stu-id="932db-453">Point.Z</span></span> | <span data-ttu-id="932db-454">✔</span><span class="sxs-lookup"><span data-stu-id="932db-454">✔</span></span> | <span data-ttu-id="932db-455">✔</span><span class="sxs-lookup"><span data-stu-id="932db-455">✔</span></span> | <span data-ttu-id="932db-456">✔</span><span class="sxs-lookup"><span data-stu-id="932db-456">✔</span></span> | <span data-ttu-id="932db-457">✔</span><span class="sxs-lookup"><span data-stu-id="932db-457">✔</span></span>
<span data-ttu-id="932db-458">ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="932db-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="932db-459">✔</span><span class="sxs-lookup"><span data-stu-id="932db-459">✔</span></span> | <span data-ttu-id="932db-460">✔</span><span class="sxs-lookup"><span data-stu-id="932db-460">✔</span></span> | <span data-ttu-id="932db-461">✔</span><span class="sxs-lookup"><span data-stu-id="932db-461">✔</span></span> | <span data-ttu-id="932db-462">✔</span><span class="sxs-lookup"><span data-stu-id="932db-462">✔</span></span>
<span data-ttu-id="932db-463">GetInteriorRingN (int)</span><span class="sxs-lookup"><span data-stu-id="932db-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="932db-464">✔</span><span class="sxs-lookup"><span data-stu-id="932db-464">✔</span></span> | <span data-ttu-id="932db-465">✔</span><span class="sxs-lookup"><span data-stu-id="932db-465">✔</span></span> | <span data-ttu-id="932db-466">✔</span><span class="sxs-lookup"><span data-stu-id="932db-466">✔</span></span> | <span data-ttu-id="932db-467">✔</span><span class="sxs-lookup"><span data-stu-id="932db-467">✔</span></span>
<span data-ttu-id="932db-468">NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="932db-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="932db-469">✔</span><span class="sxs-lookup"><span data-stu-id="932db-469">✔</span></span> | <span data-ttu-id="932db-470">✔</span><span class="sxs-lookup"><span data-stu-id="932db-470">✔</span></span> | <span data-ttu-id="932db-471">✔</span><span class="sxs-lookup"><span data-stu-id="932db-471">✔</span></span> | <span data-ttu-id="932db-472">✔</span><span class="sxs-lookup"><span data-stu-id="932db-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="932db-473">추가 자료</span><span class="sxs-lookup"><span data-stu-id="932db-473">Additional resources</span></span>

* [<span data-ttu-id="932db-474">SQL Server의 공간 데이터</span><span class="sxs-lookup"><span data-stu-id="932db-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="932db-475">SpatiaLite 홈 페이지</span><span class="sxs-lookup"><span data-stu-id="932db-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="932db-476">Npgsql 공간 설명서</span><span class="sxs-lookup"><span data-stu-id="932db-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="932db-477">PostGIS 설명서</span><span class="sxs-lookup"><span data-stu-id="932db-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
