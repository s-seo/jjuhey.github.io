---
layout: post
title: React Datepicker Customizing
nav_order: 85
last_modified_date: 2021-04-02 17:36
lastmod: 2021-04-02 17:36
---

# **React DatePicker 커스텀 컴포넌트 만들기**
## Version 업그레이드
회사에서 [react-datepicker](https://reactdatepicker.com/)를 이용하고 있다. 날짜의 resolution에 따라서 다른 캘린더를 띄워주고 싶은데, 그게 예전버전에서는 지원이 안되는 것 같아 업그레이드를 진행하였다.
(타입스크립트도 사용중이고 해당 라이브러리에서 지원하기 때문에 같이 버전업하였다.)
```shell
> yarn upgrade react-datepicker@^3.6.0
> yarn upgrade @types/react-datepicker@^3.1.8
```
버전에 대한 정보는 npm에 들어가면 볼 수 있는데 ([참고](https://www.npmjs.com/package/react-datepicker)) 여기에서 가장 최신 버전이면서 최근 다운로드수가 많은 버전을 선택해서 올려줬다. 아참! 또다른 주의해야 할 점은 node나 react버전에 따라서 지원하는 버전이 다를 수 있으므로 체크하는 것이 좋다.
```
React
We're always trying to stay compatible with the latest version of React. We can't support all older versions of React.

Latest compatible versions:

React 16 or newer: React-datepicker v2.9.4 and newer
React 15.5: React-datepicker v2.9.3
React 15.4.1: needs React-datepicker v0.40.0, newer won't work (due to react-onclickoutside dependencies)
React 0.14 or newer: All above React-datepicker v0.13.0
React 0.13: React-datepicker v0.13.0
pre React 0.13: React-datepicker v0.6.2
```
공식문서에 이렇게 친절하게 나와있으니 참고하면 된다.

* * *

## DatePicker Customizing
회사에서 사용하는 datepicker는 라이브러리 그대로를 사용하는 컴포넌트에 모두 그대로 import해와서 사용하였다. 그런데 이러면 공통적으로 설정해줘야하는 것을 모두 수동으로 해줘야하고, 사용하는 사람들마다 이 라이브러리에 대해 파악할 필요가 있는 등 여러가지 어려움이 있다.
그래서 우리회사에서는 이렇게 라이브러리를 사용해야 할 때, 한번 우리 커스터마이징 컴포넌트를 하나 생성해서 우리가 자주 사용하는 기능과 props 등을 정의해서 사용한다.
```typescript
import DatePicker from 'react-datepicker'
const DatePickerWrapper = ({ onChangeDate }: PropsType) => {
  const onChange = (date: Date) => {
    onChangeDate(date.toString())
  }

  return (
    <DatePicker
      onChange={onChange}
    />
  )
}
```

### **DatePicker Input 만들기**
공식문서에 가면 이런식으로 커스텀 Input을 만들 수 있도록 지원해준다.
```javascript
() => {
  const [startDate, setStartDate] = useState(new Date());
  const ExampleCustomInput = forwardRef(
    ({ value, onClick }, ref) => (
      <button className="example-custom-input" onClick={onClick} ref={ref}>
        {value}
      </button>
    ),
  );
  return (
    <DatePicker
      selected={startDate}
      onChange={date => setStartDate(date)}
      customInput={<ExampleCustomInput />}
    />
  );
};
```
reference를 이용해서 참조값을 전달해주도록 해야한다. 또한 타입스크립트를 사용중이라 타입을 설정해줘야 하는데,
```javascript
const CustomInput = React.forwardRef<HTMLInputElement, { value: any; onClick(): void }>(
  ({ value, onClick }, ref) => (
    <input className={buttonClass} onClick={onClick} ref={ref} value={value} disabled={disabled} />
  ),
)
```
타입을 추측하기가 애매해서, 일단 이렇게 하면 에러도 안나고 잘 작동하긴 한다🤣 그리고 `customInput={<CustomInput />}`으로 해주면 된다.


### **Click 제어하기: Show DatePicker**
datepicker만 있으면 문제가 없는데, timepicker까지 사용한다면, 문제가 발생한다. date를 클릭하면 팝업이 닫히게 된다. 최신 버전에서는 `showTimeSelect`는 문제가 없는데, `showTimeInput`에서는 어김없이 date를 클릭하면 팝업이 닫혀버린다.
그러니까 하고싶은건
1. date를 선택하면 팝업이 닫히지 않음
2. time을 선택할 때 팝업이 닫혀야 함

* `shouldCloseOnSelect={false}`: 기본적으로 팝업이 닫히지 않게 설정한다.
* DatePicker의 show 여부를 제어하기 위해 ref를 할당해야한다.
  ```javascript
  const [date, setDate] = useState(new Date());
  const calendar = useRef<DatePicker>(null)

  return (
    <DatePicker
      selected={date}
      onChange={handleDate}
      ref={calendar}
      shouldCloseOnSelect={false}
    />
  );
  ```
* `handleDate`에서 시간이 바뀌면 팝업이 닫히도록 설정했다.
  ```javascript
  const handleDate = (changedDate: Date) => {
      onChangeDate(changedDate)
      setDate(changedDate)

      if (showTimePicker && changeTimeOnly(date, changedDate)) {
        calendar.current?.setOpen(false)
      } else if (!showTimePicker) {
        calendar.current?.setOpen(false)
      }
    }

  function changeTimeOnly (originDate: Date, changeDate: Date) {
  const isSameTime = originDate.getTime() === changeDate.getTime()
  const changeTime = Math.abs(originDate.getDate() - changeDate.getDate()) < 1

  return !isSameTime && changeTime
  }
  ```

### **Range를 지원하도록 만들기**
* 위에서 제어했던 것처럼 show 제어를 위해 두 캘린더 모두 ref값을 던져줬고, state도 각각 만들어준다.
* isRange를 부모 컴포넌트에서 props로 받아서 range일때는 start calendar & end calendar를 둘다 볼 수 있도록 해준다.
```javascript
const [startDate, setStartDate] = React.useState<Date>(defaultDate || new Date())
const [endDate, setEndDate] = React.useState<Date>(defaultEndDate || defaultDate || new Date())
const startCalendar = React.useRef<DatePicker>(null)
const endCalendar = React.useRef<DatePicker>(null)

return (
  <div className='datepickerContainer'>
    <DatePicker
      selected={startDate}
      onChange={handleStartDate}
      customInput={<CustomInput value={startDate} onClick={openStartCalendar} />}
      ref={startCalendar}
      selectsStart={isRange}
      startDate={isRange ? startDate : undefined}
      endDate={isRange ? endDate : undefined}
    />
    {isRange && (
      <>
        <span className='between'>~</span>
        <DatePicker
          selected={endDate}
          onChange={handleEndDate}
          customInput={<CustomInput value={endDate} onClick={openEndCalendar} />}
          ref={endCalendar}
          selectsEnd
          startDate={startDate}
          endDate={endDate}
          minDate={startDate}
        />
      </>
    )}
  </div>
)
```

* * *
### **Format에 따라 calendar형태 자동으로 바꾸기**
dateFormat에 따라서 datepicker의 캘린더 형태(datepicker, monthpicker, yearpicker...)가 자동으로 설정될 수 있도록 하였다.
```typescript
let dateProps: Partial<ReactDatePickerProps> = { dateFormat: format }
switch (fomat) {
  case 'yyyy-MM-dd hh:mm':
    dateProps = {
      ...dateProps,
      showTimeInput: showTimeInput,
      showTimeSelect: !showTimeInput,
      timeIntervals: timeIntervals || 30,
    }
    break
  case 'yyyy':
    dateProps = {
      ...dateProps,
      showYearPicker: true,
    }
    break
  case 'yyyy-MM':
    dateProps = {
      ...dateProps,
      showMonthYearPicker: true,
    }
    break
  default:
    dateProps = {
      dateFormat: 'yyyy-MM-dd',
      showMonthDropdown: true,
      showYearDropdown: true,
      dropdownMode: 'select',
    }
}

return (
  <DatePicker
    {...dateProps}
    onChange={handleDate}
    selected={date}
  />
)
```
