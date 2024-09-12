# How does Server Side Swift Workout in the Real World?

## [Introduction](docs/01-introduction.md)
  * Brief overview of Heartwitch: The inspiration behind gBeat
  * Introduction to gBeat: A health and fitness live streaming app
## [The Power of Full Stack Swift](docs/02-full-stack-swift.md)
   * Unified Language Ecosystem
   * Type Safety Across the Stack
   * Performance Benefits
   * Apple's Ecosystem Integration
   * Growing Server-Side Swift Community
   * Simplified Deployment and Scaling
   * Developer Productivity
   * Future-Proofing
   * Case Study: gBeat's Experience with Full Stack Swift
## [The gBeat Ecosystem](docs/03-gbeat.md)
   * How the applications work
   * Integration with Apple Watch
   * User experience highlights
## [Technical Deep Dive: Full Stack Swift Implementation](docs/04-technology)
### [Authentication system with Sign in with Apple](docs/04-technology/01-authentication.md)
   * Implementation across iOS and watchOS
   * Challenge: Sign in with Apple not available on Apple Watch simulator
### [Development Server Auto-discovery](docs/04-technology/02-sublimation.md)   
   * The challenge of discovering local development servers in full stack development
   * Specific issues with Apple Watch development
   * Introduction to Sublimation: A Swift package solution
   * Ngrok integration (initial approach)
   * Bonjour implementation (ultimate solution)
   * Impact on gBeat development
### [Communication with Redis and WebSockets](docs/04-technology/03-websockets-redis.md)
### [Implementing Apple Push Notification System](docs/04-technology/04-apns.md)
### [Complementary iPhone App and WatchConnectivity](docs/04-technology/05-iPhone.md)
   * Developing a companion iPhone app for the AppStore
   * Implementing WatchConnectivity
   * Synchronizing workout data
   * Challenges and solutions
   * Impact on overall user experience
### [Web Development: Building the gBeat Website](docs/04-technology/05-web-development.md)
   * Choosing VueJS as the frontend framework
   * Implementing TypeScript for improved type safety
   * Utilizing Vite for build tooling
   * Webpack configuration for advanced scenarios
   * Challenges and solutions in creating a responsive fitness streaming interface
### [Challenges and Solutions](docs/05-challenges.md)
   * Android integration
   * Creating a White Label app - Issues with React Native and how Swift addressed them
   * Developing a Chrome Extension for YouTube Workouts
### [Deployment and CI/CD](docs/06-deployment.md)
   1. Deploying to Heroku
   2. Setting up Continuous Integration with GitLab and GitHub
### [Future Plans](docs/07-future.md)
   * OpenAPI Integration
   * React Native XCFramework/Node Module
   * Protobuf
   * Hack Watch Networking
   * Hosting Options
   * Other Apple Platforms
   * Other Watch Platforms
### Conclusion TBD
   1. Recap of key learnings
   2. Encouragement for attendees to explore Full Stack Swift for their projects
   

