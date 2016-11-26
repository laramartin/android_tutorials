# Android vibration
To make the phone vibrate from code, use this method: 

```java
((Vibrator)context.getSystemService(VIBRATOR_SERVICE)).vibrate(100);
```

and add the permission in the `AndroidManifest.xml`: 

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```
