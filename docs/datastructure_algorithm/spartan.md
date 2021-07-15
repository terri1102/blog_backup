## 웹 서비스 동작 원리
클라이언트 -- 서버
클라이언트(브라우저)의 역할: 서버에 요청해서 받아 온 것을 그대로 보여줌

서버에서는 요청받은 API를 통해 flask 서버에 있는 html,css,js 등 클라이언트로 보내줌

## HTML
* 뼈대: HTML
* 꾸미기: CSS
* 움직이기: Javascript
지도 넣을 때 js 사용

서버에 파일을 올려서 모든 사람들이 볼 수 있게!

- HTML은 크게 head와 body로 구성되며, head안에는 페이지의 속성 정보를, body안에는 페이지의 내용을 담습니다.
- head 안에 들어가는 대표적인 요소들: meta, script, link, title 등

    페이지의 속성을 정의하거나, 필요한 스크립트들을 부릅니다. 즉, 눈에 안 보이는 필요한 것들을 담는 것. 나중에 body 작업을 하면서 필요한 정보들을 넣어보겠습니다.

- body 안에 들어가는 대표적인 요소들!
    - **[코드스니펫] - HTML 기초**

        ```html
        <!DOCTYPE html>
        <html lang="en">

        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>스파르타코딩클럽 | HTML 기초</title>
        </head>

        <body>
            <!-- 구역을 나누는 태그들 -->
            <div>나는 구역을 나누죠</div>
            <p>나는 문단이에요</p>
            <ul>
                <li> bullet point!1 </li>
                <li> bullet point!2 </li>
            </ul>

            <!-- 구역 내 콘텐츠 태그들 -->
            <h1>h1은 제목을 나타내는 태그입니다. 페이지마다 하나씩 꼭 써주는 게 좋아요. 그래야 구글 검색이 잘 되거든요.</h1>
            <h2>h2는 소제목입니다.</h2>
            <h3>h3~h6도 각자의 역할이 있죠. 비중은 작지만..</h3>
            <hr>
            a 태그입니다: <a href="http://google.com/" target="_blank">하이퍼링크</a>
            <hr>
            img 태그입니다: <img src="https://www.google.com/images/branding/googlelogo/1x/googlelogo_color_272x92dp.png" />
            <hr>
            input 태그입니다: <input type="text" />
            <hr>
            button 태그입니다: <button> 버튼입니다</button>
            <hr>
            textarea 태그입니다: <textarea></textarea>
        </body>

        </html>
        ```

    이 외에도 아주 많으며, 개발자들도 모두 외우고 있지 않습니다. 필요할 때마다 찾아서 넣어보세요! 코딩은 `구글링`이 필수랍니다!

    **잠깐! 정렬의 중요성**
    코드의 정렬이 제대로 되어있지 않으면, 코드의 생김새를 파악할 수 없어 오류를 해결하기가 무척 어려워집니다. VSCode에선 ctrl+K+F(맥은 cmd+K+F)로 자동정렬이 가능하답니다! 

