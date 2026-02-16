# 🧾 Naming Conventions Guide

This document provides guidelines for naming variables, functions, classes, constants, and files in a clean and consistent way (suitable for Flutter/Dart and most programming languages).

---

## 🔹 1. Variables (camelCase)

Use camelCase and meaningful names.

```dart
String userName;
int userAge;
bool isLoading;
double totalPrice;
```

Avoid:

```dart
var x;
var data;
var temp;
```

---

## 🔹 2. Functions / Methods (camelCase, verb)

Start with an action verb.

```dart
void loadData();
void fetchUserProfile();
bool isValidEmail();
void saveOrder();
```

Avoid:

```dart
void data();
void user();
```

---

## 🔹 3. Classes (PascalCase)

Each word starts with a capital letter.

```dart
class User {}
class ProductService {}
class LoginController {}
class AuthRepository {}
```

Avoid:

```dart
class user {}
class product_class {}
```

---

## 🔹 4. Constants (UPPER_SNAKE_CASE)

```dart
const int MAX_COUNT = 10;
const String BASE_URL = "https://api.example.com";
```

---

## 🔹 5. Files (snake_case)

```text
user_model.dart
login_screen.dart
auth_repository.dart
product_service.dart
```

Avoid:

```text
UserModel.dart
loginScreen.dart
```

---

## 🔹 6. Booleans (start with is / has / can)

```dart
bool isLoggedIn;
bool hasError;
bool canSubmit;
```

---

## 🔹 7. Lists / Collections (plural)

```dart
List<User> users;
List<Product> products;
Map<String, String> headers;
```

---

## 🔹 8. Widgets (PascalCase)

```dart
class LoginScreen extends StatelessWidget {}
class ProductCard extends StatelessWidget {}
class ProfilePage extends StatefulWidget {}
```

---

## 🔹 9. Controllers / Services

```dart
TextEditingController emailController;
ScrollController scrollController;
AuthService authService;
UserRepository userRepository;
```

---

## 🔹 10. Enums (PascalCase)

```dart
enum UserRole { admin, user, guest }
enum AppState { loading, success, error }
```

---

## 🧠 Golden Rule

Name things as if you are explaining them to another developer.

If a variable needs a comment to explain it, rename it.

---

## 🧪 Example (Bad vs Good)

Bad:

```dart
var d;
void f() {}
class a {}
```

Good:

```dart
String userEmail;
void fetchUserData() {}
class UserRepository {}
```

---

## 📁 Example Project Structure

```text
lib/
  models/
    user_model.dart
  services/
    auth_service.dart
  screens/
    login_screen.dart
  widgets/
    product_card.dart
```

---

You can include this file in your project documentation to enforce consistent naming conventions.
