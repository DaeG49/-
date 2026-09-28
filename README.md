# 프로젝트 이름
통화 차단 상황 시뮬레이터

## 프로젝트 소개

전화번호가 차단되었을 때 발신자가 차단 여부를 쉽게 인지하는 상황에 관심이 생겨 시작한 프로젝트입니다.

기존 통화 차단 상황에서는 통화음이 짧게 들린 후 음성사서함으로 연결되는 등 발신자가 상대방의 차단 여부를 명확하게 판단하기 어려운 상황이 발생할 수 있습니다.

본 프로젝트에서는 실제 통신망이나 스마트폰의 전화 기능을 직접 변경하지 않고, 가상의 통화 환경을 프로그램으로 구현하여 다양한 통화 연결 상황을 시뮬레이션하고자 합니다.

특히 일정 횟수 동안 통화음을 반복한 후 음성사서함으로 연결되는 등의 상황을 구현하고, 일반적인 미응답 상황과 차단 상황을 비교하여 사용자 인식 차이를 확인하는 것을 목표로 합니다.

---

## 개발 환경

- 개발 언어: Python
- GUI: Tkinter
- 개발 프로그램: Visual Studio Code
- 실행 환경: Windows

---

## 예상 사용자

- 차단 사실을 비대면으로 명확히 전달하고 싶은 수신자
- 전화 통화 기반 고객지원 및 영업 관리자

---

## 구현할 기능

- [x] 통화 시뮬레이션 기본 화면 구현
- [x] 통화 시작 및 종료 기능 구현
- [x] 통화음 반복 재생 기능
- [x] 통화음 재생 횟수 설정
- [ ] 일반적인 미응답 상황 시뮬레이션
- [ ] 차단 상황 시뮬레이션
- [x] 음성사서함 연결 상황 시뮬레이션
- [x] 통화 상태별 화면 구성
- [ ] 실행 화면 추가하기


---

## 참고한 서비스나 프로젝트

