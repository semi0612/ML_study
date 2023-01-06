# 📃 장바구니 분석을 통한 재구매 물품 예측

<img src="https://user-images.githubusercontent.com/51469989/210502490-5881ad58-4d8c-4fea-bfce-f6d04b565e89.png" width="60%">

## 자료구성
- 분석대상(Analysis Target) : 2014년 ~ 2015년(2년간) L그룹 4개 계열사에서 구매한 고객 일부 발췌
- 제공범위(Scope of Offer) : L그룹 4개 계열사의 구매이력(계열사 정보 비공개), 고객 성향을 파악할 수 있는 데이터 제공

## 데이터 목록 및 컬럼
구분
- 구매내역 정보 : 제휴사(ABCD), 영수증번호, 대분류코드, 중분류코드, 소분류코드, 고객번호, 점포코드, 구매일자, 구매시간, 구매금액
- 채널(온/오프라인) 이용 : 2015년 10월 ~ 12월. 총 3개월치
- 경쟁사 이용 : L사 4개 계열사 별 경쟁사 이용년월 정보
- 멤버십여부 : 멤버십에 가입한 고객 저보
- 데모(고객정보) : 20341개
- 상품분류 : 4386개

## 주제
기업에서 가장 원하는 건 매출 상승. 어떻게 하면 매출을 상승 시킬 수 있을까?
→ 신규고객을 유치하는 방향 : 포괄적으로 들어가는 광고 비용도 많이 들고 어렵다고 판단
→ 기존 고객들을 대상으로 구매 상승을 시키는 방향으로 선회
→ 기존 고객들이 관심있는, 관심을 보일만한 물품을 위주로 프로모션 하면 되지 않을까?
→ 관심있는 물품을 어떻게 찾을 수 있을까
→ 장바구니(연관) 분석을 통한 재구매 물품 예측

### 데이터 분석 목적
고객들의 개인적인 취향과 니즈를 파악하여 맞춤형 상품 추천을 제공하여 고객들이 원하는(관심있을 만한) 상품에 쉽게 접근하고 구매할 수 있게끔, 고객들의 구매이력을 활용하여 구매 예측률이 높은 모델을 만들어보려 한다.

### 장바구니 분석(basket analysis = 연관 분석; Association Analysis;)
연관 규칙(association rule)은 비지도 학습¹의 하나로 대형 데이터베이스에서 변수간의 흥미로운 관계를 발견하기 위한 규칙기반 기계학습 방법이다. 연관 규칙 분석이란 대량의 정보로부터 개별 데이터 사이에서 연관규칙을 찾는 것인데, 예를 들어 마켓의 구매내역 중 특정 물건의 판매 발생 빈도를 기반으로 'A물건을 구매하는 사람들은 B물건을 함께 구매하는 경향이 있다' 라는 규칙을 찾을 수 있다. 따라서 다른말로 장바구니 분석이라고 불리기도 하는 것이다.

연관규칙분석의 대표적인 알고리즘으로는 Apriori, FP-growth, DHP 등이 있는데 그 중 Apriori 알고리즘이 비교적 구현이 간단하고 높은 성능을 보여준다는 의견이 많아 프로젝트에 Apriori을 사용해보기로 했다.

#### Apriori 알고리즘
추천시스템의 1세대라고 할 수 있는 Apriori 알고리즘은 빈발항목집합²을 추출하는 것이 원리이다. 이해하기 쉬운 간단한 원리를 가지고 있으며 상품간의 많은 연관규칙을 발견할 수 있다는 장점이 있지만, 상품 수가 많을 수록 그 계산량이 기하급수적으로 늘어난다는 단점 역시 가지고 있다.
<br><br>
apriori 알고리즘을 사용법은 아래와 같다. 설정 지지도³(기본 값은 0.5, min_support 설정으로 값을 조절해줄 수 있다) 이상의 값을 가지는 연관 상품들을 얻어낼 수 있다.
```python
itemset = apriori(df, min_support=0.3, use_colnames=True)
itemset
```
이후 association_rules를 사용하여 설정 신뢰도⁴(기본 값은 0.8, min_threshold 설정으로 값을 조절해 줄 수 있다) 이상의 값을 가지는 목록을 확인할 수 있다.
```python
from mlxtend.frequent_patterns import association_rules
association_rules(itemset, metric="confidence", min_threshold=0.2)
```
association_rules 의 결과 값 중 lift(향상도)⁵라는 컬럼이 있는데, 이 수치가 클수록 우연히 일어나지 않았다는 표시이다. 아무런 관계가 없다면 1로 표현된다.


<sub>¹ 비지도 학습 : 목적변수(반응변수; 종속변수; 목표변수; 출력값)에 대한 정보 없이 학습이 이루어지는 학습</sub> <br>
<sub>² 빈발항목집합 : 최조지지도 이상을 가지는 항목집합. 모든 항목집합을 대상으로 했을 시 계산량이 복잡해질 수 있어, 최소지지도를 정한 후 그 이상의 값만 찾아 연관규칙을 생성한다. </sub> <br>
<sub>³ 지지도 : 전체 거래에서 특정물품 A와 B가 동시에 거래되는 비중. 해당 규칙이 얼마나 의미있는지 보여준다.<br> [A와 B가 동시에 일어난 횟수 / 전체 거래 횟수]</sub> <br>
<sub>⁴ 신뢰도 : A를 포함하는 거래 중 A와 B가 동시에 거래되는 비중.<br> [A와 B가 동시에 일어난 횟수/A가 일어난 횟수]</sub> <br>
<sub>⁵ 향상도 : A라는 상품에서 신뢰도가 동일한 상품 B와 C가 존재할 때, 어떤 상품을 더 추천해야 좋을지 판단. <br> [A와 B가 동시에 일어난 횟수 / A, B가 독립된 사건일 때 A, B가 동시에 일어날 확률]</sub> <br>