# CSVParser
CSV Parsing 

![image](https://github.com/user-attachments/assets/15f5f4df-5212-4ca5-b985-d8e1fb144d1b)

**로컬에 CSV 파일을 보관하는 프로젝트의 경우 처리 **

**[기존]**
CSVManager의 Inspector에 추가되는 csv 파일들을 각각 변수로 추가 및 파싱하는 함수를 구현해야했다. 
* 문제점
  1) csv 파일의 수가 증가할때마다 CSVManager의 Inspector가 너무 길어짐.
  2) csv 파일 수가 증가할때마다 CSVManager에 파싱하는 함수까지 동일하게 추가해줘야해서 CSVManager의 길이가 너무 길어짐
 

**[구현]**
1) CSVParserBase 라는 추상 클래스를 상속받아 각 csv 데이터에 맞는 파싱 클래스를 구현한다.
2) CSVManager에서 InitializeParsers() 에서 CSVParserBase를 상속받은 모든 파서 클래스를 찾아서 자동으로 초기화한다. [리플렉션 활용]
   ㄴ 각 파서 클래스에 적용된 TableTypeAttribute를 활용해서 TABLE_TYPE과 매핑한다.
3) 각 생성된 파서 인스턴스들을 parserDictionary에 TABLE_TYPE을 키로 해서 저장한다.
4) LoadCSVData에서 각 파서의 Parse() 메소드를 호출하여 데이터를 파싱하고 결과를 각 dataDictionary에 저장한다.
5) 외부에서 파싱된 데이터에 접근할때는 GetDataList<T>를 사용한다.
6) 새로운 CSV 데이터 타입이 추가될때마다 CSVParserBase를 상속받는 파서 클래스를 생성하고 ScriptableObject에 등록한다.

* 개선된 점
  1) 기존과 달리 CSVManager에 ScriptableObject 하나만 등록해두면 됨.
  2) 다른 csv 데이터들이 추가될때마다 각 csv 데이터에 맞는 파서 클래스만 구현하고 CSVManager 스크립트는 추가 수정 필요 없음

* 주의사항
  1) 리플렉션을 사용한 데이터 세팅은 성능에 영향을 끼칠 수 있으므로, 초기화 시점에서만 사용할 수 있도록 한다.
  2) GetDataList<T>를 사용할 때 T 에 올바른 데이터 타입을 지정 필요.
  3) 
