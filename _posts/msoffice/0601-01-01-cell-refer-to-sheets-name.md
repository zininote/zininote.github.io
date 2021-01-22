---
layout: post
title: "CELL: Sheet 이름을 불러오는 함수식"
updated: 2021-01-20
tags: [msoffice,excel]
---

## Sheet 이름을 불러오는 함수식

말 그대로 Sheet 이름을 불러오는 함수식이다.

```excel
= MID(CELL("filename", 셀), FIND("]", CELL("filename", 셀))+1, 256)
```
{:.excel}

![그림00](/img/msoffice/excel-0601-01-01-00.png)

함수식의 `셀` 부분에는 이름을 얻고자하는 Sheet 안의 셀을 아무거나 지정해주면 된다. Sheet 이름을 바꾸면 함수의 결과도 자동으로 달라지는 것을 확인할 수 있다.

주의할 점이 있는데, **반드시 파일이 저장된 상태에서 함수를 사용**해야한다. 예를들어, 엑셀 문서를 새로 만들고 저장을 한번도 안한 상태에서 함수식을 사용하면 에러를 발생시킨다. 이 때에는 저장하고 F9 키를 눌러서 새로고침을 해줘야 한다.

에러가 나는 이유는 CELL 함수 때문인데, 이 함수의 첫번째 인수에 "filename" 을 넣으면, 폴더경로, 파일이름, Sheet 이름이 모두 포함된 정보를 반환한다. (이 정보를 FIND, MID 함수로 Sheet 이름만 발라내는 구조다.) 따라서 생성한 뒤 한번도 저장을 안한 엑셀문서라면 당연히 파일정보라는 것이 있을 수 없으므로 에러가 나오게 된다.

## VBA 사용자함수로 구현

같은 기능을 VBA 사용자함수로도 구현할 수 있다. 아래는 그 코드이다.

```vbnet
Function SheetName(c As Range) As String
    Application.Volatile
    SheetName = c.Parent.Name
End Function
```
{:.vba}

```excel
= SheetName(셀)
```
{:.excel}

`셀` 부분에는 이름을 얻고자 하는 Sheet 의 아무셀이나 지정하면 된다.

참고로 위 사용자함수는 파일로 저장된 상태가 아니어도 사용이 가능하다.