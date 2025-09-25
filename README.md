# Week 4 실습: Process와 Thread 관리

## 🚀 실습 개요

이 실습에서는 Windows API를 사용하여 프로세스와 스레드를 생성, 관리, 동기화하는 방법을 학습합니다.

### 📋 실습 목표
1. **프로세스 관리**: CreateProcess를 사용한 프로세스 생성과 제어
2. **멀티스레딩**: CreateThread를 사용한 병렬 처리
3. **동기화**: Critical Section을 사용한 스레드 간 데이터 보호
4. **성능 분석**: 단일 vs 멀티스레드 성능 비교

### 🔧 개발 환경 설정

#### 필수 요구사항
- **운영체제**: Windows 10/11
- **컴파일러**: Microsoft Visual C++ (MSVC) 또는 MinGW-w64
- **IDE**: Visual Studio, Code::Blocks, 또는 명령행 환경

#### 명령행 환경 설정 (권장)
1. **Visual Studio Developer Command Prompt** 또는
2. **Windows SDK Command Prompt** 사용

---

## 💻 컴파일 및 실행 가이드

### Method 1: Visual Studio Developer Command Prompt 사용

1. **Developer Command Prompt 실행**:
   ```cmd
   # 시작 메뉴에서 "Developer Command Prompt for VS" 검색 후 실행
   ```

2. **작업 디렉토리로 이동**:
   ```cmd
   cd /d "경로\week04\practicum"
   ```

3. **컴파일**:
   ```cmd
   # Problem 1 컴파일
   cl problem1.c /Fe:problem1.exe

   # Problem 2 컴파일
   cl problem2.c /Fe:problem2.exe

   # Problem 3 컴파일
   cl problem3.c /Fe:problem3.exe
   ```

4. **실행**:
   ```cmd
   # Problem 1 실행
   problem1.exe

   # Problem 2 실행
   problem2.exe

   # Problem 3 실행
   problem3.exe
   ```

### Method 2: MinGW-w64 사용

1. **MinGW-w64 설치 확인**:
   ```cmd
   gcc --version
   ```

2. **컴파일**:
   ```cmd
   # Problem 1 컴파일
   gcc -o problem1.exe problem1.c -lkernel32

   # Problem 2 컴파일
   gcc -o problem2.exe problem2.c -lkernel32

   # Problem 3 컴파일
   gcc -o problem3.exe problem3.c -lkernel32
   ```

3. **실행**:
   ```cmd
   problem1.exe
   problem2.exe
   problem3.exe
   ```

### Method 3: 통합 빌드 스크립트

**build.bat** 파일 생성:
```batch
@echo off
echo ===== Building Week 4 Practice Problems =====
echo.

echo [1/3] Compiling Problem 1 (Process Manager)...
cl problem1.c /Fe:problem1.exe
if %errorlevel% neq 0 (
    echo ERROR: Problem 1 compilation failed!
    pause
    exit /b 1
)

echo [2/3] Compiling Problem 2 (Multi-thread Calculator)...
cl problem2.c /Fe:problem2.exe
if %errorlevel% neq 0 (
    echo ERROR: Problem 2 compilation failed!
    pause
    exit /b 1
)

echo [3/3] Compiling Problem 3 (Producer-Consumer)...
cl problem3.c /Fe:problem3.exe
if %errorlevel% neq 0 (
    echo ERROR: Problem 3 compilation failed!
    pause
    exit /b 1
)

echo.
echo ===== Build Completed Successfully! =====
echo.
echo Available executables:
echo - problem1.exe (Process Manager)
echo - problem2.exe (Multi-thread Calculator)
echo - problem3.exe (Producer-Consumer Simulator)
echo.
pause
```

**실행**: `build.bat`

---

## 📚 실습 문제별 가이드

### 🔴 Problem 1: 프로세스 실행기

#### 학습 목표
- CreateProcess 함수의 매개변수 완전 이해
- STARTUPINFO와 PROCESS_INFORMATION 구조체 활용
- 다중 프로세스 동시 실행 및 관리
- 프로세스 상태 모니터링과 에러 처리

#### 주요 기능
1. **프로세스 실행**: 사용자가 지정한 프로그램을 CreateProcess로 실행
2. **상태 모니터링**: 실행 중인 프로세스들의 상태를 실시간 확인
3. **다중 프로세스**: 최대 5개까지 동시 실행 관리
4. **종료 관리**: 프로세스 완료 대기 및 종료 코드 확인

#### 실행 예제
```cmd
problem1.exe
```

**예상 출력**:
```
===== 프로세스 실행기 =====
1. 프로세스 실행
2. 실행 중인 프로세스 확인
3. 프로세스 종료 대기
4. 모든 프로세스 종료
0. 종료
선택: 1
실행할 프로그램: notepad.exe
[INFO] 프로세스 실행됨 - PID: 1234
```

#### 실습 과제
1. 메모장(notepad.exe) 3개를 동시에 실행해보세요
2. 각 프로세스의 PID와 실행 시간을 확인하세요
3. WaitForMultipleObjects로 모든 프로세스 종료를 대기해보세요

### 🟡 Problem 2: 멀티스레드 계산기

#### 학습 목표
- CreateThread와 스레드 매개변수 전달 방법
- 대용량 계산 작업의 스레드 분할 기법
- WaitForMultipleObjects를 사용한 스레드 동기화
- 단일 스레드 vs 멀티스레드 성능 비교 분석

#### 주요 기능
1. **작업 분할**: 1부터 10,000,000까지의 제곱근 합을 여러 스레드로 분할
2. **성능 측정**: 단일/멀티스레드 실행 시간 비교
3. **진행 모니터링**: 각 스레드별 진행률 실시간 표시
4. **결과 수집**: 모든 스레드 완료 후 최종 결과 계산

#### 실행 예제
```cmd
problem2.exe
```

**예상 출력**:
```
===== 멀티스레드 계산기 =====
계산 범위: 1 ~ 10,000,000 (제곱근의 합)

[단일 스레드 실행]
진행률: ████████████████████ 100% 완료!
결과: 21081851083600.445312
실행 시간: 2.847초

[멀티 스레드 실행 - 4개 스레드]
Thread 1: ████████████████████ 100% (1 ~ 2,500,000)
Thread 2: ████████████████████ 100% (2,500,001 ~ 5,000,000)
Thread 3: ████████████████████ 100% (5,000,001 ~ 7,500,000)
Thread 4: ████████████████████ 100% (7,500,001 ~ 10,000,000)

결과: 21081851083600.445312
실행 시간: 0.756초
성능 향상: 3.77배
```

#### 실습 과제
1. 스레드 개수를 1, 2, 4, 8개로 변경하며 성능을 측정하세요
2. 계산 범위를 1억까지 늘려서 실행해보세요
3. CPU 사용률을 작업 관리자에서 확인하세요

### 🟢 Problem 3: Producer-Consumer 시뮬레이터

#### 학습 목표
- Critical Section을 사용한 스레드 동기화
- Producer-Consumer 패턴의 실제 구현
- Race Condition 방지 및 데이터 무결성 보장
- 다양한 생산/소비 속도에서의 동작 이해

#### 주요 기능
1. **다중 생산자/소비자**: 최대 4개의 생산자와 소비자 스레드
2. **버퍼 관리**: 크기 20의 원형 버퍼로 데이터 저장
3. **동기화**: Critical Section으로 버퍼 접근 보호
4. **모니터링**: 실시간 버퍼 상태와 스레드 활동 표시

#### 실행 예제
```cmd
problem3.exe
```

**예상 출력**:
```
===== Producer-Consumer 시뮬레이터 =====

설정:
- 생산자: 2개 스레드 (100ms 간격)
- 소비자: 1개 스레드 (150ms 간격)
- 버퍼 크기: 20
- 총 아이템: 100개

[실시간 모니터링]
시간: 00:03
Buffer: [▓▓▓▓▓▓▓░░░░░░░░░░░░░] 7/20

Producer 1: 생산 완료 - Item #23 (총 생산: 23)
Producer 2: 생산 완료 - Item #24 (총 생산: 24)
Consumer 1: 소비 완료 - Item #15 (총 소비: 15)

생산 속도: 15.2 items/sec
소비 속도: 10.1 items/sec
버퍼 사용률: 35%
```

