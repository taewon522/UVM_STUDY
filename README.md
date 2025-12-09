# UVM_STUDY
# 🏗️ UVM Testbench 계층 구조도

      +---------------------------------------------------+
      |                     Test (uvm_test)               |
      |   - 시퀀스 실행, 환경 설정, run_test() 호출        |
      +---------------------------------------------------+
                           |
                           v
      +---------------------------------------------------+
      |                Environment (uvm_env)              |
      |   - 여러 Agent 관리, Scoreboard, Coverage 포함     |
      +---------------------------------------------------+
                           |
               -----------------------------
               |                           |
               v                           v
      +-------------------+       +-------------------+
      |   Agent (uvm_agent)       |   Agent (uvm_agent) ...
      |   - Driver                |   - Driver
      |   - Sequencer             |   - Sequencer
      |   - Monitor               |   - Monitor
      +-------------------+       +-------------------+
               |
               v
      +---------------------------------------------------+
      | DUT (Design Under Test)                           |
      |   - 실제 설계 블록                                |
      +---------------------------------------------------+
# 📘 UVM Testbench 기본 용어 & 메서드


## 1. Phase 관련 메서드
UVM은 시뮬레이션을 여러 단계(phase)로 나눠서 실행합니다.

build_phase()

컴포넌트 생성 단계 (env, agent, driver, monitor 등 인스턴스화)

connect_phase()

포트(TLM port, analysis port 등) 연결 단계

end_of_elaboration_phase()

구조가 완성된 후 최종 점검 단계

run_phase()

실제 시뮬레이션 실행 단계 (driver 동작, monitor 관찰 등)

report_phase()

시뮬레이션 결과 보고 단계

## 2. Component 계층
uvm_test : 전체 테스트를 정의하는 최상위 클래스

uvm_env : 여러 agent와 scoreboard, coverage를 포함하는 환경

uvm_agent : 특정 인터페이스를 담당하는 단위 (driver, sequencer, monitor 포함)

uvm_driver : transaction을 DUT 신호로 변환해 구동

uvm_sequencer : sequence와 driver를 연결하는 중재자

uvm_monitor : DUT 신호를 관찰해 transaction으로 변환

uvm_scoreboard : 기대값과 실제 DUT 결과를 비교

## 3. Sequence 관련
uvm_sequence : stimulus(입력 데이터)를 생성하는 객체

start_item() / finish_item() : transaction을 생성하고 sequencer에 전달하는 메서드

uvm_sequence_item : transaction을 정의하는 클래스

## 4. TLM 통신 관련
TLM Port 메서드

put() : 데이터를 전달

get() : 데이터를 가져옴

peek() : 데이터를 확인만 함

transport() : 요청-응답 형태로 데이터 교환

Analysis Port 메서드

write() : 데이터를 단방향으로 broadcast (monitor → scoreboard/coverage)

## 5. Configuration & Factory
uvm_config_db : 컴포넌트 간 설정값 전달 (예: virtual interface 연결)

Factory Override : 기본 클래스를 다른 클래스로 교체해 재사용성 확보

## 6. Objection 메커니즘
raise_objection() / drop_objection()

시뮬레이션 종료를 제어하는 메서드

모든 objection이 drop되면 run_phase가 종료됨

📌 핵심 요약
Phase 메서드: build, connect, run, report → 시뮬레이션 흐름 제어

Component 계층: test → env → agent → driver/sequencer/monitor → DUT

Sequence: stimulus 생성, sequencer와 driver를 통해 DUT에 전달

TLM/Analysis Port: 컴포넌트 간 transaction 전달 (양방향 vs 단방향)

Config/Factory: 설정 공유와 클래스 재사용

Objection: 시뮬레이션 종료 제어
              
# 📘 TLM Port vs Analysis Port
<img width="788" height="334" alt="image" src="https://github.com/user-attachments/assets/28af4ac4-1268-43b4-9f02-343a612256bd" />
