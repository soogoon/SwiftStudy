# 8. Alamofire / Codable

## [Alamofire](https://github.com/Alamofire/Alamofire)
Swift 에서 HTTP 네트워킹을 위한 대표적인 오픈소스 라이브러리 이다.  
  
* Request 와 Response 가 연계되는 함수제공
* URL / JSON 형태 파라미터 인코딩
* File / Data / Stream / MultipartFormData 업로드 기능
* HTTP Response 의 Validation

### Request
서버에 요청을 보내기 위해 제공되는 함수  
  
`Alamofire.request` 에 다양한 argument 가 존재한다.  
  
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
  > HTTPMethod enum 이 `Alamofire`에  정의되어있다.  
  대표적으로 REST API 에서 쓰이는 get, post, put, delete 를 인자로 전달할 수 있다.
  ```swift
  Alamofire.request("https://myurl.com/get")
  Alamofire.request("https://myurl.com/post", method: .post)
  Alamofire.request("https://myurl.com/put", method: .put)
  Alamofire.request("https://myurl.com/delete", method: .delete)
  ```
  > `method` 파라미터의 default 값은 `.get`
 
#### Paramether Encoding - `parameters`, `encoding`   
  > * Alamofire는 URL, JSON, PropertyList 등 3가지 매개 변수 인코딩 유형을 지원한다.  
  > * `ParameterEncoding` protocel을 준수하는 범위에서 custom encoding을 지원한다.  
  > * 아래에서는 `JSONEncoding`만 소개함  
* **JSONEncoding**
  > `JSONEncoding` 타입은 `parameters` 객체를 JSON 표현으로 인코딩  
  > HTTP Header의 `Content-Type` 이  `application/json` 으로 설정
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
  > HTTPHeaders 타입의 헤더를 설정할 수 있다.
  ```swift
  let headers: HTTPHeaders = [
	  "Authorization": "QWxhZGRpbjpvcGVuIHNlc2FtZQ==",
	  "Accept": "application/json"
  ]
  
  Alamofire.request("https://myurl.com/headers", headers: headers).responseJSON { response in
	  print(response)
  }
  ```
  > 변경되지 않는 HTTP 헤더는 `URLSessionConfiguration`에 설정하여 기본 `URLSession`에서 만든 `URLSessionTask`에 자동으로 적용하는 것이 좋다.  
  > 자세한 내용은 [Session Manager Configurations](https://github.com/Alamofire/Alamofire/blob/master/Documentation/AdvancedUsage.md#session-manager) 참조

### Response Handling
`Alamofire.request`에 연결하여 reponse handling이 가능하다.  
다양한 response handler가 제공된다.  
  ```swift
  Alamofire.request("https://myurl.com/get").responseJSON { response in
	  print("Request: \(String(describing: response.request))")   // original url request
	  print("Response: \(String(describing: response.response))") // http url response
	  print("Result: \(response.result)")                         // response serialization result
  }
  ```
  > request에 대한 response가 수신된 후 response를 처리하기 위해 `closure`형식의 콜백이 지정됨  
  > response 에 정의된 `closure` 안에서 수신된 응답에 대한 처리를 수행한다.  
  > Alamofire의 네트워킹은 비동기로 처리된다.  
  
  Alamofire 에는 다섯가지의 response handler를 제공한다.
  ```swift
// Response Handler - Unserialized Response
func response(
    queue: DispatchQueue?,
    completionHandler: @escaping (DefaultDataResponse) -> Void)
    -> Self

// Response Data Handler - Serialized into Data
func responseData(
    queue: DispatchQueue?,
    completionHandler: @escaping (DataResponse<Data>) -> Void)
    -> Self

// Response String Handler - Serialized into String
func responseString(
    queue: DispatchQueue?,
    encoding: String.Encoding?,
    completionHandler: @escaping (DataResponse<String>) -> Void)
    -> Self

// Response JSON Handler - Serialized into Any
func responseJSON(
    queue: DispatchQueue?,
    completionHandler: @escaping (DataResponse<Any>) -> Void)
    -> Self

// Response PropertyList (plist) Handler - Serialized into Any
func responsePropertyList(
    queue: DispatchQueue?,
    completionHandler: @escaping (DataResponse<Any>) -> Void))
    -> Self

  ```
  
#### Response Data Handler
  > `responseData` handler 는 서버에서 받아온 `Data`를 `responseDataSerializer`에 추출한다.  
  > 수신된 `Data`에 에러가 없다면 response의 `Result`에는 `.success`, `Data` 타입의 `value`가 set 된다.
  ```swift
  Alamofire.request("https://myurl.com/get").responseData { response in
    print("All Response Info: \(response)")

    if let data = response.result.value, let utf8Text = String(data: data, encoding: .utf8) {
    	print("Data: \(utf8Text)")
    }
  }
  ```

## Codable
> `Swift 4` 에서 새롭게 추가된 [Protocol](https://github.com/OhKanghoon/SwiftStudy/blob/master/POP.md#%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9Cprotocol).
> 
> 
