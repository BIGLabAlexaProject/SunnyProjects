# SunnyProjects
Repository that contains all of Sunny's projects.


# Writeup for Internship at the Bristol Interaction Group

The internship was focused on using Alexa and other amazon services such as AWS Lambda, AWS Comprehend and AWS S3 to create a multitude of applications that can be used to work on a co-design
system with sight impaired students in schools.

The Co-Design principal meant that we aim at creating different systems that can be shown and discussed with our co-designers and help create and allow for new ideas for interaction for the final system.

My work specifically related to creating various methods and examples of Alexa app templates and API work that could be expanded on for future projects.

These various apps are explained conceptually below:

## Alexa - Comprehend Examples:

[This app](https://github.com/sunnyMiglani/AlexaTestSkill) was built to show how to use external APIs with Alexa using a "middle layer" or a `helper.js` file that can launch python programs and grab outputs from them.

It also shows the uses of `Sentiment Analysis` and `Entity Analysis` tools available on AWS Comprehend. The code is currently hardcoded to use the shakespeare play "Much Ado About Nothing" and returns back the main 2 sentiments of the play, along with the characters it finds from the play.

## Alexa - Speaking Test:

[This app](https://github.com/sunnyMiglani/AlexaSpeakingTest) was built to create a small test program to use during a Co-Design session with the students to understand or differentiate on how much information can be given out via voice before they stop paying attention / the information becomes too heavy.

Currently it uses a passage scraped from the BBC Education website [Bitesize](https://www.bbc.com/education/levels/z98jmp3), specifically history under the OCR A system.

The text it runs can be changed inside the `index.js` file.

The repository also contains a **Alexa Speech Helper** tool that replaces the {`,` `.``;`} in a sentence with different amounts of pauses to allow for a better understanding from Alexa voice.

It uses the systems of pauses given by the SSML system, an amazon developed markup language to handle different voice interactions.

## Alexa - Big Quiz:

[This app](https://github.com/sunnyMiglani/AlexaBigQuizMaster) was built as a system that can take in a quiz input of questions and answers in a Multiple Choice Questions (MCQs) format and use the skill to question users on the various answers.

The skill does have some improvements that can be made, which are outlined on the README and shall be fixed over time.

The overall use of this program is to intergrate it with an alternative form of interactions **such as buttons, wearables or external devices**. 

The Co-Design session would look at the different forms of interaction and what's preferred in a collaborative environment.

## Python - Web Scraping:

[This program](https://github.com/sunnyMiglani/WebScrapingTest) was made to allow the user to scrape any BBC education website along with it's related pages.

The program was initially developed to provide data for the AWS Comprehend Topic Modelling, but was unable to be run due to limitations on the AWS Comprehend Topic Modelling system.

The data currently does not include an listed or unlisted items (bullet points) and can be expanded to include those as well. 

--- 
From the above apps there's a lot to be learned in the forms of how to interacti with Alexa and what's the best methods for creating and using a system for Alexa.


# Tips and Tricks for Alexa:

There are many tips and tricks to be learned from Alexa and development of the system. Some of these are outlined below:

### 1.: Alexa does not speak well unless you make her:
##### Cues in communication
As humans, during communication there are a lot of non verbal signals we make during communication, such as changes in facial expressions while thinking or movement of body before adding to a discussion. These simple things cannot be easily or feasibly mimiced by a system like Alexa that's very voice based.

There are however things that can assist in understanding and using these devices, for example having phrases that help the user know that something is going on, or they must wait for some amount of time before obtaining a response.

This is mimiced as an example in the `Big Shakespeare` skill, where knowing that doing certain calculations would take a long amount of time, Alexa tells the user to wait for a few moments before the response arrives.

##### Pauses in speech.
While Alexa and Amazon have worked hard on creating a voice that does not sound overly robotic, due to the system of using SSML, it falls onto the developer to create a script that allows and accommodates pauses in the speech for Alexa.

A big problem in understanding "her" is the lack of pauses during a sentence and the fact that she wouldn't stop in a long string of text to make sure the user has understood what's been said.

The latter is a problem in all voice assistance and voice based systems, as it assumes the user is perfect at paying attention and grasping all concepts as they hear it. 

The `Big Historian` app (speakingTestSkill) was made to test the differnce in whether the user would like a constant system that keeps track of progress, or prefer a system that talks and repeats different parts of it.

The gramatically accurate pauses are necessary for understanding and giving time for the user to comprehend what's been said by the device.
 
### 2. Alexa and AWS Lambda are different things and interact differently:
This might be a quite obvious observation, but the difference in Alexa and AWS Lambda comes into play during testing of the various applications.

Alexa allows you to keep the "state" of the conversation, which means that you can send and receive some variables through the `requests` and `responses` passed around in the system.

These variables are [stored as Session Attributes](https://developer.amazon.com/blogs/alexa/post/2279543b-ed7b-48b4-a3aa-d273f7aab609/alexa-skill-recipe-using-session-attributes-to-enable-repeat-responses)

```
As an example, the session attributes are stored by using the code

    response.this.attributes.<key> = <value>;
```

This makes sense as you're replying to the recent request with a particular response,and it contains certain variables. The advantage of using this is that in the next `request` your lambda function receives, it will have these attributes stored.

Now to test this on Alexa is straightforward by just using the attributes, and using the "test" tab on your alexa developer console for that particular skill. This allows you to see the JSON for the requests/responses and look for mistakes.

**However,** this on AWS Lambda was a problem as the method of testing on the online console is a simple test JSON that's passed into the function in the format you give it.

This works well when you're testing simple intents, but not complex conversational systems.
I faced this problem while attempting to create the `Big Quiz` skill, and the solution that was developed was to ignore the session attributes entirely. It's much simpler to just use the nodeJs variables and `var`s to create the memory of the system.

And to use something longer lasting, using a file system that can keep track of the data is quite helpful as well.

### 3. Interaction and helping users with commands
Interaction with alexa is straightforward for someone developing the system, and keeping track of the various phrases and  triggers that can be used to start intents for Alexa.

However, a core part of the project involved making it easy and accessible, which implies allowing a broad range of triggers and phrases that people can use.

When the developer creates the app, he/she/they have an idea of what exactly should be used as instructions and phrases, however when a new user looks at the system, they have no idea what exactly the limitations, phrases and triggers could be.

A simple way of guiding them through it is by having a "story" of how they interact with the app.

Always starting from the  `help` section / intent, we guide the user to the main purpose of the app (such as a quiz), and teach them the ways to reply / use the feature (quiz) of the app, by suggesting commands such as adding a line to the help section that says

```
Alexa Says: "Welcome to the help section for <app> ... / ... / ... to start the <feature>, say 
Alexa, ask <app> to <triggerThatStartsFeature>

```
This is beneficial for the user as it allows them to start off without your assistance, and it's always a great idea to stay with the users and list down how they interact with your app.

For example,
1. Some users like to use a directional method of navigating through an app saying `go back 3` or `go forward 1` etc. 
2. Some users prefer using a `next/previous` methodoligy.
3. Some users will often swap between the two making it entirely confusing on which they prefer, and therefore having to accommodate both.

This is tedious for the front end method, but it allows for a much easier way of interacting with the app.
