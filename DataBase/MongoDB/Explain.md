# Explain

- full-scan인지 아닌지는 `queryPlanner` 실행 후 
- “winningPlan” -> “inputStage” -> “indexName” 의 유무를 확인하면 된다.

그리고 자세한 사항은 아래 세가지만 보면 된다.

- __executionStats.nReturned__
  - return 할 doc 갯수    
  - mongoDB 메뉴얼에 row 를 `doc` 이라 표현 한다.
- __.totalKeysExamined__
  - index 를 탄 doc 갯수
- __.totalDocsExamined__
  - scan 한 doc 갯수
  - 이 녀석이 클 수록 성능 저하를 불러온다.
