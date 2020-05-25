# AJAX

## 코드 작성방법

JQuery 에서의 Ajax 사용방식 아래와 같습니다.

```javascript
$.ajax({
    url: '',
    method: 'GET or POST or PUT or DELETE',
    contentType: '',
    dataType: '',
    data: { ... },
    async: true or false,
    success: function (data) { ... },
    error: function (e) { ... }
})
```

## 필요한 파라미터



| No | Name | Data Type | Description |
| :---: | :---: | :---: | :--- |
| 1 | url | string | 요청을 보낼 URL 주소 |
| 2 | method | string | HTTP 요청 방식 |
| 3 | contentType | string | 서버가 받을 데이터의 타입 |
| 4 | dataType | string | 서버로부터 받을 데이터 타입 |
| 5 | data | object | 요청을 보낼 URL 에 필요한 파라미터를 담아서 보내주는  |
| 6 | async | boolean | 비동기 처리 여부 \(동기: false, 비동기: true\) |
| 7 | success | function | 요청이 성공할 시 실행되 함수 |
| 8 | error | function | 요청이 실패할 시 실행되는 함수 |

## Module 패턴

모듈은 총 4가지 영역으로 나눌 수 있습니다.

| No | Name | Description |
| :---: | :---: | :--- |
| 1 | 매개변수 | 모듈 생성시 필요한 파라미터 |
| 2 | 지역변수 | 모듈 내에서만 쓰이는 변수 |
| 3 | private 함수 | 모듈 내에서만 쓰이는 함수 |
| 4 | public  함수 | 모듈에서 제공하는 함수로서, 모듈을 사용하는 경우에 제공되는 함수 |

```javascript
var Http = function( /* 매개변수 */ ){
    
    /* 지역변수 */
    var hello1 = 'hello1';
    var hello2 = 'hello2';
    
    /* private 함수 */
    function decoration(message){
        return '!!' + message + '!!';
    }
    
    /* public 함수 */
    return {
        popup1: function(){
            alert(hello1);
        },
        popup2: function(){
            alert(hello2);
        }
    }
}
```

## HTTP Module 만들기

위에서 언급한 총 4가지의 영역에 따라서 아래와 같이 모듈을 정의합니다.

option 은 AJAX 의 기본적인 파라미터를 의미합니다. url / method / dataType / data / success / error 등이 있습니다.

baseUrl 은 http://api.sample.com/users,  http://api.sample.com/user/1 과 같이 쓰이는 경우에 http://api.sample.com 이라는 공통 url 주소를 의미합니다.

ajax 함수는 공통부분을 따로 빼서 정의하였고, ajax 함수를 구체적으로 정의한 get / post / put / delete 함수를 public 함수로 정의하여 외부에서 REST API 형태로 이용할 수 있도록 작성하였습니다.

| No | Name | Variable | Description |
| :---: | :---: | :---: | :--- |
| 1 | 매개변수 | options | AJAX 파라미터 \(url, method, dataType ...\) |
| 2 | 지역변수 | baseUrl | API의 기본 URL 주소 \(**http://api.sample.com/**users\) |
| 3 | private 함수 | ajax | ajax 함수의 공통부분만 따로 정의한 private 함수 |
| 4 | public  함수 | get, post, put, delete | ajax 함수를 구체적으로 정의한 public 함수 |

```javascript
var Http = function(options){
    var baseUrl = options.baseUrl;

    function ajax(url, data){
        options.url = baseUrl + url;
        options.data = data;
        return {
            then: function(success){
                options.success = success;
                return {
                    catch: function(error){
                    options.error = error;
                    $.ajax(options);
                }
            }
        }
    }
    
    return {
        get:function(url, data, async){
            options.async = (async === undefined) ? false : async;
            options.method = 'GET';
            return ajax(url, data);
        },
        post:function(url, data, async){
            options.async = (async === undefined) ? false : async;
            options.method = 'POST';
            return ajax(url, data);
        },
        put:function(url, data, async){
            options.async = (async === undefined) ? false : async;
            options.method = 'PUT';
            return ajax(url, data);
        },
        delete:function(url, data, async){
            options.async = (async === undefined) ? false : async;
            options.method = 'DELETE';
            return ajax(url, data);
        }
    }
}
```

## HTTP Module 사용하기





```javascript
var WEATHER = new Http({
  baseUrl:'https://apis.weather.com/',
  method:'',
  async:false,
  contentType: "application/json",
  dataType: 'json',
  data: {},
  beforeSend: function(req) {
    req.setRequestHeader('Authorization', 'Bearer ' + WEATHER_CSRF_TOKEN);
  },
  success: function(e){},
  error: function(e){}
});
```

















