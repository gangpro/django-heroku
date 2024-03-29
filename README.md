

# Django heroku 웹에 배포하기



1. 기존에 설치한 가상환경 삭제

2. 새롭게 가상환경 생성

3. django 설치

   ```
   pip install django
   ```

   

4. 추가로 설치한 모듈 설치
   bootstrap4, Ipython, django-allauth, 

   ```
   pip install django-bootstrap4
   pip install etccccc
   ```

   

5. 설치한 모듈 .txt 파일로 저장

   ```
   pip freeze > requirements.txt
   ```

   

6. 최상단 프로젝트 .gitignore 파일 코드 수정
   맨 마지막에 아래 내용 추가 

   ```
   # Text backup files
   *.bak
   
   # Database
   *.sqlite3
   
   # 환경 설정 관련 파일
   .env
   ```



7. github repository에 remote, add, push 하기

   ```
   만약 깃 리모트 변경하고 싶다면
   ➜  DjangoForm git:(master) rm -rf .git
   
   ➜  DjangoForm git init
   
   /Users/kang/Documents/PycharmProjects/DjangoForm/.git/ 안의 빈 깃 저장소를 다시 초기화했습니다
   
   ➜  DjangoForm git:(master) ✗ git remote add origin https://github.com/kangpro404/DjangoForm.git
   ```

   

8. 토큰값, 보안정보 숨기는 방법 python-decouple 설치

   ```
   pip install python-decouple
   ```

   
   
9. 최상단에 .env 파일 생성

   ```
   # .env
   SECRET_KEY=
   DEBUG=
   TELEGRAM_API_KEY=
   ```

   

10. 시크릿 키와 디버그 값 변경

    ```
    django_form - settings.py 코드
    
    
    import os
    from decouple import config  # .env 안에 환경변수를 사용하게 함
    
    (기존) DEBUG = True
    ->
    (변경) DEBUG = config('DEBUG')
    
    
    (기존) SECRET_KEY = 'm@yb&y=6w=a-fc3en!8!e*^m$5-q@ld0xq+#cfe@he1hb(bn6m'
    ->
    (변경) SECRET_KEY = config('SECRET_KEY')
    ```

11. deploy이 하기전 checklist를 확인 할 수 있음

    ```
    python manage.py check --deploy
    ```

    

12. brew에서 postgresql 설치

    ```
    brew install postgresql
    ```

    

13. heroku 모듈 통해서 배포할 예정

    ```
    pip install django-heroku
    ```

    

14. 해로크 관련 django_form - settings.py 코드 수정

    ```
    STATIC_URL = '/static/'
    
    STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
    
    # settings.AUTH_USER_MODEL
    AUTH_USER_MODEL = 'accounts.User'
    
    LOGIN_REDIRECT_URL = 'boards:index'
    
    # Configure Django App for Heroku.
    import django_heroku
    django_heroku.settings(locals())
    ```

    

15. 웹이 구동되는데 필요한 프로세스 목록 적기
    Procfile 파일 생성

    ```
    web: gunicorn django_form.wsgi --log-file -
    
    # 파일 내에 주석 달면 안됨
    # web: gunicorn <프로젝트이름>.wsgi --log-file -
    # django-admin startproject django_form . 에서 썼던 프로젝트 이름
    ```

    

16. django와 함께 사용되는 heroku에서 추천하는 http 서버

    ```
    pip install gunicorn
    ```

    

17. requirements.txt 파일 생성

    ```
    pip freeze > requirements.txt
    ```

    

18. python 버전 확인

    ```
    (venv) ➜  DjangoForm git:(master) ✗ python -V
    Python 3.7.3
    ```

    

19. 프로젝트 최상단에 runtime.txt 파일 생성

    ```
    python-3.7.3
    ```

    

20. Heroku 가입하기
    https://www.heroku.com/

21. Heroku 로그인 접속
    https://www.heroku.com/home
    https://devcenter.heroku.com/articles/heroku-cli

22. Heroku cli 설치
    터미널에서 

    ```
    brew tap heroku/brew && brew install heroku
    ```

    

