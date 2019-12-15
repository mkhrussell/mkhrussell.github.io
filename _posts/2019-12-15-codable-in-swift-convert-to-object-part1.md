---
title: "Codable in Swift - Convert JSON Response to Swift Object - Part 1"
date: 2019-12-15T07:09:27+00:00
author: Kamrul Hasan
categories:
  - blog
tags:
  - codable
  - JSON
  - urlrequest
  - url
  - swift
  - network
  - iOS
---

Parsing **JSON** object to native Swift model object and vice-versa is a very common task while working with REST API in iOS Applications. **Codable** protocol made this job very easy.
You can simply conform Codable for your model structs and classes then use JSONEncoder and JSONDecoder to convert them.

In the following example, I used APIs from [JSONPlaceholder](https://jsonplaceholder.typicode.com/) to demonstrate converting JSON to Swift object.

For [user API: https://jsonplaceholder.typicode.com/users/1](https://jsonplaceholder.typicode.com/users/1), JSON response is like below for user id: 1:

```javascript
{
  "id": 1,
  "name": "Leanne Graham",
  "username": "Bret",
  "email": "Sincere@april.biz",
  "address": {
    "street": "Kulas Light",
    "suite": "Apt. 556",
    "city": "Gwenborough",
    "zipcode": "92998-3874",
    "geo": {
      "lat": "-37.3159",
      "lng": "81.1496"
    }
  },
  "phone": "1-770-736-8031 x56442",
  "website": "hildegard.org",
  "company": {
    "name": "Romaguera-Crona",
    "catchPhrase": "Multi-layered client-server neural-net",
    "bs": "harness real-time e-markets"
  }
}
```

Let's first create model structs based on the above JSON:

```swift
struct Geo: Codable {
    let lat: String
    let long: String

    enum CodingKeys: String, CodingKey {
        case lat
        case long = "lng"
    }
}

struct Address: Codable {
    let street: String
    let suite: String
    let city: String
    let zipcode: String
    let geo: Geo
}

struct Company: Codable {
    let name: String
    let catchPhrase: String
    let business: String

    enum CodingKeys: String, CodingKey {
        case name, catchPhrase
        case business = "bs"
    }
}

struct User: Codable {
    let id: Int
    let name: String
    let username: String
    let email: String
    let address: Address
    let phone: String
    let website: String
    let company: Company
}
```

You can see, our structures are pretty simple: just conformed **Codable** protocol. Generally, property names will become keys in JSON but in few cases we defined **CodingKeys** enum where keys differs from the property names.

Now, let's create a simple client for [user API: https://jsonplaceholder.typicode.com/users/1](https://jsonplaceholder.typicode.com/users/1). I created the client class so that it can be extended later for other APIs (users, posts, etc.).

```swift
class TypicodeClient {
    let host = "jsonplaceholder.typicode.com"

    enum Api {
        case user(id: Int)
        // ...
    }

    func getUrl(api: Api) -> URL {
        var urlComponents = URLComponents()
        urlComponents.host = host
        urlComponents.scheme = "https"

        let path: String
        switch api {
            case .user(let id):
                path = "/users/\(id)"
        }

        urlComponents.path = path
        return urlComponents.url!
    }

    func fetchUser(id: Int, completion: @escaping (User?) -> Void) {
        var urlRequest = URLRequest(url: getUrl(api: .user(id: id)))
        urlRequest.httpMethod = "GET"

        let task = URLSession.shared.dataTask(with: urlRequest) { data, response, error in
            if let jsonData = data {
                do {
                    let jsonDecoder = JSONDecoder()
                    let user = try jsonDecoder.decode(User.self, from: jsonData)
                    completion(user)
                } catch {
                    completion(nil)
                }
            } else {
                completion(nil)
            }
        }
        task.resume()
    }

    // ...

}
```

Here, we used *JSONDecoder.decode(_:from:)* method to convert the JSON response we got from the URLRequest to our **User** model.

```swift
let jsonDecoder = JSONDecoder()
let user = try jsonDecoder.decode(User.self, from: jsonData)
```

Finally, for demo purpose, we populated a UITextView, using the user information.

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var tvUserInfo: UITextView!

    var user: User?

    override func viewDidLoad() {
        super.viewDidLoad()

        loadData()
    }

    private func loadData() {
        let client = TypicodeClient()
        client.fetchUser(id: 1) { (user) in
            if let user = user {
                DispatchQueue.main.async {
                    self.user = user
                    self.updateUI()
                }
            }
        }
    }

    private func updateUI() {
        guard let user = self.user else { return }

        let userInfo = """
            Id: \(user.id)
            Name: " \(user.name)
            Nickname: \(user.nickname)
            Email: \(user.email)
            Address:
                Street: \(user.address.street)
                Suite: \(user.address.suite)
                City: \(user.address.city)
                Zipcode: \(user.address.zipcode)
                Geo-location:
                    lat: \(user.address.geo.lat)
                    long: \(user.address.geo.long)
            Phone: \(user.phone)
            Website: \(user.website)
            Company:
                Name: \(user.company.name)
                Catch Phrase: \(user.company.catchPhrase)
                Business: \(user.company.business)
            """

        tvUserInfo.text = userInfo
    }

}
```


You can check source code of this demo here [https://github.com/mkhrussell/SwiftRestClientDemo](https://github.com/mkhrussell/SwiftRestClientDemo).
{: .notice--info}
