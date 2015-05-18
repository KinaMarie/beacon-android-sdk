# beacon-android-sdk
Beacon library for adHawk implementations on Android.

This purpose of this library is to effeciently monitor and relay beacon information to apps wishing to integrate with the RevelDigital platform. Beacon information is delivered via broadcast to any listening ```BroadcastReceiver``` registered to an application. Once the libary is initialized the app will begin to receive various broadcasts when in the vicinity of a beacon registered on the RevelDigital platform.

# Dependencies

  * bson4jackson-2.4.0 ```compile 'de.undercouch:bson4jackson:2.4.0'```
  * gson2.3.1 ```compile 'com.google.code.gson:gson:2.3.1'```
  * okio-1.3.0 ```compile 'com.squareup.okio:okio:1.3.0'```
  * okhttp-urlconnection-2.3.0 ```compile 'com.squareup.okhttp:okhttp-urlconnection:2.3.0'```
  * okhttp-2.3.0 ```compile 'com.squareup.okhttp:okhttp:2.3.0'```
  * retrofit-1.9.0 ```compile 'com.squareup.retrofit:retrofit:1.9.0'```
  * support-v4-22.0.0 ```compile 'com.android.support:support-v4:22.1.1'```
  * joda-time-2.7 ```compile 'joda-time:joda-time:2.7'```
  * play-services-base-7.0.0 ```compile 'com.google.android.gms:play-services-base:7.0.0'```
  * play-services-maps-7.0.0 ```compile 'com.google.android.gms:play-services-maps:7.0.0'```
  * play-services-location-7.0.0 ```compile 'com.google.android.gms:play-services-location:7.0.0'```
  * reveldigital-api-1.4.4 ```compile 'com.reveldigital:reveldigital-api:1.4.4'```
  * playerapi-1.0-SNAPSHOT ```request access by emailing support@reveldigital.com```

# Usage

## To start

```
try {
  BeaconClient beaconClient = new BeaconClient.Builder(this).startOnBoot(true).build();
  beaconClient.connect();
} catch (BeaconClient.MalformedRevelBeaconException e) {
  e.printStackTrace();
}
```

## To stop

```
beaconClient.disconnect();
```

## How to receive beacon data

Register a ```BroadcastReceiver``` either manually or via an ```intent-filter``` in your ```AndroidManifest.xml```

An example of intent-filter:

```
<intent-filter>
  <action android:name="com.reveldigital.adhawk.lib.action.BEACON_FOUND"/>
</intent-filter>
<intent-filter>
  <action android:name="com.reveldigital.adhawk.lib.action.BEACON_EXPIRED"/>
</intent-filter>
```

Extract the beacon data from the broadcast intent:

```
if (intent.getAction().equals(ACTION_BEACON_FOUND)) {
  Bundle extras = intent.getExtras();
  Bundle b = extras.getBundle(EXTRA_BEACON_BUNDLE);
  b.setClassLoader(Beacon.class.getClassLoader()); // for cross process applications
  Beacon beacon = b.getParcelable(EXTRA_BEACON);
}
```

The two actions containing beacon data include
  * ```ACTION_BEACON_EXPIRED```
  * ```ACTION_BEACON_FOUND```
  
# Authorization

An authorization key is required in order to communicate with the RevelDigital API. This key is generated by RevelDigital and provided to app developers on a request basis.

The auth key must be included in your ```strings.xml``` resource file using the ```rd_beacon_sdk_auth_key``` name.

```
<string name="rd_beacon_sdk_auth_key">YOUR KEY HERE</string>
```

To request your key please email info@reveldigital.com with your company and contact details.
