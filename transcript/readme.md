[00:00:00] Leo Dion (host): today we're gonna talk about, Server Site Swift. How does it work out as in physical fitness in the real world? long wait. There we go. A little bit about myself. I run a freelance company called Bright Digit. I have, I live in the USAI live in Michigan with my wife and my six wonderful children.

[00:00:31] Leo Dion (host): And, been doing development for, yeah, like 25 years. I can't believe it. And it's crazy. Time flies. I've been doing iOS development since 20, 2010, and I've been running my own company, broad digit since 20 20 12. and I have a, podcast and a YouTube channel there. You can also check out my code, at my GitHub profile as well.

[00:00:58] Leo Dion (host): I. today we're gonna talk about, we're going, this is gonna be kinda like a workout. It's not just a presentation. get your activity rings. it is gonna be fantastic. we're gonna start with doing a warmup. just kinda learning the basics, of fitness and server side. Swift.

[00:01:18] Leo Dion (host): I'm gonna teach you how a gBeat the specific application I'm gonna be talking about today that I've been working on. We're gonna do some stretching, then we're gonna do a low intensity workout, which is a technical deep dive of some swift code that I hope you find useful and helpful. and then we're gonna go a little bit outside our comfort zone folks.

[00:01:36] Leo Dion (host): there's other devices that people use that don't run swift. So yeah, this is gonna be really intense. So we're gonna try to get your heart rate up with that. And then lastly, we'll do a cool down of future plans and lessons learned. So let's warm up everybody, and learn about fitness and Server site Swift.

[00:01:54] Leo Dion (host): So I'm gonna take, go back in time and way back to the year, 2018, and there's a conference that goes on called, try Swift in New York. And I'm looking at this and I get, I went to it and there's this workshop build a cloud Native Swift app. And I'm like, okay, first of all, Swift's fairly new.

[00:02:15] Leo Dion (host): It's pretty much a iOS thing, right? who else is gonna use Swift? Why would it be cloud native? This is crazy. And so I go to this workshop in this IBM building in New York, and I'm introduced to this little thing called Couture, and I'm like, eh, that's, interesting. That's cool. And then when I get home, I get a little curious and I'm looking around and I'm trying to figure out, okay, is there some other way of doing this?

[00:02:38] Leo Dion (host): Like people talk about this thing called Vapor, like what is that? And I fell in love with it. I was like, this is, amazing. It's super simple. it's fast when it runs. It uses all the advantages of Swift. And so I'm like thinking what kind of app do I wanna now build with this new vapor, server?

[00:02:59] Leo Dion (host): And obviously there's like a to-do to-do list, right? Everybody wants to build a to-do list when they first get a thing, Hey, there's a Vision Pro, let's build a to-do list for the Vision Pro so we can see our to-do list in 3D. Fantastic. but but then I'm like, one thing I'm thinking of is I was really in, I'm still really into the Nintendo Switch and I'm like, I wanna track all the games I have, a wishlist and I can pull the info for, scrape it from the Nintendo website, blah, blah, blah.

[00:03:25] Leo Dion (host): But I was like, nah, that's not cool enough. And then I thought to myself, how about a workout app? But how, why would you ever have a workout app on the server? That doesn't make any sense. here's what I was thinking is, first of all, there's this brand new device that's $17,000 that runs Swift, and I'm like, oh, this is fantastic.

[00:03:49] Leo Dion (host): Why don't we run Swift on a giant server and at the same time run it on the smallest device possible. Of course, this is before embedded, right? But you get the idea. And I'm into like working out kid this, I work out and I'll have like joycons in my hand while I'm working out on elliptical and it's like, my way of getting my workout in while playing whatever favorite video game I wanna play at the same time.

[00:04:14] Leo Dion (host): It's it's multitasking, right? This is fantastic. And two, there's this like whole community of people who do speed running. And I don't know if you notice this, but down there in the bottom left, there's a little, thing right there showing the person's heart rate as they're speed running. And I'm like thinking to myself, ah, okay, what if there is a way I can live stream my heart rate in live as I am, doing something?

