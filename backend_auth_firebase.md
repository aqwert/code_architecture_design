# Firebase Auth on the Backend

This design is an opinionated view of using Firebase authentication as a service from your own backend services. The basic concept is that the client application will own the firebase user authentication part and the backend service will validate the firebase token. If it is valid the backend service will create its own token (so it does not have to revalidate every request against firebase auth) for use within the application

![Diagram](http://www.plantuml.com/plantuml/proxy?idx=0&src=https://raw.githubusercontent.com/aqwert/code_architecture_design/master/diagrams/backend_auth_firebase.puml) 
