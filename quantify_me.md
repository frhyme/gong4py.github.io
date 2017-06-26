
# Quantify myself
seunghoon lee

#### data set
    - 데이터 수집기간: 2013.12.30~
    - (날짜, 식사분류(점심, 저녁, 술_맥주 등), 만난 사람)
    - 다만, '식사분류'의 기준에 균일적이지 못한 부분이 있고, 사람이름에도 문제가 있어서 데이터 전처리가 필요함
        - 예를 들어, 처음에는 '야식(술)'로 애매하게 작성하다가 이후에는 술_맥주, 술_소주 등으로 분화해서 작성
        - 또한, 프라이버시 측면에서 문제가 되는 부분이 있어서 사람 이름 등을 변환하는 것도 필요
    - 또한 사람에 대한 정보는 내가 일일이 입력해야 해서 시간이 좀 더 소요됨

### 개요
- 분석은 크게 두 가지로 나뉜다고 생각함.
    - 탐색적 분석
    - 네트워크 분석

#### 탐색적 분석
- 데이터를 다양한 관점에서 aggregation/projection 하며 유의미한 발견이 있는지를 확인
- 가능한 분석안은 다음과 같음
    - 전체 기간 내에서 가장 많이 만난 사람은 누구인가?
    - 전체 기간 내에서 가장 많이 만난 상위 20명은 기간 내에 만남 횟수가 어떻게 변화하는가?
        - 시간에 따른 변화 그래프
    - node의 특성을 고려한 분석
        - 남성을 만나는 횟수와 여성을 만나는 횟수는 차이가 있는가?
        - 학교 내 학생과 학교 밖 학생의 만남은 비율적으로 차이가 있는가?
    - edge의 특성을 고려한 분석
        - 보통 몇 명의 사람들을 자주 만나는가?
        - 자주 만나는 상위 10명의 사람이 모든 만남에서 비율을 얼마나 차지하는가?
            - 특히, 식사분류별로 나누어 보는 것이 필요함
                - 예) 평일 점심은 80% 이상 연구실 사람들과 먹는다
                - 예) 술은 맨날 80% 이상의 사람과 소수의 사람들과 먹는다
        - 만남의 특성별로 만나는 사람들은 다른가?
            - 예를 들어, 술 마실때 만나는 사람과 저녁을 먹을 때 만나는 사람은 같은가? 다른가?
            - 평일과 주말에 만나는 사람은 각각 다른가?
        - 술은 얼마나 자주 먹는가?
            - 여자친구가 생긴 다음에 술을 먹는 횟수가 줄었는가?

#### 네트워크 분석
- networkx를 사용하여 네트워크 분석을 수행
    - 단순히 만남 횟수를 세는 것보다 더 복잡한 분석을 수행하기 위함
- degree centrality, closeness centrality, betweenness centrality 비교
    - 가장 중요한 사람은 누구인가?
- 내가 만나는 네트워크는 어떤 cluster, community로 구성되어 있는가?
    - 모든 노드는 연결되어 있는가?
    - 모든 노드는 여섯 단계 안에 다 연결되는가?
- edge의 특성, node의 특성을 고려하여 분석
    - 식사분류 유형, 사람의 특성 등을 고려하여 네트워크 도출 및 분석
    - node는 class로 설계하고
- 케빈 베이컨) 정말 모든 사람은 6 단계 안에 다 연결되는가?

#### contribution?
- 구글 드라이브 api 활용
    - 데이터 관리는 구글 드라이브 구글 시트로 하고 있으며, 차후에는 구글시트에서 값을 바로 긁어와서 처리하는 식으로 진행할 계획
- graph visualization
    - networkx + matplotlib.pyplot