[00:04:41] Leo Dion (host): But then the question ends up being is how am I gonna go ahead and do this right? let's talk about how this is gonna actually work. So the way it works is your heart rate, you get health kit, right? You get your permissions all set up, et cetera, et cetera, and a heart rate comes in through health kit, and then I make a post call to the server, and Vapor then takes that post call.

[00:05:06] Leo Dion (host): And it then opens a website, or sorry, excuse me, it posts that to the server. Meanwhile, you have a browser window and that browser window opens a web socket listening for that heart rate. So then that gets communicated back to the browser through the web socket. And then what you do is you, get your fancy Mac Pro running OBS.

[00:05:27] Leo Dion (host): And you go ahead and there's a, list. OBS if anybody has ever used is massive, super complicated. You could do pretty much everything you want, including breaking your live stream. So you go into OBS and you set up a source for, not a window or a, screen, but like you can actually set it to a browser, URL.

[00:05:46] Leo Dion (host): And you put that URL in OBS, right? So you put the URL on there. And then OBS now points to your web browser that's showing your heart rate. And then basically what you could do is overlay that and you can do a lot of things with OBS, like doing a green screen or anything like that. And that will live stream your web browser and your video, and that's how you get your heart rate, into your livestream using, our app.

[00:06:13] Leo Dion (host): And, this is where I created the app called Har Twitch. it was a good name at the time. It's a horrible name. Now, I'll explain that later. But you can sign up to Har Twitch and, it will, you can set up a live stream to, share your heart rate. And I have been using it, I was doing a lot of live streams, so see if you could see here in the upper right, this doesn't work, but in the upper right you can see my calorie count, for instance, while I'm playing Galaxy here.

[00:06:43] Leo Dion (host): or I got really into Ring Fit. Has anybody played Ring Fit before? Okay. Like ring fit's a blast. So I was live streaming, playing ring fit while using hard twitch at the same time. And it would do like a little, this is the best I could do, by the way, for doing the activity ring in HTML and CSS.

[00:07:00] Leo Dion (host): But, I have a little activity ring showing how many calories I'm doing up to my goal, so this is fantastic. and then, those of you who know me know I have a podcast, so I might talk about a lot of my projects and, I get a call from or an email, from this person on the left, Chris, and Chris is like, Hey, this is a really great idea, but how about let's do something a little bit better.

[00:07:26] Leo Dion (host): I don't know if you know this, but there's a pandemic going on and people aren't going to the gym. Why don't we take this heart? except for this poor guy, right? and her like wearing their mask. They do not look happy in that photo. and people are doing fitness at home, like Apple Fitness is fantastic.

[00:07:45] Leo Dion (host): I use that too as well. And what if there's a way we could help people, live, show their heart rate, for instance, to their instructor, or if an instructor wants to live stream their heart rate, they can do that as well. And so this is where the genesis, the idea of, gBeat comes from is rather than just doing it for video games, crazy idea, but like actually doing it for fitness classes.

[00:08:11] Leo Dion (host): And that's where gBeat comes in. So let me show you a little demo how gBeat works. So you go in, you sign in and you create an account, or you just sign in and automatically will create an account on your web browser. You have the app installed on the watch. That's what you're seeing to the right. Then you go ahead

[00:08:35] Leo Dion (host): and you will create a new class, for instance, whatever, class you want. And these usually match up with workout types that come with health kit. So that way we can, we know how to, when the user starts the class, what workout types start on their watch. You can add a date and time. So if you wanna say oh, my class is only Wednesdays at 6:00 AM or whatever, you can do that as well.

[00:09:01] Leo Dion (host): for testing purposes, I didn't add that because it would restrict when I could start the class. So in this case, I don't have a class. Then I can go ahead and, we can start the class.

[00:09:17] Leo Dion (host): There it is. My new class called Mixed Cardio. I. And then notice on the watch, we get a nice little notification if we're the instructor that, Hey, you started your class on the web. You want to go ahead and join your class on the watch. Then you go ahead and you tap that.

