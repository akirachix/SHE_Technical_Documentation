# Frontend Mobile Documentation

## 1. Overview

The SHE++ mobile application is a Flutter-based mobile frontend developed for the SHE++ digital safety platform.

The application provides a user-facing mobile interface through which users can access digital safety learning activities, manage their account, and submit support requests.

The mobile frontend communicates with the SHE++ backend API through HTTP requests. Authentication, game data, and support-related information are handled through the backend API rather than through direct database access from the mobile application.

### Main Functional Areas

The mobile application provides the following functionality:

* User registration and login
* Automatic authentication/session restoration
* Password recovery and password reset
* Password change
* Home screen and application navigation
* Digital safety topics
* Quizzes
* Puzzles
* Scenarios
* Game scores and progress
* Rewards and completion screens
* User profile settings
* Support request workflow
* Reusable navigation and interface components

The mobile application is designed as a client application and relies on the backend API for server-side authentication, data processing, and persistence.

---

# 2. Technology Stack

| Technology        | Purpose                                             |
| ----------------- | --------------------------------------------------- |
| Flutter           | Mobile application framework                        |
| Dart              | Application programming language                    |
| HTTP              | REST API communication                              |
| SharedPreferences | Local persistence of authentication/session data    |
| ChangeNotifier    | Authentication state management                     |
| Material Design   | Base Flutter UI framework                           |
| Android SDK       | Android development and APK generation              |
| Appetize          | Browser-based application demonstration and testing |

## 2.1 Flutter

Flutter is used to build the mobile application's user interface and application logic.

The application is written in Dart and uses Flutter's widget-based architecture.

## 2.2 HTTP

The `http` package is used to communicate with the SHE++ backend API.

The dependency can be added with:

```bash
flutter pub add http
```

## 2.3 SharedPreferences

`shared_preferences` is used for local persistence of authentication tokens.

The dependency can be added with:

```bash
flutter pub add shared_preferences
```

Dependencies are restored with:

```bash
flutter pub get
```

## 2.4 ChangeNotifier

The authentication state is managed using a `ChangeNotifier`-based view model.

The authentication view model exposes authentication state to the application and notifies the relevant UI components when the state changes.

---

# 3. Project Structure

The mobile application is organized into core configuration, services, models, screens, state management, and reusable widgets.

```text
lib/
├── app.dart
├── main.dart
│
├── core/
│   ├── colors.dart
│   ├── routes.dart
│   ├── theme.dart
│   └── services/
│       ├── api_service.dart
│       ├── auth_api.dart
│       └── game_api.dart
│
├── models/
│   ├── auth-exception.dart
│   ├── core/
│   │   ├── constants/
│   │   │   └── app_colors.dart
│   │   └── widgets/
│   │       └── bottom_nav.dart
│   ├── game_models.dart
│   ├── game_payload.dart
│   ├── game_topic.dart
│   ├── login_response.dart
│   ├── quiz_question.dart
│   ├── support_request.dart
│   ├── token_response.dart
│   ├── topic.dart
│   └── user.dart
│
├── screens/
│   ├── auth/
│   │   ├── change_password_screen.dart
│   │   ├── forgot_password_screen.dart
│   │   ├── login_screen.dart
│   │   ├── reset_password_screen.dart
│   │   └── signup_screen.dart
│   │
│   ├── game/
│   │   ├── activity_launcher.dart
│   │   ├── finish/
│   │   ├── game_choice_screen.dart
│   │   ├── puzzle_screen.dart
│   │   ├── quiz_intro_screen.dart
│   │   ├── quiz_score_screen.dart
│   │   ├── quiz_screen.dart
│   │   ├── reward_screen.dart
│   │   ├── scenario_screen.dart
│   │   ├── score_screen.dart
│   │   └── topic_screen.dart
│   │
│   ├── splash/
│   │   └── splash_screen.dart
│   │
│   ├── support/
│   │   ├── description_screen.dart
│   │   ├── duration_screen.dart
│   │   ├── phone_screen.dart
│   │   ├── support_category_screen.dart
│   │   ├── support_screen_1.dart
│   │   ├── support_screen_2.dart
│   │   └── thank_you_screen.dart
│   │
│   └── ...
│
├── view_model/
│   └── auth_view.dart
│
└── widgets/
    ├── app_top_bar.dart
    ├── avatar_card.dart
    ├── bottom_na.dart
    ├── bottom_nav.dart
    ├── custom_button.dart
    ├── custom_textfield.dart
    ├── game_progress_bar.dart
    ├── option_card.dart
    ├── progress_bar.dart
    ├── quiz_option.dart
    ├── topic_card.dart
    └── topic_option.dart
```

