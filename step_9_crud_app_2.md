நிச்சயமாக. உங்கள் விருப்பப்படியே, கேமரா செயல்பாட்டையும் இதில் சேர்ப்போம்.

நான் முன்பு கூறியது போல், இது ஒரு மேம்பட்ட மற்றும் சிக்கலான தலைப்பு. எனவே, ஒவ்வொரு படியையும் மிகவும் கவனமாகப் பின்பற்ற வேண்டும். மாணவர்கள் தங்கள் கணினியில் இதைச் செய்யும்போது, குறிப்பாக அனுமதிகள் (Permissions) பகுதியில், சில சிக்கல்களைச் சந்திக்க வாய்ப்புள்ளது. இதை மனதில் வைத்து, நான் குறிப்புகளை மிகவும் தெளிவாகத் தருகிறேன்.

நாம் ஏற்கனவே உருவாக்கிய SQLite CRUD செயலியிலிருந்து இதைத் தொடர்வோம்.

---

### **பகுதி 6 (விரிவாக்கம்): SQLite, Camera உடன் முழுமையான CRUD செயலி**

**புதிய நோக்கம்:**
1.  பயனர்களைச் சேர்க்கும்போது, அவர்களின் புகைப்படத்தையும் கேமரா அல்லது கேலரியில் இருந்து தேர்ந்தெடுக்க அனுமதிப்பது.
2.  பயனரின் புகைப்படத்தை சாதனத்தில் சேமித்து, அதன் பாதையை (path) மட்டும் SQLite தரவுத்தளத்தில் சேமிப்பது.
3.  பயனர்களின் பட்டியலைக் காட்டும்போது, அவர்களின் புகைப்படத்தையும் காட்டுவது.

#### **படி 1: புதிய தொகுப்புகளைச் சேர்த்தல் (Adding New Packages)**

`pubspec.yaml` கோப்பில் இந்த இரண்டு புதிய தொகுப்புகளையும் சேர்க்கவும்:
1.  **`image_picker`**: கேமரா மற்றும் கேலரியில் இருந்து படங்களைத் தேர்ந்தெடுக்க உதவும்.
2.  **`path_provider`**: செயலிக்கான பிரத்யேக கோப்பகத்தின் (directory) பாதையைப் பெற உதவும். இங்குதான் நாம் படங்களைச் சேமிப்போம்.

```yaml
dependencies:
  flutter:
    sdk: flutter
  sqflite: ^2.3.0
  path: ^1.8.3
  
  # Add these two new packages
  image_picker: ^1.0.4
  path_provider: ^2.1.1
```
கோப்பைச் சேமித்து `flutter pub get` கட்டளையை இயக்கவும்.

#### **படி 2: தளவாரியான அனுமதிகளை அமைத்தல் (Platform-Specific Permissions)**

**இது மிக மிக முக்கியமான படி.** இதைச் செய்யத் தவறினால், செயலி செயலிழந்துவிடும் (crash).

**அ) Android-க்காக (`android/app/src/main/AndroidManifest.xml`):**
இந்தக் கோப்பைத் திறந்து, `<manifest>` குறிச்சொல்லுக்குள் (tag) இந்த வரிகளைச் சேர்க்கவும்:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android">
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
    ...
    <application ...>
        ...
    </application>
</manifest>
```

**ஆ) iOS-க்காக (`ios/Runner/Info.plist`):**
இந்தக் கோப்பைத் திறந்து, `<dict>` குறிச்சொல்லுக்குள் இந்த வரிகளைச் சேர்க்கவும்:

```xml
<dict>
    ...
    <key>NSCameraUsageDescription</key>
    <string>This app needs camera access to take user profile pictures.</string>
    <key>NSPhotoLibraryUsageDescription</key>
    <string>This app needs photo library access to select user profile pictures.</string>
    ...
