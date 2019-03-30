# JobHub

Landing Page for the JobHub project.

## Motivation and Reason

With the increasing complexity of the job searching process, it has become extremely difficult to keep track of one's job applications. Indeed, it is not rarely that one ends up receiving a call from a company to which they don't even remember applying to. This is largely due to how distributed the recruitment process has become: a job searcher has to find out about openings on different portals (Glassdoor, Linkedin, ...) and then subsequently has to fill out an application in different companies' career sites (Facebook's, Amazon's, ...). Naturally, in such circumstances, a person can become overwhelmed and can quite easily lose track of where one has applied, especially when aggressively applying to a plethora of openings (as it is often the case for students looking for internships or for people in desperate need of a job). Our product's aim is to facilitate the job hunt process by providing an seamless platform which allows users to efficiently manage their applications. Users can, in a click of our Chrome extension, easily add the application to which they've just applied (from Google's career site, Indeed.com, Linkedin, ...) into their own personal backlog of applications, without having to, for example, manually fill out entries inside of an Excel spreadsheet. This backlog, or history of applications, can quickly be accessed from our web portal, in which users can view relevant information related to a particular application in a very expressive way (company, position, title, deadlines, ...) and/or perform grooming tasks such as, for example, update its status (applied, rejected, offer, ...). Additionally, we provide functionalities that allow users to externalize memories instead of worrying about remembering them; an example of this would be our "interview questions" feature in which users can easily associate interview questions that they actually had to a particular application. We believe that through the organizational power that our neat interface provides, users are going to be able to gain meaningful insights into the progress of their search for employment and manage it with much more confidence. Jobhub's ultimate goal is to help its users remain organized and methodical throughout their job hunt in hopes of lightening the anxiety of what is already an extremely burdensome process.

In prediction of the fact that the uniqueness of our tool would attract a large initial userbase, we also decided to add recruiters into the platform. The motivation for such an expansion of our scope is the following: given that we amass a large number of users primarily due to the functionalities described in the previous paragraph, we also want to provide recruiters with an opportunity to reach out to such a large pool of candidates, and, therefore, grow the number of visitors into our website even further. Hence, recruiters are also able to add job postings inside of our platform, and users are able to apply to these with a few clicks. This can also be seen as an attempt to tackle the problem of decentralization of the recruitment process by unifying both, applications and applications, under one shared system.

## Project Management

## The Architecture

[TODO: Talk about overall architecture, i.e. microservices]

### Authentication

#### Description

👮 Provides authentication/user management for all jobhub microservices. Uses JWT for authentication.

Each user has the following required attributes:

- `id`: A unique ID generated for each user.
- `email`: An email address used for login.
- `password`: The users password. All passwords are hashed using bcrypt.
- `type`: The type of user. Can be Applicant or Recruiter.
- `verified`: Whether the user has verfied their email after creating their account. Required to be able to login.

#### Getting Started

```bash
git clone https://github.com/scrum-gang/authentication.git
cd authentication
npm install
npm start
```

#### Deployment

Builds are automated using Travis and deployed on Heroku.

There are two Heroku deployments:

- Staging: <https://jobhub-authentication-staging.herokuapp.com/>
- Production: <https://jobhub-authentication.herokuapp.com/>

The staging deployment should be used for all development/testing purposes, in order to keep production from being poluted with test data.

Please note that any new builds on the **development** branch will **wipe** the staging database.

#### Typical usage

1. Create user using `/signup`.
2. Verify new user by clicking link in email received.
3. Login using `/login`, keep JWT token.
4. Can get logged in user using `/users/self` and passing token in header.

#### User Schema

Details all the fields in the User model.

