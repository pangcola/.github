# Project instructions for GitHub Copilot (VS2022)

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