[00:09:37] Leo Dion (host): And we have the heart rate here, and we show the heart rate on the, web browser as well. And if we wanted to, we could go ahead and bring that into OBS or we could just have the instructor just look at it kinda like I'm looking at this comfort monitor over here while they're doing their fitness class.

[00:09:54] Leo Dion (host): I end the class and then automatically ends the class on the watch right away. So that's kinda the basics of how gBeat works and how it works with live streaming. Let's take a little bit of a technical deep dive and look at some of the things that we built along the way that helped both with the app and the developer experience.

[00:10:17] Leo Dion (host): So with heart Twitch, I didn't wanna deal with username and login. Username and login or username and password on the Apple Watch is just an awful experience. and I don't wanna have to tell Siri on the Apple Watch, it's XVZ six J ampersand. I wanted to keep it simple. So any, anything that could reduce any like login interface on the watch is great.

[00:10:43] Leo Dion (host): So what I came up with was using cloud kit because you already have the watch linked to your Cloud Kitt account, right? And I even built a mis kit here that you see the GitHub, URL is specifically for that purpose. So that way Vapor can talk to cloud kit, for getting any sort of information from a private database.

[00:11:04] Leo Dion (host): But it, it felt kinda not really the way to do it. So when we got to gBeat, we ended up. Opting in for signing in with Apple and that was perfect. That worked out awesome. there's a really great, I'll have, links in the end to all the GitHub packages that we use, but the, vapor a, the vapor stuff and the JWT stuff was signing in with Apple is absolutely fantastic.

[00:11:28] Leo Dion (host): And so this was awesome. So now you can sign in on the watch and it's automatically connected to the account you have on the web. Great. And then you start doing watch development.

[00:11:42] Leo Dion (host): Hold on. Oh yeah. There we go. Good. Okay. Nope. Wait. Oh crap. Yeah. that can slow down development, quite a bit. So what's, what can we do instead? I do wanna test the, on the actual device for sure, but not for simple code changes, right? So obviously, we could do, we could, we use a simulator, ta-da.

[00:12:09] Leo Dion (host): That's great. Simulator is gonna allow me to do a quick Apple development. And then, when I feel ready to test on the actual device, then I could do that. but there's one big problem about this sign in with Apple and the watch simulator don't work. They just don't like you try to sign in with Apple, it tells you have to link an account and then you try to link an account and it's oh, you have to have face ID or something to do it.

[00:12:37] Leo Dion (host): It's like it's not set up. how the heck am I gonna run this app on the simulator when I want to do quick code changes, right? I got to thinking and I was like, wait a second, I have access to the server. If I'm running both the server and the simulator, I should have be able to manipulate the simulator through my Mac, right?

[00:13:01] Leo Dion (host): And so what I ended up doing was user will try to get on the simulator and nothing will happen. Then if they go on the website and they log in there, vapor can then take that token that I logged in with and save it to the simulator because I have access to the simulator. I can get, what the data directory is or what the temp directory is or whatever, and copy that information there.

[00:13:28] Leo Dion (host): And then from there it'll automatically, it'll automatically enable a button to use that token to log in. So the user goes into the website, they sign in with Apple Vapor, takes that token, saves it to the simulator, and then the Save simulator, excuse me, enables that button for sign in with simulator.

[00:13:51] Leo Dion (host): So here's some example of that code that I use. So,

[00:14:03] Leo Dion (host): there's this list here that you can see. And what that does is it runs, simctl, if any of you're familiar with simctl. It's like the backbone of doing anything with the simulator. It runs X run CTL to get the list of the devices in JSON format. And then it uses Codeable to then decode that it runs the process within, Vapor.

[00:14:25] Leo Dion (host): And then it grabs that Jason, that gives me a list of the devices.

[00:14:35] Leo Dion (host): And then it goes through, it looks at the list of devices, finds the one that is actually booted. So every device that's booted. And then I find when I take that list of devices that are booted and then I go ahead and run, get app container, giving the app bundle to, gBeat for instance. and then what container type.

[00:14:54] Leo Dion (host): In this case I use the data container type and then the device ID of whatever device is booted. And so the way that works is we just add in further arguments, for the GET app container, which is get app container, the simulator id, the app bundle, and then the container description with whether it's data or temp or whatever directory in the simulator.

