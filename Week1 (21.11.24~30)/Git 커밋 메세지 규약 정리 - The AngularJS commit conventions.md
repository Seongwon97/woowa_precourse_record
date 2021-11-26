# 목표
- 스크립트로 CHANGELOG.md를 작성할 수 있다.
- **git bisect**를 사용하여 중요하지 않은 커밋을 무시할 수 있다. (Formatting과 같은 중요한 커밋은 아닌 경우)
- 커밋 기록 탐색시 더 좋은 정보를 제공한다.

> **※ git bisect**
커밋의 특정 범위 내에서 이진 탐색을 통해 문제가 발생한 최초의 커밋을 찾는데 도움을 주는 git의 기능이다.

<hr>

# CHANGELOG.md 생성
- 새로운 기능 (new Feature)
- 버그 수정 (bug fixs)
- 주요 변경 사항 (breaking changes)
변경 로그에서는 위의 3가지 내용을 사용합니다.

위의 3가지 목록들은 릴리즈를 수행할 때 관련 커밋에 대한 링크와 함께 스크립트로 생성할 수 있습니다.
물론 실제 릴리즈 전에 변경로그를 수정하고 배포할 수도 있습니다.

마지막 릴리즈 이후의 모든 제목 목록을 출력합니다.
※ 제목 (subject) : 커밋 메세지의 첫 번째 줄
```shell
git log <last tag> HEAD --pretty=format:%s
```

해당 릴리스의 새로운 기능
```shell
git log <last release> HEAD --grep feature
```

## 중요하지 않은 커밋 식별
- Formatting change(공백, 빈 줄 추가/제거, 들여쓰기)
- 세미콜론 누락
- 주석 
등의 중요하지 않은 커밋의 경우 무시할 수 있습니다.

git bisect을 통한 이진 탐색을 할 때 위와 같은 중요하지 않은 커밋은 무시할 수 있습니다.
```shell
git bisect skip $(git rev-list --grep irrelevant <good place> HEAD)
```

## 기록 조회 시 더 많은 정보 제공
다음과 같이 일종의 'Context'정보를 추가해야합니다.

다음 메시지들은 Angular 커밋에서 가져온 것입니다.
- Fix small typo in docs widget (tutorial instructions)
- Fix test for scenario.Application - should remove old iframe
- docs - various doc fixes
- docs - stripping extra new lines
- Replaced double line break with single when text is fetched from Google
- Added support for properties in documentation

모든 메시지들은 변경 사항이 있는 위치를 명시하려고 하지만 어떠한 규칙이 없는것 같습니다.

다음 메시지들을 보십시오
- fix comment stripping
- fixing broken links
- Bit of refactoring
- Check whether links do exist and throw exception
- Fix sitemap include (to work on case sensitive linux)

해당 메시지들은 장소 지정을 놓치고 있어 어떠한 내용 변경이 있었는지 짐작할 수 없습니다.
물론 변경 내용을 일일이 찾아보면 찾아볼 수 있지만 이는 속도가 매우 느립니다.

그래서 커밋 메시지에 docs, docs-parser, compiler, scenario-runner, …와 같은 정보들을 저장해줘야합니다.

<hr>

# 커밋 메시지 형식
```shell
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```
커밋 메시지의 각 줄은 100자를 넘을 수 없습니다. 이래야지 github와 다양한 git도구들에서 쉽게 메시지를 읽을 수 있습니다.

> 커밋 메시지를 작성하기 위한 IntelliJ도구
https://plugins.jetbrains.com/plugin/9861-git-commit-template

## Subject Line (제목)
제목에는 변경사항에 대한 간결한 설명을 포함시킵니다.

### `<`type`>`에 들어갈 수 있는 항목들
- feat (feature) : 새로운 기능 추가
- fix (bug fix) : 버그 수정
- docs (documentation) : 문서 관련 내용
- style (formatting, missing semi colons, …) : 스타일 변경
- refactor : 코드 리팩토링
- test (when adding missing tests) : 테스트 관련 코드
- build : 빌드 관련 파일 수정
- ci : CI 설정 파일 수정
- perf : 성능 개선
- chore (maintain) : 그 외의 수정
  
### `<`scope`>`에 들어갈 수 있는 내용들
어디가 변경되었는지에 관하여 변경된 부분은 모두 들어갈 수 있습니다.
$location, $browser, $compile, $rootScope, ngHref, ngClick, ngView, 등등...이 들어갈 수 있으며 **scope는 생략 가능합니다.**
  
