---
nav_title: Creating Feature Flags
article_title: Creating Feature Flags
page_order: 20
description: "This reference article covers how to create feature flags to coordinate new feature rollouts."
tool: Feature Flags
platform:
  - iOS
  - Android
  - Web

---

# Creating feature flags

> Feature flags allow you to remotely enable or disable functionality for a selection of users. Create a new feature flag within the Braze dashboard. Provide a name and an `ID`, a target audience, and a percentage of users for whom to enable to this feature. Then, using that same `ID` in your app or website's code, you can conditionally run certain parts of your business logic.

Looking to learn more about what feature flags are and how you can use them in Braze? Check out [About feature flags][5] before proceeding.

## Prerequisites

To use feature flags, ensure your SDKs are up to date with at least these minimum versions:

{% sdk_min_versions swift:5.9.0 android:24.2.0 web:4.6.0 unity:4.1.0 cordova:5.0.0 reactnative:4.1.0 flutter:6.0.0 roku:1.0.0 %}

{% alert important %} 
Feature flags are currently in beta. [Click here](https://dashboard.braze.com/engagement/feature_flags) to learn more about joining the beta program.
{% endalert %}

## Implement feature flags in the dashboard

Create, edit, and archive feature flags from **Messaging** > **Feature Flags**. This page displays a list of existing feature flags for this workspace.

{% alert note %}
If you are using the [older navigation]({{site.baseurl}}/navigation), you can find **Feature Flags** under **Engagement**.
{% endalert %}

![A list of previously created feature flags on the Braze dashboard][1]{: style="max-width:75%"}

### Create a new feature flag

To create a new feature flag, click the **Create Feature Flag** button. Then, define your feature flag's [details](#details), [properties](#properties), user [targeting](#targeting), and [rollout traffic](#rollout-traffic).

![A blank feature flag form][2]{: style="float:right;max-width:55%;margin-left:15px;"}

#### Details

Give your new feature flag a **Name** and **ID**.

* The **Name** field allows you to provide a human-readable title for this feature flag that will be used by marketers and administrators.
* The **ID** field will be referenced in your code to determine whether the feature is enabled for a particular user. This must be unique and cannot be modified once created.
* The **Description** field is an optional field that allows you to provide additional context around this feature flag.

Choose an `ID` thoughtfully as it will be used as you develop your feature. Practice good naming conventions to ensure that your code is readable by your colleagues (and your future self).

For example, it's common to use a naming convention of `{verb}_{product}_{feature}`, such as `enable_rider_new_profile_page` to make it clear what enabling the feature flag does.

{% alert important %} 
To prevent breaking production app behavior, feature flag `ID`s must be unique and cannot be modified once created. 

Feature flags are shared across apps within a workspace so that different platforms (iOS/Android/Web) can share references to the same feature.
{% endalert %}

#### Properties {#properties}

Custom properties can be defined as part of your feature flag. These properties will be accessible by your app through the Braze SDK when the feature is enabled. Defining properties is an optional step.

Variables can be **strings**, **boolean** values, or **numbers**. Define both the variable key and default value for each property.

##### Example properties

For example, if we are defining a feature flag that shows an out-of-stock banner for our ecommerce store, we might set the following properties, which our app will use when displaying the banner:

|Property Name|Type|Value|
|--|--|--|
|`banner_height`|`number`|`75`|
|`banner_color`|`string`|`blue`|
|`banner_text`|`string`|`Widgets are out of stock until July 1.`|
|`dismissible`|`boolean`|`false`|

{% alert tip %}
There is no limit to the number of properties you can add, though a feature flag's properties are limited to 10kB in total.
{% endalert %}

#### Targeting

To begin the rollout of a feature flag, you must choose a particular [segment]({{site.baseurl}}/user_guide/engagement_tools/segments/) of users.

Use the **Add Filter** dropdown menu to filter users out of your target audience. Add multiple filters to narrow your audience.

![Two dropdown menus. The first reads Target Users by Segment. The second reads Additional Filters.][3]

#### Rollout traffic

Feature flags always start as turned off to allow you to separate the timing of the feature's release and activation in your users' experience. 

When you are ready to rollout your new feature, specify an audience and then use the **Rollout Traffic** slider to define the random percentage of your targeted user base to receive the new feature. Set the **Rollout Traffic** slider to set a percentage between 0% (no users) and 100% (the entire target audience). 

![A slider labeled Rollout Traffic, spanning between 0 and 100.][4]

{% alert tip %}
Do not set your rollout traffic above 0% until you are ready for your new feature to go live. When you initially define your feature flag in the dashboard, leave this setting at 0%.
{% endalert %}

## Implement the feature flag in your application

Once you have defined your feature flag, configure your app or site to check whether or not it is enabled for a particular user. When it is enabled, you'll set some action or reference the feature flag's variable properties based on your use case. The Braze SDK provides getter methods to pull your feature flag's status and its properties into your app. 

Feature flags are refreshed automatically at session start so that you can display the most up-to-date version of your feature upon launch. The SDK caches these values so they can be used while offline. 

Let's say you were to rolling out a new type of user profile for your app. You might set the `ID` as `expanded_user_profile`. Then, you would have your app check to see if it should display this new user profile to a particular user. For example:

{% tabs %}
{% tab Javascript %}

```javascript
const featureFlag = braze.getFeatureFlag("expanded_user_profile");
if (featureFlag.enabled) {
  console.log(`expanded_user_profile is enabled`);
} else {
  console.log(`expanded_user_profile is not enabled`);
}
```

{% endtab %}
{% tab Swift %}

```swift
let featureFlag = braze.featureFlags.featureFlag(id: "expanded_user_profile")
if featureFlag.enabled {
  print("expanded_user_profile is enabled")
} else {
  print("expanded_user_profile is not enabled")
}
```
{% endtab %}
{% tab Java %}
```java
FeatureFlag featureFlag = braze.getFeatureFlag("expanded_user_profile");
if (featureFlag.getEnabled()) {
  Log.i(TAG, "expanded_user_profile is enabled");
} else {
  Log.i(TAG, "expanded_user_profile is not enabled");
}
```

{% endtab %}
{% tab Kotlin %}

```kotlin
val featureFlag = braze.getFeatureFlag("expanded_user_profile")
if (featureFlag.enabled) {
  Log.i(TAG, "expanded_user_profile is enabled.")
} else {
  Log.i(TAG, "expanded_user_profile is not enabled.")
}
```

{% endtab %}
{% tab React Native %}

```javascript
const featureFlag = await Braze.getFeatureFlag("expanded_user_profile");
if (featureFlag.enabled) {
  console.log(`expanded_user_profile is enabled`);
} else {
  console.log(`expanded_user_profile is not enabled`);
}
```

{% endtab %}
{% tab Unity %}
```csharp
var featureFlag = Appboy.AppboyBinding.GetFeatureFlag("expanded_user_profile");
if (featureFlag.Enabled) {
  Console.WriteLine("expanded_user_profile is enabled");
} else {
  Console.WriteLine("expanded_user_profile is not enabled");
}
```
{% endtab %}

{% tab Cordova %}
```javascript
const featureFlag = await BrazePlugin.getFeatureFlag("expanded_user_profile");
if (featureFlag.enabled) {
  console.log(`expanded_user_profile is enabled`);  
} else {
  console.log(`expanded_user_profile is not enabled`);
}
```
{% endtab %}
{% tab Flutter %}
```dart
BrazeFeatureFlag featureFlag = await braze.getFeatureFlagByID("expanded_user_profile");
if (featureFlag.enabled) {
  print("expanded_user_profile is enabled");
} else {
  print("expanded_user_profile is not enabled");
}
```
{% endtab %}

{% tab Roku %}
```brightscript
featureFlag = m.braze.getFeatureFlag("expanded_user_profile")
if featureFlag.enabled
  print "expanded_user_profile is enabled"
else
  print "expanded_user_profile is not enabled"
end if
```
{% endtab %}
{% endtabs %}

### Accessing properties {#accessing-properties}

To access the properties of a feature flag, use one of the following methods depending on the type you defined in the dashboard.

If a feature flag is not enabled, or a property you reference does not exist, these methods will return `null`.

{% tabs %}
{% tab Javascript %}

```javascript
// Feature flag instance
const featureFlag = braze.getFeatureFlag("expanded_user_profile");
// String properties
const stringProperty = featureFlag.getStringProperty("color");
// Boolean properties
const booleanProperty = featureFlag.getBooleanProperty("expanded");
// Number properties
const numberProperty = featureFlag.getNumberProperty("height");
```

{% endtab %}
{% tab Swift %}

```swift
// Feature flag instance
let featureFlag: FeatureFlag = braze.featureFlags.featureFlag(id: "expanded_user_profile")
// String properties
let stringProperty: String? = featureFlag.stringProperty(key: "color")
// Boolean properties
let booleanProperty: Bool? = featureFlag.boolProperty(key: "expanded")
// Number properties
let numberProperty: Double? = featureFlag.numberProperty(key: "height")
```

{% endtab %}
{% tab Java %}

```java
// Feature flag instance
FeatureFlag featureFlag = braze.getFeatureFlag("expanded_user_profile");
// String properties
String stringProperty = featureFlag.getStringProperty("color");
// Boolean properties
Boolean booleanProperty = featureFlag.getBooleanProperty("expanded");
// Number properties
Number numberProperty = featureFlag.getNumberProperty("height");
```

{% endtab %}
{% tab Kotlin %}

```kotlin
// feature flag instance
val featureFlag = braze.getFeatureFlag("expanded_user_profile")
// string properties
val stringProperty = featureFlag.getStringProperty("color")
// boolean properties
val booleanProperty = featureFlag.getBooleanProperty("expanded")
// number properties
val numberProperty = featureFlag.getNumberProperty("height")
```

{% endtab %}
{% tab React Native %}

```javascript
// String properties
const stringProperty = await Braze.getFeatureFlagStringProperty("expanded_user_profile", "color");
// Boolean properties
const booleanProperty = await Braze.getFeatureFlagBooleanProperty("expanded_user_profile", "expanded");
// Number properties
const numberProperty = await Braze.getFeatureFlagNumberProperty("expanded_user_profile", "height");
```

{% endtab %}
{% tab Unity %}

```csharp
// Feature flag instance
var featureFlag = Appboy.AppboyBinding.GetFeatureFlag("expanded_user_profile");
// String properties
var stringProperty = featureFlag.getStringProperty("color");
// Boolean properties
var booleanProperty = featureFlag.getBooleanProperty("expanded");
// Number property as integer
var integerProperty = featureFlag.getIntegerProperty("height");
// Number property as double
var doubleProperty = featureFlag.getDoubleProperty("height");
```
{% endtab %}
{% tab Cordova %}

```javascript
// String properties
const stringProperty = await BrazePlugin.getFeatureFlagStringProperty("expanded_user_profile", "color");
// Boolean properties
const booleanProperty = await BrazePlugin.getFeatureFlagBooleanProperty("expanded_user_profile", "expanded");
// Number properties
const numberProperty = await BrazePlugin.getFeatureFlagNumberProperty("expanded_user_profile", "height");
```
{% endtab %}
{% tab Flutter %}
```dart
BrazeFeatureFlag featureFlag = await braze.getFeatureFlagByID("expanded_user_profile");
// String properties
var stringProperty = featureFlag.getStringProperty("color");
// Boolean properties
var booleanProperty = featureFlag.getBooleanProperty("expanded");
// Number properties
var numberProperty = featureFlag.getNumberProperty("height");
```
{% endtab %}
{% tab Roku %}
```brightscript
' String properties
color = featureFlag.getStringProperty("color")
' Boolean properties
expanded = featureFlag.getBooleanProperty("expanded")
' Number properties
height = featureFlag.getNumberProperty("height")
```
{% endtab %}
{% endtabs %}

You can also get a list of all enabled feature flags:

{% tabs %}
{% tab Javascript %}

```javascript
const features = getAllFeatureFlags();
for(const feature of features) {
  console.log(`Feature: ${feature.id}`, feature.enabled);
}
```

{% endtab %}
{% tab Swift %}

```swift
let features = braze.featureFlags.featureFlags
for let feature in features {
  print("Feature: \(feature.id)", feature.enabled)
}
```

{% endtab %}
{% tab Java %}

```java
List<FeatureFlag> features = braze.getAllFeatureFlags();
for (FeatureFlag feature: features) {
  Log.i(TAG, "Feature: ", feature.getId(), feature.getEnabled());
}
```

{% endtab %}
{% tab Kotlin %}

```kotlin
val featureFlags = braze.getAllFeatureFlags()
featureFlags.forEach { feature ->
  Log.i(TAG, "Feature: ${feature.id} ${feature.enabled}")
}
```

{% endtab %}
{% tab React Native %}

```javascript
const features = await Braze.getAllFeatureFlags();
for(const feature of features) {
  console.log(`Feature: ${feature.id}`, feature.enabled);
}
```

{% endtab %}
{% tab Unity %}

```csharp
List<FeatureFlag> features = Appboy.AppboyBinding.GetAllFeatureFlags();
foreach (FeatureFlag feature in features) {
  Console.WriteLine("Feature: {0} - enabled: {1}", feature.ID, feature.Enabled);
}
```

{% endtab %}
{% tab Cordova %}
```javascript
const features = await BrazePlugin.getAllFeatureFlags();
for(const feature of features) {
  console.log(`Feature: ${feature.id}`, feature.enabled);
}
```
{% endtab %}
{% tab Flutter %}
```dart
List<BrazeFeatureFlag> featureFlags = await braze.getAllFeatureFlags();
featureFlags.forEach((feature) {
  print("Feature: ${feature.id} ${feature.enabled}");
});
```
{% endtab %}
{% tab Roku %}
```brightscript
features = m.braze.getAllFeatureFlags()
for each feature in features
      print "Feature: " + feature.id + " enabled: " + feature.enabled.toStr()
end for
```
{% endtab %}
{% endtabs %}

### Refresh feature flags {#refreshing}

You can refresh the current user's feature flags mid-session to pull the latest values from Braze.

{% alert tip %}
Refreshing happens automatically upon session start. Refreshing is only needed prior to important user actions, such as before loading a checkout page, or if you know a feature flag will be referenced.
{% endalert %}

{% tabs %}
{% tab Javascript %}

```javascript
braze.refreshFeatureFlags(() => {
  console.log(`Feature flags have been refreshed.`);
}, () => {
  console.log(`Failed to refresh feature flags.`);
});
```

{% endtab %}
{% tab Swift %}

```swift
braze.featureFlags.requestRefresh { result in
  switch result {
  case .success(let features):
    print("Feature flags have been refreshed:", features)
  case .failure(let error):
    print("Failed to refresh feature flags:", error)
  }
}
```

{% endtab %}
{% tab Java %}

```java
braze.refreshFeatureFlags();
```

{% endtab %}
{% tab Kotlin %}

```kotlin
braze.refreshFeatureFlags()
```

{% endtab %}
{% tab React Native %}

```javascript
Braze.refreshFeatureFlags();
```

{% endtab %}
{% tab Unity %}

```csharp
Appboy.AppboyBinding.RefreshFeatureFlags();
```

{% endtab %}
{% tab Cordova %}
```javascript
BrazePlugin.refreshFeatureFlags();
```
{% endtab %}
{% tab Flutter %}
```dart
braze.refreshFeatureFlags();
```
{% endtab %}
{% tab Roku %}
```brightscript
m.Braze.refreshFeatureFlags()
```
{% endtab %}
{% endtabs %}


### Listen for changes {#updates}

You can configure the Braze SDK to listen and update your app when feature flags have been refreshed.

This is useful if you want to update your app if a user is no longer eligible for a feature. For example, setting some state in your app based on whether or not a feature is enabled, or one of its property values.

{% tabs %}
{% tab Javascript %}

```javascript
// Register an event listener
const subscriptionId = braze.subscribeToFeatureFlagsUpdates((features) => {
  console.log(`Features were updated`, features);
});
// Unregister this event listener
braze.removeSubscription(subscriptionId);
```

{% endtab %}
{% tab Swift %}

```swift
// Create the feature flags subscription
// - You must keep a strong reference to the subscription to keep it active
let subscription = braze.featureFlags.subscribeToUpdates { features in
  print("Feature flags were updated:", features)
}
// Cancel the subscription
subscription.cancel()
```

{% endtab %}
{% tab Java %}

```java
braze.subscribeToFeatureFlagsUpdates(event -> {
  Log.i(TAG, "Feature flags were updated.");
  for (FeatureFlag feature: event.getFeatureFlags()) {
    Log.i(TAG, "Feature: ", feature.getId(), feature.getEnabled());
  }
});
```

{% endtab %}
{% tab Kotlin %}

```kotlin
braze.subscribeToFeatureFlagsUpdates() { event ->
  Log.i(TAG, "Feature flags were updated.")
  event.featureFlags.forEach { feature ->
    Log.i(TAG, "Feature: ${feature.id}")
  }
}
```

{% endtab %}
{% tab React Native %}

```javascript
// Register an event listener
Braze.addListener(braze.Events.FEATURE_FLAGS_UPDATED, (featureFlags) => {
  console.log(`featureFlagUpdates`, JSON.stringify(featureFlags));
});
```

{% endtab %}
{% tab Unity %}

To listen for changes, set the values for **Game Object Name** and **Callback Method Name** under **Braze Configuration** > **Feature Flags** to the corresponding values in your application.

{% endtab %}
{% tab Cordova %}
```javascript
// Register an event listener
BrazePlugin.subscribeToFeatureFlagUpdates((featureFlags) => {
    console.log(`featureFlagUpdates`, JSON.stringify(featureFlags));
});
```
{% endtab %}
{% tab Flutter %}
```dart
// Create stream subscription
StreamSubscription featureFlagsStreamSubscription;

featureFlagsStreamSubscription = braze.subscribeToFeatureFlags((featureFlags) {
  print("Feature flags were updated");
});

// Cancel stream subscription
featureFlagsStreamSubscription.cancel();
```
{% endtab %}
{% tab Roku %}
```brightscript
' Define a function called `onFeatureFlagChanges` to be called when feature flags are refreshed
m.BrazeTask.ObserveField("BrazeFeatureFlags", "onFeatureFlagChanges")
```
{% endtab %}
{% endtabs %}

## Best practices

### Naming conventions

- Consider following a pattern such as `{product}.{feature}.{action}`. 
  - For example, in a ride sharing app your feature ID may be `driver.profile.show_animation_v3`
- This also helps when searching for a specific product area or team's feature flags.
- Make sure that the default state for a feature flag is disabled in your app.
  - For example, it is an anti-pattern if you have a flag named `disable_feature_xyz`. There may be exceptions, but try to avoid confusing a feature's "enabled" status with the actual enabled behavior (disabling feature xyz).

### Planning ahead

Always play it safe. When considering new features that may require a kill-switch, it's better to release new code with a feature flag and not need it than it is to realize a new app update is required.

### Be descriptive

Add a description to your feature flag. While this is an optional field in Braze, it can help answer questions others may have when browsing available feature flags.

- Contact details for who is responsible for the enablement and behavior of this flag
- When this flag should be disable
- Links to documentation or notes about the new feature this flag controls
- Any dependencies or notes on how to use the feature

### Clean up old feature flags

We're all guilty of leaving features on at 100% rollout for longer than necessary.

To help keep your code (and Braze dashboard) clean, remove permanent feature flags from your code base once all users have upgraded and you no longer need the option to disable the feature. This helps reduce the complexity of your development environment, but also keeps your list of feature flags tidy.

[1]: {% image_buster /assets/img/feature_flags/feature-flags-list.png %} 
[2]: {% image_buster /assets/img/feature_flags/feature-flags-create.png %}
[3]: {% image_buster /assets/img/feature_flags/feature-flags-targeting.png %}
[4]: {% image_buster /assets/img/feature_flags/feature-flags-rollout.png %}
[5]: {{site.baseurl}}/developer_guide/platform_wide/feature_flags/about/