#### 실습 과제
1. 생산자 2개, 소비자 1개로 실행하여 버퍼 증가를 관찰하세요
2. 생산자 1개, 소비자 2개로 실행하여 버퍼 감소를 관찰하세요
3. 동일한 수의 생산자/소비자로 균형 상태를 만들어보세요
4. Critical Section 코드를 주석처리하고 실행하여 Race Condition을 관찰하세요

---

## 🐛 문제 해결 가이드

### 컴파일 오류

**1. 'cl'이 인식되지 않는 경우**
```cmd
# Visual Studio Developer Command Prompt를 사용하거나
# 다음 경로를 PATH에 추가:
# C:\Program Files\Microsoft Visual Studio\2022\Community\VC\Tools\MSVC\[version]\bin\Hostx64\x64
```

**2. Windows SDK 관련 오류**
```cmd
# Windows SDK가 설치되어 있는지 확인
# Visual Studio Installer에서 "Windows SDK" 설치
```

**3. 링크 오류**
```cmd
# 필요한 라이브러리가 자동으로 링크되지 않는 경우
cl problem1.c /Fe:problem1.exe kernel32.lib user32.lib
```

### 실행 오류

**1. 프로세스 실행 실패 (Problem 1)**
- 실행하려는 프로그램의 경로가 정확한지 확인
- 전체 경로를 사용: `C:\Windows\System32\notepad.exe`

**2. 스레드 생성 실패 (Problem 2, 3)**
- 메모리 부족일 수 있음 - 스레드 수를 줄여서 테스트
- 스택 크기를 명시적으로 지정: `CreateThread(NULL, 1024*1024, ...)`

**3. 동기화 문제 (Problem 3)**
- Critical Section 초기화 확인
- EnterCriticalSection과 LeaveCriticalSection이 쌍을 이루는지 확인

### 성능 관련

**1. 멀티스레드 성능이 떨어지는 경우 (Problem 2)**
- CPU 코어 수보다 많은 스레드를 생성하지 마세요
- 작업량이 너무 작으면 오버헤드가 더 클 수 있습니다

**2. Producer-Consumer 불균형 (Problem 3)**
- 생산자와 소비자의 처리 속도를 조정하세요
- 버퍼 크기를 늘려서 여유를 두세요

---

## 📊 학습 점검 체크리스트

### Problem 1 완료 기준
- [ ] CreateProcess로 프로세스를 성공적으로 실행
- [ ] 여러 프로세스를 동시에 관리
- [ ] 프로세스 상태를 실시간으로 모니터링
- [ ] WaitForMultipleObjects로 모든 프로세스 종료 대기
- [ ] 에러 상황에서 적절한 예외 처리

### Problem 2 완료 기준
- [ ] CreateThread로 여러 스레드 생성
- [ ] 작업을 스레드 수에 따라 균등 분할
- [ ] 각 스레드의 실행 시간 측정
- [ ] 단일 vs 멀티스레드 성능 비교
- [ ] 스레드 수에 따른 성능 변화 분석

### Problem 3 완료 기준
- [ ] Critical Section을 사용한 동기화 구현
- [ ] Producer-Consumer 패턴 정확히 구현
- [ ] 버퍼 오버플로우/언더플로우 방지
- [ ] 실시간 상태 모니터링
- [ ] Race Condition 없이 안전한 데이터 처리

---

## 🎯 심화 학습 과제

### Level 1: 기본 확장
1. **Problem 1**: 프로세스 실행 시 명령행 인수 전달 기능 추가
2. **Problem 2**: 다른 수학 함수(sin, cos, log 등)로 계산 변경
3. **Problem 3**: 여러 종류의 데이터를 처리하는 버퍼 구현

### Level 2: 고급 기능
1. **성능 프로파일링**: 각 함수별 실행 시간 측정
2. **메모리 사용량 모니터링**: 프로세스/스레드별 메모리 사용량 추적
3. **로그 시스템**: 모든 활동을 파일로 기록

### Level 3: 실무 응용
1. **네트워크 서버**: Producer-Consumer 패턴으로 다중 클라이언트 처리
2. **파일 처리**: 멀티스레드로 대용량 파일 병렬 처리
3. **시스템 모니터링**: 실시간 시스템 리소스 모니터링 도구

---

## 프로세스와 스레드 개념

### 프로세스 (Process)

프로세스는 실행 중인 프로그램의 인스턴스입니다. 각 프로세스는 독립된 메모리 공간과 자원을 가지며, 운영체제에 의해 관리됩니다.

**프로세스의 특징:**
- **독립성**: 각 프로세스는 별도의 메모리 공간(주소 공간)을 가짐
- **보안**: 다른 프로세스의 메모리에 직접 접근할 수 없음
- **안정성**: 한 프로세스의 오류가 다른 프로세스에 영향을 주지 않음
- **오버헤드**: 생성/전환 시 상당한 시스템 자원 소모

**프로세스 메모리 구조:**
```
[그림 1: 프로세스 메모리 구조]
+-----------------+
|      Stack      | ← 지역변수, 함수 매개변수
+-----------------+
|       ↓         |
|    Free Space   |
|       ↑         |
+-----------------+
|      Heap       | ← 동적 할당 메모리
+-----------------+
|      Data       | ← 전역변수, 정적변수
+-----------------+
|      Code       | ← 프로그램 코드
+-----------------+
```

**프로세스 상태 전이도:**
```
[그림 2: 프로세스 상태 전이도]
New → Ready → Running → Terminated
        ↑        ↓
        └─ Waiting ←┘
```

### 스레드 (Thread)

스레드는 프로세스 내에서 실행되는 경량화된 실행 단위입니다. 같은 프로세스의 스레드들은 메모리 공간을 공유합니다.

**스레드의 특징:**
- **경량성**: 생성/전환 오버헤드가 프로세스보다 훨씬 적음
- **메모리 공유**: 같은 프로세스의 스레드들은 코드, 데이터, 힙 영역을 공유
- **독립적 스택**: 각 스레드는 고유한 스택과 레지스터를 가짐
- **동시성**: 멀티코어 시스템에서 진정한 병렬 처리 가능

**스레드 메모리 공유 구조:**
```
[그림 3: 멀티스레드 메모리 구조]
Process Memory Space
+-----------------+
| Thread 1 Stack  |
+-----------------+
| Thread 2 Stack  |
+-----------------+
| Thread 3 Stack  |
+-----------------+
|    Shared       |
|     Heap        | ← 모든 스레드가 공유
+-----------------+
|    Shared       |
|     Data        | ← 모든 스레드가 공유
+-----------------+
|    Shared       |
|     Code        | ← 모든 스레드가 공유
+-----------------+
```

### 프로세스 vs 스레드 비교

| 특성 | 프로세스 (Process) | 스레드 (Thread) |
|------|----------|--------|
| 메모리 공간 | 독립적 | 공유 |
| 생성 오버헤드 | 높음 | 낮음 |
| 통신 방법 | IPC (Inter-Process Communication: 파이프, 공유메모리 등) | 공유변수, 메시지큐 |
| 안정성 | 높음 (격리됨) | 낮음 (메모리 공유로 인한 위험) |
| 디버깅 난이도 | 쉬움 | 어려움 (Race Condition 등) |
| 확장성 | 제한적 | 우수 |

**프로세스와 스레드 실행 모델:**
```
[그림 4: 프로세스 vs 스레드 실행 모델]

멀티프로세스:
Process A  Process B  Process C
[Thread]   [Thread]   [Thread]
    ↕          ↕          ↕
 Memory A   Memory B   Memory C

멀티스레드:
      Process A
   ┌─────┬─────┬─────┐
Thread1 Thread2 Thread3
   └─────┴─────┴─────┘
       Shared Memory
```

### 동기화 (Synchronization)

멀티스레드 환경에서는 공유 자원에 대한 동시 접근을 제어해야 합니다.

**주요 동기화 기법:**
1. **Critical Section (임계 영역)**: Windows에서 제공하는 경량 동기화 객체
2. **Mutex (Mutual Exclusion, 상호배제)**: 프로세스 간에도 사용 가능한 상호배제 객체
3. **Semaphore (세마포어)**: 카운팅 세마포어, 제한된 자원 접근 제어
4. **Event (이벤트)**: 스레드 간 신호 전달