[00:15:17] Leo Dion (host): You want. And then we go ahead and we save that to the simulator. we grab the container pass, we give it a new name for that specific file that will just have the string of the token. And we go ahead and just write those to the simulators that are booted. And what that'll do is that'll enable the button, because that watch os is watching for that file.

[00:15:39] Leo Dion (host): And once it sees that file exists, it tries to authenticate and then it enables the button. And so that's how I get around the issue of sign in with Apple on the simulator is by taking advantage of running vapor on the actual machine. The simulator is on another situation, and I don't know if anybody has run into this, but let's say you're doing full Stack Swift development.

[00:16:00] Leo Dion (host): You're doing it in Xcode, you're doing Vapor, you're doing Apple Watch, you're doing phone. Okay? But then when I'm running on the actual device, how's that device gonna know where that dev server is? That's one of the issues, we ran into pretty quickly. Because it, we could do something obvious, right?

[00:16:18] Leo Dion (host): We could just go into Xcode and say, base URL is whatever the IP address of my machine is. But that could change. I could be somewhere else. so this is when I came up with the idea of, sublimation. the idea of sublimation is the swift package is giving it the ability for the phone or the watch, to be able to find the development server within your local network.

[00:16:46] Leo Dion (host): And at first, the way I did this is by running NR and then saving the URL. How many of you are familiar with NR? Eng Rock is a service where you give it, some local address and it'll automatically, it'll create a tunnel basically to your local deaf machine, so that way you can have access to it. And so I would take that URL, I would save that to the cloud and with some like key that I would know.

[00:17:11] Leo Dion (host): And then on the device, I would just look up the URL based on that key in bucket name for instance. But then I thought to myself, there's gotta be a better way to do this. I don't know if any of you know this, but there's a built-in technology called Bonjour, and that is when I decided, this is an even better way, because it's all automatic, no configuration for the developer.

[00:17:32] Leo Dion (host): It can just get this up and running without setting up buckets or keys or any of that crap. So here's an example of setting up Bonjour to do this. you set up a binding configuration. This is an object that has the list of hosts and, has what port the server is running on, whether it's HGPS or not.

[00:17:53] Leo Dion (host): You take that information, you, give bonjour that information, and then it'll advertise that, within your network, letting them know where your server is. The way it works is we take that binding configuration, the binding confi configuration we used, protobuf because I wanted something really small, protobuf as a way of encoding structs and data in binary format as opposed to json.

[00:18:19] Leo Dion (host): It's really fantastic. You should definitely take a look. And we take that data, we encode it as a string, we split it into two, about 200 characters each, and we've created a dictionary of, that information. And what you can see here in, in like sublimation underscore with the offset, and then we create a text record.

[00:18:41] Leo Dion (host): there's other ways of doing this. Text records were the best way because of working with the Apple Watch and the limitations of that. So then that text record is then attached to the Bonjour service when we, advertise it. Now on the client, we have a browser that goes ahead and it has a default URL configuration just to fill in any blanks that it might not know about, and then it goes ahead and every time the browser results change, it calls pars results.

[00:19:11] Leo Dion (host): Meanwhile, we have an async stream to get the URLs, and that will just keep listening for any new URL that comes from the bond jewel service. But the pars results, you can see here, this is where we take that we, only want results that are of the Bonjour type with the text record. And we take that text record and we go ahead and re pars that out into a binding configuration.

[00:19:38] Leo Dion (host): And then the binding configuration. Has a URL's property that goes ahead and returns the URLs based on that, list of hosts, port name and HPS. So that way we now have new URLs for the base URL, and this can be set up either with Vapor, I even have it set up now to work with, lifecycle the new lifecycle service stuff.

[00:20:03] Leo Dion (host): So this works with HummingBeatird as well. And this allows me to easily, communicate. set up the dev server easily, for, the, for a developer. So there's different suites depending on what you're doing. If you wanna stay with Eng Rock, you could do that. If you're using a vapor, you could do that, or you can use, sublimation service if you want to use a lifecycle service as well.

