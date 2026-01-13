### **பகுதி 3: Flutter உடன் தொடங்குதல் (Getting Started with Flutter)**

#### **1. Flutter-ன் மையக் கருத்து: "எல்லாமே விட்ஜெட்கள் தான்!" (Everything is a Widget!)**

Flutter-ல், நீங்கள் திரையில் பார்க்கும் ஒவ்வொரு கூறும் ஒரு விட்ஜெட் தான். ஒரு பட்டன், ஒரு டெக்ஸ்ட், ஒரு படம், ஏன் முழு செயலியின் திரையே ஒரு விட்ஜெட் தான்.

**விட்ஜெட் மரம் (Widget Tree):**
Flutter செயலிகள் விட்ஜெட்களால் ஆன ஒரு மரத்தைப் போல கட்டமைக்கப்படுகின்றன. ஒரு விட்ஜெட் மற்றொரு விட்ஜெட்டைக் கொண்டிருக்கலாம். இந்த அமைப்புதான் உங்கள் செயலியின் முழு UI-ஐயும் உருவாக்குகிறது.

உதாரணமாக:
*   `Scaffold` (அடிப்படைத் திரை அமைப்பு)
    *   `AppBar` (மேல் பட்டி)
        *   `Text` (தலைப்பு)
    *   `Column` (நெடுவரிசை அமைப்பு)
        *   `Image` (படம்)
        *   `Text` (விளக்கம்)
        *   `ElevatedButton` (ஒரு பட்டன்)

இந்த அமைப்புதான் உங்கள் செயலியின் கட்டமைப்பைத் தீர்மானிக்கிறது.

---

#### **2. Stateless vs Stateful Widgets (இது மிக முக்கியம்)**

இது Flutter-ல் உள்ள இரண்டு மிக முக்கியமான விட்ஜெட் வகைகளாகும்.

**அ) `StatelessWidget` (நிலை இல்லாத விட்ஜெட்):**

*   **பொருள்:** இந்த விட்ஜெட்கள் மாறாதவை (immutable). ஒருமுறை உருவாக்கப்பட்ட பிறகு, அவற்றின் உள்ளடக்கத்தையோ அல்லது தோற்றத்தையோ மாற்ற முடியாது.
*   **எப்போது பயன்படுத்த வேண்டும்?:** திரையில் ஒருமுறை காட்டப்பட்ட பிறகு மாறாத கூறுகளுக்கு (static content) இதைப் பயன்படுத்தலாம்.
    *   உதாரணங்கள்: `Icon`, `Text` (ஒருமுறை தலைப்பைக் காட்டிய பிறகு மாறாது), `AppBar`.
*   **முக்கிய அம்சம்:** இதற்குள் `state` (நிலை) இருக்காது. இது ஒருமுறை `build` செய்யப்படும், அவ்வளவுதான்.

**StatelessWidget-க்கான குறியீட்டு அமைப்பு:**
```dart
import 'package:flutter/material.dart';

// A simple widget that displays a static text.
class GreetingText extends StatelessWidget {
  // It can receive data via its constructor.
  final String message;

  const GreetingText({Key? key, required this.message}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // The build method returns the widget's UI.
    // This method is called only once when the widget is created.
    return Text(
      message,
      style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold),
    );
  }
}
```

**ஆ) `StatefulWidget` (நிலை உள்ள விட்ஜெட்):**

*   **பொருள்:** இந்த விட்ஜெட்கள் தங்களின் வாழ்நாளில் மாறக்கூடியவை (mutable). பயனர் செயல்கள் (user interactions) அல்லது தரவு மாற்றங்களுக்கு ஏற்ப அவற்றின் தோற்றத்தை மாற்றிக்கொள்ள முடியும்.
*   **எப்போது பயன்படுத்த வேண்டும்?:** பயனர் உள்ளீடு, அனிமேஷன்கள், அல்லது தரவு மாறும்போது UI-ஐ புதுப்பிக்க வேண்டிய இடங்களில் இதைப் பயன்படுத்த வேண்டும்.
    *   உதாரணங்கள்: ஒரு checkbox, ஒரு counter, ஒரு படிவம் (form).
*   **முக்கிய அம்சம்:** இது இரண்டு வகுப்புகளைக் கொண்டது: `StatefulWidget` மற்றும் அதனுடன் இணைந்த `State` வகுப்பு.
    *   `State` வகுப்புதான் இந்த விட்ஜெட்டின் நிலையை (data) வைத்திருக்கும்.
    *   `setState()` என்ற முறையை அழைப்பதன் மூலம், Flutter-க்கு "இந்த விட்ஜெட்டின் நிலை மாறிவிட்டது, UI-ஐ மீண்டும் வரையவும் (`rebuild`)" என்று சொல்கிறோம்.

