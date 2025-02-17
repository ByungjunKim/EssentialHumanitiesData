# 3. 『프랑켄슈타인』 디지털 에디션 변환 과정과 인문 데이터 포맷

## 3.1. 서론

메리 셸리의 *Frankenstein; or, the Modern Prometheus*(『프랑켄슈타인』)은 근대 공상과학(SF)과 고딕소설의 시초로 불리며, 시대와 판본에 따라 텍스트가 변화되어 온 대표적인 문학 작품이다. 본 문서는 이 작품이 종이책이나 필사본과 같은 물리적 자료에서 **디지털 에디션**으로 전환되는 과정을 설명하고, **디지털 인문학**(Digital Humanities) 연구에서 자주 활용되는 **기계가독형 데이터 포맷**(CSV/TSV, JSON, XML/TEI)의 특징과 활용 방안을 소개한다. 또한 디지털 바리오럼(variorum) 에디션 제작 과정과 판본 대조 및 변이 분석이 어떻게 이루어지는지 구체적으로 서술하고, 이러한 **텍스트 구조화** 작업이 전통 문헌 연구에 미치는 영향을 논의한다.

『프랑켄슈타인』을 디지털 에디션으로 만들면, 1818년 초판과 1831년 개정판을 비롯하여 다양한 판본을 한데 모아 **동시에 비교**하고 **검색**하며 **주석 정보**를 확인할 수 있다. 이는 전통적인 종이책 대조 작업으로는 시간과 노력이 많이 들었던 문헌 연구를 보다 체계화·자동화할 수 있도록 도와준다. 따라서 고전 문학의 디지털 에디션화는 학술적·역사적·교육적 중요성을 모두 지니며, *프랑켄슈타인*은 이러한 변환 작업의 대표 사례로 꼽힌다.

---

## 3.2. 종이에서 디지털로: 『프랑켄슈타인』의 디지털 에디션 변환 과정

이 절에서는 전통적인 판본이 디지털 에디션으로 전환되는 과정을 **세부 단계**별로 살펴본다.

#### (1) OCR 및 텍스트 추출

