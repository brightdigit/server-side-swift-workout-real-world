# Authentication with no UI

- It's difficult to enter information on the Apple Watch - let alone username and password
- How can we avoid the user having to enter anything
- Heartwitch solution was to link their account via iCloud
- gBeat used Sign in with Apple 

## Implementation across iOS and watchOS

- gBeat uses Sign in with Apple for the user to authenticate via iPhone and Apple Watch

## Challenge: Sign in with Apple not available on Apple Watch simulator

- Developing for the Apple Watch can be a challenge `how is Apple Watch a challenge`
- Using the Simulator makes it even easier
- Simulator can fake HealthKit stats be not Sign In With Apple `what happens`
- How can we fake **sign in with apple**
	 
### Impact on development and testing process 

`link to simulator services and code snippet`

- On development server save the token as a file on the Mac and copy it to the simulator directly
- Login via web site
- Simulator then watches for the file and _logs in_

### Workarounds and solutions 