**Race Condition 예시:**
```
[그림 5: Race Condition 발생 시나리오]
공유변수 count = 0

Thread A        Thread B
읽기: count=0   읽기: count=0
증가: 0+1=1     증가: 0+1=1
저장: count=1   저장: count=1

결과: count=1 (예상: 2)
```

### Windows API 상세 설명

#### 프로세스 관리 API

**1. CreateProcess() 함수**
```c
BOOL CreateProcess(
    LPCSTR lpApplicationName,       // 실행 파일 경로 (NULL 가능)
    LPSTR lpCommandLine,           // 명령줄 인수
    LPSECURITY_ATTRIBUTES lpProcessAttributes,  // 프로세스 보안 속성
    LPSECURITY_ATTRIBUTES lpThreadAttributes,   // 스레드 보안 속성
    BOOL bInheritHandles,          // 핸들 상속 여부
    DWORD dwCreationFlags,         // 생성 플래그
    LPVOID lpEnvironment,          // 환경 변수
    LPCSTR lpCurrentDirectory,     // 현재 디렉터리
    LPSTARTUPINFO lpStartupInfo,   // 시작 정보
    LPPROCESS_INFORMATION lpProcessInformation // 프로세스 정보
);
```

**매개변수 설명:**
- `dwCreationFlags` 주요 값:
  - `0`: 기본 생성
  - `CREATE_NEW_CONSOLE`: 새 콘솔 창 생성
  - `CREATE_NO_WINDOW`: 창 없이 실행
  - `CREATE_SUSPENDED`: 일시정지 상태로 생성

**2. 프로세스 대기 및 정보 확인**
```c
// 프로세스 종료 대기
DWORD WaitForSingleObject(HANDLE hHandle, DWORD dwMilliseconds);
// dwMilliseconds: INFINITE (무한 대기) 또는 밀리초 단위

// 종료 코드 확인
BOOL GetExitCodeProcess(HANDLE hProcess, LPDWORD lpExitCode);
// STILL_ACTIVE: 아직 실행 중

// 프로세스 강제 종료
BOOL TerminateProcess(HANDLE hProcess, UINT uExitCode);
```

#### 스레드 관리 API

**1. CreateThread() 함수**
```c
HANDLE CreateThread(
    LPSECURITY_ATTRIBUTES lpThreadAttributes,  // 보안 속성
    SIZE_T dwStackSize,                       // 스택 크기 (0=기본값)
    LPTHREAD_START_ROUTINE lpStartAddress,    // 스레드 함수 주소
    LPVOID lpParameter,                       // 스레드 매개변수
    DWORD dwCreationFlags,                    // 생성 플래그
    LPDWORD lpThreadId                        // 스레드 ID 저장
);
```

**스레드 함수 형식:**
```c
DWORD WINAPI ThreadFunction(LPVOID lpParam) {
    // 스레드 실행 코드
    return 0; // 종료 코드
}
```

**2. 스레드 동기화 함수**
```c
// 단일 객체 대기
DWORD WaitForSingleObject(HANDLE hHandle, DWORD dwMilliseconds);

// 다중 객체 대기
DWORD WaitForMultipleObjects(
    DWORD nCount,           // 핸들 개수
    const HANDLE* lpHandles, // 핸들 배열
    BOOL bWaitAll,          // TRUE: 모두 대기, FALSE: 하나만 대기
    DWORD dwMilliseconds    // 대기 시간
);

// 스레드 종료
void ExitThread(DWORD dwExitCode);

// 스레드 일시 정지/재개
DWORD SuspendThread(HANDLE hThread);
DWORD ResumeThread(HANDLE hThread);
```

#### 동기화 객체

**1. Critical Section (임계 영역)**
```c
// 초기화
void InitializeCriticalSection(LPCRITICAL_SECTION lpCriticalSection);

// 진입 (블로킹)
void EnterCriticalSection(LPCRITICAL_SECTION lpCriticalSection);

// 진입 시도 (논블로킹)
BOOL TryEnterCriticalSection(LPCRITICAL_SECTION lpCriticalSection);

// 해제
void LeaveCriticalSection(LPCRITICAL_SECTION lpCriticalSection);

// 삭제
void DeleteCriticalSection(LPCRITICAL_SECTION lpCriticalSection);
```

**2. Mutex (상호 배제 객체)**
```c
// 생성
HANDLE CreateMutex(
    LPSECURITY_ATTRIBUTES lpMutexAttributes, // 보안 속성
    BOOL bInitialOwner,                      // 초기 소유권
    LPCSTR lpName                            // 이름 (NULL=익명)
);

// 기존 뮤텍스 열기
HANDLE OpenMutex(
    DWORD dwDesiredAccess,  // 접근 권한
    BOOL bInheritHandle,    // 상속 여부
    LPCSTR lpName           // 뮤텍스 이름
);

// 해제
BOOL ReleaseMutex(HANDLE hMutex);
```

**3. Semaphore (세마포어)**
```c
// 생성
HANDLE CreateSemaphore(
    LPSECURITY_ATTRIBUTES lpSemaphoreAttributes, // 보안 속성
    LONG lInitialCount,                          // 초기 카운트
    LONG lMaximumCount,                          // 최대 카운트
    LPCSTR lpName                                // 이름
);

// 해제 (카운트 증가)
BOOL ReleaseSemaphore(
    HANDLE hSemaphore,      // 세마포어 핸들
    LONG lReleaseCount,     // 증가시킬 값
    LPLONG lpPreviousCount  // 이전 카운트 저장
);
```

**4. Event (이벤트 객체)**
```c
// 생성
HANDLE CreateEvent(
    LPSECURITY_ATTRIBUTES lpEventAttributes, // 보안 속성
    BOOL bManualReset,                       // 수동/자동 리셋
    BOOL bInitialState,                      // 초기 상태 (신호/비신호)
    LPCSTR lpName                            // 이름
);

// 신호 상태로 설정
BOOL SetEvent(HANDLE hEvent);

// 비신호 상태로 설정
BOOL ResetEvent(HANDLE hEvent);

// 신호 후 자동으로 비신호로 변경
BOOL PulseEvent(HANDLE hEvent);
```

#### 프로세스 정보 수집 (Problem 4용)

**1. Toolhelp API (Tool Help Application Programming Interface)**
```c
#include <tlhelp32.h>

// 프로세스 스냅샷 생성
HANDLE CreateToolhelp32Snapshot(
    DWORD dwFlags,      // TH32CS_SNAPPROCESS
    DWORD th32ProcessID // 0 (전체 시스템)
);

// 첫 번째 프로세스 정보
BOOL Process32First(HANDLE hSnapshot, LPPROCESSENTRY32 lppe);

// 다음 프로세스 정보
BOOL Process32Next(HANDLE hSnapshot, LPPROCESSENTRY32 lppe);

// PROCESSENTRY32 구조체
typedef struct tagPROCESSENTRY32 {
    DWORD dwSize;               // 구조체 크기
    DWORD cntUsage;             // 참조 카운트
    DWORD th32ProcessID;        // 프로세스 ID
    ULONG_PTR th32DefaultHeapID; // 기본 힙 ID
    DWORD th32ModuleID;         // 모듈 ID
    DWORD cntThreads;           // 스레드 수
    DWORD th32ParentProcessID;  // 부모 프로세스 ID
    LONG pcPriClassBase;        // 우선순위 클래스
    DWORD dwFlags;              // 플래그
    CHAR szExeFile[MAX_PATH];   // 실행 파일명
} PROCESSENTRY32;
```

**2. 프로세스 성능 정보**
```c
#include <psapi.h>

// 메모리 정보
BOOL GetProcessMemoryInfo(
    HANDLE Process,                    // 프로세스 핸들
    PPROCESS_MEMORY_COUNTERS ppsmemCounters, // 메모리 카운터
    DWORD cb                           // 구조체 크기
);

// CPU 시간 정보
BOOL GetProcessTimes(
    HANDLE hProcess,        // 프로세스 핸들
    LPFILETIME lpCreationTime,   // 생성 시간
    LPFILETIME lpExitTime,       // 종료 시간
    LPFILETIME lpKernelTime,     // 커널 시간
    LPFILETIME lpUserTime        // 사용자 시간
);
```