## 3.1 Application Entry

`main.dart` is the application entry point.

`app.dart` contains the main application configuration and application-level setup.

## 3.2 Core

The `core` directory contains application-wide configuration and services.

| File               | Responsibility                  |
| ------------------ | ------------------------------- |
| `colors.dart`      | Application colour definitions  |
| `theme.dart`       | Application theme configuration |
| `routes.dart`      | Application route configuration |
| `api_service.dart` | General API communication       |
| `auth_api.dart`    | Authentication API operations   |
| `game_api.dart`    | Game-related API operations     |

## 3.3 Models

The `models` directory contains data structures used by the application and API integration.

Authentication models include:

* `user.dart`
* `login_response.dart`
* `token_response.dart`
* `auth-exception.dart`

Game-related models include:

* `game_models.dart`
* `game_payload.dart`
* `game_topic.dart`
* `quiz_question.dart`
* `topic.dart`

Support-related data is represented by:

* `support_request.dart`

## 3.4 Screens

The `screens` directory contains the user-facing Flutter screens.

Screens are grouped according to their functionality.

### Authentication

```text
screens/auth/
```

Contains:

* Login
* Signup
* Forgot password
* Reset password
* Change password

### Games

```text
screens/game/
```

Contains:

* Game activity launcher
* Topic selection
* Quiz introduction
* Quiz
* Quiz score
* Game choice
* Puzzle
* Scenario
* Score
* Rewards
* Game completion

### Support

```text
screens/support/
```

Contains the screens used in the support-request workflow.

### Splash

```text
screens/splash/
```

Contains the application's splash screen.

## 3.5 View Model

The authentication state-management implementation is located at:

```text
lib/view_model/auth_view.dart
```

The view model coordinates authentication operations and exposes the current authentication state to the UI.

## 3.6 Widgets

The `widgets` directory contains reusable UI components.

Examples include:

* `custom_button.dart`
* `custom_textfield.dart`
* `topic_card.dart`
* `topic_option.dart`
* `quiz_option.dart`
* `option_card.dart`
* `progress_bar.dart`
* `game_progress_bar.dart`
* `avatar_card.dart`
* `app_top_bar.dart`
* Navigation widgets

Reusable widgets provide consistency across screens and reduce duplicated UI implementation.

---

# 4. Architecture Layers

The application uses a layered architecture that separates the user interface, application state, API communication, and data models.

```text
┌────────────────────────────────────┐
│         Presentation Layer         │
│      Screens + Reusable Widgets    │
└──────────────────┬─────────────────┘
                   │
                   ▼
┌────────────────────────────────────┐
│       State Management Layer       │
│           AuthViewModel            │
└──────────────────┬─────────────────┘
                   │
                   ▼
┌────────────────────────────────────┐
│            Service Layer           │
│  API Service / Auth API / Game API │
└──────────────────┬─────────────────┘
                   │
                   ▼
┌────────────────────────────────────┐
│            Model Layer             │
│   User / Game / Token / Responses  │
└──────────────────┬─────────────────┘
                   │
                   ▼
┌────────────────────────────────────┐
│         SHE++ Backend API          │
│              FastAPI               │
└────────────────────────────────────┘
```

## 4.1 Presentation Layer

The presentation layer consists of Flutter screens and reusable widgets.

Its responsibilities include:

* Displaying application information
* Receiving user input
* Displaying validation feedback
* Displaying loading states
* Displaying API errors appropriately
* Navigating between screens
* Displaying game progress and scores

## 4.2 State Management Layer

The application uses `AuthViewModel` to manage authentication state.

The authentication lifecycle includes:

```text
Authenticating
      │
      ├──► Authenticated
      │
      └──► Unauthenticated
```

The view model notifies the UI when authentication state changes.

## 4.3 Service Layer

The service layer contains the application's API communication logic.

The main services are:

```text
api_service.dart
auth_api.dart
game_api.dart
```

This separation prevents individual screens from having to implement API communication directly.

## 4.4 Model Layer

The model layer represents structured application data.

Examples include:

* User data
* Login responses
* Authentication tokens
* Game data
* Quiz questions
* Topics
* Support requests

