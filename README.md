## Volley
웹서버에 요청하고 응답을 받을 때는 HttpURLConnection을 사용할 수 있다.  
하지만 요청과 응답을 위한 코드의 양이 많은 데다가 스레드를 사용하면서 더 많은 코드를 넣어주게 된다.  
때문에 안드로이드에서 제공하는 HTTP 라이브러리를 사용해 쉽고 빠르게 네트워크 통신이 가능하다.  
여러 라이브러리중 대표적으로 Volley 라이브러리를 이용해 HTTP통신이 가능하다.

+ 사용방법  
  요첨(Request)객체를 만들고 이 요청 객체를 요청큐(RequestQueue)라는 곳에 넣어주면된다.  
  이후에는 요청큐가 알아서 웹서버에 요청하고 응답까지 받아 우리가 사용할 수 있도록 메소드를 호출해준다.

+ 장점
1. 스레드를 신경쓰지 않아도 됨  
2. 네트워크 요청 우선순위를 자동으로 관리   
3. 동시에 여러 네트워크 요청 가능  
4. 요청시 cache적용 여부를 의식하지 않아도 됨  

요청큐가 내부에서 스레드를 만들고 웹서버에 요청하고 응답을 받고나면 메인 스레드에서  
결과를 처리할 수 있도록 만든 후 본인이 설정한 리스너의 메소드를 호출해준다.  
(따라서 화면에 결과를 표시할 때 핸들러를 사용하지 않아도 된다.)

+ Volley 사용
1. 외부라이브러리 이므로 build.gradle에 추가, 인터넷권한 부여
2. 버튼클릭시 요청 객체를 만들고 요청 큐에 넣어줌
3. 본 예제에서 요청 객체는 문자열을 주고받기 위해 StringRequest 클래스를 이용해 만듦
  (Volley라이브러리가 제공하는 다른 유형의 요청 객체 가능), 일반적으로는 StringRequest 클래스만으로도 충분함
```JAVA
StringRequest request = new StringRequest(
        Request.Method.GET,
        url,
        new Response.Listener<String>() {
            @Override
            public void onResponse(String response) {
                println("응답 -> " + response);

                processResponse(response);
            }
        },
        new Response.ErrorListener() {
            @Override
            public void onErrorResponse(VolleyError error) {
                println("에러 -> " + error.getMessage());
            }
        }
) {
    @Override
    protected Map<String, String> getParams() throws AuthFailureError {
        Map<String,String> params = new HashMap<String,String>();

        return params;
    }
};
```
요청 객체를 new 연산자로 만들 때는 네 개의 파라미터를 전달할 수 있다. 첫 번째 파라미터로는 GET 또는 POST 메소드를 전달한다.(요청 방식을 지정) 두 번째 파라미터로는 웹서버의 URL 정보를 전달한다.
 세 번째는 리스너 객체인데 응답을 성공적으로 받았을 때 이 리스너의 onResponse 메소드를 자동으로 호출해준다. 네 번째는 에러가 발생했을 때 호출될 리스너 객체이다.  
만약 POST 방식에서 전달할 요청 파라미터가 있다면 getParams 메소드에서 반환하는 HashMap 객체에 넣어준다. 이렇게 만든 요청 객체는 요청 큐에 넣어주는 것만 해주면 된다.  
```JAVA
AppHelper.requestQueue.add(request);
```
요청 큐는 앱이 시작되었을 때 초기화되어 있기만 하면 되고 한 번 만들어두면 계속 사용할 수 있다. 따라서 Application 클래스를 정의하고 앱에 등록하여 사용하는 경우에는 이 Application 클래스 안에 넣어둘 수도 있고 AppHelper와 같은 별도의 클래스를 만들어 그 안에 넣어둘 수도 있다.
AppHelper 클래스 안에는 static 키워드를 이용해 앱의 어느 곳에서도 접근할 수 있도록 만들어준다.  
```JAVA
public class AppHelper {
    public static RequestQueue requestQueue;
}
```

<img src="https://user-images.githubusercontent.com/63087903/115216886-e4e29c00-a13f-11eb-9e55-20574bc9b729.jpg" width="250" height="530">
