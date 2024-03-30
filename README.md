
# SwiftUIPathway (Beta)

### Elevating SwiftUI Development

SwiftUIPathway is a cutting-edge library crafted to illuminate the path for SwiftUI developers. By leveraging the power of SwiftUIPathway, you embark on a journey that simplifies navigation, enriches networking capabilities, and enforces a clean architecture within your SwiftUI applications.

## Features

- **Clean Architecture Support**: With SwiftUIPathway, adhering to the principles of clean architecture becomes natural, enabling you to build scalable and maintainable applications.
- **SwiftUI Navigation**: Effortlessly manage all types of navigation in SwiftUI, from horizontal transitions to full-screen modal presentations.
- **Network Layer Integration**: Harness the power of Alamofire for all your network requests, making API calls seamless and efficient.
- **Dependency Injection**: Utilize our built-in support for dependency injection using a Factory pattern, promoting a decoupled and testable codebase.

## Quick Start

### Embracing Clean Architecture

SwiftUIPathway facilitates a clean architectural approach, enabling you to structure your application into coherent layers:

- **View**: Your UI layer, where SwiftUI views reside.
- **ViewModel**: Manages the presentation logic, bridging the View and UseCase.
- **UseCase**: Encapsulates specific business logic of the application.
- **Repository**: Acts as the data access layer, managing data sources.
- **Domain Model**: Represents the core business logic of the application.
- **DTO (Data Transfer Objects)**: Used for transferring data between layers.
- **Router**: Manages navigation logic within the modules.
- 
### Navigation

Navigate through your app with ease:

```swift
// Horizontal Navigation
navigationState.selectedRoute = .profile(id: user.id)

// Full Screen Presentation
navigationState.selectedFullScreenRoute = .profile(id: user.id)
```

### Networking

Make network requests effortlessly within your repositories or any desired layer:

```swift
apiManager.session.request(Profile.getUserInfo)
    .responseDecodable(of: ResponseDto<UserInfoDTO>.self) { response in
        // Process response
    }
```

### Dependency Injection

Inject dependencies anywhere with simplicity:

```swift
@Injected(\.userProfile) var userProfileUseCase: UserProfileUseCase
```

By integrating SwiftUIPathway into your SwiftUI projects, you gain access to a suite of tools designed to streamline your development process, allowing you to focus on what matters most: creating amazing applications. Start with SwiftUIPathway today and experience the difference in your SwiftUI development journey.

## How to Use SwiftUIPathway

To integrate SwiftUIPathway into your project, follow these steps to ensure a smooth setup:

### Step 1: Download and Add the Framework

First, download the SwiftUIPathway framework and add it to your project. This step is crucial for making all the SwiftUIPathway features available in your application.

### Step 2: Add Required Packages

SwiftUIPathway depends on several external packages for its full functionality. Add the following packages to your project's package dependencies:

```swift
dependencies: [
    .package(url: "https://github.com/hmlongco/Factory.git", from: "1.3.7"),
    .package(url: "https://github.com/Alamofire/Alamofire.git", from: "5.9.0"), -> **Make sure you add only AlamofireDynamic**
    .package(url: "https://github.com/kishikawakatsumi/KeychainAccess.git", .branch("master")),
    .package(url: "https://github.com/AppsFlyerSDK/AppsFlyerFramework.git", from: "6.13.2")
]
```

### Step 3: Import SwiftUIPathway

With the framework added and the dependencies set up, you can now import SwiftUIPathway into your Swift files:

```swift
import SwiftUIPathway
```

### Step 4: Initialize SwiftUIPathway

Before using SwiftUIPathway, it must be initialized. This ensures that the framework checks for access rights based on your bundle ID:

```swift
SwiftUIPathwayInit()
```

### Obtaining Access

Upon initialization, SwiftUIPathway will verify that your bundle ID has been granted access. If you encounter an access denial, it means your bundle ID has not been registered with our system.

To gain access, please send an email to mohammad.mugish@gmail.com with your bundle ID. Once we grant access from our system, you will be able to fully utilize SwiftUIPathway in your project.

Note -
if you get an error like this "iphoneos/PackageFrameworks/AlamofireDynamic.framework/AlamofireDynamic' (no such file)" while running in the real device 
Please make sure that AlamofireDynamic.framework is correctly embedded in your app. In Xcode, go to your app target's "General" tab and look in the "Frameworks, Libraries, and Embedded Content" section. The framework should be listed here and set to "Embed & Sign". This is crucial for dynamic frameworks as they need to be bundled with the app.


## Code Samples

### Router
```swift
enum ProfileRoute: RouteProtocol {
    
    static func == (lhs: ProfileRoute, rhs: ProfileRoute) -> Bool {
        lhs.hashValue == rhs.hashValue
    }
    
    case profileDetails(id: String)
    
    // You can add "n" number of navigations related to profile view
   
    var destination: AnyView {
        switch self {
        
        case .profileDetails(let id):
            return AnyView(Text("hello \(id)"))
        }
    }
}
```

### First View
```swift
struct ContentView: View {
    
    @StateObject var navigationState = NavigationState<ProfileRoute>()
    
    var body: some View {
        CustomNavView(navigationState: navigationState) {
            VStack {
                Image(systemName: "globe")
                    .imageScale(.large)
                    .foregroundStyle(.tint)
                Text("Hello, world!")
                    .foregroundStyle(Colors.Primary.solidBlue)
                
                Button(action: {
                    navigationState.selectedRoute = .profileDetails(id: "UniqueId")
                }) {
                    Text("Profile details - navigatoin")
                }
                
                Button(action: {
                    navigationState.selectedFullScreenRoute = .profileDetails(id: "UniqueId")
                }) {
                    Text("Profile details - full screen")
                }
            }
            .customNavigationDestination(navigationState)
            .customFullScreenCover(navigationState)
            .overlay(
                CustomNavLink(
                    isActive: $navigationState.shouldNavigate,
                    route: navigationState.selectedRoute,
                    label: { EmptyView() }
                )
            )
        }
    }
}
```



