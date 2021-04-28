---
layout: post
title: Highcharts library 적응기
parent: 오답노트(trouble shooting)
nav_order: 4
last_modified_date: 2021-04-28 18:20
lastmod: 2021-04-28 18:20
---

## Highcharts library

### **개요**
[Highchart library](https://www.highcharts.com/)를 React와 사용하면서 여러가지 있었던 이슈? 문제?들에 대해 적어보려고 한다. 공식 API문서가 잘 되어있어서 보면 예제도 많이 있고 하지만.. 또 적용하다 보니 계속해서 여러가지 문제가 생기고 내가 원하는대로 잘 안될 때도 많았다.

### **개발환경**
* javascript/typescript
* React v16
* Highcharts library
* 필요한 라이브러리: [highcharts](https://www.npmjs.com/package/highcharts), [@types/highcharts](https://www.npmjs.com/package/@types/highcharts/), [highchart-react-official](https://www.npmjs.com/package/highcharts-react-official)

### **사용방법**
간단하게 HighchartsWrapper 컴포넌트를 만들어서 사용할 수 있다.
```tsx
import * as React from 'react'
import * as Highcharts from 'highcharts'
import HighchartsReact from 'highcharts-react-official'
import HC_more from 'highcharts/highcharts-more'
import heatMap from 'highcharts/modules/heatmap'

HC_more(Highcharts)
heatMap(Highcharts)

interface PropsType = {
  options: Highcharts.Options;
}

const HighchartsWrapper: React.FC<PropsType> = ({ options }: PropsType) => {
  return (
    <HighchartsReact
      highcharts={Highcharts}
      options={options}
    />
  )
}
```
* heatmap과 highcharts-more도 필요해서 추가해주었다.

### **Trouble Shooting**
1. 문제점: A와 B를 나타내는 그래프가 있는데, 이걸 A->B로 바꿔도 제대로 바꿔지지 않았다. 마치 A에 B를 머지한 것처럼 그래프가 그려졌다. 
    * 이 문제는 highchart-react-official에서의 default 설정값이 `immutable={false}` 였기때문에 나타난 문제였다.

    ```tsx
    <HighchartsReact
      highcharts={highcharts}
      options={options}
      immutable={true}
    />
    ```
    * immutable은 props가 업데이트 되었을 때 re-initialize시켜주는 것이다. 즉, 업데이트시 그래프를 다시 그려주기 위해 사용하는 설정값이다.

2. 문제점: 원하는대로 legend의 grid를 그리고 싶다.
    * 가끔 하이차트가 맘대로 그려줘서 짜증날 때가 있었다. 이것도 그 중 하나.
    * [colorAxis](https://api.highcharts.com/highcharts/colorAxis): 컬러로 뭔가 값을 표현해야할 때 쓰는데, 대부분 matrix 차트를 사용할 때 많이 쓴다.
    * [stops](https://api.highcharts.com/highcharts/colorAxis.stops): 컬러 그라데이션을 내가 지정한 색으로 하고싶을 때 쓴다.
    * [tickPositions](https://api.highcharts.com/highcharts/colorAxis.tickPositions): 축(x, y, color)에 해당하는 grid 표시를 내 맘대로 할 수 있게 해준다.
    ```typescript
    const options: Highcharts.Options = {
      colorAxis: {
        stops: [
          [0, '#000000'],
          [0.5, '#eeeeee'],
          [1, '#ffffff'],
        ],
        tickPositions: [0, 0.25, 0.5, 0.75, 1],
        min: 0,
        max: 1,
      },
    }
    ```

3. 문제점: 타입스크립트 사용해야하는데, `Highcharts.Options`의 거의 모든 속성들이 Optional로 걸려있어서 이게 어지간히 짜증나는게 아니다. 계속 undefined될 경우를 처리해주어야 하고 series의 경우 타입유추가 조금 힘들어지는 상황이 발생한다.
    * 그래서 내가 새로 ChartOptions type을 만들어서 사용하고 있다.

    ```typescript
    import { Options, SeriesOptionsType } from 'highcharts'

    export interface ChartOptions<T extends SeriesOptionsType> extends Options {
      chart: ChartOptions,
      xAxis: XAxisOptions[];
      yAxis: YAxisOptions[];
      series: T[];
    }
    ```
    * 내 어플리케이션에서 무조건 `options`에 사용한다면, `ChartOptions`에 required로 명시해준다.
    * Series는 데이터값이 들어가는 속성이기 때문에 `T[]`라는 타입으로 정의해주면, 나중에 사용할 때 `const options: ChartOptions<SeriesAreaOptions>={...}`로 사용할 수 있다. 그러면 as를 이용한 type assertion을 일일이 해주지 않아도 된다.

4. 문제점: 우리 어플리케이션에 (모달창에서)크게보기 기능이 있는데, 이게 원래는 링크걸리듯이 되야하는데, 이게 안된다....ㅠㅠ 
  * 아직 해결책 못찾음. [비슷한 문제: 참고](https://stackblitz.com/edit/highcharts-cloning-chart-7pqgge)