#### 파일 I/O (Input/Output)와 디렉터리 작업 (Problem 5용)

**1. 파일 처리**
```c
// 파일 열기
HANDLE CreateFile(
    LPCSTR lpFileName,          // 파일명
    DWORD dwDesiredAccess,      // 접근 권한 (GENERIC_READ: 읽기 권한, GENERIC_WRITE: 쓰기 권한)
    DWORD dwShareMode,          // 공유 모드
    LPSECURITY_ATTRIBUTES lpSecurityAttributes, // 보안 속성
    DWORD dwCreationDisposition, // 생성 방식 (OPEN_EXISTING: 기존 파일 열기, CREATE_ALWAYS: 항상 새 파일 생성)
    DWORD dwFlagsAndAttributes, // 파일 속성
    HANDLE hTemplateFile        // 템플릿 파일
);

// 파일 읽기
BOOL ReadFile(
    HANDLE hFile,           // 파일 핸들
    LPVOID lpBuffer,        // 버퍼
    DWORD nNumberOfBytesToRead,  // 읽을 바이트 수
    LPDWORD lpNumberOfBytesRead, // 실제 읽은 바이트 수
    LPOVERLAPPED lpOverlapped    // 비동기 I/O (Input/Output) (NULL=동기)
);

// 파일 쓰기
BOOL WriteFile(
    HANDLE hFile,           // 파일 핸들
    LPCVOID lpBuffer,       // 버퍼
    DWORD nNumberOfBytesToWrite, // 쓸 바이트 수
    LPDWORD lpNumberOfBytesWritten, // 실제 쓴 바이트 수
    LPOVERLAPPED lpOverlapped    // 비동기 I/O (Input/Output)
);

// 파일 크기 확인
DWORD GetFileSize(HANDLE hFile, LPDWORD lpFileSizeHigh);
```

**2. 디렉터리 검색**
```c
// 파일 검색 시작
HANDLE FindFirstFile(LPCSTR lpFileName, LPWIN32_FIND_DATA lpFindFileData);

// 다음 파일 검색
BOOL FindNextFile(HANDLE hFindFile, LPWIN32_FIND_DATA lpFindFileData);

// 검색 종료
BOOL FindClose(HANDLE hFindFile);

// WIN32_FIND_DATA 구조체
typedef struct _WIN32_FIND_DATA {
    DWORD dwFileAttributes;      // 파일 속성
    FILETIME ftCreationTime;     // 생성 시간
    FILETIME ftLastAccessTime;   // 마지막 접근 시간
    FILETIME ftLastWriteTime;    // 마지막 수정 시간
    DWORD nFileSizeHigh;         // 파일 크기 (상위)
    DWORD nFileSizeLow;          // 파일 크기 (하위)
    DWORD dwReserved0;
    DWORD dwReserved1;
    CHAR cFileName[MAX_PATH];    // 파일명
    CHAR cAlternateFileName[14]; // 8.3 형식 파일명
} WIN32_FIND_DATA;
```

#### 시간 및 성능 측정

**1. 시간 측정**
```c
// 시스템 시간 (밀리초)
DWORD GetTickCount(void);
ULONGLONG GetTickCount64(void); // 64비트 버전

// 고해상도 타이머
BOOL QueryPerformanceCounter(LARGE_INTEGER *lpPerformanceCount);
BOOL QueryPerformanceFrequency(LARGE_INTEGER *lpFrequency);

// 시간 계산 예시
LARGE_INTEGER start, end, frequency;
QueryPerformanceFrequency(&frequency);
QueryPerformanceCounter(&start);
// ... 측정할 코드 ...
QueryPerformanceCounter(&end);
double elapsed = (double)(end.QuadPart - start.QuadPart) / frequency.QuadPart;
```

**2. 시스템 정보**
```c
// 시스템 정보 구조체
typedef struct _SYSTEM_INFO {
    WORD wProcessorArchitecture;     // 프로세서 아키텍처 (CPU Architecture)
    WORD wReserved;
    DWORD dwPageSize;                // 페이지 크기
    LPVOID lpMinimumApplicationAddress; // 최소 주소
    LPVOID lpMaximumApplicationAddress; // 최대 주소
    DWORD_PTR dwActiveProcessorMask;    // 활성 프로세서 마스크 (CPU Core Mask)
    DWORD dwNumberOfProcessors;         // 프로세서 수 (CPU Core Count)
    DWORD dwProcessorType;              // 프로세서 타입 (CPU Type)
    DWORD dwAllocationGranularity;      // 할당 단위
    WORD wProcessorLevel;               // 프로세서 레벨 (CPU Level)
    WORD wProcessorRevision;            // 프로세서 리비전 (CPU Revision)
} SYSTEM_INFO;

// 시스템 정보 가져오기
void GetSystemInfo(LPSYSTEM_INFO lpSystemInfo);

// 메모리 상태
typedef struct _MEMORYSTATUSEX {
    DWORD dwLength;                 // 구조체 크기
    DWORD dwMemoryLoad;             // 메모리 사용률 (%)
    DWORDLONG ullTotalPhys;         // 전체 물리 메모리 (Physical RAM)
    DWORDLONG ullAvailPhys;         // 사용 가능한 물리 메모리 (Available Physical RAM)
    DWORDLONG ullTotalPageFile;     // 전체 페이징 파일 크기 (Total Page File Size)
    DWORDLONG ullAvailPageFile;     // 사용 가능한 페이징 파일 (Available Page File)
    DWORDLONG ullTotalVirtual;      // 전체 가상 메모리 (Total Virtual Memory)
    DWORDLONG ullAvailVirtual;      // 사용 가능한 가상 메모리 (Available Virtual Memory)
    DWORDLONG ullAvailExtendedVirtual; // 예약됨
} MEMORYSTATUSEX;

BOOL GlobalMemoryStatusEx(LPMEMORYSTATUSEX lpBuffer);
```

## 실제 사용 예제

### 예제 1: 간단한 프로세스 생성

```c
#include <windows.h>
#include <stdio.h>

int main() {
    STARTUPINFO si;
    PROCESS_INFORMATION pi;

    // 구조체 초기화
    ZeroMemory(&si, sizeof(si));
    si.cb = sizeof(si);
    ZeroMemory(&pi, sizeof(pi));

    // 메모장 프로세스 생성
    if (CreateProcess(
        NULL,                   // 실행 파일 경로
        "notepad.exe",          // 명령줄
        NULL,                   // 프로세스 보안 속성
        NULL,                   // 스레드 보안 속성
        FALSE,                  // 핸들 상속 여부
        0,                      // 생성 플래그
        NULL,                   // 환경 변수
        NULL,                   // 현재 디렉터리
        &si,                    // STARTUPINFO
        &pi)                    // PROCESS_INFORMATION
    ) {
        printf("메모장이 실행되었습니다. PID: %d\n", pi.dwProcessId);

        // 프로세스 종료 대기
        WaitForSingleObject(pi.hProcess, INFINITE);
        printf("메모장이 종료되었습니다.\n");

        // 핸들 정리
        CloseHandle(pi.hProcess);
        CloseHandle(pi.hThread);
    } else {
        printf("프로세스 생성 실패: %d\n", GetLastError());
    }

    return 0;
}
```

### 예제 2: 멀티스레드로 계산 작업 분할

