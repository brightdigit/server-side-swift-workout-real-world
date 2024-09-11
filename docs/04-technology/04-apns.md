# Implementing Apple Push Notification System (APNs)

didRegisterForRemoteNotifications

```
	  mobileDevice = CreateMobileDeviceRequestContent(
	model: AppInterfaceType.currentDevice.name,
	operatingSystem: AppInterfaceType.currentDevice.systemVersion,
	topic: topic,
	deviceToken: deviceToken
  )
```

// primary id of MobileDeviceRegistration
   @AppStorage("MobileDeviceRegistrationID") private var mobileDeviceRegistrationID: String?
   
```

CREATE TABLE "MobileDevices" (
	id uuid PRIMARY KEY,
	"userID" uuid NOT NULL REFERENCES "User"(id) ON DELETE CASCADE ON UPDATE CASCADE,
	model text NOT NULL,
	"operatingSystem" text NOT NULL,
	topic text NOT NULL,
	"deviceToken" bytea NOT NULL UNIQUE,
	"createdAt" timestamp with time zone,
	"updatedAt" timestamp with time zone
);

```