</dict>
```

#### **படி 3: தரவுத்தளம் மற்றும் மாதிரியைப் புதுப்பித்தல்**

**அ) `DatabaseHelper`-ஐப் புதுப்பித்தல்:**
`users` அட்டவணையில், படப் பாதையைச் சேமிக்க `imagePath` என்ற புதிய நிரலை (column) சேர்க்க வேண்டும்.

**முக்கிய குறிப்பு:** நீங்கள் ஏற்கனவே செயலியை இயக்கி, தரவுத்தளத்தை உருவாக்கியிருந்தால், இந்த மாற்றத்தைப் பயன்படுத்த, **பழைய செயலியை உங்கள் சாதனத்திலிருந்து uninstall செய்துவிட்டு, மீண்டும் இயக்கவும்.** அப்போதுதான் `onCreate` மீண்டும் அழைக்கப்பட்டு, புதிய அட்டவணை உருவாக்கப்படும்.

`database_helper.dart`-ல் `_onCreate` முறையை மாற்றவும்:
```dart
// inside database_helper.dart
Future<void> _onCreate(Database db, int version) async {
  await db.execute('''
    CREATE TABLE users (
      id INTEGER PRIMARY KEY AUTOINCREMENT,
      name TEXT NOT NULL,
      email TEXT NOT NULL,
      imagePath TEXT  -- Add this new column
    )
  ''');
}
```

**ஆ) `User` Model-ஐப் புதுப்பித்தல்:**
`user_model.dart`-ல் `imagePath` என்ற புதிய property-ஐச் சேர்க்கவும்.

```dart
// inside user_model.dart
class User {
  final int? id;
  String name;
  String email;
  String? imagePath; // Make it nullable

  User({this.id, required this.name, required this.email, this.imagePath});

  Map<String, dynamic> toMap() {
    return {
      'id': id,
      'name': name,
      'email': email,
      'imagePath': imagePath, // Add to map
    };
  }

