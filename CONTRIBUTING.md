# <a name="contributing-to-the-entity-framework-documentation"></a>Entity Framework 설명서에 기여

Entity Framework 설명서에 문서 및 코드 샘플을 제공하는 프로세스를 아래에 설명합니다. 기여는 오타 수정만큼 간단할 수도 있고 새 문서처럼 복잡할 수도 있습니다.

## <a name="how-to-make-a-simple-correction-or-suggestion"></a>간단한 수정 또는 제안을 수행하는 방법

문서는 이 리포지토리에 Markdown 파일로 저장됩니다. Markdown 파일의 내용을 간단히 변경하려면 브라우저 창의 오른쪽 위에서 **편집** 링크를 클릭합니다. **편집** 링크를 표시하려면 **옵션** 막대를 확장해야 할 수 있습니다. PR(끌어오기 요청)을 만들려면 지시를 따르세요. EF 팀에서는 PR을 검토하고 변경 사항을 수락하거나 제안합니다.

## <a name="how-to-make-a-more-complex-submission"></a>더 복잡한 제출을 수행하는 방법

[Git 및 GitHub.com](https://guides.github.com/activities/hello-world/)의 기본적인 내용을 이해하고 있어야 합니다.

* 기존 문서를 변경하거나 새 문서를 만드는 등 원하는 작업을 설명하는 [문제](https://github.com/dotnet/EntityFramework.Docs/issues/new)를 엽니다. 많은 시간을 투자하기 전에 EF 팀의 승인을 기다리세요.
* [dotnet/EntityFramework.Docs](https://github.com/dotnet/EntityFramework.Docs/) 리포지토리를 포크하고 변경 내용에 대한 분기를 만듭니다.
* 마스터에게 변경 내용이 포함된 PR(끌어오기 요청)을 제출합니다.
* PR 피드백에 응답합니다.

## <a name="markdown-syntax"></a>Markdown 구문

문서는 [GFM(GitHub-flavored Markdown)](https://guides.github.com/features/mastering-markdown/)의 상위 집합인 [DFM(DocFx-flavored Markdown)](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html)으로 작성됩니다. EF 설명서에서 일반적으로 사용되는 UI 기능의 DFM 구문 및 메타데이터 예제는 .NET Core 리포지토리 스타일 가이드의 [메타데이터 및 Markdown 템플릿](https://github.com/dotnet/docs/blob/master/styleguide/template.md)을 참조하세요.

## <a name="folder-structure-conventions"></a>폴더 구조 규칙

이미지 및 다른 정적 콘텐츠는 사이트의 각 영역/폴더 내에 있는 `_static` 폴더에 저장됩니다.

코드 샘플은 `samples` 루트 폴더에 저장됩니다. 설명서 구조를 모방하는 폴더 구조로 구성되었습니다(`entity-framework` 루트 폴더 아래에 있음).

## <a name="code-snippets"></a>코드 조각

문서에는 요점을 설명하는 코드 조각이 포함된 경우가 많습니다. DFM을 통해 Markdown 파일에 코드를 복사하거나 별도의 코드 파일을 참조할 수 있습니다. 코드에서 오류 가능성을 최소화하기 위해 가능하면 언제나 별도의 코드 파일을 사용하십시오. 코드 파일은 위의 샘플 프로젝트에서 설명한 폴더 구조를 사용하여 리포지토리에 저장되어야 합니다.

[DFM 코드 조각 구문](http://dotnet.github.io/docfx/spec/docfx_flavored_markdown.html#code-snippet)의 예제는 다음과 같습니다.

전체 코드 파일을 코드 조각으로 렌더링하려면:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs)]
```

줄 번호를 사용하여 파일의 일부를 코드 조각으로 렌더링하려면:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?range=1-10]
```

C# 코드 조각의 경우 [C# 지역](https://msdn.microsoft.com/library/9a1ybwek.aspx)을 참조할 수 있습니다. 줄 번호보다는 영역을 사용하십시오. 코드 파일의 줄 번호는 변경되거나, Markdown의 줄 번호 참조와 비동기화될 가능성이 높습니다. C# 지역은 중첩할 수 있습니다. 외부 영역을 참조하는 경우 내부 `#region` 및 `#endregion` 지시문이 코드 조각에서 렌더링되지 않습니다.

"snippet_Example"이라는 C# 지역을 렌더링하려면:

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example)]
```

렌더링된 코드 조각에서 선택된 줄을 강조 표시하려면(일반적으로 노란색 배경으로 렌더링됨):

``` none
[!code-csharp[Main](../../../samples/core/saving/Program.cs?name=snippet_Example&highlight=1-3,10,20-25)]
```

## <a name="test-your-changes-with-docfx"></a>DocFX로 변경 내용 테스트

로컬에서 호스팅되는 사이트 버전을 만드는 [DocFX 명령줄 도구](https://dotnet.github.io/docfx/tutorial/docfx_getting_started.html#2-use-docfx-as-a-command-line-tool)로 변경 내용을 테스트하세요. DocFX는 docs.microsoft.com용으로 생성된 스타일 및 사이트 확장을 렌더링하지 않습니다.

DocFX에는 Windows용 .NET Framework나 Linux 또는 macOS용 Mono가 필요합니다.

### <a name="windows-instructions"></a>Windows 지침

* [DocFX 릴리스](https://github.com/dotnet/docfx/releases)에서 *docfx.zip*을 다운로드하고 압축을 풉니다.
* PATH에 DocFX를 추가합니다.
* 명령줄 창에서 복제된 리포지토리(*docfx.json* 파일 포함)로 이동하고 다음 명령을 실행합니다.

   ``` console
   docfx -t default --serve
   ```

* 브라우저에서 `http://localhost:8080`로 이동합니다.

### <a name="mono-instructions"></a>Mono 지침

* Homebrew를 통해 Mono 설치 - `brew install mono`.
* [최신 버전의 DocFX](https://github.com/dotnet/docfx/releases/tag/v2.7.2)를 다운로드합니다.
* `\bin\docfx`로 추출합니다.
* **docfx**의 별칭을 만듭니다.

  ``` console
  function docfx {
    mono $HOME/bin/docfx/docfx.exe
  }

  function docfx-serve {
    mono $HOME/bin/docfx/docfx.exe serve _site
  }
  ```

* 복제된 리포지토리에서 **docfx**를 실행하여 사이트를 빌드하고 **docfx-serve**를 실행하여 `http://localhost:8080`에서 사이트를 봅니다.

## <a name="voice-and-tone"></a>어투 및 어조

가장 폭넓은 잠재 고객이 쉽게 이해할 수 있는 설명서를 작성하는 것이 목표입니다. 이에 따라 Microsoft는 기여자가 지켜주었으면 하는 문장 스타일에 대한 지침을 설정했습니다. 자세한 내용은 .NET 리포지토리에서 [어투 및 어조 지침](https://github.com/dotnet/docs/blob/master/styleguide/voice-tone.md)을 참조하세요.
