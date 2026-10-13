# Smart Digital Marketing Solution: Firebase setup

1. Go to console.firebase.google.com and create a project.
2. Build > Authentication > Get started > turn on **Email/Password**.
3. Build > Firestore Database > Create database (production mode).
4. Firestore > Rules, paste this and publish (anyone can send an enquiry, nobody can read them in the browser):

   rules_version = '2';
   service cloud.firestore {
     match /databases/{db}/documents {
       match /enquiries/{id} {
         allow create: if request.resource.data.keys().hasOnly(['name','email','service','message','createdAt'])
                       && request.resource.data.message.size() < 2000;
         allow read, update, delete: if false;
       }
     }
   }

5. Project settings > Your apps > add a Web app (</>) and copy the config into `firebaseConfig` near the top of the script in index.html.
6. Authentication > Settings > Authorized domains: add your website domain.
7. Deploy: `npm i -g firebase-tools`, then `firebase login`, `firebase init hosting` (public folder: the one holding index.html), `firebase deploy`.
8. View enquiries in the Firebase console under Firestore > enquiries.

## Send enquiries to your email (Web3Forms, free plan)

1. Go to web3forms.com and enter firaolbire2424@gmail.com, then click Create Access Key.
2. Open your Gmail inbox (check Spam too). Web3Forms emails you the access key. Copy it.
3. In index.html find `const WEB3FORMS_KEY="PASTE_WEB3FORMS_KEY";` and replace the text with your key, keeping the quotes.
4. Run `firebase deploy` again.
5. Open your live site, send a test message through the contact form, and check your inbox (and Spam the first time).
6. In Gmail, mark the first message "Not spam" so later ones arrive in your inbox.
Replying to the email goes straight to the client, because their address is set as the reply-to.
