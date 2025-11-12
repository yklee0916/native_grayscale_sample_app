# Native Grayscale Sample App

A native iOS sample application demonstrating grayscale image conversion using the `FlutterGrayscaleSDK`. This app shows how to use Flutter plugins in a native iOS app through the SDK bridge.

## Features

- Select images from photo library (iOS)
- Convert selected images to grayscale using Flutter logic
- Real-time preview of original and converted images
- Error handling and logging
- Flutter plugin integration in native app

## Requirements

- Xcode 14.0+
- iOS 13.0+
- Swift 5.0+
- CocoaPods

## Dependencies

This app uses the following:

- `FlutterGrayscaleSDK` - SDK that bridges Flutter plugins to native iOS
- `flutter_grayscale` - CoreImage-based grayscale conversion plugin (via SDK)
- `flutter_log` - Logging utility (via SDK)

## Getting Started

1. Clone the repository:
```bash
git clone https://github.com/yklee0916/native_grayscale_sample_app.git
cd native_grayscale_sample_app
```

2. Install CocoaPods dependencies:
```bash
cd ios
pod install
cd ..
```

3. Open the workspace:
```bash
open ios/NativeGrayscaleSample.xcworkspace
```

4. Build and run the app in Xcode

## Project Structure

```
NativeGrayscaleSample/
├── AppDelegate.swift              # App lifecycle, SDK initialization
├── MainApp.swift                  # App entry point
├── views/
│   └── ContentView.swift          # Main UI view
├── viewmodels/
│   └── GrayscaleViewModel.swift   # Business logic and state management
└── listeners/
    └── ConsoleLogListener.swift    # Log listener implementation
```

## Architecture

The app follows the MVVM (Model-View-ViewModel) pattern and integrates with Flutter plugins through the SDK:

### Architecture Diagram
```
┌────────────────────────────────────────────────────────────┐
│                    Native iOS App (SwiftUI)                │
├────────────────────────────────────────────────────────────┤
│  ┌──────────────┐    ┌──────────────────┐                  │
│  │   ContentView│───▶│GrayscaleViewModel│                  │
│  │   (View)     │    │  (ViewModel)     │                  │
│  └──────────────┘    └────────┬─────────┘                  │
│                               ▼                            │
│                    ┌──────────────────────┐                │
│                    │  FlutterGrayscaleSDK │                │
│                    │  (SDK Bridge Layer)  │                │
│                    └──────────┬───────────┘                │
│                    ┌──────────┴───────────┐                │
│                    ▼                      ▼                │
│          ┌─────────────────┐   ┌─────────────────┐         │
│          │ Flutter Engine  │   │ Method Channels │         │
│          │                 │   │                 │         │
│          │  - Image Channel│   │  - Log Channel  │         │
│          └────────┬────────┘   └────────┬────────┘         │
└───────────────────┼─────────────────────┼──────────────────┘
                    ▼                     ▼
┌────────────────────────────────────────────────────────────┐
│              Flutter Module (flutter_grayscale_sdk)        │
├────────────────────────────────────────────────────────────┤
│          ┌──────────────────┐    ┌──────────────────┐      │
│          │ImageMethodChannel│    │ LogMethodChannel │      │
│          └────────┬─────────┘    └────────┬─────────┘      │
│                   ▼                       ▼                │
│          ┌──────────────────┐    ┌──────────────────┐      │
│          │ flutter_grayscale│    │   flutter_log    │      │
│          │    (Plugin)      │    │     (Plugin)     │      │
│          └──────────────────┘    └──────────────────┘      │
│          ┌──────────────────────────────────────────┐      │
│          │     Native Platform Implementation       │      │
│          │ (iOS: CoreImage, macOS: AppKit/CoreImage)│      │
│          └──────────────────────────────────────────┘      │
└────────────────────────────────────────────────────────────┘
```

### Component Description

1. **View Layer** (`ContentView`)
   - SwiftUI-based UI components
   - Displays original and converted images
   - User interaction handling

2. **ViewModel Layer** (`GrayscaleViewModel`)
   - Business logic and state management
   - Communicates with FlutterGrayscaleSDK
   - Handles image selection and conversion requests

3. **SDK Bridge Layer** (`FlutterGrayscaleSDK`)
   - Initializes and manages Flutter Engine
   - Creates Method Channels for communication
   - Bridges native Swift code with Flutter Dart code

4. **Flutter Module** (`flutter_grayscale_sdk`)
   - Contains Flutter plugins (`flutter_grayscale`, `flutter_log`)
   - Handles method channel calls from native side
   - Executes Flutter plugin logic

5. **Native Platform Implementation**
   - iOS: Uses CoreImage for grayscale conversion
   - macOS: Uses AppKit and CoreImage for grayscale conversion

### How It Works

1. **SDK Initialization**: When the app launches, `FlutterGrayscaleSDK` initializes a Flutter Engine and sets up Method Channels.

2. **Method Channel Communication**: 
   - Native app calls SDK methods (e.g., `convertToGrayscale`)
   - SDK sends requests through Method Channels to Flutter module
   - Flutter module processes requests using Flutter plugins
   - Results are returned back through Method Channels

3. **Plugin Execution**: 
   - Flutter plugins (`flutter_grayscale`, `flutter_log`) execute their logic
   - Platform-specific implementations (iOS/macOS) are called
   - Results are returned to the native app

## Usage

1. Launch the app
2. Tap "이미지 선택" (Select Image) button
3. Choose an image from your photo library
4. Tap "Grayscale 변환" (Convert to Grayscale) button
5. View the converted grayscale image

## SDK Integration

The app uses `FlutterGrayscaleSDK` to access Flutter plugins from native code:

```swift
import FlutterGrayscaleSDK

// Initialize SDK
FlutterGrayscaleSDK.shared.initialize { success in
    if success {
        // Set log interceptor
        FlutterGrayscaleSDK.shared.setLogInterceptor(ConsoleLogListener())
        
        // Convert image to grayscale
        FlutterGrayscaleSDK.shared.convertToGrayscale(imagePath: path) { result in
            switch result {
            case .success(let outputPath):
                print("Success: \(outputPath)")
            case .failure(let error):
                print("Error [\(error.code)]: \(error.message)")
            }
        }
    }
}
```

## Platform Support

- ✅ iOS

## License

MIT License. See [LICENSE](LICENSE) for details.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Author

Younggi Lee
