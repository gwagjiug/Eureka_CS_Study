### Reset.css VS Nomalize.css
---
* `reset.css` 는 브라우저의 기본 스타일을 완전히 제거하여 모든 브라우저 환경에서 동일한 스타일을 제공할 수 있도록 설계된 CSS 파일을 말한다.

* 이 파일은 다양한 HTML 요소의 기본 스타일을 리셋하여, 스타일링을 새로 시작할 수 있는 깨끗한 상태를 제공한다.

* 쉽게 생각해보자. chrome,safari,ie 등 각 브라우저마다 HTML 요소의 기본 스타일을 가지고 있기 때문에, CSS 로 스타일링을 적용할 때 이러한 특징이

* 동일한 스타일 적용을 방해할 수 있다. 때문에 이를 해결하기 위해서 기본 스타일을 `백지`로 만들기 위한 초기화 기법이라고 생각하면 된다.

---

#### Reset.css

* `Eric Meyer CSS Reset` : 말 그대로 Eric Meyer가 만든 CSS Reset 파일이다.

> 기본 HTML 페이지 (Reset 전)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Before CSS Reset</title>
    <style>
        /* 기본 스타일 */
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            color: #333;
            margin: 20px;
        }

        h1 {
            font-size: 2em;
            margin-bottom: 0.5em;
        }

        p {
            line-height: 1.6;
            margin: 0 0 1em;
        }

        ul {
            padding: 0;
            list-style-type: disc;
        }
    </style>
</head>
<body>
    <h1>제목</h1>
    <p>내용</p>
    <ul>
        <li>List item 1</li>
        <li>List item 2</li>
    </ul>
</body>
</html>
```

![image](https://github.com/user-attachments/assets/b7b8bebd-e023-4aa6-8f94-917615ad81d5)

> Eric Meyer's CSS Reset 적용 후

```css
/* Eric Meyer’s CSS Reset */
html, body, div, span, applet, object, iframe,
h1, h2, h3, h4, h5, h6, p, blockquote, pre,
a, abbr, acronym, address, big, cite, code,
del, dfn, em, font, img, ins, kbd, q, s, samp,
small, strike, strong, sub, sup, tt, var,
b, u, i, center,
dl, dt, dd,
ol, ul, li,
fieldset, form, label, legend,
table, caption, tbody, tfoot, thead, tr, th, td {
  margin: 0;
  padding: 0;
  border: 0;
  font-size: 100%;
  font: inherit;
  vertical-align: baseline;
}

article, aside, details, figcaption, figure,
footer, header, hgroup, main, mark, nav, section,
summary {
  display: block;
}

body {
  line-height: 1;
}
```
![image](https://github.com/user-attachments/assets/274f4607-bcf3-4068-9712-e7d61c47228a)

* 속성이 초기화 되어 body에 기본으로 설정된 8px의 여백이 사라지고 Heading 태그는 모두 동일한 크기로 나오게 된다.

---

#### CSS Normalize

* 다음은 `CSS Normalize` 이다 CSS Reset 과의 차이점은 CSS Reset은 모든 속성 값을 0으로 초기화하는 방식이라면, CSS Normalize는 모든 값을 0으로 초기화 하는 것이 아니라

* 필요한 부분은 유지하면서, 서로 다른 브라우저 간의 스타일 차이를 최소화 하는 CSS 파일을 말한다. 기본적인 브라우저 스타일을 유지하면서도 일관된 스타일을 보장하려고 한다.

 ```html
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Before CSS Reset</title>
    <link rel="stylesheet" href="normalize.css">
    <style>
/* Normalize.css*/
html {
    line-height: 1.15; /* 1 */
    -webkit-text-size-adjust: 100%; /* 2 */
  }
  body {
    margin: 0;
  }
  main {
    display: block;
  }
  h1 {
    font-size: 2em;
    margin: 0.67em 0;
  }
  hr {
    box-sizing: content-box; /* 1 */
    height: 0; /* 1 */
    overflow: visible; /* 2 */
  }
  pre {
    font-family: monospace, monospace; /* 1 */
    font-size: 1em; /* 2 */
  }

  a {
    background-color: transparent;
  }
    </style>
</head>
<body>
    <div class="App">
        <div class="Box"></div>
        <h1>이건 Heading 1</h1>
        <h2>이건 Heading 2</h2>
        <h3>이건 Heading 3</h3>
        <p>요건 P</p>
    </div>
</body>
</html>
```

```css
.Box {
    background-color: #4285F4;
    height: 100px;
    width: 100px;
    border: 3px solid;
  }
```
![image](https://github.com/user-attachments/assets/775cfd48-2295-46d4-ae2d-4120010bf135)

* CSS Reset과 같이 여백은 사라졌지만 Heading 태그는 살아있는 모습을 볼 수 있다.

[Normalize.css 원본 링크](https://github.com/necolas/normalize.css)


--- 
### CSS Reset

* CSS Reset은 웹 개발자가 완전히 사용자 정의 스타일을 적용하고 싶을 때 유용하게 쓸 수 있으며, 모든 브라우저에서 동일한 출발점을 제공한다.

* 즉, 기본 스타일을 제거하고 새로 스타일을 정의해야 하는 경우 CSS Reset이 유용한 것.

* 모든 브라우저에서 스타일이 동일하게 보장되므로,  디자인의 일관성을 유지할 수 있다는 단점이 있지만, 작업량이 많아진다.. 

### Nomalize.css

* Nomalize.css 는 브라우저 간 기본 스타일 차이를 최소화하고, 일관된 스타일을 제공하고자 할 때 유용하다. 기본 스타일이 그대로 유지되므로, 특정 디자인 요구 사항을 충족 시키기 어려울 수 있음

* 즉, 특정 디자인 가이드라인이 존재하는 팀 혹은 프로젝트 요구 사항이 있을 경우 CSS Reset을 선택할 수 있지만,

* 대부분 `Normalize.css` 를 선호한다. 기본 스타일을 일부 유지하면서도 브라우저 간 일관성을 유지할 수 있기 때문이다.

### 한줄요약

* `reset CSS` 는 CSS의 style을 초기화 시키는 것이라면,

* `normalize CSS` 는 브라우저의 사용하기 좋은 기본값은 유지하면서 브라우저간의 차이점을 수정하는 코드이다.

* normalize CSS는 CDN 기법을 사용하여 head 영역에 link 태그로 삽입할 수 있으며 부분적으로 개선하기 때문에 성능면에서 유리.

