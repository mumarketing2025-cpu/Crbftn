# Firebase Setup Instructions for CRBFTN Website

## 🔥 Complete Firebase & Firestore Integration Setup

### Step 1: Create Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Create a project" or select existing project
3. Enter project name: `crbftn-website` (or your preferred name)
4. Enable Google Analytics (optional)
5. Click "Create project"

### Step 2: Enable Firestore Database
1. In your Firebase project, go to "Firestore Database" in the left sidebar
2. Click "Create database"
3. Choose "Start in test mode" (we'll secure it later)
4. Select your preferred location (closest to your users)
5. Click "Done"

### Step 3: Get Firebase Configuration
1. Go to "Project Settings" (gear icon in sidebar)
2. Scroll down to "Your apps" section
3. Click "Web" icon (</>) to add a web app
4. Register app name: `CRBFTN Website`
5. Copy the configuration object that looks like this:

```javascript
const firebaseConfig = {
    apiKey: "AIzaSyAbc123...",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "1:123456789:web:abc123"
};
```

### Step 4: Add Firebase Environment Variables to Netlify
Since you're using Netlify with environment variables (which is the secure approach), follow these steps:

1. **Get your Firebase configuration** from Step 3 above
2. **Go to your Netlify dashboard**
3. **Navigate to:** Site Settings → Environment Variables
4. **Add these environment variables:**

```
FIREBASE_API_KEY = your-actual-api-key
FIREBASE_AUTH_DOMAIN = your-project.firebaseapp.com  
FIREBASE_PROJECT_ID = your-project-id
FIREBASE_STORAGE_BUCKET = your-project.appspot.com
FIREBASE_MESSAGING_SENDER_ID = your-sender-id
FIREBASE_APP_ID = your-app-id
```

5. **Deploy your site** - Netlify will automatically inject these values during build

### Step 5: Verify Environment Variables
The website is already configured to use environment variables. During the Netlify build process:
- The `inject-firebase-config.sh` script runs automatically
- It replaces placeholders with your actual Firebase config
- Firebase is initialized securely without exposing credentials

### Step 5: Set Up Firestore Security Rules
1. In Firebase Console, go to "Firestore Database"
2. Click on "Rules" tab
3. Replace the default rules with these secure rules:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow anyone to create quotes (for website submissions)
    match /quotes/{document} {
      allow create: if true;
      allow read, update, delete: if request.auth != null && 
        request.auth.token.admin == true;
    }
    
    // Allow anyone to add emails to mailing list
    match /mailing-list/{document} {
      allow create: if true;
      allow read, update, delete: if request.auth != null && 
        request.auth.token.admin == true;
    }
    
    // Deny all other access
    match /{document=**} {
      allow read, write: if false;
    }
  }
}
```

4. Click "Publish" to save the rules

### Step 6: Test the Integration
1. Open your website in a browser
2. Add some items to cart
3. Click "Request Quote"
4. Fill in the email modal form
5. Submit the quote
6. Check your Firebase Console → Firestore Database

You should see two collections created:
- `quotes` - Contains quote requests with customer info and cart items
- `mailing-list` - Contains customer emails for marketing

### Step 7: View Your Data
To view collected data:
1. Go to Firebase Console → Firestore Database
2. Click on "Data" tab
3. You'll see collections: `quotes` and `mailing-list`
4. Click on any document to view the full data

### Data Structure

**Quotes Collection:**
```javascript
{
  customerEmail: "customer@example.com",
  customerName: "John Doe",
  customerMessage: "Interested in bulk pricing",
  items: [
    {
      name: "CRBFTN Premium Hoodie",
      size: "M",
      quantity: 2,
      price: 899,
      total: 1798
    }
  ],
  totalAmount: 1798,
  itemsCount: 2,
  timestamp: "2025-11-03T10:30:00Z",
  status: "pending",
  quoteId: "CRBFTN-1699012345678"
}
```

**Mailing List Collection:**
```javascript
{
  email: "customer@example.com",
  name: "John Doe",
  source: "quote-request",
  subscribed: true,
  timestamp: "2025-11-03T10:30:00Z",
  status: "active"
}
```

### Troubleshooting

**If data isn't saving:**
1. Check browser console for errors
2. Verify Firebase environment variables are set in Netlify
3. Ensure Firestore rules are published
4. Check that your domain is authorized in Firebase settings
5. Verify the build process completed successfully in Netlify

**If you see "Firebase config not available" in console:**
1. Check that all environment variables are set in Netlify dashboard
2. Redeploy your site to trigger the build process
3. Verify the `inject-firebase-config.sh` script ran during build

**Environment Variables Setup:**
- Go to Netlify Dashboard → Site Settings → Environment Variables
- Add all 6 Firebase environment variables
- Deploy the site to apply changes

### Next Steps

1. **Set up Firebase Authentication** for admin access to view/manage data
2. **Create admin dashboard** to view quotes and mailing list
3. **Set up email notifications** when new quotes are received
4. **Export data** to CSV for external processing
5. **Set up automated backups** of your Firestore data

### 🎉 Success!

Your website now automatically saves:
- ✅ Customer emails to a mailing list
- ✅ Complete quote requests with cart items
- ✅ Customer information and messages
- ✅ Timestamps and tracking data

All data is securely stored in Firebase Firestore and accessible through the Firebase Console!