[00:20:27] Leo Dion (host): Let's take a look specifically at how the communication between the heart rate and the browser works using web sockets. And we built this on top of Redis. So the way this works is that, we have a struc set up, or a class, actually, no, excuse me. We have a actor set up, and this has a list of web sockets based on an id.

[00:20:50] Leo Dion (host): And whenever we want to, whenever we get the heart rate from a post call, we go ahead and publish that to Redis. And then at the other end, every time went back,

[00:21:08] Leo Dion (host): every time the, Redis says that there's a new heart rate. I. And then goes in and looks at that dictionary with the array of web sockets and it posts sends that text. That's Jason over to the web socket. And then the main part here is this, Gianni, who's here. He helped me a lot with this part. What we do is we subscribe to Redis for any new subscription that comes or anything that comes in through Redis, and we go ahead and we create a message outta that.

[00:21:39] Leo Dion (host): And that will have the information about what web socket it go or what sets the web sockets it goes to. And then every time a new message comes into the async Stream, then it goes ahead and it calls that syntax that we saw previously and sends that to the web socket.

[00:22:02] Leo Dion (host): Then there's, push notifications. You probably saw that earlier. So every time a new device comes in, we set up, we set it up so that you receive a push notification if you, the instructor has started a new class and what we've done is we have a polyfill. So we do have a iPhone app and an Apple Watch app.

[00:22:21] Leo Dion (host): And we have a polyfill that works with both. And then we set up a, did register for no remote notifications for both the phone and the watch. Once that is called, we go ahead and call this method here where we do the thing on the watch where we ask if it's okay to give you notifications, and then we create what's called a mobile device request content, and this creates the information that is then SA saved to the database.

[00:22:46] Leo Dion (host): Before we do that, we check if the device already has an ID or device token that came from push notifications, and then we go ahead and if it's already there, we do a modify. If we, already have one and they did not want notifications, we remove it from the database. And then if they don't want to have one in the database, but they do have, they do allow notifications, we go ahead and create that and add that to the table.

[00:23:16] Leo Dion (host): So once we have that, we go ahead and, crate, this is where we save it using, fluent. And then what we've done now is every time a new workout class has started, we're using the middleware model middleware that comes with fluent and vapor to go ahead and every time a new workout started, we go ahead and we call that push notification.

[00:23:42] Leo Dion (host): We create what's called a workout group response, having all the unnecessary info that we need for the push notification, and then we use Vapor A PNS to go ahead and send that out. on the client side, the, we did have to end up creating a iPhone app as much as I didn't want to because it was so important for us to have, actual people being able to download the app easily.

[00:24:08] Leo Dion (host): And we found that doing that through the watch app store wasn't as easy. So we ended up creating an iPhone app. And this, of course, uses everybody's favorite framework, watch connectivity. but that has its own layer of issues. So what we ended up doing is I created a combined based library to then listen to different messages and decode them through watch connectivity.

[00:24:33] Leo Dion (host): here's a really poor gif of me doing communicating colors through the Apple Watch. This is a library called Sundial. Okay. which uses that. Now we're gonna get outside the comfort zone. So hold on folks. This is gonna get really tough. So let's talk about non-ST stuff. so we do have a web front end.

[00:24:58] Leo Dion (host): It uses web pack vi, VJs and TypeScript, which I actually TypeScript so much better than, JavaScript, I have to admit. and I like it for a lot of reasons. I don't. Preferred over Swift, but it is pretty darn good for what it does in checking things out. The thing with v js is I'm running a VJS router and I wanna make sure that every page is still goes through AD HTML index because HTML index has all the code needed for doing the V js routing.

[00:25:30] Leo Dion (host): So I've set up a couple of middle middleware pieces in the router that, first of all, the directory index middleware, which checks if the file exists, and then if it does it go, has go, goes ahead and serves that particular file like an image, or any assets. If it doesn't, it then goes to the index serving middleware.