Models allow API data to be represented consistently within the Flutter application.

---

# 5. Core Components

## 5.1 Application Routing

Application routes are defined in:

```text
lib/core/routes.dart
```

Routes provide centralized navigation configuration.

Authentication startup handling is implemented through:

```text
lib/screens/redirect.dart
```

The redirect flow determines whether the application should display the authenticated application or the login flow.

```text
Application Start
       │
       ▼
Authentication Check
       │
   ┌───┴────┐
   │        │
Valid      Invalid
Session    Session
   │        │
   ▼        ▼
 Home     Login
```

## 5.2 API Service

The general API service is:

```text
lib/core/services/api_service.dart
```

It provides the communication layer between the Flutter application and the backend API.

Its responsibilities include:

* Creating HTTP requests
* Sending request headers
* Sending authentication information where required
* Processing API responses
* Handling unsuccessful responses

## 5.3 Authentication Service

Authentication API functionality is implemented in:

```text
lib/core/services/auth_api.dart
```

The authentication service handles authentication-related communication with the backend.

The implementation includes functionality for:

* Login
* Current-user retrieval
* Token refresh
* Authentication response handling
* Authentication error handling

## 5.4 Authentication View Model

The authentication view model is:

```text
lib/view_model/auth_view.dart
```

It coordinates the authentication state of the application.

When the application starts, the view model checks for locally stored authentication tokens.

If a valid access token exists, the application attempts to retrieve the current user.

If the access token is invalid but a refresh token exists, the application attempts to refresh the authentication session.

If authentication cannot be restored, the session is cleared and the user is returned to the unauthenticated state.

## 5.5 Local Storage

Authentication tokens are persisted locally using `SharedPreferences`.

The application uses local storage to retain:

* Access token
* Refresh token

This allows the application to restore an authenticated session when the application is reopened.

When the user logs out, the stored authentication tokens are removed.

## 5.6 Game API

Game-related backend communication is handled by:

```text
lib/core/services/game_api.dart
```

The game service works with the game models and game screens to provide the application's learning activities.

The game functionality includes:

* Topics
* Quizzes
* Puzzles
* Scenarios
* Scores
* Rewards
* Completion flows

## 5.7 Reusable UI Components

Reusable components are stored in:

```text
lib/widgets/
```

They provide consistent visual and interactive behaviour across the application.

Examples include:

```text
CustomButton
CustomTextField
TopicCard
TopicOption
QuizOption
OptionCard
ProgressBar
GameProgressBar
AvatarCard
AppTopBar
```

---

# 6. Security Measures

Security is implemented through authentication controls, token management, input validation, protected API communication, and error handling.

## 6.1 Authentication

The mobile application maintains an authentication state and uses the backend authentication mechanism before accessing protected functionality.

Unauthenticated users are directed to the authentication flow.

## 6.2 Token Management

The application uses access and refresh tokens to maintain authenticated sessions.

The authentication flow:

1. Checks for stored authentication tokens.
2. Attempts to use the access token.
3. Attempts token refresh when required.
4. Stores newly issued tokens.
5. Retrieves the current user after successful authentication.
6. Clears authentication information when session restoration fails.

## 6.3 Input Validation

User-entered information is validated before being processed or submitted to the backend where applicable.

This includes authentication and support-request input.

Validation helps prevent incomplete, invalid, or incorrectly formatted information from being submitted.

## 6.4 API Communication

The mobile application communicates with the backend through defined API services.

The mobile application does not directly access the database.

Backend authentication and authorization controls remain responsible for protecting API resources.

## 6.5 HTTPS

The deployed dadasafeAPI is accessed using HTTPS.

HTTPS protects information transmitted between the mobile application and backend during network communication.

## 6.6 Error Handling

API and authentication failures are handled by the application.

Authentication-specific errors are represented through the authentication exception and response models.

The application should present appropriate user-facing feedback rather than exposing internal backend implementation details.

## 6.7 Image and File Handling

The current Flutter project does not contain a dedicated image-scanning or image-security service.

Therefore, image scanning is not documented as an implemented security control.

Application images used by the interface are packaged as application assets and referenced by the Flutter application.

---

# 7. Installation and Setup

## 7.1 Prerequisites

The following are required:

* Flutter SDK
* Dart SDK
* Android SDK
* Android Studio or another Flutter-compatible IDE
* Android emulator or physical Android device
* Git
* Internet connection
* Access to the SHE++ backend API

