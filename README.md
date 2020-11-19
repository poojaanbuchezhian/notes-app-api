A note taking application powered by a serverless API written completely in JavaScript.

AWS ACCOUNT SETUP

Initial steps involved account creation, IAM(identity and access management) user
creation which enables us to manage users and user permissions in AWS. By doing so,
we can get hold of Access key ID and Secret access key. This will be used by AWS
CLI and Serverless Framework. To make it easier to work with a lot of the AWS
services, we used the AWS CLI. They’ll be connecting to the AWS API directly and will
not be using the Management Console.

SETTING UP THE SERVERLESS BACKEND

To build the backend for our notes app, We used DynamoDB to store the data.
Amazon DynamoDB is a fully managed NoSQL database that provides fast and
predictable performance with seamless scalability. Similar to other databases,
DynamoDB stores data in tables. Each table contains multiple items, and each item is
composed of one or more attributes.
After getting our database table ready, to get things set up for handling file uploads, we
had to handle file uploads because each note can have an uploaded file as an
attachment. Amazon S3 provides storage service through web services interfaces like
REST. It can store any object in S3 including images, videos, files, etc. Objects are
organized into buckets, and identified within each bucket by a unique, user-assigned
key. We created an S3 bucket which will be used to store user uploaded files from our
notes app.
Our notes app needs to handle user accounts and authentication in a secure and
reliable way. To do this we made use of Amazon Cognito. Amazon Cognito User Pool
makes it easy for developers to add sign-up and sign-in functionality to web and mobile
applications. It serves as our own identity provider to maintain a user directory. It
supports user registration and sign-in, as well as provisioning identity tokens for
signed-in users.
User Pool was created to enable users to to be able to sign up and login with their
email as their username. Further app client was created and a unique domain name for
our notes app was selected.
Since we are going to be using AWS Lambda and Amazon API Gateway to create our
backend, Serverless Framework comes into play. The Serverless Framework enables
developers to deploy backend applications as independent functions that will be
deployed to AWS Lambda. It also configures AWS Lambda to run our code in response
to HTTP requests using Amazon API Gateway. API Gateway makes it easy for
developers to create, publish, maintain, monitor, and secure APIs.

BUILDING A SERVERLESS REST API

API to create a note will take the note object as the input and store it in the database
with a new id. The note object will contain the content field (the content of the note) and
an attachment field (the URL to the uploaded file). Testing is done locally by mocking
the input parameters.
We’ve created a basic CRUD (create, read, update, and delete) APIs.

● API to retrieve a note given its ID

● API that returns a list of all the notes a user has

● API that allows a user to update a note with a new note object given its id

● API that allows a user to delete a given

We made a small addition to this by adding an endpoint that works with a 3rd party API
to work with environment variables and to accept credit card payments using Stripe.
In the case of our notes app we are going to allow our users to pay a fee for storing a
certain number of notes.

1. The user is going to select the number of notes he wants to store and puts in his
credit card information.

2. We are going to generate a one time token by calling the Stripe SDK on the
frontend to verify that the credit card info is valid.

3. We will then call an API passing in the number of notes and the generated token.

4. The API will take the number of notes, figure out how much to charge based on
our pricing plan, and call the Stripe API to charge our user.

DEPLOYING THE BACKEND

We have the User Pool that is going to store all of our users and help sign in and sign
them up. We also have an S3 bucket that we will use to help our users upload files as
attachments for their notes. The final piece that ties all these services together in a
secure way is called Amazon Cognito Federated Identities.
Amazon Cognito Federated Identities enables developers to create unique identities for
your users and authenticate them with federated identity providers. With a federated
identity, we can obtain temporary, limited-privilege AWS credentials to securely access
other AWS services such as Amazon DynamoDB, Amazon S3, and Amazon API
Gateway.
We will be using our User Pool as the identity provider. Once a user is authenticated via
our User Pool, the Identity Pool will attach an IAM Role to the user. We will define a
policy for this IAM Role to grant access to the S3 bucket and our API. This is the
Amazon way of securing your resources.
Finally, Deployment and Testing of the API are done.
