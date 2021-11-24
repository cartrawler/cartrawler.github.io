---
layout: default
title: SwiftUI 
parent: iOS
nav_order: 7
---

# SwiftUI

{: .no_toc }

The SDK can be integrated into a SwiftUI project with the following steps. 

--- 

## Create a view controller that will host the SDK
This is your view controller from which the SDK will be presented. It will also act as the SDK delegate.

```java
import UIKit
import CarTrawlerSDK

class SDKHostViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}

extension SDKHostViewController: CarTrawlerSDKDelegate {
    func didProduce(inPathPaymentRequest request: [AnyHashable: Any], vehicle: CTInPathVehicle, payment: Payment) {
    }
    func didReceive(_ reservationDetails: CTReservationDetails) {
    }
}
```

## Create a UIViewControllerRepresentable
This is needed to use the host controller in our SwiftUI code. 
  
```java
struct SDKHostVCRepresentable: UIViewControllerRepresentable {
    func updateUIViewController(_ uiViewController: UIViewControllerType, context: Context) {
    }
    
    func makeUIViewController(context: Context) -> some UIViewController {
      let hostingController = SDKHostViewController()
      let context = CTContext(clientID: "your clientID", flow: .standAlone)
      let defaults = UserDefaults.standard
      let language = "en"
      let currency = "EUR"

      context.languageCode = language == "no" ? "nb" : language
      context.currencyCode = currency
      context.delegate = hostingController
      CarTrawlerSDK.sharedInstance().present(from: hostingController, context: context)
        
      return hostingController
  }
}
```

## Create a SwiftUI view to place the UIViewControllerRepresentable inside
  
```java
struct SDKView: View {
    var closeAction: (() -> Void) = {}
    var body: some View {
        ZStack {
            SDKHostVCRepresentable()
        }.onDisappear(perform: self.closeAction)
    }
}
```

## Place SDKView in the SwiftUI View you want to launch the SDK From
You can place the SDKView inside a ZStack along with the button your user will tap to launch the SDK.
  
```java
import SwiftUI

struct TestView: View {
    
    @State private var showSDK = false
    
    var body: some View {
        ZStack {
            
            if showSDK {
                SDKView(closeAction: {
                    withAnimation(.easeOut(duration: 0.25)) { self.showSDK = false }
                }).transition(.identity)
            }
            
            VStack() {
                Spacer()
                Button("Show SDK") {
                    self.showSDK = true
                }
                Spacer()
            }
            .padding()
        }
    }
}
```