Verify Flutter:

```bash
flutter --version
```

Verify the development environment:

```bash
flutter doctor
```

Check connected devices:

```bash
flutter devices
```

## 7.2 Install Dependencies

From the project root:

```bash
flutter pub get
```

To add the HTTP package:

```bash
flutter pub add http
```

To add local storage support:

```bash
flutter pub add shared_preferences
```

After adding dependencies:

```bash
flutter pub get
```

## 7.3 Run the Application

Start the application on a connected Android device or emulator:

```bash
flutter run
```

---

# 8. Code Analysis and Build Preparation

## 8.1 Flutter Analyze

The team uses `flutter analyze` to identify source-code and static-analysis problems.

```bash
flutter analyze
```

This can identify issues including:

* Syntax errors
* Type errors
* Undefined references
* Missing imports
* Unused imports
* Invalid code references
* Other Dart/Flutter analysis issues

## 8.2 Clean Project

When build artifacts or dependency-related issues occur:

```bash
flutter clean
```

Dependencies can then be restored with:

```bash
flutter pub get
```

A standard troubleshooting sequence is:

```bash
flutter analyze
flutter clean
flutter pub get
flutter run
```

---

# 9. APK Build

## 9.1 Debug APK

The debug APK is used for development and internal testing.

Build the debug APK with:

```bash
flutter build apk --debug
```

The generated file is:

```text
build/app/outputs/flutter-apk/app-debug.apk
```

## 9.2 Release APK

The release APK is generated for distribution, demonstration, and final testing.

Build the release APK with:

```bash
flutter build apk --release
```

The generated file is:

```text
build/app/outputs/flutter-apk/app-release.apk
```

The release APK should be tested before distribution or deployment to a demonstration platform.

---

# 10. Frontend References

## 10.1 UI Prototype

The implemented Flutter screens are based on the team's Product design  prototype.

**Prototype:** [https://www.figma.com/proto/fszs1ZQFfdwEBAYyKsuETy/She--?node-id=74-4&t=HLC0KB6rnFXfpfOy-1]

The prototype is provided as a reference for the intended screen designs, layouts, navigation flows, and user interactions.

## 10.2 Appetize

The release APK is uploaded to Appetize to provide browser-based access to the implemented mobile application.

**Appetize:** [https://appetize.io/app/b_6tf2pqfgetc2nydj7lrnuo2nie]

The Appetize deployment is used for application demonstration and testing of the implemented frontend without requiring direct APK installation on every testing device.

---

# 11. Troubleshooting

| Problem                                | Resolution                                                                   |
| -------------------------------------- | ---------------------------------------------------------------------------- |
| Package is missing                     | Run `flutter pub get` or `flutter pub add <package>`                         |
| Dart/Flutter errors are displayed      | Run `flutter analyze` and resolve the reported issues                        |
| Build/cache problems occur             | Run `flutter clean` followed by `flutter pub get`                            |
| Application does not start             | Run `flutter analyze` and inspect the Flutter console output                 |
| API requests fail                      | Verify the backend URL, internet connection, and backend availability        |
| Login fails                            | Verify authentication credentials, API endpoints, and token handling         |
| User is repeatedly redirected to Login | Check access-token and refresh-token storage and authentication bootstrap    |
| Authentication state does not update   | Check `AuthViewModel` state changes and listener notifications               |
| Images/assets are missing              | Verify asset paths and `pubspec.yaml` asset declarations                     |
| Android device is not detected         | Run `flutter devices` and verify the device/emulator                         |
| Debug APK fails to build               | Run `flutter analyze`, `flutter clean`, and `flutter pub get`, then rebuild  |
| Release APK fails to build             | Check build configuration and repeat the clean dependency/build process      |
| Appetize upload fails                  | Verify that a valid Android APK was generated and upload the appropriate APK |

---

# 12. Development Workflow

The mobile development workflow is:

```text
Code Changes
     │
     ▼
flutter analyze
     │
     ▼
Fix Errors
     │
     ▼
flutter pub get
     │
     ▼
flutter run
     │
     ▼
Functional Testing
     │
     ▼
flutter clean
     │
     ▼
flutter build apk --release
     │
     ▼
Upload APK to Appetize
     │
     ▼
Final Testing
```

This workflow provides a consistent process for developing, validating, building, and demonstrating the DadaSafe mobile application.
