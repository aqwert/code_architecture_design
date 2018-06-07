# Firebase Auth on the Backend

This design is an opinionated view of using Firebase authentication as a service from your own backend services. The basic concept is that the client application will own the firebase user authentication part and the backend service will validate the firebase token. If it is valid the backend service will create its own token (so it does not have to revalidate every request against firebase auth) for use within the application

Some points to note:

* The Google Cloud Function is a convientnet way to access the Firebase admin functoinality without needing to use access keys within your own backend. Since the function is running within google it has full rights to call Firebase Auth admin `verifyIdToken`
* It is up to the App (client) to generate the refres token if it gets an unauthorizde error code. It may not know that it is unauthorised because of an expired token, but it does give it an opportunity to refresh and retry without having to re-prompt the user for credentials. This means that their experience is a continual access to the backend service without having to re-login all the time
* This method is deliberately avoids using [Firebase's Custom token ](https://firebase.google.com/docs/auth/admin/create-custom-tokens) since that requires the backend service (the one you write) needing to handle authentication. By using Firebase Auth directly from the client, all (or partiall) the features that come with Firebase on the client does not need to be created on the backend. This decouples the backend service from Firebase more cleanly, where the only knowledge is built in the middleware of the service to validate the firebase token when the backend token expires.
* The firebase auth token expired every 60 mins
* The backend auth token can be have any timeout but is recommended to be short. Anywhere under that 60 mins mark would be good since it ensures that it needs to re-verify against firebase auth at least every hour. Having a shorter backend token (say 5 or 10 mins) can allow changing of claims that may exist on that token more easily, even with in the same session. This would still require client side app smarts to react to those changes. 
    * i.e. Your backend may but claims within the backend token for what the user can do. If that changes then the user will need to get a re-issued claim from the backend without having them to re-sign in. This can achieved since the firebase token is used as a "fallback" to verify the user. 

![Diagram](http://www.plantuml.com/plantuml/proxy?idx=0&src=https://raw.githubusercontent.com/aqwert/code_architecture_design/master/diagrams/backend_auth_firebase.puml) 