23. 터미널에서 heroku 실행해보기
    아래와 같이 뜨면 설치가 잘 된거임.

    ```
    (venv) ➜  DjangoForm git:(master) ✗ heroku
    CLI to interact with Heroku
    
    VERSION
      heroku/7.25.0 darwin-x64 node-v11.14.0
    
    USAGE
      $ heroku [COMMAND]
    
    COMMANDS
      access          manage user access to apps
      addons          tools and services for developing, extending, and operating your app
      apps            manage apps on Heroku
      auth            check 2fa status
      authorizations  OAuth authorizations
      autocomplete    display autocomplete installation instructions
      buildpacks      scripts used to compile apps
      certs           a topic for the ssl plugin
      ci              run an application test suite on Heroku
      clients         OAuth clients on the platform
      config          environment variables of apps
      container       Use containers to build and deploy Heroku apps
      domains         custom domains for apps
      drains          forward logs to syslog or HTTPS
      features        add/remove app features
      git             manage local git repository for app
      help            display help for heroku
      keys            add/remove account ssh keys
      labs            add/remove experimental features
      local           run heroku app locally
      logs            display recent log output
      maintenance     enable/disable access to app
      members         manage organization members
      notifications   display notifications
      orgs            manage organizations
      pg              manage postgresql databases
      pipelines       groups of apps that share the same codebase
      plugins         list installed plugins
      ps              Client tools for Heroku Exec
      psql            open a psql shell to the database
      redis           manage heroku redis instances
      regions         list available regions for deployment
      releases        display the releases for an app
      reviewapps      disposable apps built on GitHub pull requests
      run             run a one-off process inside a Heroku dyno
      sessions        OAuth sessions
      spaces          manage heroku private spaces
      status          status of the Heroku platform
      teams           manage teams
      update          update the Heroku CLI
      webhooks        setup HTTP notifications of app activity
    ```

    

24. 터미널에서 heroku login
    중간에 login 엔터하기 
    인터넷에서 로그인 성공하면 아래와 같이 뜸

    ```
    (venv) ➜  DjangoForm git:(master) ✗ heroku login
    heroku: Press any key to open up the browser to login or q to exit: 
    oginOpening browser to https://cli-auth.heroku.com/auth/browser/1a8aefff-b784-4902-8372-bc6d470e034d
    Logging in... done
    Logged in as email@email.com
    (venv) ➜  DjangoForm git:(master) ✗ ogin
    ```

    

25. heruku app 생성

    ```
    (venv) ➜  DjangoForm git:(master) ✗ heroku create djangoform
    Creating ⬢ djangoform... done
    https://djangoform.herokuapp.com/ | https://git.heroku.com/djangoform.git
    (venv) ➜  DjangoForm git:(master) ✗ 
    
    
    # heroku create <앱이름>
    ```

    

26. 시크릿 키 heroku에 알려주기

    ```
    heroku config:set SECRET_KEY=m@yb&y=6w=a-fc3en!8!e*^m$5-q@ld0xq+#cfe@he1hb(bn6m
    ```

    

27. 디버그 heroku에 알려주기

    ```
    (venv) ➜  DjangoForm git:(master) heroku config:set DEBUG=False
    Setting DEBUG and restarting ⬢ djangoform... done, v4
    DEBUG: False
    (venv) ➜  DjangoForm git:(master) 
    ```

    

28. heroku에 올리기

    ```
    git add .
    git commit -m "@@@@"
    git push heroku master
    ```

    

29. heroku open

    ```
    지금은 DB가 없어서 오류 발생
    ```

    

30. heroku 대시보드에서 migrate 하기

    ```
    heroku 대시보드 - More - Console 실행
    python manage.py migrate
    ```

    

31. 내가 만든 앱 오픈

    ```
    https://djangoform.herokuapp.com/admin/login/?next=/admin/
    ```

32. 슈퍼유저만들기

    ```
    heroku 대시보드 - More - Console 실행
    python manage.py createsuperuser
    
    사용자 이름 : 
    이메일 주소 :
    비밀번호 : 
    비밀번호(확인) :
    ```

    