**StatefulWidget-க்கான குறியீட்டு அமைப்பு:**
```dart
import 'package:flutter/material.dart';

// This widget displays a number that increments when a button is pressed.
class CounterWidget extends StatefulWidget {
  const CounterWidget({Key? key}) : super(key: key);

  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}

class _CounterWidgetState extends State<CounterWidget> {
  // The 'state' of this widget. This variable can change.
  int _counter = 0;

  void _incrementCounter() {
    // setState() tells Flutter that the state has changed and the UI needs to be rebuilt.
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    // This build method is called again every time setState() is called.
    return Column(
      mainAxisAlignment: MainAxisAlignment.center,
      children: <Widget>[
        Text(
          'You have pushed the button this many times:',
        ),
        Text(
          '$_counter',
          style: Theme.of(context).textTheme.headlineMedium,
        ),
        ElevatedButton(
          onPressed: _incrementCounter, // Call the method to change the state
          child: Icon(Icons.add),
        )
      ],
    );
  }
}
```

---

#### **3. அடிப்படை மற்றும் லேஅவுட் விட்ஜெட்டுகள் (Basic & Layout Widgets)**

ஒரு செயலியை உருவாக்கப் பயன்படும் மிக முக்கியமான சில விட்ஜெட்களைப் பார்ப்போம்.

**அ) முக்கிய விட்ஜெட்கள் (Core Widgets):**

*   **`Scaffold`:** ஒரு செயலியின் அடிப்படைத் திரை அமைப்பை வழங்குகிறது. இது `AppBar`, `body`, `FloatingActionButton` போன்ற பொதுவான UI கூறுகளை எளிதாக அமைக்க உதவுகிறது.
*   **`AppBar`:** திரையின் மேலே தோன்றும் பட்டி. பொதுவாக தலைப்பு, செயல்கள் (actions), மற்றும் வழிசெலுத்தல் (navigation) ஐகான்களைக் கொண்டிருக்கும்.
*   **`Container`:** இது ஒரு பெட்டி போன்றது. இதற்கு வண்ணம் (`color`), அளவு (`width`, `height`), உள் இடைவெளி (`padding`), வெளி இடைவெளி (`margin`), மற்றும் எல்லை (`border`) போன்ற பண்புகளைக் கொடுக்கலாம். UI-ஐ வடிவமைக்க இது மிகவும் பயனுள்ள விட்ஜெட்.
*   **`Text`:** திரையில் உரைகளைக் காட்டப் பயன்படுகிறது. இதற்கு `style` என்ற பண்பு மூலம் எழுத்துரு அளவு, நிறம், தடிமன் போன்றவற்றை மாற்றலாம்.
*   **`Icon`:** ஐகான்களைக் காட்டப் பயன்படுகிறது. Material Icons நூலகத்திலிருந்து ஆயிரக்கணக்கான ஐகான்களை நாம் பயன்படுத்தலாம்.
*   **`Image`:** படங்களைக் காட்டப் பயன்படுகிறது. இணையத்திலிருந்து (`Image.network`), செயலியின் கோப்புகளிலிருந்து (`Image.asset`), அல்லது கோப்பு அமைப்பிலிருந்து (`Image.file`) படங்களைக் காட்டலாம்.

**ஒரு எளிய திரையை உருவாக்கும் குறியீடு:**
```dart
import 'package:flutter/material.dart';

class SimpleProfileScreen extends StatelessWidget {
  const SimpleProfileScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('My Profile'),
        backgroundColor: Colors.blue,
      ),
      body: Container(
        color: Colors.grey[200], // Background color for the whole body
        child: Center(
          child: Text(
            'Welcome to my first Flutter screen!',
            style: TextStyle(fontSize: 22),
          ),
        ),
      ),
    );
  }
}
```

**ஆ) லேஅவுட் விட்ஜெட்கள் (Layout Widgets):**

இந்த விட்ஜெட்கள் மற்ற விட்ஜெட்களை எப்படி திரையில் ஒழுங்கமைப்பது என்பதைத் தீர்மானிக்கின்றன.

*   **`Column`:** விட்ஜெட்களை ஒன்றன் கீழ் ஒன்றாக, செங்குத்தாக (vertically) அடுக்கப் பயன்படுகிறது.
*   **`Row`:** விட்ஜெட்களை ஒன்றன் பின் ஒன்றாக, கிடைமட்டமாக (horizontally) அடுக்கப் பயன்படுகிறது.
    *   **முக்கிய பண்புகள் (`mainAxisAlignment`, `crossAxisAlignment`):** `Row`-க்கு `mainAxisAlignment` என்பது கிடைமட்டமாகவும், `crossAxisAlignment` என்பது செங்குத்தாகவும் இருக்கும். `Column`-க்கு இது நேர்மாறாக இருக்கும்.
