## Request

Configuration of a DataRequest.

override the SLRequest

```swift
import SolarNetwork
import Alamofire

class GitHubSigninRequest: SLRequest {
    
    // override the loadRequest method
    override func loadRequest() {
        super.loadRequest()
        
        //specify the method if is diffrent from target.method
        self.method = .put
        
        //specify the path
        self.path = "/authorizations/clients/\(GitHubAppClientID)"
    }
    
    /**
    the subClass's properties will encode to Parameters by Reflection.
    [
    "client_secret" : GitHubAppClientSecret, 
    "scopes": ["repo", "user"]
    ] 
    */
    var client_secret = GitHubAppClientSecret
        
    var scopes = ["repo", "user"]
}
```

Usage

```swift
//When you click the Signin button.
if let userName = userName, let password = password {
    
    let signinRequest = GitHubSigninRequest()
    //basic Authentication
    signinRequest.basicAuthentication = (userName, password)
    
    GitHubNetwork.request(signinRequest) { [weak self] (response) in
        guard let strongSelf = self else { return }
        
        if let model = response.decode(to: GitHubAuthenticationModel.self) {
            
            if model.token.count > 0 {
                //save token
            }
            
        }
        else if let error = response.error {
            //show error
        }
        
    }
}
```
You can also specify the following properties if you need.

```swift
public var method: HTTPMethod

public var path: String

//if specify URLString, it will ignore the target's baseURLString.
public var URLString: String

//if specify parameters, it will ignore the properties Reflection in subClass.
public var parameters: Parameters?

//specify the parameterEncoding if is diffrent from target.parameterEncoding.
public var parameterEncoding: ParameterEncoding

public var headers: [String: String]?

public var credential: URLCredential?
    
public var basicAuthentication: (user: String, password: String)?

//specify the dataKeyPath if is diffrent from target.status.dataKeyPath.
public var dataKeyPath: String?
```

Control the state of Request 

```swift
let request = SubSLRequest()

request.pause()

request.resume()

//If cancel, you should send the request again.
request.cancel()
```
