## 인문 데이터와 디지털 에디션

### 1. 기계가독형 데이터와 디지털 에디션의 중요성

디지털 시대의 인문학 연구는 새로운 도구와 방법론을 필요로 한다. 단순히 텍스트를 디지털화하는 것을 넘어, 컴퓨터가 이해하고 분석할 수 있는 형태로 데이터를 구조화하는 것이 중요해졌다. 김동인의 소설 「감자」를 예시로, 인문학 연구에서 활용되는 주요 데이터 형식들을 살펴보고, 디지털 에디션을 위한 데이터 처리 과정을 알아본다.

### 2. 인문학 데이터 파일 형식

#### 2.1 텍스트 파일 (TXT)

- 가장 기본적인 텍스트 저장 방식
- 서식이나 특별한 구조가 없는 순수 텍스트
- 용량이 작고 호환성이 뛰어나다

```
[감자]
작가: 김동인
발표: 1925년
장르: 단편소설

싸움, 간통, 살인, 도둑, 구걸, 징역, 이 세상의 모든 비극과 활극의 근원지인, 칠성문 밖 빈민굴로 오기 전까지는...
```

#### 2.2 CSV/TSV 파일 (표 형식 데이터)

- CSV(Comma-Separated Values): 쉼표로 구분된 데이터
- TSV(Tab-Separated Values): 탭으로 구분된 데이터
- 행과 열로 구성된 구조화된 정보
- 스프레드시트 프로그램과 호환성이 좋다

```csv
이름,신분,나이,성별,직업,성격특성,거주지,가족관계
복녀,농민,28,여성,소작농,"순박함,성실함,순종적",산골 마을,"남편(정선달-사망), 자녀(딸-사망)"
김 주사,지주,50,남성,지주겸 고리대금업자,"탐욕스러움,교활함,이기적",산골 마을,없음
```

```
장	사건	시기	장소	관련인물	내용	결과
1	남편과 딸의 죽음	이야기 시작 전	산골 마을	복녀,정선달	남편과 딸이 병으로 사망	복녀 혼자 남음
2	김 주사의 착취 시작	봄	감자밭	복녀,김 주사	소작료 인상 요구	복녀의 생활고 심화
```

#### 2.3 JSON 파일 (JavaScript Object Notation)

- 계층적 구조를 가진 데이터 표현 방식
- 객체와 배열을 통해 복잡한 관계 표현 가능
- 웹 환경에서 널리 사용된다
- 사람이 읽고 쓰기 쉬운 형식

```json
{
  "작품": {
    "제목": "감자",
    "저자": "김동인",
    "출판년도": 1925,
    "주요_등장인물": [
      {
        "이름": "복녀",
        "신분": "농민",
        "나이": 28,
        "성격특성": [
          "순박함",
          "성실함",
          "순종적"
        ],
        "변화과정": [
          {
            "단계": "농사꾼의 아내",
            "상태": "가난하지만 성실한 삶"
          },
          {
            "단계": "과부",
            "상태": "생계를 위한 고군분투"
          }
        ]
      }
    ]
  }
}
```

#### 2.4 XML/TEI 파일 (Text Encoding Initiative)

- 텍스트의 구조와 의미를 표현하는 마크업 언어
- 인문학 연구에서 널리 사용되는 표준 형식
- 다양한 분석 도구와의 호환성이 높다

```xml
<?xml version="1.0" encoding="UTF-8"?>
<TEI xmlns="http://www.tei-c.org/ns/1.0">
  <teiHeader>
    <fileDesc>
      <titleStmt>
        <title>감자</title>
        <author>김동인</author>
        <respStmt>
          <resp>TEI 인코딩</resp>
          <name>디지털 인문학 연구소</name>
        </respStmt>
      </titleStmt>
      <publicationStmt>
        <publisher>개벽</publisher>
        <date>1925</date>
      </publicationStmt>
    </fileDesc>
  </teiHeader>
  <text>
    <body>
      <div type="character">
        <person xml:id="BOKNYEO">
          <persName>복녀</persName>
          <trait type="personality">
            <label>성격</label>
            <desc>순박함</desc>
          </trait>
          <residence>산골 마을</residence>
        </person>
      </div>
    </body>
  </text>
</TEI>
```