[00:25:52] Leo Dion (host): And what I do is I set up, a specific path for my API, if it matches any of the API paths. Then it goes ahead and it runs whatever a PII have set up. If it doesn't, then it goes ahead and it serves the, cache HTML index page, assuming that the VJS router will then take care of it.

[00:26:18] Leo Dion (host): So one of the other issues that we ran into was like swift pack or package up Swift files. Were just getting massive with all the dependencies, we're running everything else. So I had one of the developers on my team look at it and, see if he can organize it. This is, Ben, and he did this and I was like, wait, that's really cool.

[00:26:39] Leo Dion (host): I like this. This is well organized. Easy to understand. And it works with Swift packages. But then I thought to myself, why don't I do something like this, but like Swift ui? And so that's where I ran. I reduced it all the way down to this. we have two entries for our products, for the stuff that runs on the watch, and then the actual command that gets built.

[00:27:03] Leo Dion (host): And then we have some test targets. We say we only wanna support stuff from 2023, and we have the default localization. This looks nice. This looks neat. It works out well. of course the secret behind it is that behind all this, there's all this code that gets generated that you don't see, but it's a lot easier to manage.

[00:27:25] Leo Dion (host): and so I created this DSL for Swift packages to make me do this. And the way it works is you just basically, you create all these little structs for each product or test target. And there's some stuff with swift settings, and then I, all I need to do is run a script to do this. So package DSL all it contains really is the package sh, which is a bash script that does this fancy code here.

[00:27:53] Leo Dion (host): Wow, cat. Pretty amazing, right? and it works like all I have to do is just concatenate. You don't need to install anything. It just builds the packaged out Swift file, so it's easy to use. I never touch the package out Swift file, but I do create the files that are needed for, the different, for the Swift package.

[00:28:13] Leo Dion (host): And then there's these support files set up for things like products and targets that you bring in. And it all gets ated in one file. this is an example of that, how to create a package. And you can see this, a bunch of code that takes the, all the, pro, excuse me, result builders and turns it into a usable swift package.

[00:28:39] Leo Dion (host): I've actually modified this script quite a bit and added stuff Like per, you could specify what Swift tools version, it'll minify it. All sorts of stuff that I've been using now on my larger projects.

[00:28:57] Leo Dion (host): this is the uncomfortable part. We do have an Android app. Sorry. My Android developer was actually like, yeah, swift on the server has been amazing. It's worked really well. we really love it. the only issue I've run into honestly, is Google. Like the Google wear stuff is not as nice as Apple Watch if you can't believe that.

[00:29:16] Leo Dion (host): And, but otherwise he's had nothing. But everything's been great. we had, we added login with Google, so we just use the Google O stuff that, they provide from, for JWT. And we also use, we have a setup for, fire, we use Firebase notifications to do notifications for Android. and we have a way of checking that in our database, and that's worked fantastic.

[00:29:46] Leo Dion (host): Secondly, we have worked with a client who wants our watch app on the React Native app. That has a whole set of issues, but we do have it working. we'll talk about that later. And then lastly, we have a Chrome extension. Which will, what it'll end up doing is you can, start, you can watch a YouTube video of a workout, and then you can start a workout for yourself.

[00:30:08] Leo Dion (host): So that way you can, and that gives you the push notification on your watch. You go ahead and start the workout on the watch, and then the Chrome extension will then show your, zones and your heart rate. All that information on top of your YouTube video.

[00:30:30] Leo Dion (host): Time. Can I have a little bit more time I can go over? Okay. Yeah. at first we set up GitLab, this is our repo setup for GitLab. Oh boy. Yeah. and then this is how the CI was set up. and then we realized GitLab costs a lot of money and and it was just like if I did the wrong thing. It would just collapse, right?

[00:31:01] Leo Dion (host): So we had to keep doing it. we switched over to GitHub and that's which GitHub is better. it is a little bit, it is a bit better, but the cost was better and it gave me an opportunity to really clean this up. as far as set up for the ci, we have a thing that's built to two. So just to make sure the Swift package builds on Ubuntu.

