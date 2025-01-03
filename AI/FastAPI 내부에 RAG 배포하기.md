# FastAPI 내부에 RAG 배포하기

- 단계별 RAG 파이프라인
  
  - Search : Load -> Split -> Embed -> Store
  
  - Generation: Retrieve -> Prompt -> LLM -> Answer

- RAG 파이프라인 설계 체크리스트
  
  - Document Loading : 활용할 텍스트 데이터
    
    - 제품 메뉴열 문서 (ex. Galaxy A 12 등)
  
  - Text Splitting : 데이터 Chunking 최적화
    
    - Recursive Text Splitter
  
  - Embedding : 임베딩 모델 최적화
    
    - Upstage Solar Embedding
  
  - Vector Store (=Indexing)
    
    - Pinecone
  
  - Retrieving : 검색 방식 최적화
    
    - Ensemble Retriever
  
  - Generating : 답변 생성
    
    - Update Solar Pro Chat Model

- 실습 전 체크 사항
  
  - (Windows 환경) Posershell 7 이상 필요
  
  - `.env` 파일 필요
    
    - OpenAI api key, Pinecone api key, Upstage api key 필요
    
    - key 값이 public에 절대 노출되지 않도록 주의
  
  - 파일 구조 확인 필요
    
    - embed.py : 파일 chunging, vectorize, vector db upload 진행
      
      - 최초 진행 시 배포 전 1회 실행이 필요
      
      - 폴더 내부의 "Galaxy_A_35.pdf" 파일에 대해 pinecone 구축이 진행됨
    
    - app.py : 사용자 입력에 대한 pinecone 검색 후 rag 실행, single-turn 답변 반환
      
      - 실행 후 swagger를 통한 query에 적절한 답변이 오는지 확인

## RAG 배포하기

- embed.py : PDF 문서 읽기 -> 텍스트 파싱 -> 텍스트 분할 -> 임베딩 생성 -> Pinecone 저장
  
  1. PDF 문서 로드 및 파싱
     
     - Upstage Document Parse Loader를 사용하여 PDF 문서를 HTML형태로 파싱
     
     - PDF 파일의 텍스트 데이터를 파싱하여 사용 가능한 형태로 변환
     
     - OCR좌표 정보 제외
  
  2. 텍스트 분할 및 전처리
     
     - Recursive Character Text Splitter를 활용하여 텍스트를 지정된 크기(1000자)로 분할, 중복처리
     
     - 대량의 텍스트를 검색하고 임베딩의 효율성을 높이기 위해 분할 수행
  
  3. 벡터 임베딩 생성 및 저장
     
     - Upstage Embeddings를 사용하여 텍스트를 벡터 형식으로 변환
     
     - Pinecone Vector Store를 사용하여 임베딩을 인덱스에 저장
  
  4. Pinecone 인덱스 설정
     
     - Pinecone API를 통해 벡터 데이터를 저장할 새로운 인덱스 생성
     
     - 코사인 유사도를 기준으로 검색이 가능한 인덱스 설정

- app.py : 사용자 질문 -> Pinecone 검색 -> 문서 -> LLM 응답 -> 사용자 반환
  
  1. Pinecone 인덱스 설정
     
     - Pinecone API key를 사용하여 클라이언트를 설정하고 새로운 인덱스 설정
     
     - 벡터 차원(dimension)과 코사인 유사도를 기준으로 데이터 검색 수행
  
  2. 데이터 검색 및 응답 생성_Pinecone 검색 설정
     
     - MMR(Mininal Marginal Relevance) 알고리즘을 사용하여 관련성 높은 문서를 검색
  
  3. 데이터 검색 및 응답 생성_Retrieval QA 구성
     
     - 문서 검색 결과를 기반으로 upstage llm 모델을 호출해 응답 생성
  
  4. 주요 API 엔드포인트
     
     - `/chat` POST 엔드포인트
       
       - 질문을 받아 검색 결과와 함께 응답 생성
       
       - (입력) 사용자 질문 message(JSON) & (출력) 생성된 응답 reply
     
     - `/health` GET 엔드포인트
       
       - 서버의 상태 확인을 위한 엔드포인트
       
       - (출력) 서버 상태


