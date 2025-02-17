# 『프랑켄슈타인』 디지털 에디션 변환 과정과 인문 데이터 포맷  
*텍스트 분석 기법 및 인문 데이터 포맷의 활용 사례를 함께 다룸*

---

## 1. 서론

본 장은 메리 셸리의 *Frankenstein; or, the Modern Prometheus*(『프랑켄슈타인』)이 종이 책이나 필사본과 같은 물리적 자료에서 어떻게 디지털 에디션으로 전환되었는지를 설명하고, 디지털 인문학 연구에서 널리 활용되는 기계가독형 데이터 포맷(CSV/TSV, JSON, XML/TEI)의 특징과 활용 방안을 소개한다. 이 장에서는 디지털 바리오럼(variorum) 에디션 제작 과정 및 판본 대조와 변이 분석 과정을 구체적으로 서술하며, 텍스트 데이터를 구조화하는 작업이 전통 문헌 연구에 미치는 영향을 논의한다.

---

## 2. 본론

### 2.1 종이에서 디지털로: 『프랑켄슈타인』의 디지털 에디션 변환 과정

본 섹션에서는 전통적인 판본이 디지털 에디션으로 전환되는 과정을 세부 단계별로 설명한다.

#### (1) OCR 및 텍스트 추출

먼저, 1823년의 드문 중간판본과 같이 종이 자료 또는 스캔 이미지를 **광학 문자 인식**(OCR) 기술을 통해 디지털 문자열로 추출한다. 예를 들어, 구글 북스에 스캔된 인쇄본 이미지를 OCR로 판독하고, 철저한 교정을 거쳐 1818년·1831년 판본과 양식이 일치하는 깨끗한 텍스트 파일을 생성하였다 ([Balisage: Rebuilding a Digital Frankenstein by 2018](https://www.balisage.net/Proceedings/vol20/html/Beshero-Bondar01/BalisageVol20-Beshero-Bondar01.html#:~:text=Though%20our%20initial%20goal%20was,notebook%20drafts%20of%20the%20novel)). 또한, 1818년 초판과 1831년 개정판의 경우 1990년대 제작된 기존 전자 텍스트(펜실베니아 대학의 HTML 전자본)를 활용하여, HTML 파일을 정규표현식으로 교정해 깨끗한 XML 구조로 변환하고 원본 인쇄본이나 판독 가능한 파싯밀리와 대조하여 오류를 수정하였다 ([Method](https://frankensteinvariorum.org/method#:~:text=common%20language,the%201818%20and%201831%20editions)).

#### (2) TEI/XML 인코딩과 텍스트 정교화

OCR을 통해 추출된 텍스트는 학술적 활용에 적합하도록 **텍스트 인코딩**된다. *프랑켄슈타인* 프로젝트 팀은 모든 판본을 최신 TEI P5 규범에 따른 **XML 형식**으로 표현하고자 하였으며, TEI는 문단, 장, 등장인물, 삭제 및 추가된 문구 등 인문학 연구에 필요한 세부 정보를 태그로 기록할 수 있는 국제 표준이다. 예를 들어, 1818년판의 인쇄본(일명 **Thomas 사본**)에 적힌 주변 주석(marginalia)은 TEI XML 내에 `<add>`, `<del>`, `<note>` 태그로 통합되었다 ([Method](https://frankensteinvariorum.org/method#:~:text=Elisa%20Beshero,was%20prepared%20for%20collation%20to)). 1823년 판본 역시 동일한 XML 구조로 정리되었으며, 이렇게 준비된 1818년, 1823년, 1831년(및 기타 주석판) 텍스트는 **단락**, **장(章)**, **편지글** 등의 구조적 요소에 대해 동일한 태그 체계를 갖추도록 정규화되었다 ([Method](https://frankensteinvariorum.org/method#:~:text=common%20language,the%201818%20and%201831%20editions)). 또한, **Shelley-Godwin 아카이브**에서 제공된 필사본 TEI 파일은 XSLT 스크립트를 통해 병합되고, 각 페이지 말미에 별도로 표시된 여백 삽입문이 읽기 흐름에 맞게 재배치되었다 ([Method](https://frankensteinvariorum.org/method#:~:text=prepared%20for%20the%20print%20edition,That)).

#### (3) 판본 대조 및 변이 분석

이후, 컴퓨터 보조 대조 알고리즘(예, CollateX)을 활용해 준비된 판본들 사이의 **텍스트 차이점**을 식별한다. 텍스트는 일정한 단락(chunk) 단위로 분할되고, 각 판본의 시작과 끝이 최대한 일치하도록 정렬되며, 사소한 차이는 무시하도록 **정규화** 규칙이 적용된다 ([Method](https://frankensteinvariorum.org/method#:~:text=,to%20make%20in%20the%20process)). 필사본의 `<milestone/>` 태그와 인쇄본의 `<p>` 태그 간의 대응도 정의되어, 토큰화된 단어 단위 비교가 이루어진다. 자동 대조 결과, 각 텍스트에는 **비교 정보가 주석된 새로운 XML 데이터**가 생성되며, 한 판본에만 존재하거나 수정된 문장은 해당 위치에 `<app>` 및 `<rdg>` 태그를 통해 병렬로 기록된다 ([Assembling a Monster: The Frankenstein Variorum – Matthew Lincoln, PhD](https://matthewlincoln.net/2020/01/12/frankenstein-variorum.html#:~:text=%3CrdgGrp%20xml%3Aid%3D,app)). 아래와 같이 간단한 코드 구조 예시를 통해 TEI XML 내에서 판본 읽기(reading)가 어떻게 표시되는지 확인할 수 있다.

```xml
<app xml:id="C07a_app23">
  <rdg wit="f1818">You may easily conceive that I was much gratified by the offered communication; yet I could not endure that he should renew his grief by a recital of his misfortunes.</rdg>
  <rdg wit="f1831">You may easily imagine that I was much gratified by the offered communication; yet I could not endure that he should renew his grief by a recital of his misfortunes.</rdg>
</app>
```

이와 같이 판본들 사이의 문장 단위 차이가 체계적으로 표지되어, 연구자는 **비교 장치(critical apparatus)**를 통해 어느 판본에서 어떤 내용이 달라지는지 쉽게 파악할 수 있다.

---

### 2.2 인문 데이터 포맷의 특징과 활용

*프랑켄슈타인* 디지털 에디션 프로젝트에서 생성된 텍스트와 주석 데이터는 다양한 **기계가독형 포맷**으로 표현 및 활용될 수 있다. 원본 자료를 구조화하면 연구자가 컴퓨터를 활용해 검색, 통계, 시각화, 네트워크 분석 등을 수행할 수 있게 된다. 역사적 문헌 분석에서 데이터의 구조화는 새로운 통찰을 얻기 위한 **핵심 단계**다.

#### (1) CSV/TSV: 표 형태 데이터 포맷

CSV와 TSV는 데이터를 행과 열의 표 형태로 표현하는 단순한 포맷이다. 예를 들어, 아래와 같이 *프랑켄슈타인*의 1818년판과 1831년판 사이의 문장 변화를 정리한 표를 생성할 수 있다.

```csv
문장 ID,1818년판 문장,1831년판 문장,변화 유형
S1,"You may easily conceive that I was much gratified by the offered communication; yet I could not endure that he should renew his grief by a recital of his misfortunes.","You may easily imagine that I was much gratified by the offered communication; yet I could not endure that he should renew his grief by a recital of his misfortunes.","단어 치환 (conceive→imagine)"
S2,"The infant Elizabeth, the only child of his deceased sister, was cared for in his home.","They were fond of the sweet orphan.","인물 배경 변경 (사촌→고아)"
```

**활용:** 이와 같은 CSV/TSV 데이터는 엑셀이나 Pandas와 같은 도구로 불러와 판본 간 어휘 수정이나 문장 변이의 빈도 분석, 유형별 분류 등을 수행하는 데 유용하다.

#### (2) JSON: 계층적 데이터 교환 포맷

JSON은 키-값 쌍과 중첩 구조를 사용하여 데이터를 표현한다. 아래 예시는 *프랑켄슈타인*의 인물 및 문장 변이 정보를 포함한 JSON 예시이다. 이 예시는 판본별 문장 내용, 변화 유형, 상세 주석과 함께 인물 정보도 함께 기록한다.

```json
{
  "문장ID": "S2",
  "텍스트": {
    "1818년판": "The infant Elizabeth, the only child of his deceased sister, was cared for in his home.",
    "1831년판": "They were fond of the sweet orphan."
  },
  "변화유형": "인물 배경 변경 (사촌→고아)",
  "주석": "1818년판은 친족 관계를 강조하여 'his deceased sister'를 언급하는 반면, 1831년판은 'orphan'이라는 용어로 인물 배경이 전면 재해석되었다.",
  "인물정보": {
    "인물명": "Elizabeth",
    "역할": "주요 인물",
    "설명": "원래는 고모의 딸로 소개되었으나, 개정판에서는 고아로 재해석되어 가족 관계가 변경되었다."
  }
}
```

**활용:** 이 JSON 예시는 단순한 문장 비교를 넘어, 판본별 텍스트 변이와 함께 인물 정보 및 상세 주석 정보를 중첩 구조로 제공한다. 이를 통해 웹 애플리케이션이나 데이터베이스에서 문장별 상세 분석 정보를 손쉽게 검색하고, 인물 관련 메타데이터를 함께 관리할 수 있다.

#### (3) XML/TEI: 구조적 문헌 인코딩 포맷 – 캐릭터 정보 포함 예시

XML은 TEI 표준을 활용하여 문학 텍스트의 다양한 구조적 정보를 상세히 기록할 수 있다. 아래 예시는 단순한 문장 변이 비교를 넘어서, 인물(캐릭터) 정보를 포함한 TEI 마크업 예시이다. 이 예시는 빅터 프랑켄슈타인의 대사와 관련 인물 정보를 기록하고, 판본 변이를 주석으로 첨부한 형태이다.

```xml
<TEI xmlns="http://www.tei-c.org/ns/1.0">
  <teiHeader>
    <fileDesc>
      <titleStmt>
        <title>프랑켄슈타인 (판본 비교 및 인물 정보)</title>
        <author>Mary Shelley</author>
      </titleStmt>
      <publicationStmt>
        <publisher>Digital Edition Project</publisher>
        <date>2024</date>
      </publicationStmt>
    </fileDesc>
  </teiHeader>
  <text>
    <body>
      <div type="scene" n="1">
        <u who="#Victor">I must create life!</u>
        <person xml:id="Victor">
          <persName>Victor Frankenstein</persName>
          <role type="protagonist">Creator</role>
          <desc>A brilliant yet tormented scientist whose ambition leads him to defy nature.</desc>
          <note type="variant" resp="Collation Team">
            1818년판에서는 “I must create life!”로 기록되었으나, 1831년판에서는 “I must endeavor to create life.”로 약간 완화되어 표현됨.
          </note>
        </person>
      </div>
    </body>
  </text>
</TEI>
```

**활용:** 이 TEI 예시는 단순한 문장 변이 비교뿐 아니라, 인물의 대사와 관련 메타데이터(인물 이름, 역할, 설명, 주석 등)를 함께 기록함으로써 연구자가 인물 분석 및 텍스트 비평을 수행할 수 있도록 돕는다. TEI/XML 마크업을 통해 인문학 텍스트의 구조와 의미를 풍부하게 보존할 수 있으므로, 다양한 판본 및 인물 정보 분석에 활용할 수 있다.

---

## 3. 결론

본 장에서는 *프랑켄슈타인*의 디지털 에디션 변환 과정을 OCR을 통한 텍스트 획득, TEI/XML 인코딩 및 정교화, 그리고 판본 대조와 변이 분석의 세 단계로 나누어 상세히 설명하였다. 또한, CSV/TSV, JSON, XML/TEI와 같은 기계가독형 데이터 포맷의 구체적인 예시를 통해, 텍스트 데이터를 구조화하는 작업이 전통 문헌 연구에 미치는 영향을 논의하였다. 이러한 데이터 변환 및 활용은 방대한 문헌을 컴퓨터로 비교·분석할 수 있게 하며, 정량적·시각적 분석 도구와 결합해 인문학 연구의 새로운 지평을 열어준다.

---

## 4. 연습 문제

1. *프랑켄슈타인*의 OCR 결과와 TEI/XML 인코딩 파일을 비교하여, 특정 구절의 변이가 어떻게 표기되었는지 분석하라.  
2. CSV/TSV 파일로 변환된 판본 대조 데이터를 활용해, 판본별 단어 빈도 차이를 시각화하라.  
3. JSON 데이터에 포함된 문장 주석 정보를 바탕으로, 1818년판과 1831년판의 차이를 정리하라.

---

## 5. 참고 문헌

- [Balisage: Rebuilding a Digital Frankenstein by 2018](https://www.balisage.net/Proceedings/vol20/html/Beshero-Bondar01/BalisageVol20-Beshero-Bondar01.html#:~:text=In%20the%20fall%20of%202016%2C,24%20the%20Frankenstein%20edition%20on)
- [Hierarchies Made to Be Broken: The Case of the Frankenstein Bicentennial Variorum Edition – DH2018](https://dh2018.adho.org/en/hierarchies-made-to-be-broken-the-case-of-the-frankenstein-bicentennial-variorum-edition/#:~:text=6%20%20We%20are%20also,in%20composing%2C%20amending%2C%20and%20substantially)
- [Method](https://frankensteinvariorum.org/method#:~:text=common%20language,the%201818%20and%201831%20editions)
- [Assembling a Monster: The Frankenstein Variorum – Matthew Lincoln, PhD](https://matthewlincoln.net/2020/01/12/frankenstein-variorum.html#:~:text=%3CrdgGrp%20xml%3Aid%3D,app)
- [DHQ: Digital Humanities Quarterly: Reassessing the locus of normalization in machine-assisted collation](https://www.digitalhumanities.org/dhq/vol/14/3/000489/000489.html#:~:text=CollateX%20supports%20several%20output%20views,with%20just%20the%20n%20property)
- [DHQ: Digital Humanities Quarterly: From Archive to Database: Using Crowdsourcing, TEI, and Collaborative Labor to Construct the Maria Edgeworth Letters Project](https://digitalhumanities.org/dhq/vol/18/2/000424/000424.html#:~:text=from%20different%20sources,to%20meet%20new%20use%20cases)
- [Converting Books to JSON: A Digital Humanities Project – Digital Born](https://digitalborn.org/converting-books-to-json-a-digital-humanities-project/#:~:text=to%20perform%20document%20analysis%20across,This)

---