먼저, 1823년의 드문 중간판본과 같이 종이 자료나 스캔 이미지를 **광학 문자 인식**(OCR) 기술로 디지털 텍스트로 추출한다. 예컨대 구글 북스(Google Books)에 스캔된 인쇄본 이미지를 OCR로 읽고, 교정 과정을 통해 오탈자를 수정해 1818년과 1831년 판본 모두 양식이 일관된 텍스트 파일을 얻을 수 있다. 1818년 초판과 1831년 개정판의 경우, 1990년대에 이미 전자 텍스트(펜실베니아 대학의 HTML본)로 만들어진 것을 재활용해, **정규표현식**(Regular Expression)으로 필요 없는 태그를 정리하고 원본 인쇄본 혹은 읽기 가능한 파싯밀리(facsimile)와 대조하여 오류를 수정한다.  
(*참고: [Balisage: Rebuilding a Digital Frankenstein by 2018](https://www.balisage.net/Proceedings/vol20/html/Beshero-Bondar01/BalisageVol20-Beshero-Bondar01.html#:~:text=Though%20our%20initial%20goal%20was,notebook%20drafts%20of%20the%20novel) / [Method](https://frankensteinvariorum.org/method#:~:text=common%20language,the%201818%20and%201831%20editions)*)

#### (2) TEI/XML 인코딩과 텍스트 정교화

OCR로 얻은 텍스트는 **학술적 활용**에 적합하도록 **텍스트 인코딩** 과정을 거친다. *프랑켄슈타인* 프로젝트 팀은 모든 판본을 **TEI P5** 규범에 따른 **XML 형식**으로 표현했다. TEI는 문단(`<p>`), 장(`<div>`), 등장인물(`<person>`), 삭제(`<del>`), 추가(`<add>`) 등 **인문학 연구에 필요한 세부 정보를 태그**로 표시할 수 있는 국제 표준이다. 1818년판(일명 *Thomas 사본*), 1823년 판본, 1831년판(및 기타 주석판)을 각각 동일한 XML 구조로 정리해, **단락**·**장**·**편지글** 등의 구조적 요소가 일관되도록 맞추었다. 또한 **Shelley-Godwin 아카이브**에서 제공된 필사본 TEI 파일을 병합하여, 각 페이지 말미의 여백 삽입문을 읽기 흐름에 자연스럽게 배치했다.  
(*참고: [Method](https://frankensteinvariorum.org/method#:~:text=common%20language,the%201818%20and%201831%20editions)*)

#### (3) 판본 대조 및 변이 분석

그다음, 컴퓨터 보조 대조 알고리즘(예: CollateX)을 이용해 판본들 간 **텍스트 차이점**을 식별한다. 텍스트를 일정한 단락(chunk) 단위로 나누고 시작·끝 지점을 정렬하여, 필수적인 철자 차이만 남기거나(불필요한 차이는 무시) 단어 단위로 정교하게 비교하는 방식이다. 이 과정을 통해 한 판본에만 존재하거나 수정된 문장은 **<app>, <rdg>** 태그로 병렬 저장되며, 결과적으로 **비교 정보가 주석된 XML 데이터**가 생성된다. 아래는 TEI XML 내부에서 한 구절의 두 판본이 어떻게 표시되는지 보여주는 간단한 예시다.

```xml
<app xml:id="C07a_app23">
  <rdg wit="f1818">
    You may easily conceive that I was much gratified by the offered communication; 
    yet I could not endure that he should renew his grief by a recital of his misfortunes.
  </rdg>
  <rdg wit="f1831">
    You may easily imagine that I was much gratified by the offered communication; 
    yet I could not endure that he should renew his grief by a recital of his misfortunes.
  </rdg>
</app>
```

이처럼 판본 간 **문장 단위 차이**를 체계적으로 표지함으로써, 연구자는 어느 판본에서 어떤 내용이 달라지는지 쉽게 파악할 수 있다.  
(*참고: [Assembling a Monster: The Frankenstein Variorum – Matthew Lincoln, PhD](https://matthewlincoln.net/2020/01/12/frankenstein-variorum.html#:~:text=%3CrdgGrp%20xml%3Aid%3D,app)*)

---

## 3.3. 인문 데이터 포맷의 특징과 활용

*프랑켄슈타인* 디지털 에디션 프로젝트에서 생성된 텍스트와 주석 데이터는 **다양한 기계가독형 포맷**(CSV/TSV, JSON, XML/TEI)으로 표현 및 활용될 수 있다. 원본 자료를 구조화하면 **검색**, **통계**, **시각화**, **네트워크 분석** 등을 수행할 수 있으며, 이는 **디지털 인문학**에서 새로운 통찰을 얻는 핵심 단계가 된다.

아래 표는 세 가지 주요 포맷의 **구조**와 **장단점**을 간단히 비교한 것이다. 예시 텍스트는 아래 표 뒤에 **본문** 형태로 따로 보여 준다.

| 데이터 형식 | 구조                  | 장점                                                                                                                       | 단점                                                                                             |
|-------------|-----------------------|---------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------|
| **CSV**     | 행과 열을 이용한 **단순 테이블** 구조. 쉼표(`,`)나 탭(\t)으로 각 필드를 구분                                    | 구조가 단순하고 엑셀·Pandas 등에서 바로 쓸 수 있음<br> 가벼운 파일로 관리·배포 용이<br> 표준적으로 지원됨 | 계층 구조·메타데이터 풍부화가 어려움<br> 본문 내 쉼표나 줄바꿈이 들어갈 경우 처리가 번거로움                       |
| **JSON**    | 키-값 쌍과 배열(`[]`)을 포함하는 **중첩 계층** 구조<br> 웹·앱 개발에서 사실상 표준으로 쓰임                    | 계층적 데이터 표현 가능<br> 웹·앱 환경에서 활용도 높음<br> 비교적 간결한 문법으로 사람이 읽기도 무난       | 문헌학용 표준이 아니므로 장·절·문단 같은 복잡 구조를 표현하려면 별도 설계 필요<br> 구조가 복잡해질 수 있음      |
| **XML/TEI** | XML 태그로 문헌 구조(장, 절, 문단), 인물, 주석 등을 세밀히 기록<br> TEI는 인문학 연구 표준 규범에 따른 확장 XML | 문헌 구조와 의미(등장인물, 변이 정보 등)를 풍부하게 태그화<br> 판본 대조, 주석, 메타데이터 모두 표현 가능<br> 국제 표준으로 학술 재현에 강점 | 태그·스키마가 방대해 학습 곡선이 가파름<br> 파일 크기가 커질 수 있고 파싱·편집 도구 필요<br> 접근 장벽이 높을 수 있음 |

#### (1) CSV 예시

아래 예시는 *프랑켄슈타인*의 1818년판과 1831년판 사이에서 바뀐 문장과 변이 유형을 **CSV** 형태로 기록한 것이다.  
```csv
문장ID,1818년판 문장,1831년판 문장,변화 유형
S1,"You may easily conceive that I was much gratified by the offered communication; yet I could not endure that he should renew his grief by a recital of his misfortunes.","You may easily imagine that I was much gratified by the offered communication; yet I could not endure that he should renew his grief by a recital of his misfortunes.","단어 치환 (conceive→imagine)"
S2,"The infant Elizabeth, the only child of his deceased sister, was cared for in his home.","They were fond of the sweet orphan.","인물 배경 변경 (사촌→고아)"
```
- **활용 예시**: 엑셀(Excel)로 불러와 빠르게 필터링을 하거나, 파이썬 Pandas로 통계·시각화를 수행할 수 있다.  
- **장점**: 데이터 구조가 단순해 **처리와 공유**가 간편하다.  
- **단점**: 문단·주석처럼 중첩된 구조나 자세한 메타정보를 담기엔 한계가 있다.

#### (2) JSON 예시

아래 예시는 판본별 문장 변이와 함께 인물 정보, 주석을 **JSON** 구조로 표현한 사례다.
```json
{
  "문장ID": "S2",
  "텍스트": {
    "1818년판": "The infant Elizabeth, the only child of his deceased sister, was cared for in his home.",
    "1831년판": "They were fond of the sweet orphan."
  },
  "변화유형": "인물 배경 변경 (사촌→고아)",
  "주석": "1818년판은 친족 관계를 강조해 'his deceased sister'를 언급하지만, 1831년판은 orphan이라는 표현으로 인물 배경이 바뀐다.",
  "인물정보": {
    "인물명": "Elizabeth",
    "역할": "주요 인물"
  }
}
```
- **활용 예시**: 웹 애플리케이션에서 AJAX로 불러와 인터페이스에 동적으로 표시하거나, NoSQL DB에 저장해 검색할 때 활용한다.  
- **장점**: 계층화된 구조를 손쉽게 구성할 수 있고, 자바스크립트 등으로 처리하기 용이하다.  
- **단점**: **문헌학 전용 표준**이 아니므로, 장·절·주석 등 복잡한 문헌 구조 표현은 직접 정의해야 한다.

#### (3) XML/TEI 예시

아래 예시는 *프랑켄슈타인*에서 빅터 프랑켄슈타인의 대사와 판본 변이를 **TEI**로 표시한 단순화된 XML 예시다.
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
          <note type="variant">
            1818년판: "I must create life!", 1831년판: "I must endeavor to create life."
          </note>
        </person>
      </div>
    </body>
  </text>
</TEI>
```
- **활용 예시**: 문헌 구조(장, 단락, 인물), 주석, 판본 대조 정보를 풍부하게 태그로 표현한다. XSLT로 변환해 웹페이지나 PDF 등을 만들어낼 수도 있다.  
- **장점**: 원문의 **구조**와 **의미**(메타데이터, 변이, 인물 등)를 상세히 기록할 수 있으므로 **학술 에디션**에 이상적이다.  
- **단점**: XML과 TEI 스펙을 익히는 데 시간이 들고, 파일이 길어져 초심자에게는 접근 장벽이 있을 수 있다.

---

## 3.4. 결론

본 장에서는 *프랑켄슈타인*의 디지털 에디션 변환 과정을 **OCR**을 통한 텍스트 획득, **TEI/XML 인코딩** 및 정교화, 그리고 **판본 대조와 변이 분석**의 세 단계로 나누어 상세히 살펴보았다. 또한, CSV/TSV, JSON, XML/TEI 같은 데이터 포맷이 어떤 특징을 갖고 있고, 디지털 문헌 연구에서 어떻게 활용되는지 예시를 통해 설명했다. 이러한 **데이터 변환 및 구조화**는 방대한 문헌을 컴퓨터로 효율적으로 비교·분석할 수 있게 하며, 정량적·시각적 분석 도구와 결합해 **인문학 연구의 새로운 지평**을 열어준다.

디지털 에디션의 가장 큰 장점은, 여러 판본의 변이 정보를 한눈에 비교함으로써 작가가 시도한 **개작 의도**와 **시대적 변천**을 추적하기가 용이해진다는 점이다. 예컨대 *프랑켄슈타인*의 초판과 개정판은 인물 설정, 운명론적 분위기 등 많은 요소가 달라졌는데, 디지털 에디션에서는 이런 차이를 **<app>, <rdg>** 태그로 구조화해 제공하므로 사용자가 손쉽게 원문을 대조할 수 있다. 나아가 웹 애플리케이션이나 시각화 도구와 연동해 특정 단어 빈도나 구절의 수정 패턴을 그래프나 차트로 확인하는 등 **새로운 형태의 문헌 분석**도 가능해진다.

---

## 3.5. 연습 문제

1. *프랑켄슈타인*의 OCR 결과와 TEI/XML 인코딩 파일을 비교하여, 특정 구절의 변이가 어떻게 표기되었는지 분석해 보자.  
2. CSV/TSV 파일로 변환된 판본 대조 데이터를 활용해, 1818년판과 1831년판 사이에서 특정 단어가 어떻게 바뀌었는지 빈도를 비교·시각화해 보자.  
3. JSON 데이터에 포함된 문장 주석 정보를 바탕으로, 인물(예: 엘리자베스)의 설정이 각 판본에서 어떻게 달라지는지 정리해 보자.

---

## 참고 문헌

- [Balisage: Rebuilding a Digital Frankenstein by 2018](https://www.balisage.net/Proceedings/vol20/html/Beshero-Bondar01/BalisageVol20-Beshero-Bondar01.html#:~:text=In%20the%20fall%20of%202016%2C,24%20the%20Frankenstein%20edition%20on)  
- [Hierarchies Made to Be Broken: The Case of the Frankenstein Bicentennial Variorum Edition – DH2018](https://dh2018.adho.org/en/hierarchies-made-to-be-broken-the-case-of-the-frankenstein-bicentennial-variorum-edition/#:~:text=6%20%20We%20are%20also,in%20composing%2C%20amending%2C%20and%20substantially)  
- [Method](https://frankensteinvariorum.org/method#:~:text=common%20language,the%201818%20and%201831%20editions)  
- [Assembling a Monster: The Frankenstein Variorum – Matthew Lincoln, PhD](https://matthewlincoln.net/2020/01/12/frankenstein-variorum.html#:~:text=%3CrdgGrp%20xml%3Aid%3D,app)  
- [DHQ: Digital Humanities Quarterly: Reassessing the locus of normalization in machine-assisted collation](https://www.digitalhumanities.org/dhq/vol/14/3/000489/000489.html#:~:text=CollateX%20supports%20several%20output%20views,with%20just%20the%20n%20property)  
- [DHQ: Digital Humanities Quarterly: From Archive to Database: Using Crowdsourcing, TEI, and Collaborative Labor to Construct the Maria Edgeworth Letters Project](https://digitalhumanities.org/dhq/vol/18/2/000424/000424.html#:~:text=from%20different%20sources,to%20meet%20new%20use%20cases)  
- [Converting Books to JSON: A Digital Humanities Project – Digital Born](https://digitalborn.org/converting-books-to-json-a-digital-humanities-project/#:~:text=to%20perform%20document%20analysis%20across,This)