- [통신사별 수신 차단 및 음성사서함 전환 동작 원리 분석](https://chickyustory.tistory.com/entry/%EC%A0%84%ED%99%94-%EC%97%B0%EA%B2%B0%EC%9D%B4-%EB%90%98%EC%A7%80-%EC%95%8A%EC%95%84-%EC%9D%8C%EC%84%B1%EC%82%AC%EC%84%9C%ED%95%A8%EC%9C%BC%EB%A1%9C-%EB%84%98%EC%96%B4%EA%B0%80%EB%8A%94-%EC%9D%B4%EC%9C%A0%EC%99%80-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%EC%B4%9D%EC%A0%95%EB%A6%AC)
- 이동통신 단말기 기본 전화 앱의 통화 연결/차단 UX 흐름 가이드

---

### 실행 화면

> 여기에 현재 프로그램 실행 화면 사진을 추가할 예정입니다.
- 2주차
<img width="150" height="199" alt="image" src="https://github.com/user-attachments/assets/151a1acf-9994-4eee-a804-bf3536e0808d" />

- 3주차

---


## 현재 구현된 화면

현재 Python의 Tkinter를 이용하여 통화 상황 시뮬레이터의 기본 화면과 통화 기능을 구현했습니다.

현재 화면에는 다음과 같은 기능이 포함되어 있습니다.

- 통화 상황 시뮬레이터 제목 표시
- 상대방 전화번호 표시
- 현재 통화 상태 표시
- 통화 시작 및 종료 버튼
- 통화음 횟수 설정
- 현재 통화음 횟수 표시
- 설정한 횟수 이후 음성사서함 연결 상태 표시

## 2~3주차 구현코드
```

import tkinter as tk
import winsound

# 통화 중인지 확인
is_calling = False

# 현재 통화음 횟수
ring_count = 0

# 예약된 작업 저장
after_id = None


# 통화음 반복 함수
def play_ring():
    global ring_count
    global after_id

    if is_calling:
        ring_count += 1

        status.config(text=f"통화음 {ring_count}회")

        winsound.PlaySound(
            "ring.wav",
            winsound.SND_FILENAME | winsound.SND_ASYNC
        )

        # 사용자가 설정한 최대 통화음 횟수
        max_ring = int(ring_setting.get())

        if ring_count >= max_ring:
            after_id = window.after(6000, connect_voicemail)
        else:
            after_id = window.after(6000, play_ring)


# 음성사서함 연결 함수
def connect_voicemail():
    global is_calling
    global after_id

    if not is_calling:
        return

    is_calling = False
    after_id = None

    winsound.PlaySound(None, winsound.SND_PURGE)

    status.config(text="음성사서함 연결")
    call_button.config(text="통화 시작", command=start_call)

    ring_setting.config(state="normal")


# 통화 시작 함수
def start_call():
    global is_calling
    global ring_count

    is_calling = True
    ring_count = 0

    status.config(text="통화 연결 중")
    call_button.config(text="통화 종료", command=end_call)

    ring_setting.config(state="disabled")

    play_ring()


# 통화 종료 함수
def end_call():
    global is_calling
    global after_id

    is_calling = False

    if after_id is not None:
        window.after_cancel(after_id)
        after_id = None

    winsound.PlaySound(None, winsound.SND_PURGE)

    status.config(text="통화 종료")
    call_button.config(text="통화 시작", command=start_call)

    ring_setting.config(state="normal")


# 프로그램 창 만들기
window = tk.Tk()
window.title("통화 상황 시뮬레이터")
window.geometry("400x550")

# 제목
title = tk.Label(
    window,
    text="통화 상황 시뮬레이터",
    font=("맑은 고딕", 20, "bold")
)
title.pack(pady=30)

# 상대방 번호
number = tk.Label(
    window,
    text="010-XXXX-XXXX",
    font=("맑은 고딕", 14)
)
number.pack(pady=10)

# 통화음 횟수 설명
ring_label = tk.Label(
    window,
    text="통화음 횟수 설정",
    font=("맑은 고딕", 12)
)
ring_label.pack(pady=(20, 5))

# 통화음 횟수 선택
ring_setting = tk.Spinbox(
    window,
    from_=1,
    to=10,
    width=5,
    font=("맑은 고딕", 14),
    justify="center"
)
ring_setting.delete(0, "end")
ring_setting.insert(0, "3")
ring_setting.pack()

# 현재 상태
status = tk.Label(
    window,
    text="통화 대기",
    font=("맑은 고딕", 16)
)
status.pack(pady=40)

# 통화 시작 버튼
call_button = tk.Button(
    window,
    text="통화 시작",
    font=("맑은 고딕", 14),
    width=15,
    command=start_call
)
call_button.pack(pady=10)

# 프로그램 실행
window.mainloop()
```

## 현재 진행상태

```

1주차 완료: 프로젝트 주제 및 방향성 확정
(기술적/법적 한계로 인해 가상 시뮬레이터 방식으로 개발 방향 결정)

2주차 완료: Python과 Tkinter를 이용한 통화 시뮬레이터 기본 화면 구현

3주차 진행:
통화 시작 및 종료 기능 구현
통화음 WAV 파일 재생 기능 구현
통화음 반복 재생 기능 구현
통화음 횟수 표시 기능 구현
통화음 횟수 설정 기능 구현
설정한 횟수 이후 음성사서함 연결 기능 구현

진행 예정: 일반적인 미응답 상황과 차단 상황을 나누어 시뮬레이션하고 두 상황을 비교할 수 있도록 기능을 추가할 예정

```

## 주차별 기록

### 1주차

- 이번 주에 한 일: 프로젝트 주제 선정, 프로젝트 방향성 구성
- 새롭게 알게 된 것: 실제 통화기능을 변경하거나 통신망 동작을 조작하는 것은 기술적, 법적 검토가 필요한걸 알았음. 그래서 시뮬레이션으로 구현하는 방향을 정함 
- 어려웠던 점: 실제 통화 기능변경, 통신망 동작을 조작을 하고싶었으나 기술적,법적검토가 필요하다는 점이 어려웠음
- 다음 주에 할 일: 전화 통화가 연결되고 종료되는 과정/ 통화음, 음성사서함 등의 기본적인 동작을 조사, 이를 시뮬레이션하기 위해 필요한 개발 환경과 기술을 알아볼 예정

### 2주차
- 이번 주에 한 일: Python과 Tkinter를 이용해 통화 상황 시뮬레이터의 기본 화면을 구현함.
- 새롭게 알게 된 것: Tkinter를 이용해 프로그램 창, 글자, 버튼 등의 GUI를 만들 수 있다는 것을 배움.
- 어려웠던 점: GUI 코드의 구조와 각 기능의 역할을 이해하는 것이 어려웠음.
- 다음 주에 할 일: 통화 시작 버튼에 기능을 연결하고 통화음 재생 기능을 구현할 예정.

### 3주차
- 이번 주에 한 일: 통화 시작 및 종료 기능, 통화음 재생 및 반복 기능, 통화음 횟수 설정 기능, 음성사서함 연결 기능을 구현함.
- 새롭게 알게 된 것: winsound를 이용하면 WAV 파일을 재생할 수 있고, SND_ASYNC를 사용하면 프로그램이 멈추지 않고 통화음을 재생할 수 있다는 것을 알게 됨. 또한 Tkinter의 after() 함수를 이용해 일정 시간이 지난 뒤 함수를 다시 실행할 수 있다는 것을 배움.
- 어려웠던 점: 처음에는 통화음을 재생할 때 프로그램이 잠시 멈추는 문제가 있었고, 통화 종료 시 예약되어 있던 반복 작업까지 중단하는 부분이 어려웠음.
- 현재 진행상태: 통화 시작 및 종료, 통화음 반복, 통화음 횟수 표시 및 설정, 설정된 횟수 이후 음성사서함 연결 기능까지 구현함.
- 다음 주에 할 일: 일반 미응답 상황과 차단 상황을 선택할 수 있도록 만들고 두 상황을 비교할 수 있는 기능을 구현할 예정.