### `<`subject`>`
- 명령형 현제 시제를 사용해야합니다. -> changed, changes가 아닌 **change를 사용❗❗**
- 첫 글자는 대문자가 아닌 소문자로 사용합니다.
- 문장 끝에 **마침표(.)를 붙이지 말아야 합니다.**

## Message body (내용)
- 명령형과 현제 시제를 사용해야합니다.
- 변화에 대한 이유와 이전코드와 이후 코드의 차이점을 포함시켜야 합니다.

http://365git.tumblr.com/post/3308646748/writing-git-commit-messages
http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html

## Message footer
### Breaking changes (주요 변경 내용)
모든 주요 변경 사항은 
- 변경 사항에 대한 설명 (description of the change)
- 변경사유 (justification)
- 마이그레이션 참고 사항 (migration notes)
과 함께 footer에 언급되어야 합니다.

```shell
BREAKING CHANGE: isolate scope bindings definition has changed and
    the inject option for the directive controller injection was removed.
    
    To migrate the code follow the example below:
    
    Before:
    
    scope: {
      myAttr: 'attribute',
      myBind: 'bind',
      myExpression: 'expression',
      myEval: 'evaluate',
      myAccessor: 'accessor'
    }
    
    After:
    
    scope: {
      myAttr: '@',
      myBind: '@',
      myExpression: '&',
      // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
      myAccessor: '=' // in directive's template change myAccessor() to myAccessor
    }
    
    The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

### Referencing Issues (해결된 이슈?)
해결되 이슈는 커밋 메시지 하단에 `Closes #<이슈번호>`와 함께 기록해야합니다.
```
이슈가 하나인 경우
Closes #234

이슈가 여러개인 경우
Closes #123, #245, #992
```

<hr>

# 예시들
> - Installing a new dependency
-> **build: Install new dependency**
- Restructuring folder structure
-> **refactor: Reorganize folder layout**
- Updating/Changing stylesheets
-> **style: Update style sheets**
- Changing the name of an existing component
-> **refactor: Rename component**


```shell
feat($browser): onUrlChange event (popstate/hashchange/polling)

Added new event to $browser:
- forward popstate event if available
- forward hashchange event if popstate not available
- do polling when neither popstate nor hashchange available

Breaks $browser.onHashChange, which was removed (use onUrlChange instead)
```

```shell
fix($compile): couple of unit tests for IE9

Older IEs serialize html uppercased, but IE9 does not...
Would be better to expect case insensitive, unfortunately jasmine does
not allow to user regexps for throw expectations.

Closes #392
Breaks foo.bar api, foo.baz should be used instead
```

```shell
feat(directive): ng:disabled, ng:checked, ng:multiple, ng:readonly, ng:selected

New directives for proper binding these attributes in older browsers (IE).
Added coresponding description, live examples and e2e tests.

Closes #351
```

```shell
style($location): add couple of missing semi colons
```

```shell
docs(guide): updated fixed docs from Google Docs

Couple of typos fixed:
- indentation
- batchLogbatchLog -> batchLog
- start periodic checking
- missing brace
```

```shell
feat($compile): simplify isolate scope bindings

Changed the isolate scope binding options to:
  - @attr - attribute binding (including interpolation)
  - =model - by-directional model binding
  - &expr - expression execution binding

This change simplifies the terminology as well as
number of choices available to the developer. It
also supports local name aliasing from the parent.

BREAKING CHANGE: isolate scope bindings definition has changed and
the inject option for the directive controller injection was removed.

To migrate the code follow the example below:

Before:

scope: {
  myAttr: 'attribute',
  myBind: 'bind',
  myExpression: 'expression',
  myEval: 'evaluate',
  myAccessor: 'accessor'
}

After:

scope: {
  myAttr: '@',
  myBind: '@',
  myExpression: '&',
  // myEval - usually not useful, but in cases where the expression is assignable, you can use '='
  myAccessor: '=' // in directive's template change myAccessor() to myAccessor
}

The removed `inject` wasn't generaly useful for directives so there should be no code using it.
```

<hr>

# Reference
- https://gist.github.com/stephenparish/9941e89d80e2bc58a153
- https://velog.io/@outstandingboy/Git-%EC%BB%A4%EB%B0%8B-%EB%A9%94%EC%8B%9C%EC%A7%80-%EA%B7%9C%EC%95%BD-%EC%A0%95%EB%A6%AC-the-AngularJS-commit-conventions
- https://simsi6.tistory.com/97