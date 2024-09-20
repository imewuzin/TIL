# 가상환경

: Python 애플리케이션과 그에 따른 패키지들을 격리해 관리할 수 있는 독립적인 실행 환경

1. 가상환경 `venv` 생성
   
   : `python -m venv venv`
   
   - 임의 이름으로 생성 가능하나 관례적으로 `venv` 이름 사용
   
   - `venv` 라는 이름의 가상환경 생성

2. 가상환경 활성화
   
   : `source venv/Scripts/activate`
   
   - 가상환경은 on/off 개념 (비활성화는 `deactivate`)
   
   - OS 마다 명령어 다름 - macOS는 `source venv/bin/activate`
   
   - `(venv)` 표시됨

3. 환경에 설치된 패키지 목록 확인
   
   : `pip list`

4. 설치된 패키지 목록 생성
   
   : `pip freeze > requirements.txt`
   
   - 현재 Python 환경에 설치된 모든 패키지와 그 버전을 텍스트 파일로 저장
   
   - `requirements.txt` : 생성될 파일 이름 (관례적으로 사용)
   
   - 패키지 목록 파일 주요 사항
     
     - 가상 환경의 패키지 목록을 쉽게 공유 가능
     
     - 프로젝트의 의존성을 명확히 문서화
     
     - 동일한 개발 환경을 다른 시스템에서 재현 가능
     
     - 활성화된 가상환경에서 실행해야 정확한 패키지 목록 생성
     
     - 시스템 전역 패키지와 구분 필요
   
   - 패키지 목록 기반 설치
     
     : `pip install -r requirements.txt`
     
     - 생성된 `requirements.txt`로 다른 환경에서 동일한 환경 구성하기

----

## Django 프로젝트 생성 및 서버 실행

1. 프로젝트 생성
   
   : `django-admin startproject firstpjt .`

2. 서버 실행
   
   : `python manage.py runserver`

---

## 앱 생성 및 등록

1. 앱 생성
   
   : `python manage.py startapp articles`
   
   - `articles`이름의 앱 생성
   
   - 앱의 이름은 복수형으로 지정하는 것을 권장

2. 앱 등록
   
   : `setting.py`의 `INSTALLED_APPS`에 앱 이름 추가해야 함.
   
   - 예시
     
     > ```python
     > # settings.py
     > 
     > INSTALLED_APPS = [
     >     'articles',
     >     .......
     > ]
     > ```