```c
#include <windows.h>
#include <stdio.h>

#define THREAD_COUNT 4
#define TOTAL_NUMBERS 1000000

// 스레드 매개변수 구조체
typedef struct {
    int start;
    int end;
    long long sum;
} ThreadData;

// 스레드 함수: 지정된 범위의 수를 합산
DWORD WINAPI CalculateSum(LPVOID lpParam) {
    ThreadData* data = (ThreadData*)lpParam;
    data->sum = 0;

    for (int i = data->start; i <= data->end; i++) {
        data->sum += i;
    }

    printf("스레드 %d: %d부터 %d까지 합계 = %lld\n",
           GetCurrentThreadId(), data->start, data->end, data->sum);

    return 0;
}

int main() {
    HANDLE threads[THREAD_COUNT];
    ThreadData threadData[THREAD_COUNT];
    DWORD threadId;
    int numbersPerThread = TOTAL_NUMBERS / THREAD_COUNT;

    printf("멀티스레드로 1부터 %d까지 합계 계산\n", TOTAL_NUMBERS);

    // 스레드 생성
    for (int i = 0; i < THREAD_COUNT; i++) {
        threadData[i].start = i * numbersPerThread + 1;
        threadData[i].end = (i + 1) * numbersPerThread;
        if (i == THREAD_COUNT - 1) {
            threadData[i].end = TOTAL_NUMBERS; // 마지막 스레드는 나머지 처리
        }

        threads[i] = CreateThread(
            NULL,           // 보안 속성
            0,              // 스택 크기
            CalculateSum,   // 스레드 함수
            &threadData[i], // 매개변수
            0,              // 생성 플래그
            &threadId       // 스레드 ID
        );

        if (threads[i] == NULL) {
            printf("스레드 %d 생성 실패\n", i);
            return 1;
        }
    }

    // 모든 스레드 완료 대기
    WaitForMultipleObjects(THREAD_COUNT, threads, TRUE, INFINITE);

    // 결과 합산
    long long totalSum = 0;
    for (int i = 0; i < THREAD_COUNT; i++) {
        totalSum += threadData[i].sum;
        CloseHandle(threads[i]);
    }

    printf("최종 합계: %lld\n", totalSum);
    printf("검증 공식: %lld\n", (long long)TOTAL_NUMBERS * (TOTAL_NUMBERS + 1) / 2);

    return 0;
}
```

### 예제 3: Critical Section을 사용한 동기화

```c
#include <windows.h>
#include <stdio.h>

#define THREAD_COUNT 5
#define INCREMENTS_PER_THREAD 100000

// 공유 변수와 Critical Section
int sharedCounter = 0;
CRITICAL_SECTION cs;

// 동기화 없는 증가 함수 (Race Condition 발생)
DWORD WINAPI UnsafeIncrement(LPVOID lpParam) {
    int threadId = (int)lpParam;

    for (int i = 0; i < INCREMENTS_PER_THREAD; i++) {
        sharedCounter++; // 안전하지 않은 접근
    }

    printf("스레드 %d 완료 (Unsafe)\n", threadId);
    return 0;
}

// 동기화된 증가 함수 (Critical Section 사용)
DWORD WINAPI SafeIncrement(LPVOID lpParam) {
    int threadId = (int)lpParam;

    for (int i = 0; i < INCREMENTS_PER_THREAD; i++) {
        EnterCriticalSection(&cs);  // Critical Section 진입
        sharedCounter++;            // 안전한 접근
        LeaveCriticalSection(&cs);  // Critical Section 해제
    }

    printf("스레드 %d 완료 (Safe)\n", threadId);
    return 0;
}

int main() {
    HANDLE threads[THREAD_COUNT];
    DWORD threadId;

    // Critical Section 초기화
    InitializeCriticalSection(&cs);

    printf("=== 동기화 없는 실행 (Race Condition 발생) ===\n");
    sharedCounter = 0;

    // 안전하지 않은 스레드들 생성
    for (int i = 0; i < THREAD_COUNT; i++) {
        threads[i] = CreateThread(NULL, 0, UnsafeIncrement, (LPVOID)i, 0, &threadId);
    }

    WaitForMultipleObjects(THREAD_COUNT, threads, TRUE, INFINITE);

    for (int i = 0; i < THREAD_COUNT; i++) {
        CloseHandle(threads[i]);
    }

    printf("예상값: %d, 실제값: %d (차이: %d)\n",
           THREAD_COUNT * INCREMENTS_PER_THREAD, sharedCounter,
           THREAD_COUNT * INCREMENTS_PER_THREAD - sharedCounter);

    printf("\n=== 동기화된 실행 (Critical Section 사용) ===\n");
    sharedCounter = 0;

    // 안전한 스레드들 생성
    for (int i = 0; i < THREAD_COUNT; i++) {
        threads[i] = CreateThread(NULL, 0, SafeIncrement, (LPVOID)i, 0, &threadId);
    }

    WaitForMultipleObjects(THREAD_COUNT, threads, TRUE, INFINITE);

    for (int i = 0; i < THREAD_COUNT; i++) {
        CloseHandle(threads[i]);
    }

    printf("예상값: %d, 실제값: %d (차이: %d)\n",
           THREAD_COUNT * INCREMENTS_PER_THREAD, sharedCounter,
           THREAD_COUNT * INCREMENTS_PER_THREAD - sharedCounter);

    // Critical Section 정리
    DeleteCriticalSection(&cs);

    return 0;
}
```

### 예제 4: Producer-Consumer 패턴 구현

```c
#include <windows.h>
#include <stdio.h>
#include <stdlib.h>

#define BUFFER_SIZE 10
#define PRODUCER_COUNT 2
#define CONSUMER_COUNT 2
#define ITEMS_TO_PRODUCE 20

// 순환 버퍼 구조체
typedef struct {
    int buffer[BUFFER_SIZE];
    int head;   // 생산자가 데이터를 넣을 위치
    int tail;   // 소비자가 데이터를 가져올 위치
    int count;  // 현재 버퍼에 있는 아이템 수
} CircularBuffer;

CircularBuffer sharedBuffer = {0, 0, 0, 0};
CRITICAL_SECTION bufferCS;
HANDLE notEmpty; // 버퍼가 비어있지 않음을 알리는 이벤트
HANDLE notFull;  // 버퍼가 가득 차지 않음을 알리는 이벤트
int totalProduced = 0;
int totalConsumed = 0;

// 생산자 스레드
DWORD WINAPI Producer(LPVOID lpParam) {
    int producerId = (int)lpParam;

    while (totalProduced < ITEMS_TO_PRODUCE) {
        // 버퍼가 가득 차지 않을 때까지 대기
        WaitForSingleObject(notFull, INFINITE);

        EnterCriticalSection(&bufferCS);

        if (totalProduced < ITEMS_TO_PRODUCE) {
            int item = rand() % 100 + 1;
            sharedBuffer.buffer[sharedBuffer.head] = item;
            sharedBuffer.head = (sharedBuffer.head + 1) % BUFFER_SIZE;
            sharedBuffer.count++;
            totalProduced++;

            printf("생산자 %d: %d 생산 (버퍼: %d/%d)\n",
                   producerId, item, sharedBuffer.count, BUFFER_SIZE);
        }

        LeaveCriticalSection(&bufferCS);

        // 소비자에게 데이터 준비됨을 알림
        SetEvent(notEmpty);

        Sleep(100); // 생산 시뮬레이션
    }

    return 0;
}

// 소비자 스레드
DWORD WINAPI Consumer(LPVOID lpParam) {
    int consumerId = (int)lpParam;

    while (totalConsumed < ITEMS_TO_PRODUCE) {
        // 버퍼가 비어있지 않을 때까지 대기
        WaitForSingleObject(notEmpty, INFINITE);

        EnterCriticalSection(&bufferCS);

        if (sharedBuffer.count > 0 && totalConsumed < ITEMS_TO_PRODUCE) {
            int item = sharedBuffer.buffer[sharedBuffer.tail];
            sharedBuffer.tail = (sharedBuffer.tail + 1) % BUFFER_SIZE;
            sharedBuffer.count--;
            totalConsumed++;

            printf("소비자 %d: %d 소비 (버퍼: %d/%d)\n",
                   consumerId, item, sharedBuffer.count, BUFFER_SIZE);
        }

        LeaveCriticalSection(&bufferCS);

        // 생산자에게 공간 생성됨을 알림
        SetEvent(notFull);

        Sleep(150); // 소비 시뮬레이션
    }

    return 0;
}

int main() {
    HANDLE producers[PRODUCER_COUNT];
    HANDLE consumers[CONSUMER_COUNT];
    DWORD threadId;

    // 동기화 객체 초기화
    InitializeCriticalSection(&bufferCS);
    notEmpty = CreateEvent(NULL, FALSE, FALSE, NULL); // 자동 리셋 이벤트
    notFull = CreateEvent(NULL, FALSE, TRUE, NULL);   // 초기에 버퍼가 비어있으므로 신호 상태

    printf("Producer-Consumer 시뮬레이션 시작\n");
    printf("생산자 %d명, 소비자 %d명, 총 %d개 아이템\n\n",
           PRODUCER_COUNT, CONSUMER_COUNT, ITEMS_TO_PRODUCE);

    // 생산자 스레드 생성
    for (int i = 0; i < PRODUCER_COUNT; i++) {
        producers[i] = CreateThread(NULL, 0, Producer, (LPVOID)i, 0, &threadId);
    }

    // 소비자 스레드 생성
    for (int i = 0; i < CONSUMER_COUNT; i++) {
        consumers[i] = CreateThread(NULL, 0, Consumer, (LPVOID)i, 0, &threadId);
    }

    // 모든 생산자 완료 대기
    WaitForMultipleObjects(PRODUCER_COUNT, producers, TRUE, INFINITE);

    // 모든 소비자 완료 대기
    WaitForMultipleObjects(CONSUMER_COUNT, consumers, TRUE, INFINITE);

    printf("\n시뮬레이션 완료!\n");
    printf("총 생산: %d, 총 소비: %d\n", totalProduced, totalConsumed);

    // 핸들 정리
    for (int i = 0; i < PRODUCER_COUNT; i++) {
        CloseHandle(producers[i]);
    }
    for (int i = 0; i < CONSUMER_COUNT; i++) {
        CloseHandle(consumers[i]);
    }

    CloseHandle(notEmpty);
    CloseHandle(notFull);
    DeleteCriticalSection(&bufferCS);

    return 0;
}
```

