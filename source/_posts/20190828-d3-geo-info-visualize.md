---
title: d3를 이용한 지리 데이타 시각화
categories:
  - null
tags:
  - null
date: 2019-08-28 13:35:48
thumbnail:
---

이전 포스팅 된 {% post_link cyber-attack-map-ref %}의 내용처럼 지도위에 데이타 시각화를 구현 해보기로 한다. `Electron`,` Vue.js` 기반이며 `d3.js`를 이용해서 `SVG`를 다루는 내용이 주가 될것 같다.
<!-- more -->
### 화면 구성
#### 1. 지도
- d3, GeoJson
- **위도,경도** 처리

#### 2. 국가별 상세 팝업
- 영역 선택시 해당 국가 정보 팝업
- 팝업은 직접 그릴지, CSS, html 선택

#### 3. 실시간 로그 리스트
- 하단중앙에 위치하며 실시간 로그 테이블
- 로그에 따라 공격 현황 표현

#### 4. 공격 현황 표현
- {% post_link cyber-attack-map-ref %}의 내용처럼 에니메이션으로 표현
- 공격국가 , 대상국가를 각 포인트로 표현하며 그 사이에는 라인이 그어짐.
- 일반적인 라인보다는 공격효과가 돋보여야 함 ([예제 보기](http://d3.artzub.com/wbca/))
- svg 에니메이션 효과

#### 5. 기간별 실시간 추이
- 기간 설정 후 당시의 공격 진행사항을 실시간으로 확인 가능([예제 보기](http://www.digitalattackmap.com/))
      









 

### Related Posts
---
