---
layout: default
title: SwiftUI
parent: In Path with Grid View
grand_parent: iOS Integration
nav_order: 4
permalink: /docs/ios/inpath-gridview/swiftUI
---

# SwiftUI
{: .no_toc }

The SDK can be integrated into a SwiftUI project with the following steps. 

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

--- 

## Create a view controller that will host the SDK
This is your view controller from which the SDK will be presented. It will also act as the SDK delegate.

```java
import UIKit
import CarTrawlerSDK

class SDKHostViewController: UIViewController {
    weak var delegate: CarTrawlerSDKDelegate?
    
    override func viewDidLoad() {
        super.viewDidLoad()
    }
}

extension SDKHostViewController: CarTrawlerSDKDelegate {
    func didProduce(inPathPaymentRequest request: [AnyHashable: Any], vehicle: CTInPathVehicle, payment: Payment) {
    }
    
    func didReceive(_ reservationDetails: CTReservationDetails) {
    }
    
    func didFinishPrewarm(_ dictionary: [AnyHashable : Any]?) {
        print("[CT] didFinishPrewarm")
        if let delegate = delegate {
            delegate.didFinishPrewarm?(dictionary)
        }
        
    }
}
```

## Create represantables
This is needed to use the host controller in our SwiftUI code. 

```java

// To present the standlone flow
struct CTSDKViewRepresentable: UIViewControllerRepresentable {
    func updateUIViewController(_ uiViewController: UIViewControllerType, context: Context) {
    }

    func makeUIViewController(context: Context) -> some UIViewController {
      let hostingController = SDKHostViewController()
      let context = CTContext(implementationID: "YOUR IMPLEMENTATION ID",
                              clientID: "YOUR CLIENT ID",
                              flow: .standAlone)

      context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
      context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
      context.languageCode = "en" // The language associated with the device’s system region is used by default.

      context.delegate = hostingController
      CarTrawlerSDK.sharedInstance().present(from: hostingController, context: context)

      return hostingController
  }
}

// To display the grid view
struct CTGridViewRepresentable: UIViewControllerRepresentable {
    var isPrewarm: Bool
    let cartrawlerSDK = CarTrawlerSDK.sharedInstance()
    
    var onPrewarmFinished: ([AnyHashable: Any]?) -> Void
    
    func updateUIViewController(_ uiViewController: SDKHostViewController, context: Context) {}
    
    func makeUIViewController(context: Context) -> SDKHostViewController {
        let hostingController = SDKHostViewController()
        hostingController.delegate = context.coordinator
        
        let context = CTHelper.gridViewContext()
        context.delegate = hostingController
        
        let gridView = isPrewarm ? cartrawlerSDK.preWarmGridView(with: context) : cartrawlerSDK.getGridView(from: hostingController, context: context)
        hostingController.view.addSubview(gridView)
        
        // Enable Auto Layout
        gridView.translatesAutoresizingMaskIntoConstraints = false
        
        // Pin subview to all edges of the superview
        NSLayoutConstraint.activate([
            gridView.topAnchor.constraint(equalTo: hostingController.view.topAnchor),
            gridView.leadingAnchor.constraint(equalTo: hostingController.view.leadingAnchor),
            gridView.trailingAnchor.constraint(equalTo: hostingController.view.trailingAnchor),
            gridView.bottomAnchor.constraint(equalTo: hostingController.view.bottomAnchor)
        ])
        
        return hostingController
    }
    
    func makeCoordinator() -> Coordinator {
        Coordinator(onPrewarmFinished: onPrewarmFinished)
    }
    
    class Coordinator: NSObject, CarTrawlerSDKDelegate {
        var onPrewarmFinished: ([AnyHashable: Any]?) -> Void
        
        init(onPrewarmFinished: @escaping ([AnyHashable: Any]?) -> Void) {
            self.onPrewarmFinished = onPrewarmFinished
        }
        
        func didProduce(inPathPaymentRequest request: [AnyHashable: Any], vehicle: CTInPathVehicle, payment: Payment) {}
        func didReceive(_ reservationDetails: CTReservationDetails) {}
        func didFinishPrewarm(_ dictionary: [AnyHashable : Any]?) {
            onPrewarmFinished(dictionary)
        }
    }
}

// Helper to initialise the context
struct CTHelper {
    static func gridViewContext() -> CTContext {
        let context = CTContext(implementationID: "YOUR IMPLEMENTATION ID", clientID: "YOUR CLIENT ID", flow: .inPath)
        context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
        context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
        context.languageCode = "EN" // The language associated with the device’s system region is used by default.

        // Create CTFLightDetails
        let flightDetails = CTFlightDetails()
        flightDetails.flightOrigin = "DUB|LHR|2025-12-16T06:05:00|2025-12-16T07:25:00|XX8719"
        flightDetails.flightReturn = "LHR|DUB|2025-12-22T21:20:00|2025-12-23T22:45:00|XX8720"
        flightDetails.passengerBreakdown = CTFlightPassengerBreakdown(adults: 2, teens: 0, children: 0, infants: 0)
        flightDetails.firstName = "John"
        flightDetails.surName     = "Doe"
        flightDetails.customerEmail     = "john.doe@example.com"
        flightDetails.fareClass     = "business"
        flightDetails.flightFare = 175.95
        flightDetails.bags = 1
        flightDetails.loyaltyNumber = "ABC123456"
        flightDetails.loyaltyTier = "gold"

        // Set flight details on context
        context.flightDetails = flightDetails
        
        return context
    }
}
```

## Initialise the CarTrawlerSDK on your main struct
  
```java
import UIKit
import SwiftUI
import CarTrawlerSDK

@main
struct YourApp: App {
    
    init() {
        CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                                                     customParameters: nil,
                                                     production: false)
    }
    
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

## Place representables on your SwiftUI content view
  
```java
import SwiftUI

struct ContentView: View {
    
    @State private var showPrewarmed = false
    @State private var showPrewarmedButton = false
    @State private var showSDK = false
    @State private var log: String = "Prewarming..."
    
    var body: some View {
        ZStack {
            VStack() {
                if showPrewarmed {
                    CTGridViewRepresentable(isPrewarm: false) { _ in }
                        .frame(maxWidth: .infinity)
                        .frame(height: 600)
                } else {
                    Text(log).padding()
                    
                    CTGridViewRepresentable(isPrewarm: true) { dictionary in
                        log = "[SwiftUI] Prewarm finished"
                        showPrewarmedButton = true
                    }
                }
                
                if showSDK {
                    CTSDKViewRepresentable()
                        .transition(.identity)
                        .onDisappear {
                            self.showSDK = false
                        }
                }
 
                Spacer()
                
                if showPrewarmedButton {
                    Button("Display prewarmed grid view") {
                        self.showPrewarmed = true
                    }
                }
                
                Button("Present Standalone") {
                    self.showSDK = true
                }
                
                Spacer()
            }
            .padding()
        }
    }
}
```