### 예제 5: 실제 응용 - 파일 다운로드 매니저

```c
// 여러 파일을 동시에 다운로드하는 간단한 시뮬레이터
#include <windows.h>
#include <stdio.h>
#include <string.h>

#define MAX_FILES 5

typedef struct {
    char filename[256];
    int fileSize;     // KB 단위
    int downloaded;   // 현재 다운로드된 크기
    int threadId;
} DownloadInfo;

CRITICAL_SECTION progressCS;

// 파일 다운로드 시뮬레이션 스레드
DWORD WINAPI DownloadFile(LPVOID lpParam) {
    DownloadInfo* info = (DownloadInfo*)lpParam;
    info->threadId = GetCurrentThreadId();

    printf("[%d] %s 다운로드 시작 (크기: %d KB)\n",
           info->threadId, info->filename, info->fileSize);

    while (info->downloaded < info->fileSize) {
        // 다운로드 속도 시뮬레이션 (50-200 KB/s)
        int chunkSize = rand() % 151 + 50;
        if (info->downloaded + chunkSize > info->fileSize) {
            chunkSize = info->fileSize - info->downloaded;
        }

        Sleep(1000); // 1초마다 청크 다운로드

        EnterCriticalSection(&progressCS);
        info->downloaded += chunkSize;

        int progress = (info->downloaded * 100) / info->fileSize;
        printf("[%d] %s: %d%% (%d/%d KB)\n",
               info->threadId, info->filename, progress,
               info->downloaded, info->fileSize);

        LeaveCriticalSection(&progressCS);
    }

    printf("[%d] %s 다운로드 완료!\n", info->threadId, info->filename);
    return 0;
}

int main() {
    DownloadInfo files[MAX_FILES] = {
        {"document.pdf", 1500, 0, 0},
        {"video.mp4", 25000, 0, 0},
        {"music.mp3", 3500, 0, 0},
        {"image.jpg", 800, 0, 0},
        {"software.zip", 12000, 0, 0}
    };

    HANDLE threads[MAX_FILES];
    DWORD threadId;

    InitializeCriticalSection(&progressCS);

    printf("=== 멀티스레드 다운로드 매니저 ===\n\n");

    DWORD startTime = GetTickCount();

    // 각 파일에 대해 다운로드 스레드 생성
    for (int i = 0; i < MAX_FILES; i++) {
        threads[i] = CreateThread(NULL, 0, DownloadFile, &files[i], 0, &threadId);
        if (threads[i] == NULL) {
            printf("스레드 생성 실패: %s\n", files[i].filename);
        }
    }

    // 모든 다운로드 완료 대기
    WaitForMultipleObjects(MAX_FILES, threads, TRUE, INFINITE);

    DWORD endTime = GetTickCount();

    printf("\n=== 다운로드 완료 ===\n");
    printf("총 소요 시간: %.1f초\n", (endTime - startTime) / 1000.0);

    int totalSize = 0;
    for (int i = 0; i < MAX_FILES; i++) {
        totalSize += files[i].fileSize;
        CloseHandle(threads[i]);
    }

    printf("총 다운로드 크기: %d KB\n", totalSize);
    printf("평균 속도: %.1f KB/s\n", totalSize / ((endTime - startTime) / 1000.0));

    DeleteCriticalSection(&progressCS);

    return 0;
}
```

### 디버깅 팁

1. **메모리 누수 방지**
   ```c
   // 항상 핸들을 닫아야 함
   CloseHandle(hThread);
   CloseHandle(hProcess);
   DeleteCriticalSection(&cs);
   ```

2. **Race Condition 디버깅**
   ```c
   // 의도적으로 Sleep()을 추가하여 타이밍 문제 확인
   Sleep(1);  // Race condition을 더 쉽게 재현
   ```

3. **데드락 방지**
   ```c
   // 항상 같은 순서로 락을 획득
   EnterCriticalSection(&cs1);
   EnterCriticalSection(&cs2);
   // ... 작업 ...
   LeaveCriticalSection(&cs2);
   LeaveCriticalSection(&cs1);
   ```

## 실습 문제별 구현 가이드

### Problem 1: 프로세스 실행기 구현 가이드

**핵심 요구사항:**
1. 사용자로부터 실행할 프로그램 경로 입력받기
2. CreateProcess로 프로세스 생성
3. 프로세스 종료 대기 및 종료 코드 확인
4. 에러 처리 및 핸들 정리

**구현 단계:**
```c
#include <windows.h>
#include <stdio.h>

int main() {
    char programPath[MAX_PATH];
    STARTUPINFO si;
    PROCESS_INFORMATION pi;
    DWORD exitCode;

    // 1단계: 사용자 입력
    printf("실행할 프로그램 경로: ");
    fgets(programPath, sizeof(programPath), stdin);
    // 개행 문자 제거
    programPath[strcspn(programPath, "\n")] = '\0';

    // 2단계: 구조체 초기화
    ZeroMemory(&si, sizeof(si));
    si.cb = sizeof(si);
    ZeroMemory(&pi, sizeof(pi));

    // 3단계: 프로세스 생성
    if (CreateProcess(NULL, programPath, NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi)) {
        printf("프로세스 생성 성공 (PID: %d)\n", pi.dwProcessId);

        // 4단계: 프로세스 종료 대기
        printf("프로세스 종료를 기다리는 중...\n");
        WaitForSingleObject(pi.hProcess, INFINITE);

        // 5단계: 종료 코드 확인
        GetExitCodeProcess(pi.hProcess, &exitCode);
        printf("프로세스가 종료되었습니다. 종료 코드: %d\n", exitCode);

        // 6단계: 핸들 정리
        CloseHandle(pi.hProcess);
        CloseHandle(pi.hThread);
    } else {
        printf("프로세스 생성 실패: %d\n", GetLastError());
    }

    return 0;
}
```

**추가 기능 구현:**
- 여러 프로세스 동시 실행
- 프로세스 강제 종료 기능
- 실행 시간 측정

### Problem 2: 멀티스레드 계산기 구현 가이드

**핵심 요구사항:**
1. 대용량 계산을 여러 스레드로 분할
2. 각 스레드의 결과를 수집하여 합산
3. 성능 비교 (단일 스레드 vs 멀티스레드)

