### **பகுதி 1: அறிமுகம் மற்றும் அமைவு (Introduction & Setup)**

#### **Flutter என்றால் என்ன? Dart ஏன்?**

**Flutter அறிமுகம்:**

Flutter என்பது கூகிளால் உருவாக்கப்பட்ட ஒரு திறந்த மூல (open-source) UI மென்பொருள் மேம்பாட்டுக் கருவி (UI software development kit) ஆகும். இதன் முக்கிய நோக்கம், ஒரே ஒரு நிரல் குறியீட்டை (Single Codebase) மட்டும் கொண்டு Android, iOS, Web, Windows, macOS, மற்றும் Linux என அனைத்து முக்கிய தளங்களுக்கும் (platforms) செயலிகளை (applications) உருவாக்குவதுதான்.

**Flutter-ன் முக்கிய நன்மைகள்:**

1.  **வேகமான உருவாக்கம் (Fast Development):** "Hot Reload" என்ற வசதி மூலம், நீங்கள் குறியீட்டில் செய்யும் மாற்றங்களை உடனடியாகச் செயலியின் திரையில் பார்க்க முடியும். இது சோதித்துப் பார்ப்பதற்கும் (testing), பிழைகளைக் களைவதற்கும் (debugging) ஆகும் நேரத்தை வெகுவாகக் குறைக்கிறது.
2.  **அழகான மற்றும் நெகிழ்வான UI (Expressive and Flexible UI):** Flutter, விட்ஜெட்களை (Widgets) அடிப்படையாகக் கொண்டது. இதன் மூலம், உங்கள் செயலியின் ஒவ்வொரு காட்சியையும் (pixel) உங்கள் கட்டுப்பாட்டில் வைத்து, மிக அழகான மற்றும் தனித்துவமான பயனர் இடைமுகங்களை (User Interfaces) உருவாக்க முடியும்.
3.  **சிறந்த செயல்திறன் (Excellent Performance):** Flutter செயலிகள் நேரடியாக இயந்திரக் குறியீட்டிற்கு (native machine code) தொகுக்கப்படுவதால் (compiled), அவை மிக வேகமாகவும், சீராகவும் இயங்கும்.

**Dart மொழி ஏன் Flutter-க்கு பயன்படுத்தப்படுகிறது?**

Dart என்பது கூகிளால் உருவாக்கப்பட்ட, பொருள் சார்ந்த நிரலாக்க மொழி (Object-Oriented Programming language). இது Flutter-க்காகவே சிறப்பாக வடிவமைக்கப்பட்டுள்ளது.

*   **JIT (Just-In-Time) Compilation:** இது "Hot Reload" போன்ற வேகமான மேம்பாட்டு அம்சங்களைச் சாத்தியமாக்குகிறது.
*   **AOT (Ahead-Of-Time) Compilation:** செயலி வெளியிடப்படும் போது, Dart குறியீடு நேரடியாக ARM அல்லது x86 இயந்திரக் குறியீட்டிற்குத் தொகுக்கப்படுகிறது. இது செயலியை மிக வேகமாக இயங்க வைக்கிறது.

---

#### **Flutter-ஐ கணினியில் நிறுவுதல் (Setup Process)**

மாணவர்களுக்கு செயல்முறையை விளக்குவதற்கான குறிப்புகள் இங்கே.

**தேவையான மென்பொருட்கள் (Prerequisites):**

1.  **Flutter SDK (Software Development Kit):** இது Flutter செயலிகளை உருவாக்கத் தேவையான அனைத்து கருவிகளையும், நூலகங்களையும் (libraries) கொண்டது.
2.  **ஒரு குறியீட்டு எடிட்டர் (Code Editor):**
    *   **Visual Studio Code (பரிந்துரைக்கப்படுகிறது):** இலகுவானது மற்றும் Flutter மற்றும் Dart-க்கான சிறந்த நீட்டிப்புகளை (extensions) கொண்டது.
    *   **Android Studio:** இது ஒரு முழுமையான IDE. Android Emulator மற்றும் பிற கருவிகள் இதிலேயே உள்ளன.
3.  **ஒரு Emulator அல்லது Simulator:**
    *   **Android Emulator:** Android Studio-ஐ நிறுவும் போது இதை அமைத்துக் கொள்ளலாம்.
    *   **iOS Simulator:** இது Xcode உடன் வருகிறது (macOS-ல் மட்டும் கிடைக்கும்).
    *   **உண்மையான சாதனம் (Physical Device):** உங்கள் சொந்த Android அல்லது iOS போனையும் பயன்படுத்தலாம்.

**நிறுவுதலின் முக்கிய படிகள் (Key Installation Steps):**

1.  Flutter-ன் அதிகாரப்பூர்வ இணையதளத்தில் இருந்து உங்கள் இயக்க முறைமைக்கு (Operating System) ஏற்ற Flutter SDK-ஐ பதிவிறக்கம் செய்யவும்.
2.  பதிவிறக்கம் செய்த ZIP கோப்பை உங்கள் கணினியில் நிரந்தரமான இடத்தில் (உதாரணமாக, `C:\src\flutter` அல்லது `/Users/your-name/development`) பிரித்தெடுக்கவும் (extract).
3.  Flutter SDK-ன் `bin` கோப்புறையின் பாதையை (path) உங்கள் கணினியின் `environment variables`-ல் சேர்க்கவும். இதுதான் நீங்கள் எந்த இடத்திலிருந்தும் `flutter` கட்டளையை இயக்க உதவும்.
4.  உங்கள் குறியீட்டு எடிட்டரில் (VS Code/Android Studio) Flutter மற்றும் Dart நீட்டிப்புகளை (extensions/plugins) நிறுவவும்.
5.  கட்டளை வரியில் (Command Prompt/Terminal) `flutter doctor` என்ற கட்டளையை இயக்கவும்.

**`flutter doctor`-ன் பங்கு:**

`flutter doctor` என்பது ஒரு கண்டறியும் கருவி (diagnostic tool). இது உங்கள் கணினியில் Flutter-க்குத் தேவையான மென்பொருட்கள் சரியாக நிறுவப்பட்டுள்ளதா, பாதைகள் சரியாக அமைக்கப்பட்டுள்ளதா எனச் சரிபார்த்து ஒரு அறிக்கையைத் தரும். ஏதேனும் சிக்கல்கள் இருந்தால், அதை எப்படிச் சரி செய்வது என்பதற்கான வழிமுறைகளையும் வழங்கும்.

```bash
# Run this command in your terminal
flutter doctor
```

**வெற்றிகரமான `flutter doctor` வெளியீட்டின் ஒரு பகுதி:**

```
[✓] Flutter (Channel stable, 3.x.x, on Microsoft Windows ...)
[✓] Android toolchain - develop for Android devices (Android SDK version 33.0.0)
[✓] Chrome - develop for the web
[✓] Visual Studio - develop for Windows (Visual Studio Community 2022)
[✓] Android Studio (version 2022.1)
[✓] VS Code (version 1.7x.x)
[✓] Connected device (1 available)
[✓] HTTP Host Availability

• No issues found!
```

---
