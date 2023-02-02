# cosmic-baseball-init

야구는 전통적으로 많은 사람들에게 사랑받는 스포츠입니다.  
그러던 어느 날 외계인들이 야구에 흥미를 가졌고, 몇 가지 룰이 수정된 **우주 야구**가 탄생했습니다!

당신은 API 서버를 개발해야되는 백엔드 개발자 입니다.  
일반 야구를 구현하고 (레거시 코드) 외계인들이 원하는 우주 야구를 수정 개발 해야 됩니다. (업무 요건 변경)

## Background

- spring boot 2.x, java 11, gradle, mysql
- 명시되지 않은 내용은 보편적인 야구의 룰을 따릅니다.
- domain 로직과 oop 에 집중해주세요.

### Mandatory

- 우주 야구 구현은 반드시 일반 야구 구현이 선행되어야 합니다.
    - ex) 일반 야구 lv 1 > 우주 야구 lv 1 > 일반 야구 lv 2 > 우주 야구 lv 2 > ...
- 각 level 별로 사전 정의된 테스트를 만족하고 (또는 그것에 준하는) pr 에 첨부되어야 합니다.
- db 활용은 level 4 부터 허용됩니다.

### Recommend

- level 에서 요구하는 최소 조건만을 만족시키기를 추천합니다.
- 여러 개의 작은 pr 로 하나의 level 을 구현하기를 추천합니다.
- 영속성 레이어의 역할은 단순화해도 좋습니다.
    - spring data jpa 를 권장합니다. native query, mybatis, querydsl 은 권장하지 않습니다.

## Requirement

> 타격 결과, 타석 결과의 샘플 + 테스트 코드의 일부가 구현되어 있습니다.

- 플레이어는 타자로만 플레이 합니다.
- 1개의 게임은 1개의 타석으로 구성 됩니다.

| Level | 일반 야구                                                                                     | 우주 야구                                                                                              |
|-------|-------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| 1     | 타격 결과 구현<br/>- 타격 결과는 스트라이크, 볼, 안타 입니다.<br/>- 타격 결과는 모두 같은 확률을 가집니다.                      | 타격 결과 추가<br/> - 스트라이크 카운트가 2개 증가되는 `double strike`<br/> - 볼 카운트가 2개 증가되는 `double ball`             |
| 2     | 타석 결과 구현<br/>- 타석 결과는 타격 결과들로 인해 결정됩니다.<br/>- 타석 결과는 진루, 아웃 입니다.<br/>- 진루는 안타와 포볼로 결정됩니다. | 타격 결과 추가<br/>- 즉시 아웃되는 `bullseye strike` (strike 의 20%)<br/>- 즉시 출루되는 `bullseye ball` (ball 의 20%) |
| 3     | - 게임 생성시 타격 결과의 확률을 입력 받습니다.<br/>- 1개의 게임은 1개의 회로 구성됩니다.<br/>- 전광판으로 진루/득점을 표현합니다.        | - 게임 생성시 `fever inning` 을 입력 받습니다. (안타 확률 2배)<br/>- 3루가 없어집니다.                                     |
| 4     | - 1개의 게임은 3개의 회로 구성됩니다.                                                                   | - `fever inning` 의 영향이 추가됩니다. (득점 2배)<br/>- 마지막 회는 5 아웃시 종료됩니다.                                    |
| 5     |                                                                                           | - `fever inning` 이 제거 됩니다.<br/>- 타격 결과의 확률이 회마다 바뀝니다. (홀수: 항상 같음, 짝수: 입력받은 확률)                     |
| 6     | - 애플리케이션 재기동 후에도 게임을 재개할 수 있습니다.<br/>- 동시에 여러 개의 게임을 진행할 수 있습니다.                          | - idea 없이 독립적인 실행이 가능합니다.                                                                          |
