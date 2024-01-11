## 이미지 포맷

|           | PNG                                             | JPG                                              | WebP                                                        |
| --------- | ----------------------------------------------- | ------------------------------------------------ | ----------------------------------------------------------- |
| 예시      | <img src="./photo.png" />                       | <img src="./photo.jpg" />                        | <img src="./photo.webp" />                                  |
| 파일 크기 | 15.6MB                                          | 1.2MB                                            | 704KB                                                       |
| 압축 방법 | 무손실 압축                                     | 손실 압축                                        | 무손실/손실 압축                                            |
| 알파 채널 | 지원                                            | 지원X                                            | 지원                                                        |
| 요약      | 고화질 고용량, <br/> 투명도 정보를 얻을 수 있음 | 저화질 저용량, <br /> 투명도 정보를 얻을 수 있음 | 가장 효율적으로 압축되지만 브라우저에 호환되지 않을 수 있음 |

<br />

## 브라우저 호환

브라우저가 지원하는 이미지 타입에 맞춰 이미지를 노출시키기 위해 `<picture>` 태그를 사용할 수 있다.

```html
<picture>
  <source
    srcset="/media/cc0-images/surfer-240-200.jpg"
    media="(orientation: portrait)"
  />
  <img src="/media/cc0-images/painted-hand-298-332.jpg" alt="" />
</picture>
```

`<picture>` 태그는 n개의 `<source>` 요소와 1개의 `<img>`요소를 갖는다. 브라우저는 각 `<source>` 를 고려하여 가장 잘 매치되는 이미지를 보여준다. 만약 `<source>`에 매치되는 이미지가 없다면 `<img>`가 화면에 그려진다.

`<source>`는 `<picture>` 뿐만 아니라 `<video>`, `<audio>`에도 쓰인다. `srcset`, `media`, `type` 속성 등을 지정할 수 있다.

- srcset/src : 부모가 `<picture>`인 경우 srcset을 쓰고, 이외의 경우에는 src를 사용한다.
- media : 매치될 media query 지정한다.
- type : 매치될 리소스 타입을 지정한다.

<br />
<br />

---

#### Reference

프론트엔드 성능 최적화 가이드  
MDN
