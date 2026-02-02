# Memorial Fund Site

A single-page memorial fundraising site hosted on GitHub Pages with Firebase for the message/donation wall.

---

## Setup Instructions

### 1. Create a Firebase Project (free)

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **Add project** and give it a name (e.g., `memorial-fund`)
3. Disable Google Analytics (not needed) and click **Create project**

### 2. Get Your Firebase Config

1. In the Firebase Console, click the **gear icon** > **Project settings**
2. Scroll down to **Your apps** and click the **web icon** (`</>`)
3. Register the app with any nickname and click **Register app**
4. Copy the `firebaseConfig` object values
5. Open `index.html` and replace the placeholder values in `firebaseConfig`:
   ```js
   const firebaseConfig = {
     apiKey:            "paste-your-api-key",
     authDomain:        "your-project.firebaseapp.com",
     projectId:         "your-project-id",
     storageBucket:     "your-project.appspot.com",
     messagingSenderId: "your-sender-id",
     appId:             "your-app-id"
   };
   ```

### 3. Set Up Firestore Database

1. In the Firebase Console sidebar, click **Build** > **Firestore Database**
2. Click **Create database**
3. Choose **Start in production mode**, then pick a location close to you
4. After creation, go to the **Rules** tab and replace the rules with:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /donations/{docId} {
         allow read: if true;
         allow create: if request.resource.data.name is string
                       && request.resource.data.name.size() > 0
                       && request.resource.data.name.size() < 100;
         allow update, delete: if false;
       }
     }
   }
   ```
   Click **Publish**. This allows anyone to read and create donations but not edit or delete them.

### 4. Customize the Page

Open `index.html` and edit:

- **Hero section**: Replace `[Name]`, `[Year]` dates, and the memorial message
- **Photo**: Replace `Photo` text with `<img src="photo.jpg" alt="Photo">` and add an image file
- **Zelle info**: Replace `(XXX) XXX-XXXX` and `your@email.com` with real info
- **Goal amount**: Change `GOAL = 5000` in the script to your target
- **Goal display**: Update `$5,000` text in the HTML to match

### 5. Deploy to GitHub Pages

1. Create a new repository on GitHub (e.g., `memorial-fund`)
2. Push this folder:
   ```bash
   cd memorial-fund
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/YOUR_USERNAME/memorial-fund.git
   git push -u origin main
   ```
3. Go to the repo **Settings** > **Pages**
4. Under **Source**, select **Deploy from a branch**, pick `main` / `/ (root)`, and click **Save**
5. Your site will be live at `https://YOUR_USERNAME.github.io/memorial-fund/`