[00:31:23] Leo Dion (host): Then we build the web front end, so that's like the tailwind, CS, s, JavaScript, all that stuff, type script. Excuse me. If that all works, that runs in parallel, then I kick that off and I build it on my Mac OS machine to see if that works and ensures the iPhone and watch app work. If that works, then we go ahead and lint it and make sure that all our code styling is consistent.

[00:31:45] Leo Dion (host): And then, once that's done, we use Fastlane, to archive and deliver the app depending on what branch it might deliver to test flight. And it just at least ensures that the app still builds and we don't have any weirdness. And what's really nice is that Heroku then has automatic hookup. If the CI passes and depending on what branch, it'll then deploy that either to our alpha instance or our production instance.

[00:32:10] Leo Dion (host): And that's worked out really well. As far as getting this working on Heroku, one of the things that we also did is we put everything from being multiple repos to basically going mono repo. And that's been really great. And so the root of our repo is just containing Xcodegen YAML file, which creates the Excode project in the Fastlane configuration.

[00:32:36] Leo Dion (host): So that's specifically for the archiving and delivering. And then inside we'll have a directory for our Swift package, and that contains the code that's necessary to run Vapor, and that uses the vapor build pack. And then we also have the web folder, which creates the web front end, and that uses the no Js build pack.

[00:32:57] Leo Dion (host): And then because of the structure of our directory or repo, we had to add a, what's called a subdirectory, build pack because our, excuse me. The Swift code and the web directory were all in, not in the root. So we had to have a way to notify that using that build pack. So here's a video just demoing gBeat.

[00:33:18] Leo Dion (host): It's one of our ads. it does have sound. We can get the idea of how it works. Personal coaching through data web interface. and we have gyms that we're partnering up with this year that is gonna continue to grow this app. Lastly, now it's time to cool down. No more talk about Android and React.

[00:33:43] Leo Dion (host): there's a little bit of talk about React. Sorry. The first thing I want to do, is when we build the app, Giannis was super helpful. we needed an open API, spec for it and. There was no way to do this. So what we did is we used net map Zen's library to create an open API specs from our code.

[00:34:08] Leo Dion (host): And, it's not great, because it creates, it just creates all these wrappers that is confusing and it a mess to deal with. so in the future, obviously I think we wanna. We wanna switch that over and use the new, swift Open API generator. now that's available as far as React native, we have, an issue where we essentially have to share the code with the client, and I hate that, for a lot of reasons, not so much security as much as like they can mess with it.

[00:34:42] Leo Dion (host): and so what we'd like to do is figure out a way to build an XE framework we can deliver to them. There's some issues with dependencies and server sites, swift all being in the same package that kind of causes this issue. So that's definitely something we wanna do. I wanna research protobuf. I want to maybe switch over the heart rate stuff and the health stats over to protobuf instead of Jason, and maybe even some of the watch connectivity stuff would be great.

[00:35:07] Leo Dion (host): protobuf, you can set it up that, you basically create like a protobuf file and it'll generate a swift, struct for you out of that. similar to how the opi open API stuff works. Lastly, there's all sorts of weird quirks with networking on the Apple Watch. I don't know if anybody knows this, but there's limitations that aren't obvious when you start running this.

[00:35:28] Leo Dion (host): For instance, with the Bon or stuff, we ran into all sorts of issues where you can send data through Bon, And so that's why I ended up using the text record. But you run into like web sockets, don't work on the watch, but even though the API is there, et cetera. So that's something I wanna research more, maybe look into long pooling, things like that.

[00:35:51] Leo Dion (host): if you're interested, you can check gBeat.com. The app and then Har Twitch. I'm in the middle of rebranding and changing the name because I'm not interested in which spells, which apparently is what people who tag Har Twitch on Reddit are interested in. I'm gonna be renaming it. Bitus. This is a prototype icon.

[00:36:12] Leo Dion (host): You can actually go there right now and sign up if you're interested in trying that out. gBeat, you can sign up right now and use it. here's a list of the repositories that I mentioned. and then that will be you in the slides and you can also check out the notes from my talk that I'm still working on, but you can at least look at it and see it once I push.

[00:36:34] Leo Dion (host): Thank you. That's it.