### 3. 디지털 에디션

#### 3.1 디지털 에디션의 개념

디지털 에디션은 종이 문헌이나 실물 자료를 전자적으로 편집하여 연구 및 분석에 활용할 수 있도록 만드는 작업이다. 단순히 텍스트를 디지털화하는 것을 넘어서, 텍스트를 구조화하고 표준화하여 컴퓨터가 인식할 수 있는 데이터로 만드는 것이 핵심이다.

#### 3.2 디지털 에디션을 위한 데이터 처리

디지털 에디션 데이터는 여러 단계를 거쳐 만들어진다.

1. 실물 스캔 및 OCR: 종이 문헌을 스캔하고 OCR 기술을 이용해 텍스트 데이터를 추출한다.
2. TXT 생성: OCR 결과를 정제하여 순수 텍스트(.txt) 파일로 저장한다.
3. Word 문서 제작 (선택적): 추출된 텍스트에 서식과 문단 구분 등을 추가한 Word 문서를 제작한다.
4. TEI/XML 변환: Word 문서나 TXT 파일의 내용을 TEI(XML) 표준에 맞게 태깅한다.
5. XML 파싱 및 CSV 추출: TEI/XML 데이터를 파싱하여 필요한 정보를 추출한 뒤, CSV(또는 JSON)와 같은 분석 친화적 형식으로 변환한다.

#### 3.3 학생들이 만든 디지털 에디션의 장점

- 데이터 분석 용이성: 단순 텍스트와 달리, TEI/XML 형식으로 구조화된 데이터는 컴퓨터가 내용을 쉽게 파악할 수 있어, 이후의 텍스트 마이닝, 단어 빈도 분석, 네트워크 분석 등에 활용하기 좋다.
- 판본 비교 가능: 예를 들어, 프랑켄슈타인의 경우 여러 판본(출판본, 작가의 원고 등)이 존재하는데, TEI로 태깅해두면 서로 다른 판본의 변화를 한눈에 비교할 수 있다. 이를 통해 작가의 의도나 편집 변화 등을 분석할 수 있다.
- 데이터의 재사용성: TEI와 CSV 같은 표준 포맷은 다른 연구자와 공유하거나, 다른 분석 도구(예: R, Python)를 사용하여 추가 분석할 때에도 유용하다.
- 교육 효과: 학생들이 직접 텍스트를 TEI 태깅하고, 이를 CSV로 추출하여 분석하는 과정을 경험함으로써, 인문학적 데이터 모델링과 디지털 분석 방법을 실습할 수 있다.

#### 3.4 연습문제: Raw Text를 디지털 데이터로 만들기

다음은 한 문단의 raw 텍스트 예시이다. 아래 과제를 통해, 학생들은 이 텍스트를 TEI 태깅(또는 필요한 데이터 포맷으로 변환)하여 분석 가능한 데이터로 만드는 과정을 실습해보자.

**예시 Raw Text**:
조선왕조실록은 우리 민족의 역사를 생생하게 기록한 귀중한 자료입니다. 이 기록은 각 왕의 즉위, 치세, 그리고 다양한 사회·문화적 사건들을 상세히 담고 있습니다.


**연습문제**:
1. 위 텍스트를 **TXT 파일**로 저장한 후, 기본 전처리(불필요한 공백 제거 등)를 수행한다.
2. 텍스트를 TEI/XML 형식으로 변환하여, 예를 들어 문단을 `<p>` 태그로 감싸고, 중요한 키워드(예: "조선왕조실록", "역사", "왕")에 대해 `<keyword>`와 같은 사용자 정의 태그를 적용해보자.
3. 변환된 TEI/XML 파일에서 Python을 사용하여 특정 태그의 내용을 추출하고 CSV 파일로 저장해보자.
4. 추출된 CSV 데이터를 활용하여 간단한 **단어 빈도 분석**이나 **시각화**를 진행해보자.

