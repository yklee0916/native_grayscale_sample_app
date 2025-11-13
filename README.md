# Native Grayscale Sample App

A native iOS sample application demonstrating how to use Flutter plugins (`flutter_grayscale`, `flutter_log`) in a native iOS app through the `NativeGrayscaleSDK`. This app is the native equivalent of `flutter_grayscale_sample_app`, showing how to access Flutter plugin functionality via Swift Package Manager.

## Features

- Select images from photo library (iOS)
- Convert selected images to grayscale using Flutter plugin logic
- Real-time preview of original and converted images
- Error handling and logging
- Flutter plugin integration in native app via SDK

## Requirements

- Xcode 14.0+
- iOS 13.0+
- Swift 5.0+

## Dependencies

This app uses the following:

- `NativeGrayscaleSDK` - SDK that bridges Flutter plugins to native iOS (via Swift Package Manager)
  - `flutter_grayscale` - CoreImage-based grayscale conversion plugin (via SDK)
  - `flutter_log` - Logging utility (via SDK)

## Getting Started

1. Clone the repository:
```bash
git clone https://github.com/yklee0916/native_grayscale_sample_app.git
cd native_grayscale_sample_app
```

2. Open the project in Xcode:
```bash
open NativeGrayscaleSample.xcodeproj
```

3. Build and run the app in Xcode

The SDK dependency will be automatically resolved via Swift Package Manager.

## Project Structure

```
NativeGrayscaleSample/
├── AppDelegate.swift              # App lifecycle, SDK initialization
├── MainApp.swift                  # App entry point
├── views/
│   └── ContentView.swift          # Main UI view
├── viewmodels/
│   └── GrayscaleViewModel.swift  # Business logic and state management
└── listeners/
    └── ConsoleLogListener.swift  # Log listener implementation
```

## Architecture

The app follows the MVVM (Model-View-ViewModel) pattern:

- **View**: `ContentView` - UI components and layout
- **ViewModel**: `GrayscaleViewModel` - Business logic, state management, and SDK integration
- **SDK**: `NativeGrayscaleSDK` - Bridges Flutter plugins to native code
- **Model**: Handled by Flutter plugins (`flutter_grayscale`, `flutter_log`) via SDK

## Usage

1. Launch the app
2. Tap "이미지 선택" (Select Image) button
3. Choose an image from your photo library
4. Tap "Grayscale 변환" (Convert to Grayscale) button
5. View the converted grayscale image

## SDK Integration

The app uses `NativeGrayscaleSDK` (via Swift Package Manager) to access Flutter plugins:

```swift
import NativeGrayscaleSDK

// Initialize SDK
GrayscaleSDK.shared.initialize { success in
    if success {
        // Set log interceptor
        GrayscaleSDK.shared.setLogInterceptor(ConsoleLogListener())
        
        // Convert image to grayscale
        GrayscaleSDK.shared.convertToGrayscale(imagePath: path) { result in
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

## Author

Younggi Lee
