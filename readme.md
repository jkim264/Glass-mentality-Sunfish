# 유리멘탈 복치
  Achro-em  보드를 활용한 개복치 게임(임베디드시스템 팀프로젝트)
  
# 팀
2018152010 김지안  
2018152011 김채리(팀장)  
2016156021 이기웅  

# 개발목표 및 요구사항 분석
## 알
* 게임 시작 버튼을 누르면 알 그림이 뜨고 온도는 7°C, 용존산소량은 120mg/L에서 시작
* 알 단계에서는 하나의 사망 이유가 존재
* Push Switch 버튼으로 1°C 온도 조절이 가능
* 온도를 3초마다 검사해서 적정 온도가 아닐 시에 LED를 하나씩 키고 8개가 다 켜지면 알에서 부화하지 못해서 사망
* 10~20°C 사이를 30초 동안 유지하면 알에서 부화해서 유년기로 진화

## 유년기
* 유년기에는 세 가지의 사망 이유가 존재
* 적정 수온 20-30°C, 적정 용존산소량 70-150mg/L를 유지해야 스트레스 지수가 상승하지 않음
* 수온과 용존산소량 모두 20초마다 랜덤함수로 값을 받아오되 받아오는 값의 범위는 수온은 10-40°C, 용존산소량은 20-150mg/L
* Push Switch 버튼으로 온도는 1°C, 용존산소량은 10mg/L씩 조절 가능
* LED로 스트레스 지수 설정하고 온도와 용존산소량을 20초마다 검사해서 적정 온도나 적정 용존산소량이 아닐 시에 LED를 하나씩 키게 됨

## 노년기
* 특정 Push Switch 버튼을 클릭하면 개복치를 만질 수 있는데 만질 때마다 스트레스 지수가 상승하여 LED가 하나씩 켜짐
* LED 8개가 다 켜지면 스트레스로 인해 사망
* Push Switch에서 플래시로 지정된 버튼을 누르면 플래시 때문에 놀라서 사망
* 햇빛에 관한 Dot Matrix의 값 3개를 미리 지정해두고 1분마다 랜덤함수로 받아옴
* 햇빛은 Dot Matrix에 3가지 모양으로 출력되며 가장 강한 햇빛이 비춰지면 햇빛이 너무 강해서 사망
* 아무일 없이 2분이 지나면 노년기로 진화

# 시스템 수행 시나리오
## 알
<img src=https://user-images.githubusercontent.com/74261590/101538488-89a9db80-39e0-11eb-8d0c-b76f28d3101e.png>

## 유년기1
<img src=https://user-images.githubusercontent.com/74261590/101538924-194f8a00-39e1-11eb-8e10-b0b19426d1dd.png>

## 유년기2
<img src=https://user-images.githubusercontent.com/74261590/101539053-41d78400-39e1-11eb-869e-6a6150c586d8.png>

## 노년기
<img src=https://user-images.githubusercontent.com/74261590/101539120-57e54480-39e1-11eb-9427-0cf46fa2a79f.png>

# 전체 시스템 소프트웨어 구성도
<img src=https://user-images.githubusercontent.com/74261590/101615614-aa5e4980-3a51-11eb-8184-4fddb0dc44a7.png>

# 구현 상의 제약사항
* 유년기 단계의 개복치부터는 3개의 스레드가, 노년기 단계에서는 4개의 스레드가 동시에 돌아감
* 3개의 스레드 중 하나라도 종료가 되면 나머지 2개의 스레드가 동기화되어 같이 종료되어야 함
* 현재 코드에서는 pthread_join함수를 호출해서 각 스레드마다 값을 반환해주며 종료
* 유년기와 노년기에서 돌아가는 스레드들 모두 pthread_join함수를 이용하여 값을 반환하기 때문에 값을 다 반환할 때까지 스레드가 종료되지 않음
* 모든 스레드들을 동시에 조작하기 때문에 어떤 스레드가 먼저 가장 값을 반환하는지 알 수 없고 join함수 의 순서를 임의로 조정하면 프로그램 오류가 발생
* 기간 내에 문제를 해결 하지못하여 유년기에서는 햇빛 스레드, 노년기에서는 물살 스레드를 뺌 
 
