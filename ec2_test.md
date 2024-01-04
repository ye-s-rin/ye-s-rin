## AWS EC2 인스턴스 크기에 따른 비교


> 💡 **공통 조건**
- **아키텍처** : ARM
- **인스턴스** **패밀리** : t
- **인스턴스** **세대** : 4
- **스토리지** : gp3, 80GiB

- **인스턴스** **크기** : nano, small, micro


> 💡 **실험 내용**

- 인스턴스 크기만을 다르게 설정한 뒤 인스턴스 생성
- 각 인스턴스에 Anaconda 설치 후 가상 환경 생성
- 가상 환경에서 수강생의 Github 페이지를 Crawling하는 python 코드 실행
    - Crawling Code
- 5번씩 실행 후 평균 계산
        
        ```python
        import requests
        from bs4 import BeautifulSoup
        import os
        import time
        
        start = time.time()
        
        # 텍스트 파일에서 URL 한줄씩 읽기
        with open("github_profile.txt", "r") as f:
            urls = f.readlines()
        
        # 각 URL마다 정보 읽기
        for url in urls:
            url = url.strip() # 줄바꿈 제거
            response = requests.get(url)
        
            soup = BeautifulSoup(response.text, 'html.parser')
            text = soup.get_text()
        
            name = url.split("github.io/")[-1][:-1]
        
            with open(os.path.join('github', f"{name}.txt"), "w", encoding='utf-8') as f:
                f.write(text)
                f.close()
                print(name + "수집/저장 완료")
        end = time.time()
        
        print(f"{end - start} 초")
        ```
        

> 💡 **실험 결과**

- t4.nano : 2.0314 sec
- t4.micro : 2.078 sec
- t4.small : 2.1866 sec
