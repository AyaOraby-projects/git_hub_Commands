# 🔒 Flutter Security: The Simple Guide

A practical, easy-to-understand guide to keeping your Flutter app and your users' data safe.

---

## 1. Secure Storage: Where to Keep Secrets

### The Problem
```dart
// ❌ DANGER: Never do this!
SharedPreferences.setString('user_password', 'mypass123');
SharedPreferences.setString('api_key', 'sk_test_123456');
```

### The Solution: Flutter Secure Storage
```dart
import 'package:flutter_secure_storage/flutter_secure_storage.dart';

final storage = FlutterSecureStorage();

// ✅ SAFE: Save sensitive data
await storage.write(key: 'auth_token', value: 'secret_token_123');

// ✅ SAFE: Read it back
String? token = await storage.read(key: 'auth_token');

// ✅ SAFE: Delete when done
await storage.delete(key: 'auth_token');
```

**Why?**  
iOS: Keychain (hardware-encrypted)  
Android: EncryptedSharedPreferences  
Windows/Mac: Secure storage provided by the OS

---

## 2. Hiding API Keys: Don't Put Secrets in Code

### The Problem
```dart
const String apiKey = 'sk_live_abcdefghijklmnop';
const String stripeSecret = 'sk_test_123456789';
```

### Solution 1: Environment Variables (Compile-time)
.env file:
```
API_KEY=sk_live_abcdefghijklmnop
BASE_URL=https://api.myapp.com
```

pubspec.yaml:
```yaml
dependencies:
  flutter_dotenv: ^5.0.2
```

Use in code:
```dart
import 'package:flutter_dotenv/flutter_dotenv.dart';
await dotenv.load();
String apiKey = dotenv.env['API_KEY']!;
```

### Solution 2: Backend Proxy (Best for Production)
```dart
class ApiService {
  final String baseUrl = 'https://myapp.com/api';
  
  Future<Map<String, dynamic>> getData() async {
    final response = await http.get(Uri.parse('$baseUrl/data'));
    return json.decode(response.body);
  }
}
```

Backend Node.js:
```javascript
app.get('/api/data', async (req, res) => {
  const apiKey = process.env.SECRET_API_KEY;
  const data = await fetch('https://thirdparty.com/api', {
    headers: { 'Authorization': `Bearer ${apiKey}` }
  });
  res.json(data);
});
```

---

## 3. Network Security: HTTPS & Certificate Pinning

### Use HTTPS
```dart
final response = await http.get(Uri.parse('https://api.myapp.com/data'));
```

### Certificate Pinning
```dart
import 'package:flutter_secure_certificate_pinning/flutter_secure_certificate_pinning.dart';

final pinner = FlutterSecureCertificatePinning();
await pinner.addTrustedCertificates(
  domain: 'api.myapp.com',
  certificateHashes: ['sha256/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA='],
);

final response = await pinner.get('https://api.myapp.com/data');
```

---

## 4. Input Validation: Stop Injection Attacks
```dart
String safeInput(String input) {
  final sanitized = input.replaceAll(RegExp(r'<[^>]*>'), '');
  return sanitized.length > 100 ? sanitized.substring(0, 100) : sanitized;
}
```

Firebase safe query:
```dart
final users = await FirebaseFirestore.instance
    .collection('users')
    .where('email', isEqualTo: userInputEmail)
    .get();
```

---

## 5. Biometric Authentication
```dart
import 'package:local_auth/local_auth.dart';
final auth = LocalAuthentication();

Future<bool> authenticateUser() async {
  if (!await auth.canCheckBiometrics) return false;
  return await auth.authenticate(
    localizedReason: 'Please authenticate to view sensitive data',
    options: AuthenticationOptions(biometricOnly: true),
  );
}
```

---

## 6. Obfuscation
```yaml
flutter:
  build:
    obfuscate: true
    split-debug-info: /path/to/debug-info
```
```bash
flutter build apk --obfuscate --split-debug-info=debug-info/
flutter build ios --obfuscate --split-debug-info=debug-info/
```

---

## 7. Jailbreak/Root Detection
```dart
import 'package:jailbreak_detection/jailbreak_detection.dart';

Future<bool> isDeviceSecure() async {
  return !(await JailbreakDetection.jailbroken || !await JailbreakDetection.isRealDevice);
}
```

---

## 8. Quick Security Checklist
| What | How | Why |
|------|-----|-----|
| User passwords | flutter_secure_storage | Encrypted storage only |
| API keys | Backend proxy + .env | Never on device |
| Network calls | HTTPS only | Encrypted in transit |
| User input | Validate everything | Prevent injection |
| Sensitive screens | Biometric lock | Extra protection |
| App code | Obfuscate release build | Harder to reverse |
| Deep links | Validate incoming links | Prevent phishing |

---

## Complete Example
```dart
import 'package:flutter/material.dart';
import 'package:flutter_secure_storage/flutter_secure_storage.dart';
import 'package:local_auth/local_auth.dart';
import 'package:http/http.dart' as http;

class SecureLoginScreen extends StatefulWidget {
  @override
  _SecureLoginScreenState createState() => _SecureLoginScreenState();
}

class _SecureLoginScreenState extends State<SecureLoginScreen> {
  final storage = FlutterSecureStorage();
  final auth = LocalAuthentication();

  Future<void> login(String username, String password) async {
    if (username.isEmpty || password.isEmpty) return;
    final response = await http.post(Uri.parse('https://your-backend.com/login'), body: {'username': username, 'password': password});
    if (response.statusCode == 200) {
      String token = json.decode(response.body)['token'];
      await storage.write(key: 'auth_token', value: token);
      if (await auth.canCheckBiometrics) {
        await storage.write(key: 'use_biometrics', value: 'true');
      }
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ElevatedButton(
          onPressed: () => login('user', 'pass'),
          child: Text('Secure Login'),
        ),
      ),
    );
  }
}
```

**References:**
- [Flutter Security Documentation](https://flutter.dev/docs/deployment/security)
- [OWASP Mobile Security](https://owasp.org/www-project-mobile-top-10/)

