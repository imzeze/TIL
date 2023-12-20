# CSS

## text-size-adjust

**text-size-adjust** 속성은 모바일 브라우저에서 적용되는 text inflation algorithm(텍스트 팽창 알고리즘)을 제어한다.

```css
html {
  text-size-adjust: none;
  text-size-adjust: auto;
  text-size-adjust: 80%;
}
```

| 속성               | 의미                                                           |
| ------------------ | -------------------------------------------------------------- |
| none(default)      | 화면의 크기에 맞춰 조정하지 않는다.                            |
| auto               | 화면의 크기에 따라 크기를 자동으로 조정한다.                   |
| &lt;percentage&gt; | 텍스트 크기를 백분율로 지정하여 팽창 알고리즘을 활성화 시킨다. |

```css
html {
  -webkit-text-size-adjust: none; /* 구글, 사파리 */
  -moz-text-size-adjust: none; /* 파이어폭스 */
  text-size-adjust: none;
}
```
