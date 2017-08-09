---
# You don't need to edit this file, it's empty on purpose.
# Edit theme's home layout instead if you wanna make some changes
# See: https://jekyllrb.com/docs/themes/#overriding-theme-defaults
layout: home
---

*version 1.0.0*
### Introduction
An Android library providing APIs to interact with EbizuPublisher 

feature on this SDK:

 * Tracking user location based on user activity like (Walking, On Bicycle,In Vehicle, etc) on background mode
 
 see javadoc

### Installation

1. add ebizu maven repository on build.gradle (app level)

    ```
    repositories {
            maven {
                url "http://dl.bintray.com/ebizu/maven"
            }
        }
     ```
2. add ebizu publisher sdk into dependency on build.gradle (app level)
    ```
    compile 'com.ebizu.sdk:ebizupublishersdk:1.0.0'
    ```

3. Put realm plugin into dependencies section on build.gradle (project level)
    ```
    classpath "io.realm:realm-gradle-plugin:3.5.0"
    ```

### Setup EbizuPublisher Application ID

for setup application id you can put on Android Manifest, it is done as follows
    
    <meta-data
         android:name="com.ebizu.APPLICATION_ID"
         android:value="<YOUR APPLICATION ID>" />
### Initializing EbizuPublisher

Before you can use EbizuPublisher in your app, you must initialize it first. It is done as follows

    EbizuPublisher.getInstance().init(this, false, Config.PRODUCTION);

The initialization is done once, and you must provide an Android context, debug mode, and SDK environment. A good place to initialize EbizuPublisher is in onCreate() on an application subclass:

     @Override
        public void onCreate() {
            super.onCreate();
            EbizuPublisher.getInstance().init(this, false, Config.PRODUCTION);
        }

### Start EbizuPublisher
To start EbizuPublisher you must call start method. it is done as follows 
    
     EbizuPublisher.getInstance().start(this);
     
EbizuPublisher start is done once, and you must provide an Android context and also before you start EbizuPublisher, in your app must grant Location Permission, 

Example :
   
    private void checkPermissionForLocation() {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
                if ((ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED) && (ActivityCompat.checkSelfPermission(this, Manifest.permission.ACCESS_COARSE_LOCATION) != PackageManager.PERMISSION_GRANTED)) {
                    requestPermissions(new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, LOCATION_PERMISSIONS_REQUEST);
                } else {
                    EbizuPublisher.getInstance().start(this);
                }
            } else {
                EbizuPublisher.getInstance().start(this);
            }
        }
     
     @Override
         public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
             if (requestCode == LOCATION_PERMISSIONS_REQUEST) {
                 if (grantResults.length == 1 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                     EbizuPublisher.getInstance().start(this);
                 } else {
                     Toast.makeText(MainActivity.this, "Location permission denied", Toast.LENGTH_SHORT).show();
                 }
             }
         }

### Login EbizuPublisher
To login user app into EbizuPublisher, you can using following method
    
    EbizuPublisher.getInstance().login(ebizuUser);
    
this method purpose for tracking location log can related to user, if you did not call this method tracking location log only related to device.

Example : 
    
    EbizuUser ebizuUser = new EbizuUser();
    ebizuUser.setCity("Bandung");
    ebizuUser.setCountry("Indonesia");
    ebizuUser.setDateOfBirth("1990-01-01"); // format should YYYY-MM-DD
    ebizuUser.setEmail("email@mail.com");
    ebizuUser.setGender("Male");
    ebizuUser.setName("User Full Name");
    EbizuPublisher.getInstance().login(ebizuUser);
    
A good place to login to EbizuPublisher is before calling start

### Dependency

 * [Retrofit 2.1.0](http://square.github.io/retrofit/) used as HTTP Client for consume API call
 * [Realm 3.5.0](https://realm.io/docs/java/latest//) used as local database
 * [Play Service Location 10.2.6](https://developers.google.com/android/guides/setup) used for ActivityRecognized and FusedLocation