| Field           | Type    | Required | Allowed Values                  | Specified By |
| --------------- | ------- | -------- | ------------------------------- | ------------ |
| `email`         | String  | true     | n/a                             | user         |
| `password`      | String  | true     | n/a                             | user         |
| `type`          | String  | true     | Applicant, Recruiter, Moderator | user         |
| `name`          | String  | false    | n/a                             | user         |
| `address`       | String  | false    | n/a                             | user         |
| `github`        | String  | false    | n/a                             | user         |
| `linkedin`      | String  | false    | n/a                             | user         |
| `stackoverflow` | String  | false    | n/a                             | user         |
| `id`            | String  | n/a      | n/a                             | system       |
| `verified`      | Boolean | n/a      | n/a                             | system       |
| `created_at`    | Date    | n/a      | n/a                             | system       |
| `updated_at`    | Date    | n/a      | n/a                             | system       |

#### API Docs

1\. Get users:

Return list of all users.

**URL :** `/users`

**Method :** `GET`

```json
"Content-type": "application/json"
"Authorization": "Bearer [valid Moderator token]"
```

\***\*Success Reponse\*\***

**Code :** `200 OK`
**Body :** User objects

\***\*Error Response\*\***

**Condition :** JWT is invalid.
**Code :** `401`

2\. Get user by id:

Return user with corresponding ID.

**URL :** `/users/:id`

**Method :** `GET`

```json
"Content-type": "application/json"
"Authorization": "Bearer [valid Moderator token]"
```

\***\*Success Reponse\*\***

**Code :** `200 OK`
**Body :** User object

\***\*Error Response\*\***

**Condition :** JWT is invalid.
**Code :** `401`

3\. Update user by id:

Updates a user with the corresponding ID.

**URL :** `/users/:id`

**Method :** `PUT`

**Header :**

```json
"Content-Type": "application/json"
"Authorization": "Bearer [valid Moderator token]"
```

**Body :**

```json
{
  "email": "[new email]",
  "password": "[new password]",
  "type": "[new type]"
}
```

\***\*Success Reponse\*\***

**Code :** `200 OK`

\***\*Error Response\*\***

**Condition :** JWT is invalid.
**Code :** `401`

4\. Delete user by id:

Deletes a user with the corresponding ID.

**URL :** `/users/:id`

**Method :** `DELETE`

**Header :**

```json
"Content-type": "application/json"
"Authorization": "Bearer [valid Moderator token]"
```

\***\*Success Reponse\*\***

**Code :** `204 No Content`

\***\*Error Response\*\***

**Condition :** Wrong token.

**Code :** `401 Unauthorized`

5\. Signup new user:

Creates new user with given email, password and type.

**URL :** `/signup`

**Method :** `POST`

**Header :**

```json
"Content-Type": "application/json"
```

**Body :**

```json
{
  "email": "[valid email]",
  "password": "[valid password]",
  "type": "[Applicant || Recruiter]"
}
```

\***\*Success Reponse\*\***

**Code :** `201 Created`

6\. Login existing user:

Returns session token for existing User on succesful login.

**URL :** `/login`

**Method :** `POST`

**Header :**

```json
"Content-type": "application/json"
```

**Body :**

```json
{
  "email": "[valid email]",
  "password": "[valid password]"
}
```

\***\*Success Reponse\*\***

**Code :** `200 OK`

**Body :**

```json
{
  "user": {
    "_id": "[user ID]",
    "email": "[email]",
    "password": "[hashed password]",
    "type": "[user type]",
    "verified": true,
    "created_at": "[account creation date]",
    "updated_at": "[last account update]",
    "__v": 0
  },
  "iat": "[Token issued at]",
  "exp": "[Token expiry]",
  "token": "[JWT token]"
}
```

\***\*Error Response\*\***

**Condition :** If `username` or `password` is wrong.

**Code :** `401 Unauthorized`

**Body :**

```json
{
  "code": "Unauthorized",
  "message": "Authentication failed."
}
```

**Condition :** If user is unverified.

**Code :** `401 Unauthorized`

**Body :**

```json
{
  "code": "Unauthorized",
  "message": "Unverified user."
}
```

7\. Logout user:

Logs out user given valid JWT token.

**URL :** `/logout`

**Method :** `POST`

**Header :**

