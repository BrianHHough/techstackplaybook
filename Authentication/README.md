For this [Tech Stack in 10](https://www.youtube.com/embed/LoPB-3gnI6I), I'm diving into some best practices for using authentication in your full stack application.

Today's episode focuses on serverless authentication with AWS, as well as some best practices of what to do and what not to do 👨‍💻💭

Here’s a glance at what you’ll learn in this episode:

00:00 Learnings from coaching 8 people on app development
01:11 Authentication in AWS primer
01:54 AppSync and GraphQL API overview
02:16 DynamoDB and database overview
03:00 withAuthenticator best practices
05:43 Experimenting with AmplifyAuthenticator + what not to do!
08:38 Discussion on multi-user authentication
10:27 Finalizing the AWS authentication in our app
11:33 Conclusion

{% youtube LoPB-3gnI6I %}

<iframe width="839" height="472" src="https://www.youtube.com/embed/LoPB-3gnI6I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 👨‍💻 What Is User Authentication in an App Context?

When you think about developing an authentication system for your software application, you must ensure your system is highly secure and durable, scalable for a growing user base, and able to keep your resources and authorization requirements in place across the application.

User authentication is a process for confirming, and validating, the identity of a user within a system. It is necessary to create a system for account management for your software's users to ensure they must login with valid credentials before using certain (or all) parts of the product or service on the Internet.
<br>

![security camera image](https://images.unsplash.com/photo-1529490738614-4a83c19ed6b2?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1350&q=80)

<br>

Online services for authentication might require credentials, such as username and password that are used to identify your account within the pool of valid users. Perhaps you'll want to add in 2FA (Two-Factor-Authentication), an additional security mechanism for authentication to verify the users are who they say they are when they log in.
<br>
<br>

## 👨‍💻 How does Serverless Authentication Work?

Serverless authentication is a type of authentication that doesn't require the need for a physical in-house server to store your data. Instead, all of the actions, permissions, and rules are executed in the cloud and users can interact with the features and functionality without any need for a browser plugin. Authentication happens right within the app and gets users the resources and services they have been designated or signed up for.

### Let's take a real life example like the below image:

Your friend is hosting a birthday celebration and you are invited! 🎂

![door with buzzer](https://images.unsplash.com/photo-1528817466667-942353411fee?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=634&q=80)

First of all, **congrats** on the invite! Birthday parties are so much fun 🎉

So you might gather your resources, such as a birthday present and an appetizer (extra points if there's an accompanying sauce or dip), and drive out to your friend's house.

But...when you arrive, you are greeted by this metallic box with some buttons on it. Most remarkably, the door to enter is locked (there are other people's apartments here as well), so there's no way to just go up to your friend's house without a key.

Well, were you thinking you could just walk into your friend's house without some sort of verification or authentication? 🤔

<em>That wouldn't be too safe, right?</em>

This metallic box will require you to perhaps, ~buzz~ your friend's speaker box (you will have to know the URL you intend to go to, such as https://yourwebsite.com/signin) and then you will need to provide some sort of verification telling your friend who you are and that you are ready to come in (i.e. your username/password, or any such credentials).

Your friend will hear your voice and then go, "ah yes, come on up old friend!", meaning the authentication worked (your verification checks out: you are in fact who you say you are), the door will ~buzz~ (you will be authenticated: the door will unlock), and you'll be allowed to enter (you are now an authorized user: you now have access to your services/resources in the house...err...I mean app).

This is authentication in a nutshell, and hopefully gives a real life example of how this can work in an application. Software is very much the same way, and with a service like AWS Cognito user pools, this managed authentication service will let us do this with software and automation, rather than needing to ~buzz~ people in every time into our app. Could you imagine having to buzz people into your app every time?

That would be I N S A N E 🙈

Luckily with AWS Cognito, we have a powerhouse of tools to manage our users and these authentication flows!
<br>
<br>

## 👨‍💻 What is AWS Cognito?

![authentication failed photo](https://images.unsplash.com/photo-1618044619888-009e412ff12a?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1351&q=80)

[AWS Cognito](https://aws.amazon.com/cognito/) is a service that lets app developers and software engineers authenticate users without having to create their own identity system.

This specific AWS service makes it easy for any developer to get started with authentication because they don't have to build and maintain complex backend code.

It provides all the tools and infrastructure needed from the start, such as sign-up and sign-in flows, multi-factor authentication options, integration with social identity providers like Facebook and Google, and more.

One of its major benefits is that it can scale incredibly easily for millions of users as your user base grows and your users needs and requirements scale.
<br>
<br>

## 👨‍💻 How to Add Authentication to an App

Thanks to an AWS framework called Amplify, we can run the following command to add auth right into our app.

```javascript
// begin the authentication process
amplify add auth

? Do you want to use the default authentication and security configuration? `Default configuration`
? How do you want users to be able to sign in? `Email`
? Do you want to configure advanced settings?  `No, I am done.`
```

You can customize the way users sign in between `username` or `email` but make sure that whichever you choose is the one you want to use for the life of your Cognito user pool because you cannot change this specific setting once you build the Cognito User Pool.

When everything checks out and the scripts run to set up these backend resources for us in the cloud, we'll run the following command to push it to our backend environment:

```javascript
amplify push
```

Alternatively, we can import a Cognito User Pool right into our application in case we have the user pool already set up and just want to import it into our app directly without any configurations.
<br>
<br>

## 👨‍💻 Setting Up our GraphQL API

In the example in my video, I have created a ToDo application that lists a user's ToDos that they have to do.

When I set up my GraphQL API thanks to the AWS Amplify Framework, I provisioned a DynamoDB table for these ToDo types and put an `@auth` parameter on the type. This means that only the authenticated users who create a ToDo will have access to their own ToDos. More simply, I can't see your ToDos and you can't see my ToDos if we are both authenticated users of this ToDo app.

```graphql
type Todo @model @auth(rules: [{ allow: owner }]) {
  id: ID!
  name: String!
  description: String
}
```

When a user or set of users upload their ToDos, they will show up in the DynamoDB database [like so](https://www.youtube.com/embed/LoPB-3gnI6I?start=154):

<a href="https://www.youtube.com/embed/LoPB-3gnI6I?start=154" rel="AWS DynamoDB Key Value Pair Database">![DynamoDB image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/602h7nlklnfaathalxtw.png)</a>
<br>
<br>

## 👨‍💻 Adding Auth to our React JavaScript Code

Now that we've set up our resources in the backend and in the cloud, we now want them to be readily accessible for the users that we will expect to use our application.

For this, we will use the `withAuthenticator` HOC (Higher-Order-Component) which will allow us to use this module directly on the `export default App` script at the end of our `App.js` file.

```javascript
import React from "react";
import { withAuthenticator } from "@aws-amplify/ui-react";
import aws_exports from "./aws-exports";
Amplify.configure(aws_exports);

const App = () => {
  return (
    <React.Fragment>
      <div>
        <h1>Code Goes Here!</h1>
      </div>
    </React.Fragment>
  );
};
export default withAuthenticator(App);
```

Now, when we run our code, you won't be able to access anything inside of `App.js` until we (1.) Have an account; and (2.) Are authenticated in the system.

This will look [something like this](https://youtu.be/LoPB-3gnI6I?t=252):

<a href="https://youtu.be/LoPB-3gnI6I?t=252" rel="AWS Amplify withAuthenticator HOC (Higher Order Component)">![DynamoDB image](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fce5t7hebscdnikphmi5.png)</a>

We have a nice packaged-up module that has the "Create Account" and "Sign In" modules all neatly coded for us to use in our application thanks to AWS Cognito and AWS Amplify working together.

When we log into our account, we are able to see our ToDos! If our login credentials don't check out, we won't be permitted into our app.

🎉 ~ queue the confetti cannon ~ 🎉
<br>
<br>

## 👨‍💻 Some Observations In Working with the `AmplifyAuthenticator` Module

Something I noticed when experimenting with this module is that you can customize the entire auth flow with, from the stylings of the Sign-In page to the flow of the user verification when they are creating an account. It's definitely worth checking out the documentation on the website to learn more about what you can do with the `Auth` module in AWS.

One of the modules I was experimenting with was the `AmplifyAuthenticator` module and using it as a wrapper around the functional component, like below.

⚠️ As a warning, I noticed that using this module in the direct application caused some data to be cached even when you pressed "Log Out". As in, you would need a hard refresh on the page for you to "actually log out" even if you pressed the log out button when switching between users. This can pose a security risk to your users' data in case they do not automatically refresh the page but press sign-out with this module. I am not sure if this was because of being on the same device and logging into multiple users, but it's worth mentioning as I discovered this, which is why I [spoke about it here](https://youtu.be/LoPB-3gnI6I?t=343) and showed a live version of the data being cached before a hard refresh was done to the page. This module is fantastic for testing the authentication module and fine-tuning it, but always make sure what you put into production checks out in terms of the expected data and access you intend to have.

```javascript
import React from "react";
import { AmplifyAuthenticator } from "@aws-amplify/ui-react";
import aws_exports from "./aws-exports";
Amplify.configure(aws_exports);

const App = () => {
  return (
    <AmplifyAuthenticator>
      <React.Fragment>
        <div>
          <h1>Code Goes Here!</h1>
        </div>
      </React.Fragment>
    </AmplifyAuthenticator>
  );
};
export default App;
```

Based upon this, we'll want to use the `withAuthenticator` or a version of a customized Auth flow for our users to be able to use our application securely and correctly.
<br>
<br>

## 👨‍💻 Wrapping Up

Wow we went through a lot! Tech is one of those fast-paced fields where you will constantly find yourself learning and innovating as you go along. There is never only 1 right answer, and I look forward to hearing your feedback and thoughts on what areas of apps, software, and tech you want to learn more about.

Check out the full recording below:

{% youtube LoPB-3gnI6I %}

Let me know if you found this post helpful! And if you haven't yet, make sure to check out these free resources below:

- <b>Follow my Instagram for more: <a href="https://instagram.com/brianhhough" target="_blank">@BrianHHough</a></b>
- <b>Watch my latest <a href="https://youtube.com/brianhhough" target="_blank">YouTube video for more</a></b>
- <b>Listen to my Podcast on <a href="https://youtube.com/brianhhough" target="_blank">Apple Podcasts</a> and <a href="https://youtube.com/brianhhough" target="_blank">Spotify</a></b>
- <b>Join my FREE <a href="https://facebook.com/groups/techstackplaybook" target="_blank">Tech Stack Playbook Facebook Group</a></b>

Let's digitize the world together! 🚀

-- Brian

---

{% user brianhhough %}

---
