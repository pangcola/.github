# Project instructions for GitHub Copilot (VS2022)

## 전문서 및 기본태도:
- 전문가 수준의 코드를 작성하되, 불필요하게 복잡하지 않게 구현
- 명확하고 읽기 쉬운 코드에 중점
- 사용자를 전문가로 대우하며 간결한 답변 제고...

## Code style
- C#: Unity runtime 코드는 `Update/LateUpdate/FixedUpdate`에서 할당/박싱 금지.
- C++: 성능 크리티컬 경로는 `noexcept`, `[[likely]]/[[unlikely]]` 힌트 사용. RTTI/예외 비활성 빌드 기준.
- 네이밍: PascalCase(타입) / camelCase(지역, 매개변수) / s_ (static), g_ (global) 금지.

## Engine specifics
- Unity: 메인 스레드 외 Unity API 호출 금지, Burst/Jobs 사용 시 Alloc Temp 규칙 명시.
- Unreal: UObject 수명주기/GC 고려, TSharedPtr/TWeakObjectPtr 우선.

## Testing & performance
- 단위 테스트 필수: 입력/출력/경계, 멀티스레드 경쟁 조건.
- 성능 기준: PC 60FPS / 모바일 30FPS 기준 프레임 버짓 명시.
- 로깅 레벨: [Gameplay][Netcode][Perf] 등.

## Commit messages
- 첫 줄 50자 요약(명령형). 본문에 원인/해결/리스크/검증 방법 포함.
- 이슈/작업 ID를 본문 마지막 줄에 `Refs: JIRA-XXXX` 형태로 첨부.

## Do/Don't
- Do: 명확한 계약(전/후조건) 주석, 경량 할당, 캡슐화.
- Don't: 프레임 경로에서 LINQ/Boxing(유니티), 가상 함수 남용, 동적 예외 비용.


# Unity C# 프로젝트 커스텀 규칙

## 금지사항
- **null-conditional 연산자(?., ??) 사용 금지**
  - `obj?.method()` 대신 `if (obj != null) obj.method()` 사용
  - `obj ?? defaultValue` 대신 `obj != null ? obj : defaultValue` 사용
- var 키워드 남용 금지 - 명시적 타입 선언 선호
- 한 줄에 여러 변수 선언 금지
- 요청하지 않은 주석 삽입 금지

## Unity 특화 규칙
- MonoBehaviour 상속 클래스는 'C' 접두사 사용 (예: CPlayerController, CGameManager)
- ScriptableObject 클래스는 'SO' 접두사 사용 (예: SOPlayerData, SOGameConfig)
- UI 관련 클래스는 'UI' 접두사 사용 (예: UIMainMenu, UIInventory)

## 성능 고려사항
- Update() 메서드에서 GetComponent() 호출 금지
- 문자열 연결시 StringBuilder 사용 권장
- 불필요한 boxing/unboxing 방지

## 예외 처리
- 예외 상황에 대한 명확한 로깅
- null 체크는 명시적으로 수행
