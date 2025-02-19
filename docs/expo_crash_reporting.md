# Expo Crash Reporting and Error Tracking

## Overview

Enable Expo Crash Reporting and Error Tracking to get comprehensive crash reports and error trends with Real User Monitoring.

With this feature, you can access the following features:

-   Aggregated Expo crash dashboards and attributes
-   Symbolicated iOS and deobfuscated Android crash reports
-   Trend analysis with Expo error tracking

In order to symbolicate your stack traces and deobfuscate Android crashes, upload your .dSYM, Proguard mapping files and source maps to Datadog using the `expo-datadog` config plugin.

Your crash reports appear in [**Error Tracking**][1].

## Setup

Use the [`expo-datadog` package and configuration plugin][2]. For more information, see the [Expo and Expo Go documentation][3].

Add `@datadog/datadog-ci` as a development dependency. This package contains scripts to upload the source maps. You can install it with NPM:

```sh
npm install @datadog/datadog-ci --save-dev
```

or with Yarn:

```sh
yarn add -D @datadog/datadog-ci
```

Run `eas secret:create` to set `DATADOG_API_KEY` to your Datadog API key.

### Disable file uploads

You can disable some files from uploading by setting the `iosDsyms`, `iosSourcemaps`, `androidProguardMappingFiles`, or `androidSourcemaps` parameters to `false`.

```json
{
    "expo": {
        "plugins": [
            [
                "expo-datadog",
                {
                    "iosDsyms": false
                }
            ]
        ]
    }
}
```

### Setting the Datadog site

Run `eas secret:create` to set `DATADOG_SITE` to the host of your Datadog site, for example: `datadoghq.eu`. By default, `datadoghq.com` is used.

### Plugin configuration options

| Parameter                     | Default | Description                                                                                                                        |
| ----------------------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `iosDsyms`                    | `true`  | Enables the uploading of dSYMS files for the symbolication of native iOS crashes.                                                  |
| `iosSourcemaps`               | `true`  | Enables the uploading of JavaScript source maps on iOS builds.                                                                     |
| `androidProguardMappingFiles` | `true`  | Enables the uploading of Proguard mapping files to deobfuscate native Android crashes (is only applied if obfuscation is enabled). |
| `androidSourcemaps`           | `true`  | Enables the uploading of JavaScript source maps on Android builds.                                                                 |

## Further reading

{{< partial name="whats-next/whats-next.html" >}}

[1]: https://app.datadoghq.com/rum/error-tracking
[2]: https://github.com/DataDog/dd-sdk-reactnative/blob/develop/expo/README.md
[3]: https://docs.datadoghq.com/real_user_monitoring/reactnative/expo/#usage
