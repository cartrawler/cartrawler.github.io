---
title: SwiftUI
position: 6
type: iOS
description:
right_code: >-
  {: title="SwiftUI" }
---

The SDK can be integrated into a SwiftUI project with the following steps:

<b>Step 1. Create a view controller that will host the SDK. This is your view controller from which the SDK will be presented. It will also act as the SDK delegate.</b>

```swift

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

<b>Step 2. Create a UIViewControllerRepresentable so that we can use the host controller in our SwiftUI code. </b>
  
``` swift
struct SDKHostVCRepresentable: UIViewControllerRepresentable {
    func updateUIViewController(_ uiViewController: UIViewControllerType, context: Context) {
    }
    
    func makeUIViewController(context: Context) -> some UIViewController {
      let hostingController = SDKHostViewController()
      let context = CTContext(clientID: [your clientID], flow: .standAlone)
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

<b>Step 3. Create a SwiftUI view and place the UIViewControllerRepresentable inside it.</b>
  
``` swift
struct SDKView: View {
    var closeAction: (() -> Void) = {}
    var body: some View {
        ZStack {
            SDKHostVCRepresentable()
        }.onDisappear(perform: self.closeAction)
    }
}
```

<b>Step 4. Finally, in the SwiftUI View you want to launch the SDK from, place the SDKView inside a ZStack along with the button your user will tap to launch the SDK.
  
``` swift
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