```json
"Content-type": "application/json"
"Authorization": "Bearer [token]"
```

\***\*Success Reponse\*\***

**Code :** `200 OK`

\***\*Error Response\*\***

**Condition :** If token is invalid.
**Code :** `401 Unauthorized`

8\. Resend verification email for user:

Resends the verfication email for an unverified user.

**URL :** `/resend`

**Method :** `POST`

**Header :**

```json
"Content-type": "application/json"
```

**Body :**

```json
{
  "email": "[valid email]"
}
```

\***\*Success Reponse\*\***

**Code :** `200 OK`

\***\*Error Response\*\***

**Condition :** If user is already verified.

**Code :** `400 Bad Request`

**Body :**

```json
{
  "code": "BadRequest",
  "message": "User is already verified."
}
```

**Condition :** If user is does not exist.

**Code :** `400 Bad Request`

**Body :**

```json
{
  "code": "BadRequest",
  "message": "No user with given email"
}
```

9\. Get user from token

Returns corresponding user given JWT token.

**URL :** `/users/self`

**Method :** `GET`

**Header :**

```json
"Content-type": "application/json"
"Authorization": "Bearer [token]"
```

\***\*Success Reponse\*\***

**Code :** `200 OK`

**Body :**

```json
{
  "_id": "[User ID]",
  "email": "[User email]",
  "password": "[User password]",
  "type": "[User type]",
  "verified": true,
  "created_at": "[account creation date]",
  "updated_at": "[last account update]",
  "__v": 0
}
```

\***\*Error Response\*\***

**Condition :** If token is wrong.
**Code :** `401 Unauthorized`

10\. Update user by token:

Updates a user with the corresponding ID.

**URL :** `/users/self`

**Method :** `PUT`

**Header :**

```json
"Content-Type": "application/json"
"Authorization": "Bearer [valid token]"
```

**Body :**

```json
{
  "email": "[new email]",
  "password": "[new password]",
  "type": "[new type]"
}
```

\***\*Success Reponse\*\***

**Code :** `200 OK`

\***\*Error Response\*\***

**Condition :** If user does not exist.

**Code :** `404 Not Found`

**Condition :** Trying to change `verified` field.

**Code :** `401 Unauthorized`

11\. Delete user by token:

Deletes a user with the corresponding token.

**URL :** `/users/self`

**Method :** `DELETE`

**Header :**

```json
"Content-type": "application/json"
"Authorization": "Bearer [valid token]"
```

\***\*Success Reponse\*\***

**Code :** `204 No Content`

\***\*Error Response\*\***

**Condition :** Wrong token.

**Code :** `401 Unauthorized`

#### Endpoint Restrictions

All `/users` endpoints except for `/users/self` are restricted to moderators only. Moderators have unrestricted access to all endpoints. Only a moderator can promote another user to a moderator role.

Note: Restrictions on endpoints can be bypassed by passing the `secret` header in the request. Ask someone on authentication for the secret or see pinned message on authentication channel on Discord.

### Job Applications

### Resume Revision

### Inhouse Postings

### Web Application

