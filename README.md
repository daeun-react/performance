## 웹 성능 최적화

---

## 분석툴

1. 크롬 Network 탭
2. 크롬 Performance 탭
3. 크롬 LightHouse 탭
4. webpack-bundle-analyzer

---

## 로딩 성능 최적화

### 1. _이미지 사이즈 최적화_

> - 보여질 이미지의 2배 정도 크기를 가져오는 것이 적당하다
> - Image CDN 기능을 이용하자 (사이즈조정)

### 2. _Code Split / Component Preload_

> - 불필요한 코드 또는 중복 없이 적절한 사이즈의 코드가 적잘한 타이밍에 로드될 수 있도록 하는 것
>   > - (React) import {Suspense, Lazy} from "react"
>   > - (단점) 처음 렌더링 시 빨라진다는 장점이 있지만, 해당 페이지에 들어갈 때 리소스를 받아 늦어질 수 있다
>   > - (해결) Component Preload
>   >
>   > 1. 버튼 위에 마우스를 올렸을 때(onMouseEnter event)
>   > 2. 처음 페이지가 로드되고 모든 컴포넌트의 마운트가 끝났을때 미리 로딩을 시켜서 해결(useEffet)

### 3. _이미지 Preload_

> ```javascript
> useEffect(() => {
>   const image = new Image();
>   image.src = "src 주소";
> }, []);
> ```

### _4. 텍스트 압축_

> - Network 탭 > 파일의 헤더 > Content-Encoding을 통해 확인할 수 있다.
> - gzip / deflate
> - 무분별한 압축은 오히려 성능을 저하시킨다.
> - (2KB 이상 파일에 적용하는 것이 바람직)
> - serve -u 옵션

### _5. 텍스트가 너무 길면?_

> 전체 텍스트 길이를 다 들고 오지 말고, substring으로 보여질 부분만 가져오는 것이 효과적임

---

## 렌더링 성능 최적화

## _1. Bottleneck 성능 최적화_

> - Network 탭에서 연산량이 많은 작업 찾고, 불필요한 반복문 사용을 줄이자.
> - npm i --save-dev webpack-bundle-analyzer
> - npm i --save-dev cra-bundle-analyzer (CRA로 생성한 경우)

## _2. 애니메이션 최적화_

> - 일반적인 브라우저 렌더링 과정을 1~5를 걸친다.
>
> 1.  DOM/CSSOM Tree (생성)
> 2.  Render Tree (생성)
> 3.  Layout (위치, 크기계산)
> 4.  Paint (색칠)
> 5.  Composite (조합)

> _Reflow_
>
> - 1,2,3,4,5 모두 재실행
> - width/height 값 변경 시

> _Repaint_
>
> - 1,2,4,5 실행
> - Layout 생략 가능
> - color, background-color 변경 시

> _Reflow, Repaint를 피하려면?_ GPU 도움받기
>
> - 1,2,5 실행
> - Layout, Paint 생략 가능
> - transform, opacity 속성을 변경
