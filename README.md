Runtime Permission
===================

[![CircleCI](https://circleci.com/gh/florent37/RuntimePermission/tree/master.svg?style=svg)](https://circleci.com/gh/florent37/RuntimePermission/tree/master)
[![Language](https://img.shields.io/badge/compatible-java%20%7C%20kotlin%20%7C%20rx-brightgreen.svg)](https://www.github.com/florent37/RuntimePermission)

[![screen](https://raw.githubusercontent.com/florent37/RuntimePermission/master/medias/intro.png)](https://www.github.com/florent37/RuntimePermission)

Simpliest way to ask runtime permissions on Android, choose your way : 
- [Kotlin](https://github.com/florent37/RuntimePermission#kotlin)
- [Kotlin with Coroutines](https://github.com/florent37/RuntimePermission#kotlin-coroutines)
- [RxJava](https://github.com/florent37/RuntimePermission#rxjava)
- [Java8](https://github.com/florent37/RuntimePermission#java8)
- [Java7](https://github.com/florent37/RuntimePermission#java7)

**No need to override Activity or Fragment**`onPermissionResult(code, permissions, result)`**using this library, you just have to executue RuntimePermission's methods** 
This will not cut your code flow

# General Usage (cross language)

[ ![Download](https://api.bintray.com/packages/florent37/maven/runtime-permission/images/download.svg) ](https://bintray.com/florent37/maven/runtime-permission/)
```java
dependencies {
    implementation 'com.github.florent37:runtime-permission:(lastest version)'
}
```


## Detect Permissions

RuntimePermission can automatically check **all** of your needed permissions

For example, if you add to your *AndroidManifest.xml* :

[![screen](https://raw.githubusercontent.com/florent37/RuntimePermission/master/medias/manifest-permissions.png)](https://www.github.com/florent37/RuntimePermission)

You can use `askPermission` without specifying any permission

For example, in Kotlin: 
```
askPermission(){
   //all of your permissions have been accepted by the user
}.onDeclined { e -> 
   //at least one permission have been declined by the user 
}
```

[![screen](https://raw.githubusercontent.com/florent37/RuntimePermission/master/medias/permissions.png)](https://www.github.com/florent37/RuntimePermission)

Will automatically ask for **CONTACTS** and **LOCALISATION** permissions

## Manually call permissions

You just have to call `askPermission` with the list of wanted permissions

In Kotlin: 
```
askPermission(Manifest.permission.READ_CONTACTS, Manifest.permission.ACCESS_FINE_LOCATION){
   //all of your permissions have been accepted by the user
}.onDeclined { e -> 
   //at least one permission have been declined by the user 
}
```

[![screen](https://raw.githubusercontent.com/florent37/RuntimePermission/master/medias/permissions.png)](https://www.github.com/florent37/RuntimePermission)

Will ask for **CONTACTS** and **LOCALISATION** permissions

# Kotlin-Coroutines

```kotlin
launch(UI) {
   try {
       val result = askPermission(Manifest.permission.READ_CONTACTS, Manifest.permission.ACCESS_FINE_LOCATION)
       //all permissions already granted or just granted
       
       your action
   } catch (e: PermissionException) {
       //the list of denied permissions
       e.denied.forEach { permission ->
      
       }
       //but you can ask them again, eg:

       /*
        AlertDialog.Builder(this@MainActivityKotlinCoroutine )
               .setMessage("Please accept our permissions")
               .setPositiveButton("yes", { dialog, which -> e.askAgain() })
               .setNegativeButton("no", { dialog, which -> dialog.dismiss(); })
               .show();
       */

       //the list of forever denied permissions, user has check 'never ask again'
       e.foreverDenied.forEach { permission ->
       
       }
       //you need to open setting manually if you really need it
       //e.goToSettings();
   }
}
```

### Download 

[ ![Download](https://api.bintray.com/packages/florent37/maven/runtime-permission/images/download.svg) ](https://bintray.com/florent37/maven/runtime-permission/)
```groovy
implementation 'com.github.florent37:runtime-permission-kotlin:(last version)'
```

# Kotlin

```kotlin
askPermission(Manifest.permission.READ_CONTACTS, Manifest.permission.ACCESS_FINE_LOCATION){
   //all permissions already granted or just granted
  
   your action
}.onDeclined { e ->
   //the list of denied permissions
   e.denied.forEach{
       
   }
   /*
   AlertDialog.Builder(this@MainActivityKotlinCoroutine)
           .setMessage("Please accept our permissions")
           .setPositiveButton("yes", (dialog, which) -> { result.askAgain(); })
           .setNegativeButton("no", (dialog, which) -> { dialog.dismiss(); })
           .show();
   */

   //the list of forever denied permissions, user has check 'never ask again'
   e.foreverDenied.forEach{
       
   }
   // you need to open setting manually if you really need it
   //e.goToSettings();
}
```

### Download 

[ ![Download](https://api.bintray.com/packages/florent37/maven/runtime-permission/images/download.svg) ](https://bintray.com/florent37/maven/runtime-permission/)
```groovy
implementation 'com.github.florent37:runtime-permission-kotlin:(last version)'

```

RxJava
new RxPermissions(this).request(Manifest.permission.READ_CONTACTS, Manifest.permission.ACCESS_FINE_LOCATION))
    .subscribe(result -> {
        //all permissions already granted or just granted

        your action
    }, throwable -> {
        final PermissionResult result = ((RxPermissions.Error) throwable).getResult();

        if(result.hasDenied()) {
            appendText(resultView, "Denied :");
            //the list of denied permissions
            for (String permission : result.getDenied()) {
                appendText(resultView, permission);
            }
            //permission denied, but you can ask again, eg:


            new AlertDialog.Builder(RuntimePermissionMainActivityRx.this)
                    .setMessage("Please accept our permissions")
                    .setPositiveButton("yes", (dialog, which) -> {
                        result.askAgain();
                    }) // ask again
                    .setNegativeButton("no", (dialog, which) -> {
                        dialog.dismiss();
                    })
                    .show();
        }

        if(result.hasForeverDenied()) {
            appendText(resultView, "ForeverDenied :");
            //the list of forever denied permissions, user has check 'never ask again'
            for (String permission : result.getForeverDenied()) {
                appendText(resultView, permission);
            }
            // you need to open setting manually if you really need it
            result.goToSettings();
        }
    });

Download
implementation 'com.github.florent37:runtime-permission-rx:(last version)'
Java8
askPermission(this)
     .request(Manifest.permission.READ_CONTACTS, Manifest.permission.ACCESS_FINE_LOCATION)

     .onAccepted((result) -> {
         //all permissions already granted or just granted

         your action
     })
     .onDenied((result) -> {
         appendText(resultView, "Denied :");
         //the list of denied permissions
         for (String permission : result.getDenied()) {
             appendText(resultView, permission);
         }
         //permission denied, but you can ask again, eg:

         new AlertDialog.Builder(RuntimePermissionMainActivityJava8.this)
                 .setMessage("Please accept our permissions")
                 .setPositiveButton("yes", (dialog, which) -> {
                     result.askAgain();
                 }) // ask again
                 .setNegativeButton("no", (dialog, which) -> {
                     dialog.dismiss();
                 })
                 .show();

     })
     .onForeverDenied((result) -> {
         appendText(resultView, "ForeverDenied :");
         //the list of forever denied permissions, user has check 'never ask again'
         for (String permission : result.getForeverDenied()) {
             appendText(resultView, permission);
         }
         // you need to open setting manually if you really need it
         result.goToSettings();
     })
     .ask();
Download
 Download

implementation 'com.github.florent37:runtime-permission:(last version)'
Java7
askPermission(this, Manifest.permission.READ_CONTACTS, Manifest.permission.ACCESS_FINE_LOCATION)
    .ask(new PermissionListener() {
        @Override
        public void onAccepted(RuntimePermission runtimePermission, List<String> accepted) {
            //all permissions already granted or just granted

            your action
        }

        @Override
        public void onDenied(RuntimePermission runtimePermission, List<String> denied, List<String> foreverDenied) {
            if(permissionResult.hasDenied()) {
                appendText(resultView, "Denied :");
                //the list of denied permissions
                for (String permission : denied) {
                    appendText(resultView, permission);
                }

                //permission denied, but you can ask again, eg:

                new AlertDialog.Builder(RuntimePermissionMainActivityJava7.this)
                        .setMessage("Please accept our permissions")
                        .setPositiveButton("yes", (dialog, which) -> {
                            permissionResult.askAgain();
                        }) // ask again
                        .setNegativeButton("no", (dialog, which) -> {
                            dialog.dismiss();
                        })
                        .show();
            }


            if(permissionResult.hasForeverDenied()) {
                appendText(resultView, "ForeverDenied :");
                //the list of forever denied permissions, user has check 'never ask again'
                for (String permission : foreverDenied) {
                    appendText(resultView, permission);
                }
                // you need to open setting manually if you really need it
                permissionResult.goToSettings();
            }
        }
    });
    
    
# License

    Copyright 2018 TenderSoftware, Inc.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
