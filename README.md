# How does Server Side Swift Workout in the Real World?
1. [Introduction](docs/01-introduction.md)
   1. Brief overview of Heartwitch: The inspiration behind gBeat
   2. Introduction to gBeat: A health and fitness live streaming app
2. [The Power of Full Stack Swift](docs/02-full-stack-swift.md)
   1. Unified Language Ecosystem
   2. Type Safety Across the Stack
   3. Performance Benefits
   4. Apple's Ecosystem Integration
   5. Growing Server-Side Swift Community
   6. Simplified Deployment and Scaling
   7. Developer Productivity
   8. Future-Proofing
   9. Case Study: gBeat's Experience with Full Stack Swift
3. [The gBeat Ecosystem](docs/03-gbeat.md)
   1. How the applications work
   2. Integration with Apple Watch
   3. User experience highlights
4. [Technical Deep Dive: Full Stack Swift Implementation](docs/04-technology)
   1. [Authentication system with Sign in with Apple](docs/04-technology/01-authentication.md)
	  1. Implementation across iOS and watchOS
	  2. Challenge: Sign in with Apple not available on Apple Watch simulator
		 1. Impact on development and testing process
		 2. Workarounds and solutions
   2. [Development Server Auto-discovery](docs/04-technology/02-sublimation.md)   
      1. The challenge of discovering local development servers in full stack development
      2. Specific issues with Apple Watch development
      3. Introduction to Sublimation: A Swift package solution
         1. Overview of Sublimation's purpose and functionality
         2. Server and Client components
      4. Ngrok integration (initial approach)
         1. Using Ngrok for public exposure of development servers
         2. How Sublimation automates the Ngrok process
         3. Cloud setup for meta-server access (using kvdb.io)
         4. Limitations and challenges encountered
      5. Bonjour implementation (ultimate solution)
         1. How Sublimation uses Bonjour for local network discovery
         2. Benefits of the Bonjour approach over Ngrok
         3. Implementation details and code examples
         4. How it solved auto-discovery challenges in gBeat
      6. Impact on gBeat development
         1. Performance and developer experience improvements
         2. Seamless testing across iOS and watchOS devices
   3. Communication with Redis and WebSockets
   4. Implementing Apple Push Notification System (APNs)
   7. Complementary iPhone App and WatchConnectivity
     1. Developing a companion iPhone app for the AppStore
        1. Shared codebase considerations
        2. UI/UX design for iPhone vs. Apple Watch
     2. Implementing WatchConnectivity
        1. Overview of WatchConnectivity framework
        2. Strategies for efficient data transfer between devices
        3. Handling different connectivity states
     3. Synchronizing workout data
        1. Real-time updates during workouts
        2. Handling offline scenarios and data reconciliation
     4. Challenges and solutions
        1. Dealing with unreliable connectivity
        2. Battery life considerations
        3. Ensuring consistent user experience across devices
     5. Impact on overall user experience
        1. Seamless transition between iPhone and Apple Watch
        2. Enhanced functionality through device cooperation
   5. Web Development: Building the gBeat Website
	  1. Choosing VueJS as the frontend framework
		 1. Advantages for a fitness streaming application
		 2. Integration with Swift backend
	  2. Implementing TypeScript for improved type safety
		 1. Benefits in a large-scale application
		 2. Synergies with Swift's strong typing
	  3. Utilizing Vite for build tooling
		 1. Fast development experience
		 2. Optimized production builds
	  4. Webpack configuration for advanced scenarios
	  5. Challenges and solutions in creating a responsive fitness streaming interface
5. Challenges and Solutions
   1. Android integration
	  1. Integrating Firebase Cloud Messaging (FCM) for cross-platform support
	  2. Implementing Sign in with Google
   2. Creating a White Label app
	  1. Issues with React Native and how Swift addressed them
   3. Developing a Chrome Extension for YouTube Workouts
	  1. Bridging the gap between gBeat and popular YouTube fitness content
	  2. Technical challenges in integrating with YouTube's player
	  3. User experience considerations for seamless workout tracking
	  4. How this extension complements the core gBeat application
6. Deployment and CI/CD
   1. Deploying to Heroku
   2. Setting up Continuous Integration with GitLab and GitHub
7. Future Plans
   1. Upcoming features or improvements      
      5. OpenAPI Integration
        1. Generating API documentation
        2. Ensuring consistency between API specification and implementation
        3. Benefits for frontend-backend communication and third-party integrations
   2. Potential expansions of the gBeat platform
8. Conclusion
   1. Recap of key learnings
   2. Encouragement for attendees to explore Full Stack Swift for their projects