  factory User.fromMap(Map<String, dynamic> map) {
    return User(
      id: map['id'],
      name: map['name'],
      email: map['email'],
      imagePath: map['imagePath'], // Read from map
    );
  }
}
```

#### **படி 4: பயனர் இடைமுகத்தைப் புதுப்பித்தல் (`UserScreen` UI)**

இதுதான் பெரும்பாலான மாற்றங்கள் நிகழும் இடம்.

**1. தேவையான தொகுப்புகளை import செய்யவும்:**
`user_screen.dart`-ன் மேலே இந்த வரிகளைச் சேர்க்கவும்:
```dart
import 'dart:io'; // For File operations
import 'package:image_picker/image_picker.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart' as path;
```

**2. `_UserScreenState`-ல் புதிய state மாறியைச் சேர்க்கவும்:**
படத்தைத் தேர்ந்தெடுத்த பிறகு, அதைத் தற்காலிகமாக வைத்திருக்க இந்த மாறி உதவும்.
```dart
class _UserScreenState extends State<UserScreen> {
  // ... existing variables
  File? _imageFile; // To hold the selected image file
  // ...
}
```

**3. `_showFormDialog`-ஐ முழுமையாக மாற்றுதல்:**
படத் தேர்வு மற்றும் முன்னோட்டத்தைக் (preview) காட்ட, இந்த உரையாடல் (dialog) இப்போது மிகவும் சிக்கலானதாக மாறும்.

```dart
// inside _UserScreenState class
void _showFormDialog({User? user}) {
  _imageFile = null; // Reset image file on each open
  
  if (user != null) {
    _nameController.text = user.name;
    _emailController.text = user.email;
    // Note: We don't load the old image file here, just its path is stored in 'user'.
  } else {
    _nameController.clear();
    _emailController.clear();
  }

  showDialog(
    context: context,
    builder: (_) {
      // Use StatefulBuilder to manage the state of the dialog's content (image preview)
      return StatefulBuilder(
        builder: (context, setState) {
          
          Future<void> _pickImage(ImageSource source) async {
            final pickedFile = await ImagePicker().pickImage(source: source);
            if (pickedFile != null) {
              setState(() {
                _imageFile = File(pickedFile.path);
              });
            }
          }

          return AlertDialog(
            title: Text(user == null ? 'Add User' : 'Edit User'),
            content: SingleChildScrollView( // To avoid overflow
              child: Column(
                mainAxisSize: MainAxisSize.min,
                children: [
                  // Image Preview
                  CircleAvatar(
                    radius: 50,
                    backgroundImage: _imageFile != null
                        ? FileImage(_imageFile!)
                        : (user?.imagePath != null
                            ? FileImage(File(user!.imagePath!))
                            : null) as ImageProvider?,
                    child: _imageFile == null && user?.imagePath == null
                        ? Icon(Icons.person, size: 50)
                        : null,
                  ),
                  SizedBox(height: 10),
                  // Image Picker Buttons
                  Row(
                    mainAxisAlignment: MainAxisAlignment.center,
                    children: [
                      IconButton(
                        icon: Icon(Icons.camera_alt),
                        onPressed: () => _pickImage(ImageSource.camera),
                      ),
                      IconButton(
                        icon: Icon(Icons.photo_library),
                        onPressed: () => _pickImage(ImageSource.gallery),
                      ),
                    ],
                  ),
                  TextField(controller: _nameController, decoration: InputDecoration(labelText: 'Name')),
                  TextField(controller: _emailController, decoration: InputDecoration(labelText: 'Email')),
                ],
              ),
            ),
            actions: [
              TextButton(child: Text('Cancel'), onPressed: () => Navigator.of(context).pop()),
              ElevatedButton(
                child: Text(user == null ? 'Add' : 'Update'),
                onPressed: () async {
                  String? imagePath;
                  if (_imageFile != null) {
                    // Save the image to the app directory
                    final appDir = await getApplicationDocumentsDirectory();
                    final fileName = path.basename(_imageFile!.path);
                    final savedImage = await _imageFile!.copy('${appDir.path}/$fileName');
                    imagePath = savedImage.path;
                  } else {
                    // Keep the old image path if not changed
                    imagePath = user?.imagePath;
                  }

                  final userToSave = User(
                    id: user?.id,
                    name: _nameController.text,
                    email: _emailController.text,
                    imagePath: imagePath,
                  );

                  if (user == null) {
                    await dbHelper.createUser(userToSave);
                  } else {
                    await dbHelper.updateUser(userToSave);
                  }
                  
                  _refreshUserList();
                  Navigator.of(context).pop();
                },
              ),
            ],
          );
        },
      );
    },
  );
}
```

**4. `ListView.builder`-ஐப் புதுப்பித்தல்:**
பயனரின் புகைப்படத்தைக் காட்ட, `ListTile`-ன் `leading` பண்பைப் பயன்படுத்துவோம்.
```dart
// inside the build method's ListView.builder
return Card(
  margin: EdgeInsets.all(8),
  child: ListTile(
    leading: CircleAvatar(
      backgroundImage: user.imagePath != null && user.imagePath!.isNotEmpty
          ? FileImage(File(user.imagePath!))
          : null,
      child: user.imagePath == null || user.imagePath!.isEmpty
          ? Icon(Icons.person)
          : null,
    ),
    title: Text(user.name),
    subtitle: Text(user.email),
    // ... trailing icons remain the same
  ),
);
```

---

#### **முழுமையான புதுப்பிக்கப்பட்ட `user_screen.dart`**
```dart
import 'package:flutter/material.dart';
import 'dart:io';
import 'package:image_picker/image_picker.dart';
import 'package:path_provider/path_provider.dart';
import 'package:path/path.dart' as path;
import 'database_helper.dart';
import 'user_model.dart';

class UserScreen extends StatefulWidget {
  const UserScreen({Key? key}) : super(key: key);
  @override
  _UserScreenState createState() => _UserScreenState();
}

class _UserScreenState extends State<UserScreen> {
  late Future<List<User>> _userList;
  final dbHelper = DatabaseHelper();
  final _nameController = TextEditingController();
  final _emailController = TextEditingController();
  File? _imageFile;

  @override
  void initState() {
    super.initState();
    _refreshUserList();
  }

  void _refreshUserList() {
    setState(() {
      _userList = dbHelper.getAllUsers();
    });
  }

