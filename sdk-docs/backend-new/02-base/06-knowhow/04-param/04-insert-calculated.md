---
sidebar_label: 연산 데이터 삽입
---

# AddCalculation
public void **AddCalculation**(string **column**, GameInfoOperator **operator**, int **number**);  
public void **AddCalculation**(string **column**, GameInfoOperator **operator**, long **number**);  
public void **AddCalculation**(string **column**, GameInfoOperator **operator**, double **number**);

## 파라미터

| Value        | Type           | Description  | 
| :------------ |:-------------| :----- | 
| column   | string | 연산 작업할 column 명(key 값) | 
| operator   | GameInfoOperator(enum) | 연산 작업할 연산 | 
| number | int/long/double | 연산 작업할 값 |
> double형 데이터의 경우 반올림이 될 수 있습니다.  

### GameInfoOperation(enum)

| Value         | Description  | 
| :------------ | :----------- |
| addition | 저장된 값에 number 만큼 더하기 연산 수행 |
| subtraction | 저장된 값에 number 만큼 빼기 연산 수행 |

## 설명
UpdateWithCalculation 함수의 인자로 param을 넘겨줄 때 어떤 컬럼에 대해 연산을 진행할지 선언하기 위한 함수입니다.  
* **Key는 숫자로 시작할 수 없습니다.** 숫자로 시작하는 경우, 경고가 출력되며 Param에 추가되지 않습니다.  


## Example

```js
Param param = new Param();

// 덧셈 명령
param.AddCalculation("column1", GameInfoOperator.addition, 20);
// 뺄셈 명령
param.AddCalculation("column2", GameInfoOperator.subtraction, 20);

```


