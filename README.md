# JobHub

Landing Page for the JobHub project.

## Motivation and Reason

With the increasing complexity of the job searching process, it has become extremely difficult to keep track of one's job applications. Indeed, it is not rarely that one ends up receiving a call from a company to which they don't even remember applying to. This is largely due to how distributed the recruitment process has become: a job searcher has to find out about openings on different portals (Glassdoor, Linkedin, ...) and then subsequently has to fill out an application in different companies' career sites (Facebook's, Amazon's, ...). Naturally, in such circumstances, a person can become overwhelmed and can quite easily lose track of where one has applied, especially when aggressively applying to a plethora of openings (as it is often the case for students looking for internships or for people in desperate need of a job). Our product's aim is to facilitate the job hunt process by providing an seamless platform which allows users to efficiently manage their applications. Users can, in a click of our Chrome extension, easily add the application to which they've just applied (from Google's career site, Indeed.com, Linkedin, ...) into their own personal backlog of applications, without having to, for example, manually fill out entries inside of an Excel spreadsheet. This backlog, or history of applications, can quickly be accessed from our web portal, in which users can view relevant information related to a particular application in a very expressive way (company, position, title, deadlines, ...) and/or perform grooming tasks such as, for example, update its status (applied, rejected, offer, ...). Additionally, we provide functionalities that allow users to externalize memories instead of worrying about remembering them; an example of this would be our "interview questions" feature in which users can easily associate interview questions that they actually had to a particular application. We believe that through the organizational power that our neat interface provides, users are going to be able to gain meaningful insights into the progress of their search for employment and manage it with much more confidence. Jobhub's ultimate goal is to help its users remain organized and methodical throughout their job hunt in hopes of lightening the anxiety of what is already an extremely burdensome process.

In prediction of the fact that the uniqueness of our tool would attract a large initial userbase, we also decided to add recruiters into the platform. The motivation for such an expansion of our scope is the following: given that we amass a large number of users primarily due to the functionalities described in the previous paragraph, we also want to provide recruiters with an opportunity to reach out to such a large pool of candidates, and, therefore, grow the number of visitors into our website even further. Hence, recruiters are also able to add job postings inside of our platform, and users are able to apply to these with a few clicks. This can also be seen as an attempt to tackle the problem of decentralization of the recruitment process by unifying both, applications and applications, under one shared system.

## Project Management

### Project Planification

With the requirements and vision of the product being set, the development stage could be started. 

#### Product Backlog

#### Done Checklist

#### Sprints



### Scrum Rituals

#### Sprint Planning
Our team carried out sprint planning at the beginning of each sprint and during our weekly meetings where we refined our stories (backlog grooming). The scrum master facilitated the meeting with the entire team present where we discussed the stories and associated tasks involved with each feature. After the feature was discussed with the entire team, each member defined their work and effort necessary to complete their assigned task.

#### Backlog Grooming

#### Sprint Demo

#### Retrospective

#### Daily Scrum

Daily scrums were done informally within teams.

#### Weekly Scrum

The team had weekly scrum meetings to keep every member up to date with the progress of the product. Seeing the team was rather large and consisted of multiple students that.

## The Architecture

On a high level, we opted for a microservice architecture. There are two logistical reasons for this.

First, it allowed us to utilize the diverse expertise of our team's members. Nobody was restricted to a particular technology, and everyone could use their preferred stack and programming language. This helped us speed up the development process quite significantly, since it spared some of our members the time required to learn an entire new ecosystem that they're not familar with.

The second reason is distribution of tasks. By keeping ourselves localized to smaller teams corresponding to each microservice, we avoided the overhead of everyone having to deal with the potentially annoying technicalities that are proper to large teams working on the same repository and/or codebase.

As for more technical concerns, we chose microservices because it's an architecture which significantly improves fault isolation (since failures are localized to specific modules and not to the whole application as a whole), because it gave the flexibility (for those who wanted it) to experiment with new technologies without having to worry about dependency concerns or roll back changes, and finally because reasoning about microservices it's extremely easy, meaning that the functionality of our service was very easy to understand.

The following sub-sections detail the architectures, design decisions and technology choices of the particular microservices themselves.

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

#### Endpoint Restrictions

All `/users` endpoints except for `/users/self` are restricted to moderators only. Moderators have unrestricted access to all endpoints. Only a moderator can promote another user to a moderator role.

Note: Restrictions on endpoints can be bypassed by passing the `secret` header in the request. Ask someone on authentication for the secret or see pinned message on authentication channel on Discord.

### Job Applications

### Resume Revision

### Inhouse Postings

### Web Application

The web application was built to run on any modern web browser (e.g. [Firefox](https://www.mozilla.org/en-US/firefox/new/), [Chrome](https://www.google.com/chrome/), [Safari](https://www.apple.com/ca/safari/), etc.) using the open-source [ReactJS](https://reactjs.org/) view library as the backbone for our project. We chose React because it was a lightweight UI library that we could extend with different libraries (e.g. [react-router](https://github.com/ReactTraining/react-router) for routing, [react-helmet](https://github.com/nfl/react-helmet) for document head management, [Formik](https://jaredpalmer.com/formik/) for form validation, [Material-UI](https://material-ui.com/) for pre-styled visual components, etc.)

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
