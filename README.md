# AJAX

## 코드 작성방법

JQuery 에서의 Ajax 사용방식 아래와 같습니다.

```javascript
$.ajax({
    url: '',
    method: 'GET or POST or PUT or DELETE',
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
| 3 | dataType | string | 받을 데이터 타 |
| 4 | data | object | 요청을 보낼 URL 에 필요한 파라미터를 담아서 보내주는  |
| 5 | async | boolean | 비동기 처리 여부 \(동기: false, 비동기: true\) |
| 6 | success | function | 요청이 성공할 시 실행되 함수 |
| 7 | error | function | 요청이 실패할 시 실행되는 함수 |

## HTTP 모듈 만들기

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





```javascript
var Http = function(options){
    var baseUrl = options.baseUrl;

    // Base AJAX
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
    
    // GET - select
    // POST - insert 
    // PUT - update
    // DELETE - delete
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









```javascript
var Http = function(options){
    var baseUrl = options.baseUrl;

    // Base AJAX
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
    
    // GET - select
    // POST - insert 
    // PUT - update
    // DELETE - delete
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

















