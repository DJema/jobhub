# JobHub
Landing Page for the JobHub project.

## The Problem

## The Solution

## The Architecture

### Authentication

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/f193348b2004466ba69f53bec6f9de9a)](https://www.codacy.com/app/alexH2456/authentication?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=scrum-gang/authentication&amp;utm_campaign=Badge_Grade)
[![Build Status](https://travis-ci.com/scrum-gang/authentication.svg?branch=master)](https://travis-ci.com/scrum-gang/authentication)
[![Coverage Status](https://coveralls.io/repos/github/scrum-gang/authentication/badge.svg?branch=master)](https://coveralls.io/github/scrum-gang/authentication?branch=master)

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
|-----------------|---------|----------|---------------------------------|--------------|
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

__**Success Reponse**__

**Code :** `200 OK`
**Body :** User objects

__**Error Response**__

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

__**Success Reponse**__

**Code :** `200 OK`
**Body :** User object

__**Error Response**__

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

__**Success Reponse**__

**Code :** `200 OK`

__**Error Response**__

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

__**Success Reponse**__

**Code :** `204 No Content`

__**Error Response**__

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

__**Success Reponse**__

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

__**Success Reponse**__

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

__**Error Response**__

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

__**Success Reponse**__

**Code :** `200 OK`

__**Error Response**__

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
    "email": "[valid email]",
}
```

__**Success Reponse**__

**Code :** `200 OK`

__**Error Response**__

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

__**Success Reponse**__

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

__**Error Response**__

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

__**Success Reponse**__

**Code :** `200 OK`

__**Error Response**__

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

__**Success Reponse**__

**Code :** `204 No Content`

__**Error Response**__

**Condition :** Wrong token.

**Code :** `401 Unauthorized`

#### Endpoint Restrictions

All `/users` endpoints except for `/users/self` are restricted to moderators only. Moderators have unrestricted access to all endpoints. Only a moderator can promote another user to a moderator role.

Note: Restrictions on endpoints can be bypassed by passing the `secret` header in the request. Ask someone on authentication for the secret or see pinned message on authentication channel on Discord.

### Job Applications

### Resume Revision

### Inhouse Postings

### Web Application

### Chrome Extension

:pushpin: Keep track of job applications ad-hoc

[![Build Status](https://travis-ci.org/scrum-gang/jobhub-chrome.svg?branch=master)](https://travis-ci.org/scrum-gang/jobhub-chrome)
<!--- [![Coverage Status](https://coveralls.io/repos/github/scrum-gang/jobhub-chrome/badge.svg)](https://coveralls.io/github/scrum-gang/jobhub-chrome)--->

| Login Popup  | Job Posting Form |
| ------------- | ------------- |
| ![Login Popup](https://i.imgur.com/GXQQewr.png)  | ![Job Posting Form](https://i.imgur.com/rQyb5oD.png)  |


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

`token` is the state variable which is initially obtained by `Login` during the authentication process and in subsequent  execustions of the extension its validity is confirmed by `App` directly. `userID` is also obtained during the initial login process. In case of a deprecated `token`, both the `userID` and the `token` are cleared from the browser's memory. Both variables are passed onto the `JobApp` component to be able to fetch CVs and to submit job applications.

The system leverages the following modules
- `ScrapingFunctions.re`: functions related to extracting/processing HTML elements
- `Services.re`: functions related to asynchronous actions external to the extension (load files/API calls)
- `SyncStorage.re`: functions related to the Chrome storage management
- `Uilities.re`: general purpose helper functions


#### Limitations
* Currently, the only supported `posted date` scraper pattern is `"<num> days ago"`
* Currently, the extension does not have a `deadline date` scraper
* Because of the [bucklescript-chrome](https://github.com/jchavarri/bucklescript-chrome.git#start-extensions) bindings used to build the extension,
we are locked with outdated versions of core libraries such as `React` and `Bucklescript`
* For the same reason as above, we can't use Chrome's [Content Scripts](https://developer.chrome.com/extensions/content_scripts).
    Instead we inject a script into the active website to extract DOM information.
