#TIP
### 1. .clang-format
```text
Language: Cpp
BasedOnStyle: Google
AccessModifierOffset: -1
AlignAfterOpenBracket: Align
AlignConsecutiveAssignments: None
AlignOperands: Align
AllowAllArgumentsOnNextLine: true
AllowAllConstructorInitializersOnNextLine: true
AllowAllParametersOfDeclarationOnNextLine: false
AllowShortBlocksOnASingleLine: Empty
AllowShortCaseLabelsOnASingleLine: false
AllowShortFunctionsOnASingleLine: Inline
AllowShortIfStatementsOnASingleLine: Never
AllowShortLambdasOnASingleLine: Inline
AllowShortLoopsOnASingleLine: false
AlwaysBreakAfterReturnType: None
AlwaysBreakTemplateDeclarations: Yes
BinPackArguments: true
BreakBeforeBraces: Custom
BraceWrapping:
  AfterCaseLabel: false
  AfterClass: false
  AfterStruct: false
  AfterControlStatement: Never
  AfterEnum: false
  AfterFunction: false
  AfterNamespace: false
  AfterUnion: false
  AfterExternBlock: false
  BeforeCatch: false
  BeforeElse: false
  BeforeLambdaBody: false
BreakBeforeBinaryOperators: None
BreakBeforeTernaryOperators: true
BreakConstructorInitializers: BeforeColon
BreakInheritanceList: BeforeColon
ColumnLimit: 120
CompactNamespaces: false
ContinuationIndentWidth: 4
Cpp11BracedListStyle: true
DerivePointerAlignment: false
EmptyLineBeforeAccessModifier: LogicalBlock
FixNamespaceComments: true
IncludeBlocks: Preserve
IndentCaseLabels: true
IndentPPDirectives: None
IndentWidth: 4
KeepEmptyLinesAtTheStartOfBlocks: true
MaxEmptyLinesToKeep: 1
NamespaceIndentation: None
ObjCSpaceAfterProperty: false
ObjCSpaceBeforeProtocolList: true
PointerAlignment: Left
ReflowComments: false
SpaceAfterCStyleCast: false
SpaceAfterLogicalNot: false
SpaceAfterTemplateKeyword: true
SpaceBeforeAssignmentOperators: true
SpaceBeforeCpp11BracedList: false
SpaceBeforeCtorInitializerColon: true
SpaceBeforeInheritanceColon: true
SpaceBeforeParens: ControlStatements
SpaceBeforeRangeBasedForLoopColon: true
SpaceBeforeSquareBrackets: false
SpaceInEmptyParentheses: false
SpacesBeforeTrailingComments: 2
SpacesInAngles: false
SpacesInCStyleCastParentheses: false
SpacesInContainerLiterals: false
SpacesInParentheses: false
SpacesInSquareBrackets: false
Standard: c++03
TabWidth: 4
UseTab: true
```
- .clang-format은 작업폴더의 최상단에 위치 시킨다.
### 2. clang-format Extension 설치
- vscode 익스텐션으로 설치
- brew install clang-format
### 3. settings.json 편집
```json
"[cpp]": {
"editor.defaultFormatter": "xaver.clang-format"
},
"editor.formatOnSave": true,
"clang-format.executable": "/opt/homebrew/bin/clang-format"
```
- 위의 설정들을 추가 한다.

### 4. 옵션 설명
1. **Language: Cpp**: 이 `.clang-format` 파일이 C++ 언어용임을 지정합니다.
    
2. **BasedOnStyle: Google**: Google의 코드 스타일 기준을 기반으로 포맷을 설정합니다.
    
3. **AccessModifierOffset: -1**: 접근 제어자 (public, private 등)의 들여쓰기를 조절합니다. 여기서는 -1 공백이 사용됩니다.
    
4. **AlignAfterOpenBracket: Align**: 여러 줄에 걸쳐 있는 괄호 안의 요소들을 첫 번째 요소와 맞춰 정렬합니다.
    
5. **AlignConsecutiveAssignments: None**: 연속적인 할당 연산자를 정렬하지 않습니다.
    
6. **AlignOperands: Align**: 여러 줄에 걸쳐있는 연산자의 피연산자들을 정렬합니다.
    
7. **AllowAllArgumentsOnNextLine: true**: 모든 인수들이 다음 줄로 이동할 수 있습니다.
    
8. **AllowAllConstructorInitializersOnNextLine: true**: 모든 생성자 초기화 목록 항목들이 다음 줄로 이동할 수 있습니다.
    
9. **AllowAllParametersOfDeclarationOnNextLine: false**: 모든 함수 선언의 매개변수들이 다음 줄로 이동하는 것을 허용하지 않습니다.
    
10. **AllowShortBlocksOnASingleLine: Empty**: 짧은 코드 블록만 한 줄에 표시하도록 허용합니다.
    
11. **AllowShortCaseLabelsOnASingleLine: false**: 짧은 case 레이블을 한 줄에 표시하는 것을 허용하지 않습니다.
    
12. **AllowShortFunctionsOnASingleLine: Inline**: 인라인 함수만 한 줄에 표시하도록 허용합니다.
    
13. **AllowShortIfStatementsOnASingleLine: Never**: 짧은 if 문을 한 줄에 표시하는 것을 허용하지 않습니다.
    
14. **AllowShortLambdasOnASingleLine: Inline**: 인라인 람다만 한 줄에 표시하도록 허용합니다.
    
15. **AllowShortLoopsOnASingleLine: false**: 짧은 루프를 한 줄에 표시하는 것을 허용하지 않습니다.
    
16. **AlwaysBreakAfterReturnType: None**: 함수 리턴 타입 뒤에 항상 줄바꿈하지 않습니다.
    
