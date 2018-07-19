# 8. Alamofire / Codable

## [Alamofire](https://github.com/Alamofire/Alamofire)
> Swift 에서 HTTP 네트워킹을 위한 대표적인 오픈소스 라이브러리
>
> * Request 와 Response 가 연계되는 함수제공
> * URL / JSON 형태 파라미터 인코딩
> * File / Data / Stream / MultipartFormData 업로드 기능
> * HTTP Response 의 Validation

### Request

> 서버에 요청을 보내기 위해 제공되는 함수
>
> `Alamofire.request` 에 다양한 argument 가 존재

#### 기본 사용법
  ```swift
  Alamofire.request("http://myurl.com")
  ```
#### HTTP Methods - `method`
  ```swift
  public enum HTTPMethod: String {
	  case options = "OPTIONS"
	  case get     = "GET"
	  case head    = "HEAD"
	  case post    = "POST"
	  case put     = "PUT"
	  case patch   = "PATCH"
	  case delete  = "DELETE"
	  case trace   = "TRACE"
	  case connect = "CONNECT"
  }
  ```
  > HTTPMethod enum 이 이미 정의되어있다.  
  대표적으로 REST API 에서 쓰이는 get, post, put, delete 를 인자로 전달할 수 있다.
  ```swift
  Alamofire.request("https://myurl.com/get")
  Alamofire.request("https://myurl.com/post", method: .post)
  Alamofire.request("https://myurl.com/put", method: .put)
  Alamofire.request("https://myurl.com/delete", method: .delete)
  ```
  > `method` 파라미터의 default 값은 `.get`
 
#### Paramether Encoding - `parameters` - `encoding`
Alamofire는 URL, JSON, PropertyList 등 3가지 매개 변수 인코딩 유형을 지원  
`ParameterEncoding` protocel을 준수하는 범위에서 custom encoding을 지원  
`JSONEncoding`만 소개  
* **JSONEncoding**
  >  `JSONEncoding` 타입은 `parameters` 객체를 JSON 표현으로 인코딩함
  > HTTP Header의 `Content-Type` 이  `application/json` 으로 설정됨
  ```swift
  let parameters = [
	  "name": "iOS",
	  "age": 26
  ]
  // 아래 두가지 방법 동일
  Alamofire.request("https://myurl.com/post", method: .post, parameters: parameters, encoding: JSONEncoding.default)
  Alamofire.request("https://myurl.com/post", method: .post, parameters: parameters, encoding: JSONEncoding(options: []))
  
  // HTTP body: {"name": "iOS", "age": 24}
  ```
#### HTTP Headers - `headers`
  >

## Codable
> `Swift 4` 에서 새롭게 추가된 [Protocol](https://github.com/OhKanghoon/SwiftStudy/blob/master/POP.md#%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9Cprotocol).
> 
> 
