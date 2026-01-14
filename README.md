Task Management App with Firebase
A Flutter task management application built with Firebase Authentication, Cloud Firestore, and Firebase Storage.
The core focus of this project is real-time database operations, secure user-based data isolation, and scalable Firebase architecture.

🚀 Project Overview

This app allows users to:
Sign up and log in using Firebase Authentication
Create tasks with title, description, date, color, and optional image
Store tasks securely in Cloud Firestore
Upload task images to Firebase Storage
Fetch and display tasks in real time per authenticated user
Delete tasks with swipe gestures
Maintain clean separation of user data using Firebase UID
This project is designed to demonstrate practical Firebase usage, not just UI.
```
🔥 Firebase Architecture (Main Focus)
1. Firebase Authentication
Authentication is handled using Email and Password.
User signup using createUserWithEmailAndPassword
User login using signInWithEmailAndPassword
App state controlled via authStateChanges()
Automatic redirect to Home or Auth screen based on login state


FirebaseAuth.instance.authStateChanges()
Each authenticated user gets a unique UID which is used across Firestore and Storage.

2. Cloud Firestore Database Structure
Firestore is used as the primary database for task management.
Collection Structure

tasks (collection)
 └── taskId (document)
      ├── title
      ├── description
      ├── date
      ├── creator (Firebase UID)
      ├── postedAt
      ├── color
      ├── imageURL
      
Key Design Decisions
Each task has a unique document ID generated using UUID
The creator field ensures tasks belong to a specific user
Real-time updates using Firestore snapshots
Secure and scalable data access pattern
Task Creation Example

await FirebaseFirestore.instance.collection("tasks").doc(id).set({
  "title": "...",
  "description": "...",
  "creator": FirebaseAuth.instance.currentUser!.uid,
});
3. Real-Time Data Fetching
Tasks are fetched in real time using Firestore streams.


FirebaseFirestore.instance
  .collection("tasks")
  .where("creator", isEqualTo: FirebaseAuth.instance.currentUser!.uid)
  .snapshots();
This ensures:
Live updates without refresh
Each user only sees their own tasks
Scalable querying model

4. Firebase Storage Integration
Firebase Storage is used to upload task images.
Flow
Image selected from gallery
Image uploaded using unique task ID
Download URL saved in Firestore
Image rendered using NetworkImage


final ref = FirebaseStorage.instance.ref('images').child(taskId);
await ref.putFile(file);
final imageURL = await ref.getDownloadURL();
Firestore stores only the image URL, not the file itself.

5. Firebase Initialization
Firebase is initialized using FlutterFire CLI and platform-specific configuration.

await Firebase.initializeApp(
  options: DefaultFirebaseOptions.currentPlatform,
);
Supported platforms:
Android
Web
iOS
macOS

🧠 App Flow Logic
App starts
Firebase initializes
Auth state checked
User redirected:
Logged in → Home Page
Not logged in → Sign Up Page
Tasks fetched from Firestore in real time
User can add or delete tasks
This flow ensures no unauthenticated database access.

🛠 Tech Stack
Flutter
Firebase Authentication
Cloud Firestore
Firebase Storage
FlutterFire CLI
UUID
Intl
Flex Color Picker

📂 Project Structure (Simplified)

lib/
 ├── main.dart
 ├── firebase_options.dart
 ├── signup_page.dart
 ├── login_page.dart
 ├── home_page.dart
 ├── add_new_task.dart
 ├── utils.dart
 ├── widgets/
 │    ├── task_card.dart
 │    └── date_selector.dart
 
⚠️ Important Notes
Firebase must be initialized before any Auth or Firestore call
Task IDs are mandatory for future updates and deletes
Each Firestore query is scoped by Firebase UID
Image upload requires Firebase Storage rules to be configured

🔐 Firebase Security Rules (Recommended)
Firestore:
allow read, write: if request.auth != null
                  && request.auth.uid == resource.data.creator;
Storage:
allow read, write: if request.auth != null;
Do not skip this in production.

🎯 Why This Project Matters
This project is not a demo toy.
It demonstrates real Firebase engineering principles:
Auth-driven data ownership
Real-time database streams
Clean Firestore document modeling
Scalable Firebase Storage usage
Proper initialization and lifecycle handling
If you understand this project, you understand Firebase fundamentals.

📌 Next Improvements
Firestore pagination
Task update feature
Push notifications
Role-based access
Offline persistence
Firestore indexes optimization

👨‍💻 Author
Muhammad Junaid