**구현 전략:**
```c
#include <windows.h>
#include <stdio.h>

#define MAX_THREADS 8
#define TOTAL_WORK 10000000

typedef struct {
    int threadId;
    int startIndex;
    int endIndex;
    long long result;
} ThreadData;

DWORD WINAPI WorkerThread(LPVOID lpParam) {
    ThreadData* data = (ThreadData*)lpParam;
    data->result = 0;

    // 계산 작업 수행 (예: 제곱의 합)
    for (int i = data->startIndex; i <= data->endIndex; i++) {
        data->result += (long long)i * i;
    }

    printf("스레드 %d: %d~%d 구간 완료 (결과: %lld)\n",
           data->threadId, data->startIndex, data->endIndex, data->result);

    return 0;
}

int main() {
    int threadCount;
    printf("사용할 스레드 수 (1-%d): ", MAX_THREADS);
    scanf("%d", &threadCount);

    if (threadCount < 1 || threadCount > MAX_THREADS) {
        threadCount = 4; // 기본값
    }

    HANDLE threads[MAX_THREADS];
    ThreadData threadData[MAX_THREADS];
    int workPerThread = TOTAL_WORK / threadCount;

    DWORD startTime = GetTickCount();

    // 스레드 생성 및 작업 분할
    for (int i = 0; i < threadCount; i++) {
        threadData[i].threadId = i;
        threadData[i].startIndex = i * workPerThread + 1;
        threadData[i].endIndex = (i == threadCount - 1) ? TOTAL_WORK : (i + 1) * workPerThread;

        threads[i] = CreateThread(NULL, 0, WorkerThread, &threadData[i], 0, NULL);
    }

    // 모든 스레드 완료 대기
    WaitForMultipleObjects(threadCount, threads, TRUE, INFINITE);

    // 결과 합산
    long long totalResult = 0;
    for (int i = 0; i < threadCount; i++) {
        totalResult += threadData[i].result;
        CloseHandle(threads[i]);
    }

    DWORD endTime = GetTickCount();

    printf("최종 결과: %lld\n", totalResult);
    printf("실행 시간: %d ms\n", endTime - startTime);
    printf("사용한 스레드 수: %d\n", threadCount);

    return 0;
}
```

### Problem 3: Producer-Consumer (생산자-소비자) 시뮬레이터 구현 가이드

**핵심 요구사항:**
1. 순환 버퍼 (Circular Buffer) 구현
2. Critical Section (임계 영역)을 이용한 동기화
3. 생산자 (Producer)와 소비자 (Consumer) 스레드 구현

**핵심 구조체:**
```c
typedef struct {
    int* buffer;
    int size;
    int head;
    int tail;
    int count;
    CRITICAL_SECTION cs;
} CircularBuffer;

typedef struct {
    int id;
    CircularBuffer* buffer;
    int itemsToProduce;  // 생산자용
    int* totalProduced;  // 공유 카운터
    int* totalConsumed;  // 공유 카운터
} ThreadParam;
```

**버퍼 관리 함수:**
```c
void InitBuffer(CircularBuffer* buffer, int size) {
    buffer->buffer = (int*)malloc(size * sizeof(int));
    buffer->size = size;
    buffer->head = 0;
    buffer->tail = 0;
    buffer->count = 0;
    InitializeCriticalSection(&buffer->cs);
}

BOOL PutItem(CircularBuffer* buffer, int item) {
    EnterCriticalSection(&buffer->cs);

    if (buffer->count >= buffer->size) {
        LeaveCriticalSection(&buffer->cs);
        return FALSE; // 버퍼 가득 참
    }

    buffer->buffer[buffer->head] = item;
    buffer->head = (buffer->head + 1) % buffer->size;
    buffer->count++;

    LeaveCriticalSection(&buffer->cs);
    return TRUE;
}

BOOL GetItem(CircularBuffer* buffer, int* item) {
    EnterCriticalSection(&buffer->cs);

    if (buffer->count <= 0) {
        LeaveCriticalSection(&buffer->cs);
        return FALSE; // 버퍼 비어 있음
    }

    *item = buffer->buffer[buffer->tail];
    buffer->tail = (buffer->tail + 1) % buffer->size;
    buffer->count--;

    LeaveCriticalSection(&buffer->cs);
    return TRUE;
}
```

### Problem 4: 프로세스 모니터 구현 가이드

**핵심 요구사항:**
1. Toolhelp API (Tool Help Application Programming Interface)를 사용한 프로세스 정보 수집
2. 실시간 모니터링 (주기적 업데이트)
3. 사용자 인터페이스 (UI: User Interface, 콘솔 기반)

**프로세스 정보 수집 함수:**
```c
#include <tlhelp32.h>
#include <psapi.h>

typedef struct {
    DWORD processId;
    char processName[MAX_PATH];
    DWORD threadCount;
    DWORD parentProcessId;
    SIZE_T workingSetSize; // 메모리 사용량
} ProcessInfo;

int CollectProcessInfo(ProcessInfo** processes) {
    HANDLE hSnapshot;
    PROCESSENTRY32 pe32;
    int count = 0;
    int capacity = 1000;

    *processes = (ProcessInfo*)malloc(capacity * sizeof(ProcessInfo));

    hSnapshot = CreateToolhelp32Snapshot(TH32CS_SNAPPROCESS, 0);
    if (hSnapshot == INVALID_HANDLE_VALUE) {
        return 0;
    }

    pe32.dwSize = sizeof(PROCESSENTRY32);

    if (Process32First(hSnapshot, &pe32)) {
        do {
            if (count >= capacity) {
                capacity *= 2;
                *processes = (ProcessInfo*)realloc(*processes, capacity * sizeof(ProcessInfo));
            }

            (*processes)[count].processId = pe32.th32ProcessID;
            strcpy((*processes)[count].processName, pe32.szExeFile);
            (*processes)[count].threadCount = pe32.cntThreads;
            (*processes)[count].parentProcessId = pe32.th32ParentProcessID;

            // 메모리 사용량 가져오기
            HANDLE hProcess = OpenProcess(PROCESS_QUERY_INFORMATION | PROCESS_VM_READ, FALSE, pe32.th32ProcessID);
            if (hProcess) {
                PROCESS_MEMORY_COUNTERS pmc;
                if (GetProcessMemoryInfo(hProcess, &pmc, sizeof(pmc))) {
                    (*processes)[count].workingSetSize = pmc.WorkingSetSize;
                } else {
                    (*processes)[count].workingSetSize = 0;
                }
                CloseHandle(hProcess);
            } else {
                (*processes)[count].workingSetSize = 0;
            }

            count++;
        } while (Process32Next(hSnapshot, &pe32));
    }

    CloseHandle(hSnapshot);
    return count;
}
```

**모니터링 메인 루프:**
```c
void DisplayProcesses(ProcessInfo* processes, int count) {
    system("cls"); // 화면 지우기

    printf("=== 프로세스 모니터 ===\n");
    printf("총 %d개의 프로세스\n\n", count);
    printf("%-8s %-30s %-8s %-10s %-12s\n", "PID", "프로세스명", "스레드", "부모PID", "메모리(KB)");
    printf("========================================================================\n");

    for (int i = 0; i < count && i < 30; i++) { // 상위 30개만 표시
        printf("%-8d %-30s %-8d %-10d %-12d\n",
               processes[i].processId,
               processes[i].processName,
               processes[i].threadCount,
               processes[i].parentProcessId,
               (int)(processes[i].workingSetSize / 1024));
    }

    printf("\n[ESC] 종료, [R] 새로고침 (Refresh)\n");
}
```

### Problem 5: 병렬 파일 처리기 구현 가이드

**핵심 요구사항:**
1. 여러 파일을 병렬로 처리
2. 작업 큐 관리
3. 진행률 표시

**작업 큐 구조:**
```c
typedef struct {
    char filePath[MAX_PATH];
    DWORD fileSize;
    int processed; // 0: 대기, 1: 처리중, 2: 완료
} FileTask;

typedef struct {
    FileTask* tasks;
    int taskCount;
    int currentIndex;
    CRITICAL_SECTION cs;
} TaskQueue;

// 다음 작업 가져오기
BOOL GetNextTask(TaskQueue* queue, FileTask** task) {
    EnterCriticalSection(&queue->cs);

    while (queue->currentIndex < queue->taskCount) {
        if (queue->tasks[queue->currentIndex].processed == 0) {
            *task = &queue->tasks[queue->currentIndex];
            queue->tasks[queue->currentIndex].processed = 1; // 처리중 표시
            queue->currentIndex++;
            LeaveCriticalSection(&queue->cs);
            return TRUE;
        }
        queue->currentIndex++;
    }

    LeaveCriticalSection(&queue->cs);
    return FALSE; // 더 이상 작업 없음
}
```

