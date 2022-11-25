> __Note__     
> 해당 코딩 스탠다드는 비주얼 스튜디오 & 유니티  기준입니다.
> 또한 제가 개인적으로 선호하는 방식에 불과하니, 참고 바랍니다.

# Naming Convention
# 이름 짓기

## Class (클래스)
Pascal Case (파스칼 케이스)
```cs
public class ExampleClass
```
> 카멜 케이스와 달리 첫글자를 대문자로 시작합니다.

## Variable (변수)
Camel Case (카멜 케이스)
```cs
Component  exampleComponent
```
> 첫글자를 소문자로 시작하고, 다음 단어의 첫글자는 대문자로 작성합니다.

## Property (프로퍼티, 속성)
Pascal Case (파스칼 케이스)
```cs
public ExampleClass MyProperty { public get; set; }
```
## Method (메서드, 함수)
Pascal Case (파스칼 케이스)
```cs
public void ExampleMethod() {}
```
## Code Block (코드 블럭, 중괄호)
BSD 스타일을 사용합니다.
```cs
public class ExampleClass
{
	int number;
	
	public void ExMethod() 
	{
		
	}

}
```

## If - else if - Else
가끔 if-elseif-else 문을 사용하다보면 단일 문장 블록에도 중괄호를 굳이 추가해야할까 라는 생각이 들 때가 있습니다. 하지만 단일 문장 블록인 상태가 영원할 거란 보장은 없습니다. 따라서
단일 문장 블록이라 하더라도 중괄호를 추가하는게 추후를 위해 좋습니다.
```cs
if(true)
{
	return;
}
```
## 캡슐화
파생될 클래스의 경우는 멤버의 용도에 따라 설정자를 protected 혹은 private로 지정해야 합니다.
```cs
public int MyProperty { public get; protected set; }
```
## NameSpace
글로벌 네임스페이스 사용은 가급적 피해야 합니다.
형식 간의 중복이 생길 수 있으며, 제3자 패키지와 충돌이 발생할 수 있습니다.


## 일반적인 선호 스타일

### 조건문 조건
if문 등에 들어갈 조건 변수는 간결해야만 합니다. 다음과 같은 형식은 해석하는데 오랜 시간이 걸리며, 수정 작업에 어려움을 겪을 수 있습니다.
따라서 지역 변수를 사용하여 조건에 대해서 빠르게 이해할 수 있게끔 작성해야 합니다.
```cs
//BAD 
if(A && B || C == true)
{

}
---------------------------------------------
//GOOD
bool isGoodCode = A && B || C;
if(isGoodCode)
{

}
```

### 변수 형식
유니티는 GameObject 형식의 최상위에 가까운 객체 형식이 존재합니다. 
때론 그 편리함이 남용되었을 때, 혼란을 가져다줄 수 있습니다. 말 그대로 유니티 내의 어떤 GameObject든 들어갈 수 있으니까요. 예를 들어, 프리팹, 씬 오브젝트 등등.. 그 오브젝트가 어떤 역할의 오브젝트건 간에.
게다가 아래의 방식처럼 사용할 경우, 변수 형식은 GameObject이지만 코드 로직 상에서는 특정 형식의 컴포넌트를 가지고 있지 않을 경우 에러가 나기에, 허용되어선 안됩니다.
```cs
public class Example
{
	[SerializeField]
	GameObject example;
	
	
	void Start()
	{
		//BAD
		example.GetComponent<Example>().ExMethod();
	}
}
```

### 인스펙터 변수
유니티에서는 에디터 인스펙터 상에서 public 접근 지정자 혹은 [SerializeField] 헤더가 포함된 변수 혹은 프로퍼티를 수정할 수 있는 기능을 제공합니다.
프로퍼티를 에디터 인스펙터에서 수정할 수 있게 만드는 방법은 다음과 같습니다.
```cs
[field: SerializeFIeld]
public int MyProperty {get; set;}
```
### 이벤트 필드
이벤트 필드를 선언할 때에는 반드시 이벤트 콜백임을 알 수 있도록 필드명에 접두사로 On 등을 기입해 구분할 수 있도록 해야 합니다.
또한 이벤트 대리자 형식도 event 키워드로 캡슐화 할 수 있습니다.
> __Note__
> UnityEvent는 event 키워드를 사용할 수 없습니다. (delegate 형식이 아님.)

### 에러 로그
예외가 발생하였을 때, 단순히 return; 만 사용하여 처리하는 경우가 자주 있습니다.   
그러나, 해당 예외가 발생하는 경우가 에러를 발생시키는 경우, 반드시 에러 로그를 발생시켜야 합니다.
이때 로그도 치명도에 따라서 분류하여 남겨야 합니다. 치명적인 예외임에도 일반 로그 혹은 경고로만 남기는건 좋지 않습니다.
그렇지 않을 경우 개발자가 인지하기 힘든 치명적인 버그가 될 수 있습니다.

```cs
//BAD
if(Instance == null)
{
 return;
}


//GOOD
if(Instance == null)
{
 Debug.LogError("Instance is NULL");
 return;
}
```
