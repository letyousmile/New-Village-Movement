## Intent, Bundle
Intent 란 현재 액티비티에서 다른 액티비티로 혹은 다른 애플리케이션으로 보내주는 역할을 하는 것, 이런 식으로 현재 화면을 안보여주고 다른 화면을 보여주고 싶을떄 사용함. 그리고 다른 화면으로 넘기는 과정에서 데이터를 전달 해주고 싶을때 간단한 형식의 데이터를 Intent에 실어서 보낼수 있다.

Bundle은 Intent에 간단한 형식의 데이터만을 실을수 있기에 보다 다양한 데이터 즉 객체를 실기위한 택배상자와 같은 역할을 한다. 객체나 다른 리스트, 맵 들을 Bundle에 실고 이 Bundle을 Intent에 다시 실어서 다른 곳으로 보낸다.


## 명시적/암시적 intent
* 명시적 인텐트
    - 클래스명을 직접 부여해서 실행한다. 한번에 하나만 가능하다.

* 암시적 인텐트(8버전 부터 일부를 제외한 모든 Broadcast receiver 실행 불가)
    * **intent-filter**에 등록된 이름을 사용하는 방법, 8버전 이후에는 암시적인텐트를 통한 broadcast receiver를 사용하려면 그 broadcast receiver를 가진 애플리케이션이 동작 중이어야 하기에 **intent-filter**에 등록을 한 후 실행해야한다.

## intent-filter
다른 어플리케이션의 activity를 실행하고자 할때 intent-filter에 등록된 이름을 이용하여 실행가능하다. 만약 이름이 같은 activity가 2개 이상일 경우 선택하는 다이얼로그가 화면에 나타난다.

## activity action
```kotlin
var uri = Uri.parse("geo:xx.xxxx,yy.yyyyy")
var intent = Intent(Intent.ACTION_VIEW, uri)//uri의 값이 도메인 주소면 브라우저 실행, 위와 같이 좌표면 지도 실행 등등..
startActivity(intent)

var uri = Uri.parse("tel:wwwxxxxyyyy")
var intent = Intent(Intent.ACTION_DIAL, uri)//DIAL을 통해 dial 창을 띄움
startActivity(intent)

var uri = Uri.parse("tel:wwwxxxxyyyy")
var intent = Intent(Intent.ACTION_CALL, uri)//CALL을 통해 전화 걸수있음, 단 permission을 추가해야함
startActivity(intent)
```

## Serializable, Parcelable
둘다 객체를 직렬화 하여 바이트형태로 변경하는 방법, 우선 Serializable은 Java에 존재하는 인터페이스이며 직렬화 하는 과정에서 Reflection이 일어난다.

 그와 다르게 Parcelable은 안드로이드에서 만들어진 인터페이스로 Reflection이 발생하지 않게 처리하였다. 다만 구현에 귀찮음이 존재한다. 두개의 속도를 비교했을때 Parcelable이 더 빠르다.



## Parcelable
객체를 intent에 실어서 전달할때 전달을 받은 쪽에서 객체를 복원을 하기위해 정보를 가지 부분을 의미한다

```kotlin
companion object{//static 맴버
 @JVMField // 안드로이드 오브젝트가 사용할떄
 val CREATOR:Creator<Class> = object:Parcelable.Creator<Class>{
  override fun createFromParcel(parcel:Parcel?): Class{//객체를 복원할때 사용
   val test = Class()
   test.data = parcel.readInt()
   return test	
  }
   
  override fun newArray(param:Int):Array<Class>{//만약 배열이 넘어왔을때 이부분이 호출되서 배열을 만들고 위의 함수를 호출하여 객체를 복원함
   return arrayOfNulls<Class>(params)//내부에 널로 채워진 params 크기 만큼의 배열을 만듬
  }
}

//parcle 이 클래스가 가진 맴버의 값을 저장하고 있는다
override fun writeToParcel(parcle:Parcel?, int:Int){
  parcle.writeint(p1)
  parcle.writeString(p2)
}

override fun describeContents(): Int{
  return 0
}

```