학생들이 이 과정을 통해, 원시 텍스트가 어떻게 디지털 데이터로 변환되고, 이를 바탕으로 다양한 분석과 연구가 가능한지 직접 체험해 볼 수 있다.

### 3.5 사례 연구: 디지털 에디션의 실제 활용

#### 3.5.1 Women Writers Project (WWP)

[Women Writers Project](https://wwp.northeastern.edu/)는 16세기부터 19세기 여성 작가들의 텍스트를 TEI로 인코딩하여 온라인 컬렉션으로 제공하는 대표적 사례다. 이 프로젝트에서는 각 텍스트에 대해 서지 정보와 본문 구조를 TEI로 태깅하여 원문의 형태와 편집 기록을 보존하며, 고급 검색 기능과 분석 도구를 제공하여 연구자들이 쉽게 접근할 수 있도록 한다.

#### 3.5.2 Frankenstein Variorum 프로젝트

Frankenstein의 경우, 여러 판본(출판본, 작가의 원고 등)이 존재한다. Frankenstein Variorum 프로젝트는 TEI 태깅을 통해 각 판본 간의 차이를 명확히 기록하고, 판본별로 수정된 부분이나 추가/삭제된 텍스트를 `<add>`, `<del>` 등으로 표시하여 비교 분석할 수 있도록 했다.

## 4. 파이썬 코드 예시

다음은 CSV, TSV, JSON, XML 파일을 읽고 데이터를 추출하는 파이썬 코드 예시입니다.

```python
import pandas as pd
import json
import xml.etree.ElementTree as ET
import matplotlib.pyplot as plt

# CSV 파일 읽기
df_characters = pd.read_csv('characters.csv')
print("CSV 파일 내용 (등장인물):")
print(df_characters.head().to_markdown(index=False, numalign="left", stralign="left"))

# TSV 파일 읽기
df_events = pd.read_csv('events.tsv', sep='\t')
print("TSV 파일 내용 (주요 사건):")
print(df_events.head().to_markdown(index=False, numalign="left", stralign="left"))

# JSON 파일 읽기
with open('gamja.json', 'r', encoding='utf-8') as f:
  data = json.load(f)
# JSON 데이터에서 필요한 정보 추출
print("JSON 파일 내용 (작품 정보):")
print(f"제목: {data['작품']['제목']}")
print(f"저자: {data['작품']['저자']}")
print(f"출판년도: {data['작품']['출판년도']}")

# XML 파일 읽기
tree = ET.parse('gamja.xml')
root = tree.getroot()
# XML 데이터에서 필요한 정보 추출
ns = {'tei': 'http://www.tei-c.org/ns/1.0'}
title = root.find('.//tei:title', ns).text
author = root.find('.//tei:author', ns).text
print("XML 파일 내용 (작품 정보):")
print(f"제목: {title}")
print(f"작가: {author}")

# 데이터 시각화 예시
# 등장인물의 신분별 빈도수를 막대 그래프로 시각화
character_status_counts = df_characters['신분'].value_counts()
character_status_counts.plot(kind='bar')
plt.title('등장인물 신분별 빈도')
plt.xlabel('신분')
plt.ylabel('빈도수')
plt.show()
```

## 5. 결론

인문 데이터와 디지털 에디션은 디지털 시대의 인문학 연구를 위한 필수적인 요소이다. 다양한 파일 형식을 이해하고, 파이썬과 같은 도구를 활용하여 데이터를 처리하고 분석함으로써, 인문학 연구의 새로운 가능성을 열 수 있다. 디지털 에디션은 인문학 자료의 접근성과 분석 가능성을 높여 연구의 깊이와 폭을 확장하는 데 기여할 수 있다.