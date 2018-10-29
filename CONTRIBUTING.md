# <a name="contributing-to-the-entity-framework-documentation"></a><span data-ttu-id="62742-101">Entity Framework 설명서에 기여</span><span class="sxs-lookup"><span data-stu-id="62742-101">Contributing to the Entity Framework documentation</span></span>

<span data-ttu-id="62742-102">이 문서에서는 [Entity Framework 설명서 사이트](https://docs.microsoft.com/ef)에서 호스팅되는 문서 및 코드 샘플에 참여하는 프로세스를 다룹니다.</span><span class="sxs-lookup"><span data-stu-id="62742-102">The document covers the process for contributing to the articles and code samples that are hosted on the [Entity Framework documentation site](https://docs.microsoft.com/ef).</span></span> <span data-ttu-id="62742-103">기여는 오타 수정만큼 간단하거나 새 문서처럼 복잡할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-103">Contributions may be as simple as typo corrections or as complex as new articles.</span></span>

## <a name="how-to-make-a-simple-correction-or-suggestion"></a><span data-ttu-id="62742-104">간단한 수정 또는 제안을 수행하는 방법</span><span class="sxs-lookup"><span data-stu-id="62742-104">How to make a simple correction or suggestion</span></span>

<span data-ttu-id="62742-105">문서는 리포지토리에 Markdown 파일로 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="62742-105">Articles are stored in the repository as Markdown files.</span></span> <span data-ttu-id="62742-106">Markdown 파일의 콘텐츠에 대한 간단한 변경 작업은 브라우저 창의 오른쪽 위 모서리에서 **편집** 링크를 탭하여 수행할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-106">Simple changes to the content of a Markdown file can be made in the browser by tapping the **Edit** link in the upper right corner of the browser window.</span></span> <span data-ttu-id="62742-107">(축소된 브라우저 창에서 **편집** 링크를 표시하려면 **옵션** 막대를 확장해야 합니다.) PR(끌어오기 요청)을 만들려면 지시를 따르세요.</span><span class="sxs-lookup"><span data-stu-id="62742-107">(In narrow browser windows you might need to expand the **options** bar to see the **Edit** link.) Follow the directions to create a pull request (PR).</span></span> <span data-ttu-id="62742-108">EF 팀에서는 PR을 검토하고 변경 사항을 수락하거나 제안합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-108">The EF team will review the PR and accept it or suggest changes.</span></span>

## <a name="how-to-make-a-more-complex-submission"></a><span data-ttu-id="62742-109">더 복잡한 제출을 수행하는 방법</span><span class="sxs-lookup"><span data-stu-id="62742-109">How to make a more complex submission</span></span>

<span data-ttu-id="62742-110">[Git 및 GitHub.com](https://guides.github.com/activities/hello-world/)의 기본적인 내용을 이해하고 있어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-110">You'll need a basic understanding of [Git and GitHub.com](https://guides.github.com/activities/hello-world/).</span></span>

* <span data-ttu-id="62742-111">기존 문서를 변경하거나 새 문서를 만드는 등 원하는 작업을 설명하는 [문제](https://github.com/aspnet/EntityFramework.Docs/issues/new)를 엽니다.</span><span class="sxs-lookup"><span data-stu-id="62742-111">Open an [issue](https://github.com/aspnet/EntityFramework.Docs/issues/new) describing what you want to do, such as change an existing article or create a new one.</span></span> <span data-ttu-id="62742-112">많은 시간을 투자하기 전에 EF 팀의 승인을 기다리세요.</span><span class="sxs-lookup"><span data-stu-id="62742-112">Wait for approval from the EF team before you invest much time.</span></span>
* <span data-ttu-id="62742-113">[aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) 리포지토리를 포크하고 변경 내용에 대한 분기를 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="62742-113">Fork the [aspnet/EntityFramework.Docs](https://github.com/aspnet/EntityFramework.Docs/) repo and create a branch for your changes.</span></span>
* <span data-ttu-id="62742-114">마스터에게 변경 내용이 포함된 PR(끌어오기 요청)을 제출합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-114">Submit a pull request (PR) to master with your changes.</span></span>
* <span data-ttu-id="62742-115">PR 피드백에 응답합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-115">Respond to PR feedback.</span></span>

## <a name="markdown-syntax"></a><span data-ttu-id="62742-116">Markdown 구문</span><span class="sxs-lookup"><span data-stu-id="62742-116">Markdown syntax</span></span>

<span data-ttu-id="62742-117">문서는 [GitHub-flavored Markdown(GFM)](https://guides.github.com/features/mastering-markdown/)의 상위 집합인 [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html)으로 작성됩니다.</span><span class="sxs-lookup"><span data-stu-id="62742-117">Articles are written in [DocFx-flavored Markdown](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html), which is a superset of [GitHub-flavored Markdown (GFM)](https://guides.github.com/features/mastering-markdown/).</span></span> <span data-ttu-id="62742-118">EF 설명서에서 일반적으로 사용되는 UI 기능의 DFM 구문 예제는 .NET Core 리포지토리 스타일 가이드의 [메타데이터 및 Markdown 템플릿](https://github.com/dotnet/docs/blob/master/styleguide/template.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="62742-118">For examples of DFM syntax for UI features commonly used in the EF documentation, see [Metadata and Markdown Template](https://github.com/dotnet/docs/blob/master/styleguide/template.md) in the .NET Core repo style guide.</span></span> 

## <a name="folder-structure-conventions"></a><span data-ttu-id="62742-119">폴더 구조 규칙</span><span class="sxs-lookup"><span data-stu-id="62742-119">Folder structure conventions</span></span>

<span data-ttu-id="62742-120">이미지 및 다른 정적 콘텐츠는 사이트의 각 영역/폴더 내에 있는 `_static` 폴더에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="62742-120">Images, and other static content, are stored in an `_static` folder within each area/folder of the site.</span></span>

<span data-ttu-id="62742-121">코드 샘플은 `samples` 루트 폴더에 저장됩니다.</span><span class="sxs-lookup"><span data-stu-id="62742-121">Code samples are stored in the `samples` root folder.</span></span> <span data-ttu-id="62742-122">설명서 구조를 모방하는 폴더 구조로 구성되었습니다(`entity-framework` 루트 폴더 아래에 있음).</span><span class="sxs-lookup"><span data-stu-id="62742-122">They are organized into a folder structure that mimics the documentation structure (found under the `entity-framework` root folder).</span></span>

## <a name="code-snippets"></a><span data-ttu-id="62742-123">코드 조각</span><span class="sxs-lookup"><span data-stu-id="62742-123">Code snippets</span></span>

<span data-ttu-id="62742-124">문서에는 요점을 설명하는 코드 조각이 포함된 경우가 많습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-124">Articles frequently contain code snippets to illustrate points.</span></span> <span data-ttu-id="62742-125">DFM을 통해 Markdown 파일에 코드를 복사하거나 별도의 코드 파일을 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-125">DFM lets you copy code into the Markdown file or refer to a separate code file.</span></span> <span data-ttu-id="62742-126">코드에서 오류 가능성을 최소화하기 위해 가능하면 별도의 코드 파일을 사용하는 것이 좋습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-126">We prefer to use separate code files whenever possible, to minimize the chance of errors in the code.</span></span> <span data-ttu-id="62742-127">코드 파일은 위의 샘플 프로젝트에서 설명한 폴더 구조를 사용하여 리포지토리에 저장되어야 합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-127">The code files should be stored in the repo using the folder structure described above for sample projects.</span></span>

<span data-ttu-id="62742-128">[DFM 코드 조각 구문](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet)의 예제는 다음과 같습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-128">Here are some examples of [DFM code snippet syntax](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet).</span></span>

<span data-ttu-id="62742-129">전체 코드 파일을 코드 조각으로 렌더링하려면:</span><span class="sxs-lookup"><span data-stu-id="62742-129">To render an entire code file as a snippet:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

<span data-ttu-id="62742-130">줄 번호를 사용하여 파일의 일부를 코드 조각으로 렌더링하려면:</span><span class="sxs-lookup"><span data-stu-id="62742-130">To render a portion of a file as a snippet by using line numbers:</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

<span data-ttu-id="62742-131">C# 코드 조각의 경우 [C# 지역](https://msdn.microsoft.com/library/9a1ybwek.aspx)을 참조할 수 있습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-131">For C# snippets, you can reference a [C# region](https://msdn.microsoft.com/library/9a1ybwek.aspx).</span></span> <span data-ttu-id="62742-132">가능할 때마다 줄 번호보다는 지역을 사용하세요. 코드 파일의 줄 번호는 변경되거나, Markdown의 줄 번호 참조와 비동기화될 가능성이 높습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-132">Whenever possible, use regions rather than line numbers, because line numbers in a code file tend to change and get out of sync with line number references in Markdown.</span></span> <span data-ttu-id="62742-133">C#지역는 중첩될 수 있고, 외부 지역을 참조하는 경우 내부 `#region` 및 `#endregion` 지시문이 코드 조각에서 렌더링되지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-133">C# regions can be nested, and if you reference the outer region, the inner `#region` and `#endregion` directives are not rendered in a snippet.</span></span>

<span data-ttu-id="62742-134">"snippet_Example"이라는 C# 지역을 렌더링하려면:</span><span class="sxs-lookup"><span data-stu-id="62742-134">To render a C# region named "snippet_Example":</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

<span data-ttu-id="62742-135">렌더링된 코드 조각에서 선택된 줄을 강조 표시하려면(일반적으로 노란색 배경으로 렌더링됨):</span><span class="sxs-lookup"><span data-stu-id="62742-135">To highlight selected lines in a rendered snippet (usually renders as yellow background color):</span></span>

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a><span data-ttu-id="62742-136">DocFX로 변경 내용 테스트</span><span class="sxs-lookup"><span data-stu-id="62742-136">Test your changes with DocFX</span></span>

<span data-ttu-id="62742-137">로컬에서 호스팅되는 사이트 버전을 만드는 [DocFX 명령줄 도구](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool)로 변경 내용을 테스트합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-137">Test your changes with the [DocFX command line tool](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool), which creates a locally hosted version of the site.</span></span> <span data-ttu-id="62742-138">DocFX는 docs.microsoft.com용으로 생성된 스타일 및 사이트 확장을 렌더링하지 않습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-138">DocFX doesn't render style and site extensions created for docs.microsoft.com.</span></span>

<span data-ttu-id="62742-139">DocFX에는 Windows용 .NET Framework나 Linux 또는 macOS용 Mono가 필요합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-139">DocFX requires the .NET Framework on Windows, or Mono for Linux or macOS.</span></span>

### <a name="windows-instructions"></a><span data-ttu-id="62742-140">Windows 지침</span><span class="sxs-lookup"><span data-stu-id="62742-140">Windows instructions</span></span>

* <span data-ttu-id="62742-141">[DocFX 릴리스](https://github.com/dotnet/docfx/releases)에서 *docfx.zip*을 다운로드하고 압축을 풉니다.</span><span class="sxs-lookup"><span data-stu-id="62742-141">Download and unzip *docfx.zip* from [DocFX releases](https://github.com/dotnet/docfx/releases).</span></span>
* <span data-ttu-id="62742-142">PATH에 DocFX를 추가합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-142">Add DocFX to your PATH.</span></span>
* <span data-ttu-id="62742-143">명령줄 창에서 복제된 리포지토리(*docfx.json* 파일 포함)로 이동하고 다음 명령을 실행합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-143">In a command line window, navigate to the cloned repository (which contains the *docfx.json* file) and run the following command:</span></span>

   ``` console
   docfx -t default --serve
   ```

* <span data-ttu-id="62742-144">브라우저에서 `http://localhost:8080`로 이동합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-144">In a browser, navigate to `http://localhost:8080`.</span></span>

### <a name="mono-instructions"></a><span data-ttu-id="62742-145">Mono 지침</span><span class="sxs-lookup"><span data-stu-id="62742-145">Mono instructions</span></span>

* <span data-ttu-id="62742-146">Homebrew를 통해 Mono 설치 - `brew install mono`.</span><span class="sxs-lookup"><span data-stu-id="62742-146">Install Mono via Homebrew - `brew install mono`.</span></span>
* <span data-ttu-id="62742-147">[최신 버전의 DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2)를 다운로드합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-147">Download the [latest version of DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2).</span></span>
* <span data-ttu-id="62742-148">`\bin\docfx`로 추출합니다.</span><span class="sxs-lookup"><span data-stu-id="62742-148">Extract to `\bin\docfx`.</span></span>
* <span data-ttu-id="62742-149">**docfx**의 별칭을 만듭니다.</span><span class="sxs-lookup"><span data-stu-id="62742-149">Create an alias for **docfx**:</span></span>

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* <span data-ttu-id="62742-150">복제된 리포지토리에서 **docfx**를 실행하여 사이트를 빌드하고 **docfx-serve**를 실행하여 `http://localhost:8080`에서 사이트를 봅니다.</span><span class="sxs-lookup"><span data-stu-id="62742-150">Run **docfx** in the cloned repository to build the site, and **docfx-serve** to view the site at `http://localhost:8080`.</span></span>

## <a name="voice-and-tone"></a><span data-ttu-id="62742-151">어투 및 어조</span><span class="sxs-lookup"><span data-stu-id="62742-151">Voice and tone</span></span>

<span data-ttu-id="62742-152">가장 폭넓은 잠재 고객이 쉽게 이해할 수 있는 설명서를 작성하는 것이 목표입니다.</span><span class="sxs-lookup"><span data-stu-id="62742-152">Our goal is to write documentation that is easily understandable by the widest possible audience.</span></span> <span data-ttu-id="62742-153">이에 따라 Microsoft는 기여자가 지켜주었으면 하는 문장 스타일에 대한 지침을 설정했습니다.</span><span class="sxs-lookup"><span data-stu-id="62742-153">To that end we have established guidelines for writing style that we ask our contributors to follow.</span></span> <span data-ttu-id="62742-154">자세한 내용은 .NET 리포지토리에서 [어투 및 어조 지침](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md)을 참조하세요.</span><span class="sxs-lookup"><span data-stu-id="62742-154">For more information, see [Voice and tone guidelines](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md) in the .NET Core repo.</span></span>
