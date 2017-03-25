# Implement a custom ToolBar

First change your AppTheme to a NoActionBar theme in `styles.xml`: 

```
<style name="AppTheme" parent="@style/Theme.AppCompat.Light.NoActionBar">
        <item name="colorPrimary">@color/primary</item>
        <item name="colorPrimaryDark">@color/primary_dark</item>
        <item name="android:colorAccent">@color/accent</item>
        <item name="android:toolbarStyle">@style/ToolBar</item>
</style>
``` 
AppTheme now contains a reference to a custom ToolBar style: 
```
<style name="ToolBar" parent="Widget.AppCompat.Toolbar">
        <item name="android:background">@color/primary</item>
        <item name="android:popupTheme">@style/Platform.AppCompat.Light</item>
</style>
``` 

Then add the ToolBar in the desired layout: 
```
<android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        style="@style/ToolBar" />
```

Finally, register it as the ActionBar in `onCreate` method of our Activity after setContentView: 
```
Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
setSupportActionBar(toolbar);
``` 
        
