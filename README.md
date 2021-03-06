# ALDataRequestView

[![Version](https://img.shields.io/cocoapods/v/ALDataRequestView.svg?style=flat)](http://cocoapods.org/pods/ALDataRequestView)
[![Language](https://img.shields.io/badge/language-swift3.0-f48041.svg?style=flat)](https://developer.apple.com/swift)
[![License](https://img.shields.io/cocoapods/l/ALDataRequestView.svg?style=flat)](http://cocoapods.org/pods/ALDataRequestView)
[![Platform](https://img.shields.io/cocoapods/p/ALDataRequestView.svg?style=flat)](http://cocoapods.org/pods/ALDataRequestView)
[![Twitter](https://img.shields.io/badge/twitter-@twannl-blue.svg?style=flat)](http://twitter.com/twannl)

A simple way to show:

* Loading view while a producer or observable is started
* Reload view when a producer or observable is failed
* Empty view when a producer or observable returns an empty array or an object which inherits the `Emptyable` protocol and `isEmpty` is true

## Usage

To run the example project, clone the repo, and run `pod install` from the Example directory first.

### ReactiveSwift & RxSwift
Simply call 

```swift
attachTo(dataRequestView: dataRequestView)
```
on your signalProducer or observable. 

Make sure you implement the `ALDataRequestViewDataSource`:

```swift
func loadingView(for dataRequestView: ALDataRequestView) -> UIView?
func reloadViewController(for dataRequestView: ALDataRequestView) -> ALDataReloadType?
func emptyView(for dataRequestView: ALDataRequestView) -> UIView?
```

#### Examples
##### RxSwift

```swift
let request = URLRequest(url: URL(string: "http://httpbin.org/status/400")!)
rxDisposable = URLSession.shared.rx.data(request: request).attachToDataRequestView(dataRequestView: dataRequestView!).subscribe()
```
##### ReactiveSwift

```swift
let request = URLRequest(url: URL(string: "http://httpbin.org/status/400")!)
dataSignalProducer = URLSession.shared
    .reactive.data(with: request)
    .flatMap(.latest, transform: { (data, response) -> SignalProducer<Data, NSError> in
        if let httpResponse = response as? HTTPURLResponse, httpResponse.statusCode > 299 {
            return SignalProducer(error: NSError(domain: "", code: httpResponse.statusCode, userInfo: nil))
        }
        return SignalProducer(value: data)
    })
    .attachTo(dataRequestView: dataRequestView!)

```


## Installation

ALDataRequestView is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "ALDataRequestView"
```

#### Swift version vs Pod version
| Swift version | Pod version    |
| ------------- | --------------- |
| 3.X           | >= 2.0.0			|
| 2.3           | 1.0.4			   |

## Author

Antoine van der Lee, info@avanderlee.com

## License

ALDataRequestView is available under the MIT license. See the LICENSE file for more info.