*   **`Padding`:** ஒரு விட்ஜெட்டைச் சுற்றி உள் இடைவெளியை உருவாக்கப் பயன்படுகிறது.
*   **`Center`:** தனக்குக் கீழ் உள்ள விட்ஜெட்டை திரையின் மையத்தில் வைக்கிறது.
*   **`Expanded`:** `Row` அல்லது `Column`-க்குள் ஒரு விட்ஜெட்டிற்கு மீதமுள்ள முழு இடத்தையும் கொடுக்கப் பயன்படுகிறது. இது நெகிழ்வான UI-களை உருவாக்க மிகவும் முக்கியமானது.
*   **`ListView`:** உருட்டக்கூடிய (scrollable) ஒரு விட்ஜெட்களின் பட்டியலை உருவாக்கப் பயன்படுகிறது. அதிக எண்ணிக்கையிலான பொருட்களைக் காட்ட இது சிறந்தது.

**லேஅவுட் விட்ஜெட்களைப் பயன்படுத்தும் குறியீடு:**
```dart
import 'package:flutter/material.dart';

class BasicLayoutScreen extends StatelessWidget {
  const BasicLayoutScreen({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Layout Demo'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0), // Outer padding for the whole body
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start, // Align children to the start (left)
          children: [
            Text('Hello Flutter!', style: TextStyle(fontSize: 28, fontWeight: FontWeight.bold)),
            SizedBox(height: 10), // A simple way to add space
            
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceBetween, // Distribute space between children
              children: [
                Icon(Icons.thumb_up, color: Colors.green, size: 40),
                Icon(Icons.thumb_down, color: Colors.red, size: 40),
                Icon(Icons.share, color: Colors.blue, size: 40),
              ],
            ),
            
            SizedBox(height: 20),
            
            Container(
              padding: EdgeInsets.all(12),
              color: Colors.amber,
              child: Text('This is inside a Container.'),
            ),

            SizedBox(height: 20),

            // Expanded Widget Demo
            Row(
              children: [
                Container(color: Colors.red, height: 50, width: 50),
                Expanded(
                  child: Container(
                    color: Colors.green,
                    height: 50,
                    child: Center(child: Text('I take up all remaining space!')),
                  ),
                ),
                Container(color: Colors.blue, height: 50, width: 50),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

---

#### **4. பயனர் உள்ளீடு மற்றும் ஊடாடும் விட்ஜெட்டுகள் (User Input & Interactive Widgets)**

இந்த விட்ஜெட்கள் பயனரிடமிருந்து உள்ளீடுகளைப் பெறவும், செயல்களைச் செய்யவும் உதவுகின்றன.

*   **`ElevatedButton`:** ஒரு நிழலுடன் கூடிய, உயர்த்தப்பட்ட பட்டன்.
*   **`TextButton`:** ஒரு தட்டையான பட்டன், பொதுவாக உரையை மட்டும் கொண்டிருக்கும்.
*   **`TextField`:** பயனரிடமிருந்து உரையை உள்ளீடாகப் பெறப் பயன்படும் ஒரு உரைப்பெட்டி (text field).
*   **`onPressed` Callback:** பட்டன்கள் அழுத்தப்படும்போது என்ன செய்ய வேண்டும் என்பதைக் குறிப்பிடும் ஒரு செயல்பாடு. இது கட்டாயமாக வழங்கப்பட வேண்டும். `null` என்று கொடுத்தால் பட்டன் செயலிழந்துவிடும் (disabled).

**ஊடாடும் விட்ஜெட்களைப் பயன்படுத்தும் குறியீடு (StatefulWidget தேவை):**
```dart
import 'package:flutter/material.dart';

class InteractiveScreen extends StatefulWidget {
  const InteractiveScreen({Key? key}) : super(key: key);

  @override
  _InteractiveScreenState createState() => _InteractiveScreenState();
}

class _InteractiveScreenState extends State<InteractiveScreen> {
  String _displayText = 'Press the button!';
  final TextEditingController _textController = TextEditingController();

  void _updateText() {
    setState(() {
      _displayText = 'Hello, ${_textController.text}!';
      if (_textController.text.isEmpty) {
        _displayText = 'Please enter your name.';
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('User Interaction'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(20.0),
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            // Display text that changes
            Text(
              _displayText,
              style: TextStyle(fontSize: 24),
              textAlign: TextAlign.center,
            ),
            SizedBox(height: 30),

            // Text input field
            TextField(
              controller: _textController, // Controller to get the text value
              decoration: InputDecoration(
                labelText: 'Enter your name',
                border: OutlineInputBorder(),
              ),
            ),
            SizedBox(height: 20),

            // A button to trigger the action
            ElevatedButton(
              onPressed: _updateText, // Call the method when pressed
              child: Text('Submit'),
            ),
          ],
        ),
      ),
    );
  }
}
```

---