  void _showFormDialog({User? user}) {
    _imageFile = null;
    if (user != null) {
      _nameController.text = user.name;
      _emailController.text = user.email;
    } else {
      _nameController.clear();
      _emailController.clear();
    }

    showDialog(
      context: context,
      builder: (_) {
        return StatefulBuilder(
          builder: (context, setState) {
            Future<void> _pickImage(ImageSource source) async {
              final pickedFile = await ImagePicker().pickImage(source: source, imageQuality: 50);
              if (pickedFile != null) {
                setState(() {
                  _imageFile = File(pickedFile.path);
                });
              }
            }

            return AlertDialog(
              title: Text(user == null ? 'Add User' : 'Edit User'),
              content: SingleChildScrollView(
                child: Column(
                  mainAxisSize: MainAxisSize.min,
                  children: [
                    CircleAvatar(
                      radius: 50,
                      backgroundImage: _imageFile != null
                          ? FileImage(_imageFile!)
                          : (user?.imagePath != null && user!.imagePath!.isNotEmpty
                              ? FileImage(File(user.imagePath!))
                              : null) as ImageProvider?,
                      child: _imageFile == null && (user?.imagePath == null || user!.imagePath!.isEmpty)
                          ? Icon(Icons.person, size: 50)
                          : null,
                    ),
                    Row(
                      mainAxisAlignment: MainAxisAlignment.center,
                      children: [
                        IconButton(icon: Icon(Icons.camera_alt), onPressed: () => _pickImage(ImageSource.camera)),
                        IconButton(icon: Icon(Icons.photo_library), onPressed: () => _pickImage(ImageSource.gallery)),
                      ],
                    ),
                    TextField(controller: _nameController, decoration: InputDecoration(labelText: 'Name')),
                    TextField(controller: _emailController, decoration: InputDecoration(labelText: 'Email')),
                  ],
                ),
              ),
              actions: [
                TextButton(child: Text('Cancel'), onPressed: () => Navigator.of(context).pop()),
                ElevatedButton(
                  child: Text(user == null ? 'Add' : 'Update'),
                  onPressed: () async {
                    String? imagePath;
                    if (_imageFile != null) {
                      final appDir = await getApplicationDocumentsDirectory();
                      final fileName = path.basename(_imageFile!.path);
                      final savedImage = await _imageFile!.copy('${appDir.path}/$fileName');
                      imagePath = savedImage.path;
                    } else {
                      imagePath = user?.imagePath;
                    }
                    final userToSave = User(id: user?.id, name: _nameController.text, email: _emailController.text, imagePath: imagePath);
                    if (user == null) {
                      await dbHelper.createUser(userToSave);
                    } else {
                      await dbHelper.updateUser(userToSave);
                    }
                    _refreshUserList();
                    Navigator.of(context).pop();
                  },
                ),
              ],
            );
          },
        );
      },
    );
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('User Management')),
      body: FutureBuilder<List<User>>(
        future: _userList,
        builder: (context, snapshot) {
          if (!snapshot.hasData) return Center(child: CircularProgressIndicator());
          if (snapshot.data!.isEmpty) return Center(child: Text('No users found. Add one!'));
          return ListView.builder(
            itemCount: snapshot.data!.length,
            itemBuilder: (context, index) {
              final user = snapshot.data![index];
              return Card(
                margin: EdgeInsets.all(8),
                child: ListTile(
                  leading: CircleAvatar(
                    radius: 30,
                    backgroundImage: user.imagePath != null && user.imagePath!.isNotEmpty
                        ? FileImage(File(user.imagePath!))
                        : null,
                    child: user.imagePath == null || user.imagePath!.isEmpty
                        ? Icon(Icons.person)
                        : null,
                  ),
                  title: Text(user.name),
                  subtitle: Text(user.email),
                  trailing: Row(
                    mainAxisSize: MainAxisSize.min,
                    children: [
                      IconButton(icon: Icon(Icons.edit, color: Colors.blue), onPressed: () => _showFormDialog(user: user)),
                      IconButton(
                        icon: Icon(Icons.delete, color: Colors.red),
                        onPressed: () async {
                          await dbHelper.deleteUser(user.id!);
                          _refreshUserList();
                        },
                      ),
                    ],
                  ),
                ),
              );
            },
          );
        },
      ),
      floatingActionButton: FloatingActionButton(
        child: Icon(Icons.add),
        onPressed: () => _showFormDialog(),
      ),
    );
  }
}
```

இத்துடன், இந்த முழுமையான CRUD செயலி முடிந்தது. இது மாணவர்களுக்கு ஒரு பெரிய கற்றல் அனுபவமாக இருக்கும்.