**파일 처리 스레드:**
```c
typedef struct {
    int threadId;
    TaskQueue* queue;
    int* completedCount;
    CRITICAL_SECTION* progressCS;
} WorkerParam;

DWORD WINAPI FileWorkerThread(LPVOID lpParam) {
    WorkerParam* param = (WorkerParam*)lpParam;
    FileTask* task;

    while (GetNextTask(param->queue, &task)) {
        printf("[스레드 %d] 처리 시작: %s\n", param->threadId, task->filePath);

        // 파일 처리 시뮬레이션 (실제로는 파일 읽기/쓰기/변환 등)
        HANDLE hFile = CreateFile(task->filePath, GENERIC_READ, FILE_SHARE_READ,
                                  NULL, OPEN_EXISTING, FILE_ATTRIBUTE_NORMAL, NULL);

        if (hFile != INVALID_HANDLE_VALUE) {
            char buffer[4096];
            DWORD bytesRead;
            DWORD totalRead = 0;

            // 파일을 청크 단위로 읽으며 진행률 표시
            while (ReadFile(hFile, buffer, sizeof(buffer), &bytesRead, NULL) && bytesRead > 0) {
                totalRead += bytesRead;

                // 처리 시뮬레이션 (약간의 지연)
                Sleep(10);

                // 진행률 업데이트
                EnterCriticalSection(param->progressCS);
                int progress = (totalRead * 100) / task->fileSize;
                printf("[스레드 %d] %s: %d%% (Progress)\n", param->threadId, task->filePath, progress);
                LeaveCriticalSection(param->progressCS);
            }

            CloseHandle(hFile);
            task->processed = 2; // 완료 표시

            EnterCriticalSection(param->progressCS);
            (*param->completedCount)++;
            LeaveCriticalSection(param->progressCS);

            printf("[스레드 %d] 완료: %s\n", param->threadId, task->filePath);
        } else {
            printf("[스레드 %d] 파일 열기 실패: %s\n", param->threadId, task->filePath);
            task->processed = 2; // 실패로 완료 처리
        }
    }

    return 0;
}
```

**디렉터리 검색 및 파일 목록 생성:**
```c
int BuildFileList(const char* directory, const char* pattern, TaskQueue* queue) {
    WIN32_FIND_DATA findData;
    HANDLE hFind;
    char searchPath[MAX_PATH];
    int count = 0;
    int capacity = 1000;

    queue->tasks = (FileTask*)malloc(capacity * sizeof(FileTask));

    sprintf(searchPath, "%s\\%s", directory, pattern);
    hFind = FindFirstFile(searchPath, &findData);

    if (hFind != INVALID_HANDLE_VALUE) {
        do {
            if (!(findData.dwFileAttributes & FILE_ATTRIBUTE_DIRECTORY)) {
                if (count >= capacity) {
                    capacity *= 2;
                    queue->tasks = (FileTask*)realloc(queue->tasks, capacity * sizeof(FileTask));
                }

                sprintf(queue->tasks[count].filePath, "%s\\%s", directory, findData.cFileName);
                queue->tasks[count].fileSize = findData.nFileSizeLow;
                queue->tasks[count].processed = 0;
                count++;
            }
        } while (FindNextFile(hFind, &findData));

        FindClose(hFind);
    }

    queue->taskCount = count;
    queue->currentIndex = 0;
    InitializeCriticalSection(&queue->cs);

    return count;
}
```

## 공통 구현 팁 (Implementation Tips)

### 1. 에러 처리 패턴 (Error Handling Pattern)
```c
#define CHECK_HANDLE(h, msg) \
    if ((h) == NULL || (h) == INVALID_HANDLE_VALUE) { \
        printf("에러: %s (코드: %d)\n", (msg), GetLastError()); \
        goto cleanup; \
    }

// 사용 예
HANDLE hThread = CreateThread(NULL, 0, ThreadFunc, NULL, 0, NULL);
CHECK_HANDLE(hThread, "스레드 생성 실패");
```

### 2. 리소스 정리 매크로 (Resource Cleanup Macros)
```c
#define SAFE_CLOSE_HANDLE(h) \
    if ((h) != NULL && (h) != INVALID_HANDLE_VALUE) { \
        CloseHandle(h); \
        (h) = NULL; \
    }

#define SAFE_DELETE_CS(cs) \
    DeleteCriticalSection(&(cs))
```

### 3. 성능 측정 코드 (Performance Measurement Code)
```c
typedef struct {
    LARGE_INTEGER frequency;
    LARGE_INTEGER start;
    LARGE_INTEGER end;
} PerformanceTimer;

void StartTimer(PerformanceTimer* timer) {
    QueryPerformanceFrequency(&timer->frequency);
    QueryPerformanceCounter(&timer->start);
}

double EndTimer(PerformanceTimer* timer) {
    QueryPerformanceCounter(&timer->end);
    return (double)(timer->end.QuadPart - timer->start.QuadPart) / timer->frequency.QuadPart;
}
```

## 실습 환경
- Windows 운영체제 (Operating System)
- Visual Studio Community Edition (IDE: Integrated Development Environment)
- C 언어 (C Programming Language)

## 주요 약어 정리

### 시스템 관련
- **OS**: Operating System (운영체제)
- **API**: Application Programming Interface (응용프로그램 프로그래밍 인터페이스)
- **CPU**: Central Processing Unit (중앙처리장치)
- **RAM**: Random Access Memory (임의접근메모리)
- **I/O**: Input/Output (입출력)
- **UI**: User Interface (사용자 인터페이스)
- **GUI**: Graphical User Interface (그래픽 사용자 인터페이스)

### 프로세스/스레드 관련
- **PID**: Process Identifier (프로세스 식별자)
- **TID**: Thread Identifier (스레드 식별자)
- **PCB**: Process Control Block (프로세스 제어 블록)
- **TCB**: Thread Control Block (스레드 제어 블록)
- **IPC**: Inter-Process Communication (프로세스 간 통신)

### 동기화 관련
- **CS**: Critical Section (임계 영역)
- **RC**: Race Condition (경쟁 상태)
- **FIFO**: First In, First Out (선입선출)
- **LIFO**: Last In, First Out (후입선출)

### 메모리 관련
- **VM**: Virtual Memory (가상 메모리)
- **MMU**: Memory Management Unit (메모리 관리 장치)
- **TLB**: Translation Lookaside Buffer (변환 색인 버퍼)

### 개발 관련
- **IDE**: Integrated Development Environment (통합 개발 환경)
- **SDK**: Software Development Kit (소프트웨어 개발 키트)
- **DLL**: Dynamic Link Library (동적 링크 라이브러리)
- **EXE**: Executable (실행 파일)

## 학습 목표
- CreateProcess를 통한 프로세스 생성과 제어
- CreateThread를 통한 멀티스레드 프로그래밍
- Process/Thread 동기화와 통신
- 성능을 고려한 멀티스레드 설계

## 실습 문제 목록

### 문제 1: 프로세스 실행기 (problem1.c)
CreateProcess를 사용하여 다양한 프로그램을 실행하고 제어하는 프로그램
- 프로세스 생성과 대기
- 종료 코드 확인
- 에러 처리

### 문제 2: 멀티스레드 계산기 (problem2.c)
여러 스레드로 대용량 계산을 분할 처리하는 프로그램
- CreateThread 사용법
- 스레드 간 작업 분할
- 결과 수집 및 합계

### 문제 3: Producer-Consumer 시뮬레이터 (problem3.c)
생산자-소비자 패턴을 구현하는 멀티스레드 프로그램
- Critical Section 사용
- 버퍼 관리
- 스레드 간 동기화

### 문제 4: 프로세스 모니터 (problem4.c)
시스템의 프로세스 정보를 실시간으로 모니터링하는 프로그램
- Toolhelp API 사용
- 프로세스 정보 수집
- 사용자 인터페이스

### 문제 5: 병렬 파일 처리기 (problem5.c)
여러 파일을 병렬로 처리하는 멀티스레드 프로그램
- 파일 I/O와 스레드
- 작업 큐 관리
- 진행률 표시

## 실행 방법
각 문제별로 Visual Studio에서 새 프로젝트를 만들고 해당 .c 파일을 추가하여 빌드하세요.

## 주의사항
- 모든 핸들은 사용 후 반드시 CloseHandle()로 닫으세요
- 스레드 생성 시 메모리 누수에 주의하세요
- Critical Section은 반드시 초기화/삭제하세요