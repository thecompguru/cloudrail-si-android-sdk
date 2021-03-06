# Social Login Sample

This sample illustrates how you can use our library in order to support a "Login with ..." functionality. For our sample we will use a NodeJS server with a mongo database as well as an Android frontend to communicate with the server. The idea is, that the Android application lets a user sign in with Facebook, Twitter, Google or LinkedIn, sends the acquired information to our server. The server retrieves the unique identifier for the user from the chosen service, creates a new user in the database if necessary and returns a user specific token back to the frontend. This token will then be used to further communicate with the server.

## Prerequisites

In order to run the backend component you need to install NodeJS ([official website](https://nodejs.org/en/download/)) and MongoDB ([installation manual](https://docs.mongodb.com/manual/installation/)). For the Android frontend you need to have Android Studio([installation manual](https://developer.android.com/studio/install.html)) installed. In the next steps, we will assume that all of the mentioned pieces are properly installed.

Apart from the mentioned things above, you need developer credentials for the four services. Instructions on how they can be acquired can be found in our wiki. Here the links for [Facebook](https://github.com/CloudRail/cloudrail-si-android-sdk/wiki/Services#facebook), [Twitter](https://github.com/CloudRail/cloudrail-si-android-sdk/wiki/Services#twitter), [Google+](https://github.com/CloudRail/cloudrail-si-android-sdk/wiki/Services#googleplus) and [LinkedIn](https://github.com/CloudRail/cloudrail-si-android-sdk/wiki/Services#linkedin).

## Setting up the database

The example server assumes that there is a local database called login-with-sample. In order to create it you have to run the following commands:

````
# Starting the mongo console
$ mongo

# Create the database if it doesn't exist yet
> use login-with-sample

# Validate if the database was created successfully
# If so the following command will list the newly created database
> show dbs
````

## The server component

Locate the following piece of code within your *index.js* (line 45):

````javascript
var profile = new cloudrail.services[req.body.name](null, "[TwitterClientID]", "[TwitterClientSecret]");
````

Here you have to provide your credentials for Twitter. The reason why it is only for Twitter is the following: Twitter uses OAuth1.0 for authentication which requires your credentials even after you already acquired an access token. All other services don't, so it's fine if we instantiate all services with Twitter credentials.

In order to actually start the server, look at the following instructions:

````
# If you are starting it the first time, run the following two commands
$ cd <path>/login-with-sample/backend
$ npm install

# To actually start the server
$ node index.js

# If it was successfully started it will print the following
Listening on 5000
Successfully connected to Mongo DB
````

## The frontend component

Locate the following piece of code within your *ChooseService.java* (starting from line 38) and replace the placeholders with your own credentials:

````java
switch (v.getId()) {
    case R.id.Facebook: {
        profile = new Facebook(mContext, "[ClientID]", "[ClientSecret]");
        break;
    }
    case R.id.Twitter: {
        profile = new Twitter(mContext, "[ClientID]", "[ClientSecret]");
        break;
    }
    case R.id.GooglePlus: {
        profile = new GooglePlus(mContext, "[ClientID]", "[ClientSecret]");
        break;
    }
    case R.id.LinkedIn: {
        profile = new LinkedIn(mContext, "[ClientID]", "[ClientSecret]");
        break;
    }
    default:
        throw new RuntimeException("Unknown Button ID!!");
}
````
