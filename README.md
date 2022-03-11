# Add Build Info to React App by creating src/buildInfo.ts with npm version and date

https://github.com/coding-to-music/add-build-info-to-react-app

By John M. Wargo https://johnwargo.com/

From this

https://github.com/johnwargo/react-build-info

https://www.npmjs.com/package/react-build-info

https://johnwargo.com/web-development/reactjs-displaying-build-information.html

# React Build Info

While building a desktop web application using React, I realized that for support purposes I wanted the ability to display the app's build number (or even build date) in the application somewhere, and this module enables that.

## Installation

To install the module in your project, open a terminal window, navigate to a React project, and execute the following command:

```shell
npm install --save-dev react-build-info

# or

npm i react-build-info
```

## Operation

When you execute the module, it reads the local React project's `package.json` file, and, using the file's `version` property and the current date/time, creates a new file in the project at `src/buildInfo.ts` containing the following information:

```JavaScript
export const buildInfo = {
  buildVersion: "0.0.7",
  buildDate: 1611751534805,
}
```

To use the module in a React App, do something like the following:

```javascript
import React from "react";

import buildInfo from "./buildInfo";
import "./App.css";

const buildDate = new Date(buildInfo.buildDate);

class App extends React.Component {
  componentDidMount() {
    console.log(`Build Number: ${buildInfo.buildVersion}`);
    console.log(`Build Date: ${buildDate.toString()}`);
  }

  render() {
    return (
      <div>
        <header>
          <h1>Build Info</h1>
        </header>
        <p>
          Bacon ipsum dolor amet chuck pork chop drumstick pork, rump bresaola
          swine shankle landjaeger tri-tip filet mignon ham tenderloin. Ham hock
          strip steak cow jerky pig biltong, drumstick salami beef ribs pastrami
          fatback spare ribs flank tail. Venison cupim alcatra tongue, drumstick
          sirloin beef salami cow pork loin brisket jowl. Bresaola flank pork
          chop ham chislic. Shoulder pastrami sausage, frankfurter meatloaf
          corned beef pig chicken. Shank chislic spare ribs, turducken fatback
          swine short ribs ball tip shankle brisket meatball shoulder
          frankfurter kevin pork chop.
        </p>

        <footer>By John M. Wargo</footer>
      </div>
    );
  }
}

export default App;
```

## Usage

Open the project's `package.json` file and add this process to the existing `build` script entry, changing:

```text
"build": "react-scripts build",
```

to:

```text
"build": "npm version patch && react-build-info && react-scripts build",
```

The `npm version patch` part of the build step increments the patch version in the `package.json` file before calling `react-build-info`.

With this in place, when you execute `npm run build` to build a production version of the app, `npm` will update the version number in the project's `package.json` file, build an updated version of the buildinfo.js file, then generate the production build of the app.

---

You can find information on many different topics on my [personal blog](http://www.johnwargo.com). Learn about all of my publications at [John Wargo Books](http://www.johnwargobooks.com).

If you find this code useful and feel like thanking me for providing it, please consider <a href="https://www.buymeacoffee.com/johnwargo" target="_blank">Buying Me a Coffee</a>, or making a purchase from [my Amazon Wish List](https://amzn.com/w/1WI6AAUKPT5P9).

https://johnwargo.com/web-development/reactjs-displaying-build-information.html

# REACTJS: DISPLAYING BUILD INFORMATION

## Introduction

In an earlier article, I showed how to access build information in an Ionic application. When I created the solution for Ionic, I created a similar solution for ReactJS apps; I describe that solution in this article.

## Manipulating the Project Configuration File

ReactJS projects use node-based tooling like most web-based frameworks, so they already have an easy to update place to maintain the application’s version number - in the project’s package.json file. Here’s a stripped-down version of the package.json file from a new project I just created:

```java
{
 "name": "react-app",
 "version": "0.0.1",
 "private": true,
 "dependencies": {
 "@testing-library/jest-dom": "^5.11.4",
 "@testing-library/react": "^11.1.0",
 "@testing-library/user-event": "^12.1.10",
 "react": "^17.0.2",
 "react-dom": "^17.0.2",
 "react-scripts": "4.0.3",
 "web-vitals": "^1.0.1"
 },
 "scripts": {
   "start": "react-scripts start",
   "build": "react-scripts build",
   "test": "react-scripts test",
   "eject": "react-scripts eject"
 },
 "eslintConfig": {
   "extends": [
     "react-app",
     "react-app/jest"
   ]
 },
 "browserslist": {
   "production": [
     ">0.2%",
     "not dead",
     "not op_mini all"
   ],
 "development": [
   "last 1 chrome version",
   "last 1 firefox version",
   "last 1 safari version"
   ]
 }
}
```

Notice the `version` property, setup and ready for use.

Now, what I needed next was a simple and automated way to update that value every time I built a new version of the app. Fortunately, the Node Package Manager (npm) already has a feature that increments the value for you:

```java
npm version patch
```

When you execute that command from the Ionic project folder, it automatically increments the patch version (the final value in the version string):

```java
"version": "0.0.2",
```

How cool is that?

Available options are major, minor, and patch. Updating the major version increments the first value in the package.json file’s version property. Updating the minor version increments the second value in the package.json file’s version property. You’ve already seen what patch does. Here’s links to the docs: https://docs.npmjs.com/cli/v7/commands/npm-version.https://docs.npmjs.com/cli/v7/commands/npm-version.

The React project configuration file already has an automated build process:

```java
"build": "react-scripts build",
```

You can easily modify it to automatically update the version number at build time using this instead:

```java
"build": "react-build-info && react-scripts build",
```

## Generating Build Information

Unfortunately, as far as I can tell, a React project doesn’t have access to the project’s package.json file at runtime, so I needed a way to access the version number in my application.

Since React projects already use node and npm, I decided to create a simple node module that takes the version number from the package.json file and copies it into a file in the React project; the module is called react-build-info (https://www.npmjs.com/package/react-build-infohttps://www.npmjs.com/package/react-build-info).

You have to install it globally since you can’t easily execute the module locally (that I could figure out anyway), but when you execute it, it creates a file called buildinfo.js in the project’s src folder and copies over the version number from the package.json file and adds the date/time stamp for the build event. Here’s the file:

```json
    module.exports = {
    buildVersion: "0.0.1",
    buildDate: 1615323488524,
}
```

To execute the module every time you do a build, just update the build entry in the project’s package.json file:

```java
"build": "react-build-info && react-scripts build",
```

## Displaying Build Information In the App

Now that I had the build information generated and available in the app, it's time to use it in the app. What I wanted to do was display the app build information (version number and build date) on the app’s login page, but I decided instead to simply display it in the console at startup.

To do this, I opened the project’s src/App.js file and added the following import statement to the top of the file:

```java
import buildInfo from './buildInfo';
```

This loads the contents of the file into a buildInfo variable I can use in my app like this:

```java
console.log(`Build Number: ${buildInfo.buildVersion}`);
const buildDate = new Date(buildInfo.buildDate);
console.log(`Build Date: ${buildDate.toString()}`);
```

And there you have it, a quick and painless way to automatically update React app build information accessible from within the app.

https://www.npmjs.com/package/react-build-info