17. **AlwaysBreakTemplateDeclarations: Yes**: 템플릿 선언 뒤에 항상 줄바꿈합니다.
    
18. **BinPackArguments: true**: 가능하면 모든 인수들을 한 줄에 포장합니다.
    
19. **BreakBeforeBraces: Custom**: 중괄호 전에 줄바꿈을 하는 스타일을 사용자 정의로 설정합니다.
    
20. **BraceWrapping**: 중괄호 래핑에 대한 다양한 설정을 담은 객체입니다. 각 설정은 특정 구조체 또는 제어문 등에 대한 중괄호 래핑을 제어합니다.
    
21. **BreakBeforeBinaryOperators: None**: 이항 연산자 전에 줄바꿈하지 않습니다.
    
22. **BreakBeforeTernaryOperators: true**: 삼항 연산자 전에 줄바꿈을 합니다.
    
23. **BreakConstructorInitializers: BeforeColon**: 생성자 초기화 리스트에서 콜론 전에 줄바꿈을 합니다.
    
24. **BreakInheritanceList: BeforeColon**: 상속 목록에서 콜론 전에 줄바꿈을 합니다.
    
25. **ColumnLimit: 120**: 한 줄에 최대 120자까지 허용합니다.
    
26. **CompactNamespaces: false**: 네임스페이스를 한 줄에 정의하는 것을 허용하지 않습니다.
    
27. **ContinuationIndentWidth: 4**: 줄 계속시 들여쓰기 너비는 4공백입니다.
    
28. **Cpp11BracedListStyle: true**: C++11 스타일의 중괄호로 된 리스트 스타일을 사용합니다.
    
29. **DerivePointerAlignment: false**: 포인터 정렬 스타일을 기존 코드에서 유추하지 않습니다.
    
30. **EmptyLineBeforeAccessModifier: LogicalBlock**: 논리 블록 전에 빈 줄을 삽입합니다.
    
31. **FixNamespaceComments: true**: 네임스페이스 주석을 자동으로 수정합니다.
    
32. **IncludeBlocks: Preserve**: include 블록의 순서를 유지합니다.
    
33. **IndentCaseLabels: true**: case 레이블을 들여쓰기합니다.
    
34. **IndentPPDirectives: None**: 전처리기 지시문을 들여쓰기하지 않습니다.
    
35. **IndentWidth: 4**: 기본 들여쓰기 너비는 4공백입니다.
    
36. **KeepEmptyLinesAtTheStartOfBlocks: true**: 블록 시작 부분의 빈 줄을 유지합니다.
    
37. **MaxEmptyLinesToKeep: 1**: 최대 1개의 연속적인 빈 줄을 유지합니다.
    
38. **NamespaceIndentation: None**: 네임스페이스 들여쓰기를 하지 않습니다.
    
39. **ObjCSpaceAfterProperty: false**: Objective-C의 속성 뒤에 공백을 넣지 않습니다.
    
40. **ObjCSpaceBeforeProtocolList: true**: Objective-C의 프로토콜 리스트 앞에 공백을 넣습니다.
    
41. **PointerAlignment: Left**: 포인터 정렬을 왼쪽에 맞춥니다.
    
42. **ReflowComments: false**: 주석을 자동으로 리플로우하지 않습니다.
    
43. **SpaceAfterCStyleCast: false**: C 스타일 캐스트 뒤에 공백을 넣지 않습니다.
    
44. **SpaceAfterLogicalNot: false**: 논리 NOT 연산자 뒤에 공백을 넣지 않습니다.
    
45. **SpaceAfterTemplateKeyword: true**: 템플릿 키워드 뒤에 공백을 넣습니다.
    
46. **SpaceBeforeAssignmentOperators: true**: 할당 연산자 앞에 공백을 넣습니다.
    
47. **SpaceBeforeCpp11BracedList: false**: C++11 스타일의 중괄호 리스트 앞에 공백을 넣지 않습니다.
    
48. **SpaceBeforeCtorInitializerColon: true**: 생성자 초기화 리스트의 콜론 앞에 공백을 넣습니다.
    
49. **SpaceBeforeInheritanceColon: true**: 상속 리스트의 콜론 앞에 공백을 넣습니다.
    
50. **SpaceBeforeParens: ControlStatements**: 제어문의 괄호 앞에만 공백을 넣습니다.
    
51. **SpaceBeforeRangeBasedForLoopColon: true**: 범위 기반 for 루프의 콜론 앞에 공백을 넣습니다.
    
52. **SpaceBeforeSquareBrackets: false**: 대괄호 앞에 공백을 넣지 않습니다.
    
53. **SpaceInEmptyParentheses: false**: 빈 괄호 안에 공백을 넣지 않습니다.
    
54. **SpacesBeforeTrailingComments: 2**: 후행 주석 앞에 2개의 공백을 넣습니다.
    
55. **SpacesInAngles: false**: 꺽쇠 괄호 안에 공백을 넣지 않습니다.
    
56. **SpacesInCStyleCastParentheses: false**: C 스타일 캐스트 괄호 안에 공백을 넣지 않습니다.
    
57. **SpacesInContainerLiterals: false**: 컨테이너 리터럴 안에 공백을 넣지 않습니다.
    
58. **SpacesInParentheses: false**: 괄호 안에 공백을 넣지 않습니다.
    
59. **SpacesInSquareBrackets: false**: 대괄호 안에 공백을 넣지 않습니다.
    
60. **Standard: c++03**: 코드를 C++03 표준으로 포맷팅합니다.
    
61. **TabWidth: 4**: 탭 문자의 너비를 4공백으로 설정합니다.
    
62. **UseTab: true**: 들여쓰기에 탭 문자를 사용합니다.
