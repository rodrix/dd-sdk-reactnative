# React-Native Monitoring


<div class="alert alert-warning">
This feature is in open beta. Email <a href="mailto:support@datadoghq.com">support@datadoghq.com</a> to ask questions or to provide feedback on this feature.
</div>

Datadog *Real User Monitoring (RUM)* enables you to visualize and analyze the real-time performance and user journeys of your application’s individual users.

## Setup

#### NPM Installation

```sh
npm install dd-sdk-reactnative
```

#### Yarn Installation

```sh
yarn add dd-sdk-reactnative
```

### Specify application details in UI

1. Select UX Monitoring -> RUM Applications -> New Application
2. Choose `react-native` as your Application Type in [Datadog UI][1] and provide a new application name to generate a unique Datadog application ID and client token.

![image][2]

To ensure the safety of your data, you must use a client token. You cannot use only [Datadog API keys][3] to configure the `dd-sdk-reactnative` library, as they would be exposed client-side. For more information about setting up a client token, see the [Client Token documentation][4].

### Initialize the library with application context

```js
import { DdSdkReactNative, DdSdkReactNativeConfiguration } from 'dd-sdk-reactnative';


const config = new DdSdkReactNativeConfiguration(
    "<CLIENT_TOKEN>", 
    "<ENVIRONMENT_NAME>", 
    "<RUM_APPLICATION_ID>",
    true, // track User interactions (e.g.: Tap on buttons)
    true, // track XHR Resources
    true // track Errors
)
// Optional: Select your Datadog website (one of "US", "EU" or "GOV")
config.site = "US"
// Optional: enable or disable native crash reports
config.nativeCrashReportEnabled = true
// Optional: sample RUM sessions (here, 80% of session will be sent to Datadog. Default = 100%)
config.sampleRate = 80

DdSdkReactNative.initialize(config)
```

## Track View Navigation

**Note**: Automatic View tracking is relying on the [React Navigation](https://reactnavigation.org/) package. If you're using another package to handle navigation in your application, use the manual instrumentation described below.

To track changes in navigation as RUM Views, you need to set the `onready` callback of your `NavigationContainer` component, as follow:

```js
import * as React from 'react';
import { DdRumReactNavigationTracking } from 'dd-sdk-reactnative';

function App() {
  const navigationRef = React.useRef(null);
  return (
    <View>
      <NavigationContainer ref={navigationRef} onReady={() => {
        DdRumReactNavigationTracking.startTrackingViews(navigationRef.current)
      }}>
        // …
      </NavigationContainer>
    </View>
  );
}
```
**Note**: Only one `NavigationContainer` can be tracked at the time. If you need to track another container, stop tracking previous one first.

## Track Custom Attributes

You can attach user information to all RUM events to get more detailed information from your RUM sessions. 

### User information

For user specific information, you can use the following code wherever you want in your code (after the SDK has been initialized). The `id`, `name` and `email` attributes are built into the Datadog UI, but you can add any attribute that makes sense to your app.

```js
DdSdkReactNative.setUser({
    id: "1337", 
    name: "John Smith", 
    email: "john@example.com", 
    type: "premium"
})
```

### Global attributes

You can also keep global attributes to track information about a specific session, such as A/B testing configuration, advert campaign origin, or cart status.

```js
DdSdkReactNative.setAttributes({
    profile_mode: "wall",
    chat_enabled: true,
    campaign_origin: "example_ad_network"
})
```

## Manual Instrumentation

If automatic instrumentation doesn't suit your needs, you can manually create RUM Events and Logs as follow:

```js
import { DdSdkReactNative, DdSdkReactNativeConfiguration, DdLogs, DdRum } from 'dd-sdk-reactnative';

// Initialize the SDK
const config = new DdSdkReactNativeConfiguration(
    "<CLIENT_TOKEN>",
    "<ENVIRONMENT_NAME>",
    "<RUM_APPLICATION_ID>",
    true, // track User interactions (e.g.: Tap on buttons)
    true, // track XHR Resources
    true // track Errors
)
DdSdkReactNative.initialize(config);

// Send logs (use the debug, info, warn of error methods)
DdLogs.debug("Lorem ipsum dolor sit amet…", 0, {});
DdLogs.info("Lorem ipsum dolor sit amet…", 0, {});
DdLogs.warn("Lorem ipsum dolor sit amet…", 0, {});
DdLogs.error("Lorem ipsum dolor sit amet…", 0, {});

// Track RUM Views manually
DdRum.startView('<view-key>', 'View Url', new Date().getTime(), {});
//…
DdRum.stopView('<view-key>', new Date().getTime(), { custom: 42});

// Track RUM Actions manually
DdRum.addAction('TAP', 'button name', new Date().getTime(), {});

// Track RUM Errors manually
DdRum.addError('<message>', 'source', '<stacktrace>', new Date().getTime(), {});


// Track RUM Resource manually
DdRum.startResource('<res-key>', 'GET', 'http://www.example.com/api/v1/test', new Date().getTime(), {} );
//…
DdRum.stopResource('<res-key>', 200, 'xhr', new Date().getTime(), {});
```

[1]: https://app.datadoghq.com/rum/application/create
[2]: https://raw.githubusercontent.com/DataDog/dd-sdk-reactnative/main/docs/image_reactnative.png
[3]: https://docs.datadoghq.com/account_management/api-app-keys/#api-keys
[4]: https://docs.datadoghq.com/account_management/api-app-keys/#client-tokens
## License

[Apache License, v2.0](LICENSE)

## Further Reading

{{< partial name="whats-next/whats-next.html" >}}
