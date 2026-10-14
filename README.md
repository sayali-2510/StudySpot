StudySpot
Find your people. Make studying a little less solo.
StudySpot is a web app for students who want to find or organise small,
focused study sessions on campus. Instead of coordinating everything
through scattered messages, students can post a session, see how many
seats are available, and join or leave when their plans change.
Live website: https://studyspot-7503e.web.app
Why I built it
Finding someone to study with can be harder than it sounds. You might
want to revise a particular subject, work through a problem set, or
simply stay focused alongside other students---but it is not always
clear who is studying what, where, or when.
StudySpot is my attempt to make that first step easier: make a session
visible, let people find one that suits them, and keep the
available-seat count up to date.
What you can do
- Host a study session with details such as its title, subject,
  location, and seat limit.
- Join or leave a session as your plans change.
- Find relevant sessions using subject filters, text search, and
  an "open seats only" filter.
- See updates when session information or seat availability
  changes.
- Sign in with Google when Firebase Authentication is configured.
- Switch themes with light and dark mode.
- Use it on mobile with a responsive layout.
- Install it as a Progressive Web App (PWA) on supported devices,
  with an app shell designed to work offline.
Built with
- HTML
- CSS, including CSS custom properties for theming
- JavaScript using native ES modules
- Firebase Authentication for Google sign-in
- Cloud Firestore for shared session data and real-time updates
- Firebase Hosting
- Service Worker and Web App Manifest for PWA support
The project does not require a frontend framework or a build step.
How it works
The code separates the interface from the data layer:
- index.html provides the page structure.
- css/style.css handles layout, responsive styling, and theme
  variables.
- js/app.js manages the interface, form interactions, search,
  filters, and rendering.
- js/store.js provides the data operations used by the interface. It
  supports a browser-based demo mode and a Firebase-backed mode.
- js/config.js contains the Firebase web-app configuration.
- firestore.rules defines which database operations are allowed.
- firebase.json configures Firebase Hosting and Firestore rules
  deployment.
- sw.js and manifest.webmanifest support the installable app
  experience.
- icons/ contains the app icons.
A few implementation choices
A small, framework-free frontend. I used browser-native HTML, CSS,
and JavaScript modules to keep the project easy to run and inspect.
A separate data layer. The interface calls the store rather than
accessing Firebase directly. This also allows the app to run in demo
mode without requiring every visitor to configure a backend first.
Seat-limit protection. The Firestore implementation uses
transactions for joining and leaving sessions so concurrent attempts to
take the last seat can be handled safely. The deployed security rules
must also be tested against the real Firebase project.
Safer rendering of user input. User-provided text is rendered as
text rather than interpreted as HTML, reducing the risk of script
injection through session titles.
Try the website
Open https://studyspot-7503e.web.app in your browser.
The project also includes a demo mode that stores data in the current
browser. Demo-mode data is not shared with other browsers or devices.
Shared, cross-device behaviour depends on the Firebase-backed
configuration being set up correctly.
Run locally
You need Python 3 or another simple static web server.
1. Download or clone this repository.
2. Open a terminal in the project folder.
3. Start a local server:
   python3 -m http.server 8000
   On Windows, py -m http.server 8000 may be used if Python is
   installed.
4. Visit http://localhost:8000.
To force demo mode, add ?demo=1 to the page URL. Demo data is saved in
that browser's local storage.
Configure Firebase
To use shared data and Google sign-in with your own Firebase project:
1. Create a project in the Firebase
   Console.
2. Enable Google under Authentication sign-in providers.
3. Create a Cloud Firestore database.
4. Register a web app in Project settings and copy its web
   configuration into js/config.js.
5. Review firestore.rules carefully before deploying. Start with
   production-mode rules, not open test rules.
6. Install and sign in to the Firebase CLI if needed:
   npm install -g firebase-tools
   firebase login
7. From the project directory, deploy the existing configuration:
   firebase deploy
   This repository already includes firebase.json and
   firestore.rules. Review any CLI prompts carefully and do not
   overwrite these files without understanding the changes.
8. In Firebase Authentication settings, check the authorised domains.
9. Test Google sign-in, hosting a session, joining and leaving,
   cancelling a session as its host, and live updates using two
   separate accounts or devices.
Important: A successful Hosting deployment does not by itself prove
that Google sign-in, Firestore access, and security rules all work
correctly. Test those features with your own Firebase project before
relying on them.
Firebase web configuration is normally visible to users of a web app; it
is not a substitute for security rules. Never commit service-account
private keys, admin credentials, or other secrets.
Testing and current status
The project's main interactions were reported as tested in browser demo
mode, including:
- Sign-in flow and session interactions
- Hosting, joining, leaving, and cancelling a session
- Subject, text-search, and open-seat filters
- Empty states and updates across two browser tabs
- Dark mode and a phone-width layout
- Rendering user-entered HTML as text
These demo checks do not verify the production Firebase backend,
Google sign-in configuration, or deployed Firestore security rules.
Those need separate testing against the Firebase project.
Lighthouse results previously reported for a local desktop build were:
Performance 98, Accessibility 98, Best Practices 96, SEO 100.
Results can vary by browser, network, and environment; run Lighthouse on
the live site to reproduce them.
Ideas for future improvements
- Let session hosts edit an existing session.
- Add optional participant information while keeping user data
  private.
- Add reminders before a session starts.
- Add automated tests for Firestore rules using the Firebase Emulator
  Suite.
- Gather feedback from students and prioritise improvements based on
  actual use.
What I learned
Building StudySpot gave me a practical way to explore how a small
frontend can connect to a hosted backend, how UI state and stored data
relate, and why validation and database rules both matter. I used AI
assistance during development and am continuing to study the
implementation so I can explain, test, and improve it myself.
Feedback
If you try StudySpot, feedback on the session flow, usability, or
features that would make it more useful for students is welcome.
