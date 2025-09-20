
# Insight SDK — On‑Device AI Analytics (Sentiment + Keywords)

> A reusable **SDK** for HR/LMS apps that analyzes free‑text **100% on device** and emits auditable logs.

---

## Deliverables
- **iOS:** Swift Package / CocoaPod  
- **Android:** AAR via Maven Local / Gradle  
- **React Native Wrapper:** TypeScript module exposing the same API

---

## Public API

**Swift**
```swift
public struct InsightResult: Codable {
  public let sentiment: String   // "positive" | "neutral" | "negative"
  public let keywords: [String]
  public let modelVersion: String
}

public enum LogLevel { case off, error, info, debug }

public final class Insight {
  public static func configure(logLevel: LogLevel = .info)
  public static func analyze(text: String) throws -> InsightResult
}
```

**Kotlin**
```kotlin
data class InsightResult(
  val sentiment: String,
  val keywords: List<String>,
  val modelVersion: String
)

object Insight {
  fun configure(logLevel: LogLevel = LogLevel.INFO)
  fun analyze(text: String): InsightResult
}
```

**React Native**
```ts
import { analyze, configure } from "@insight-sdk/react-native";
configure({ logLevel: "info" });
const out = await analyze("The course was helpful but the quizzes were tricky.");
```

---

## How It Works (Edge‑Aware Kernel)

- Packs a compact sentiment model + lightweight keyword extractor (e.g., embedded TextRank or on‑device embedding + top‑N).  
- **No network calls.** All computation is on device for privacy and offline reliability.  
- Ships with **quantized** Core ML / TFLite models for fast inference.

---

## Traceability & Audit Logging

Every analysis call can record a tamper‑evident entry:
```
[timestamp] fn=analyze  input_sha256=0x9a...  sentiment=neutral  keywords=["helpful","quizzes"]
model=insight-mini-1.0  sdk=1.0.0  device=iPhone15,3  os=iOS17.5
```
- Configurable `logLevel` and log destination.  
- Export to JSON for **FERPA/GDPR** reviews or SOC2 evidence.

---

## Installation

**iOS (Swift Package Manager)**
1. Xcode → Add Package → `https://github.com/your-org/insight-sdk-ios`
2. `import InsightSDK`

**Android (Gradle)**
```gradle
dependencies {
  implementation "com.yourorg:insight-sdk-android:1.0.0"
}
```

**React Native Wrapper**
```bash
yarn add @insight-sdk/react-native
cd ios && pod install && cd ..
```

---

## Demo App (Consumer’s View)

A tiny RN app with a textarea and **Analyze** button:
- Shows JSON result.
- Toggle for log level.
- Export logs as a `.json` file.

> Demonstrates you understand both producer (SDK) and consumer (app) perspectives.

---

## Packaging the Models

- iOS: `.mlmodelc` in `Sources/InsightSDK/Resources/Models/`  
- Android: `.tflite` in `insight-sdk-android/src/main/assets/models/`
- Provide a script to verify hashes/versioning for SBOM/Supply‑chain checks.

---

## Security & Privacy

- No text leaves the device.  
- Optional local encryption for on‑device logs.  
- Clear **PII guidance** in README for integrators.

---

## Roadmap
- [ ] Add language detection + per‑locale sentiment tables.
- [ ] Provide WASM build for desktop POCs.
- [ ] Pluggable keyword extraction strategies.
- [ ] Signed model bundles with version pinning.

## License
Copyright (c) 2025 MonteyAI LLC. All rights reserved.

This software and associated documentation files (the "Software") are proprietary 
to MonteyAI LLC. and are protected by copyright law.

VIEWING PERMITTED: This code is available for viewing, educational, and 
evaluation purposes only.

RESTRICTIONS:
- No commercial use without explicit written permission from MONTEYcodes
- No redistribution, modification, or derivative works
- No reverse engineering or decompilation
- No use in production environments without valid license