The web application was built to run on any modern web browser (e.g. [Firefox](https://www.mozilla.org/en-US/firefox/new/), [Chrome](https://www.google.com/chrome/), [Safari](https://www.apple.com/ca/safari/), etc.) using the open-source [ReactJS](https://reactjs.org/) view library as the backbone for our project. We chose React because it was a lightweight UI library that we could extend with different libraries (e.g. [react-router](https://github.com/ReactTraining/react-router) for routing, [react-helmet](https://github.com/nfl/react-helmet) for document head management, [Formik] for form vali[Material-UI](https://material-ui.com/) for pre-styled visual components, etc.)

We bootstrapped the project using [create-react-app](https://facebook.github.io/create-react-app/), a zero-configuration starter project for ReactJS that equips developers with a toolchain allowing for newer JavaScript features (see the ECMAScript standard [here](https://en.wikipedia.org/wiki/ECMAScript)) that are not supported across all browser versions. Moreover, we decided to use [TypeScript](https://www.typescriptlang.org/) as our development language rather than plain JavaScript. TypeScript is a superset of JavaScript that adds static typing, leading to less error-prone and more scalable code. We have set up create-react-app to transpile the TypeScript down to JavaScript.

Aside from React, we also use the [axios](https://github.com/axios/axios) HTTP client to interface with all of our microservices. We wrapped each microservice into its own instance of axios and created an API layer that could be queried across the entire application.

To save persistent state, we used a mix of the browser's [localStorage API](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) to have persistence across sessions, and ReactJS' [Context API](https://reactjs.org/docs/context.html) to pass state across different components from a single source of truth. This was mostly used to keep the authentication state useable throughout different scenarios (closing your tab, navigating through different pages, etc.)



### Chrome Extension

:pushpin: Keep track of job applications ad-hoc

[![Build Status](https://travis-ci.org/scrum-gang/jobhub-chrome.svg?branch=master)](https://travis-ci.org/scrum-gang/jobhub-chrome)

<!--- [![Coverage Status](https://coveralls.io/repos/github/scrum-gang/jobhub-chrome/badge.svg)](https://coveralls.io/github/scrum-gang/jobhub-chrome)--->

| Login Popup                                     | Job Posting Form                                     |
| ----------------------------------------------- | ---------------------------------------------------- |
| ![Login Popup](https://i.imgur.com/GXQQewr.png) | ![Job Posting Form](https://i.imgur.com/rQyb5oD.png) |

#### How to setup the project

Please check our [contributors guide](.github/CONTRIBUTING.md).

#### Why use the JobHub Extension

The Chrome extension is meant to be an ad-hoc access to the JobHub services. When the users are on a job application website, they don't need to stop browsing the post in order to open the JobHub website on a separate tab. Instead, they can start tracking the given application via the popup submit directly on the job posting website.

#### Folder Structure

```bash
.
├── .github # contains Code of Conduct & templates for PR/Issues
├── build   # generated by Webpack. Contains the final js/html/css/resources
├── data    # list of companies to scrape
├── lib     # generated by Bucklescript. ReasonML -> Javascript
├── src     # ReasonReact components & global stylesheet
├── .editorconfig       # linter
├── .travis.yml         # CI
├── bsconfig.json       # Bucklescript config
├── jsconfig.json       # VSCode intellisense for Chrome API
├── manifest.json       # Chrome Extension config
├── package.json        # Node config
├── popup.html          # html of the extension
└── webpack.config.js   # Webpack config
```

#### Architecture

```bash
           popup.html
               |
             App.re
        {token, userID}
               |
   +-----------+-----------+
   |                       |
Login.re               JobApp.re
                           |
                   ScrapingInputs.re
```

`token` is the state variable which is initially obtained by `Login` during the authentication process and in subsequent execustions of the extension its validity is confirmed by `App` directly. `userID` is also obtained during the initial login process. In case of a deprecated `token`, both the `userID` and the `token` are cleared from the browser's memory. Both variables are passed onto the `JobApp` component to be able to fetch CVs and to submit job applications.

The system leverages the following modules

- `ScrapingFunctions.re`: functions related to extracting/processing HTML elements
- `Services.re`: functions related to asynchronous actions external to the extension (load files/API calls)
- `SyncStorage.re`: functions related to the Chrome storage management
- `Uilities.re`: general purpose helper functions

#### Limitations

- Currently, the only supported `posted date` scraper pattern is `"<num> days ago"`
- Currently, the extension does not have a `deadline date` scraper
- Because of the [bucklescript-chrome](https://github.com/jchavarri/bucklescript-chrome.git#start-extensions) bindings used to build the extension,
  we are locked with outdated versions of core libraries such as `React` and `Bucklescript`
- For the same reason as above, we can't use Chrome's [Content Scripts](https://developer.chrome.com/extensions/content_scripts).
  Instead we inject a script into the active website to extract DOM information.
