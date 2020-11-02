<p align="center">
  <img src="https://bordencom.com/wp-content/uploads/2016/03/Do-You-Have-Permission.png" width="400"/>
</p>

<h1 align="center">
    MultiPermitionKT (Kotlin)
</h1>

**Example Multi Check Permissions.** Request Permissions at same time, I recommend to use this step on your `First Activity`, now i use it on `MainActivity` :

#### Step 1.
**Manifest.** add permission ke file manifest. I recommend to add `requestLegacyExternalStorage=true` if you using Android 10.

```xml
<manifest >

    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />

    <application
        android:requestLegacyExternalStorage="true">

    </application>

</manifest>
```

#
#### Step 2.
Add array permission that you need :

**First Activity.** put request permission at your first activity, now i use it on `MainActivity`.

```kotlin
class MainActivity : AppCompatActivity() {

    var permissions = arrayOf<String>(
        Manifest.permission.READ_EXTERNAL_STORAGE,
        Manifest.permission.WRITE_EXTERNAL_STORAGE
    )
    
    ...

}
```

#
#### Step 3.
Add function `checkPermissions` to check permission on app, if permission not granted than popup will show and ask to `Allow`. Please click `allow` :

```kotlin
class MainActivity : AppCompatActivity() {
    
    ...

    var MULTIPLE_PERMISSIONS = 1
    private fun checkPermissions(): Boolean {
        var result: Int
        val listPermissionsNeeded: MutableList<String> = ArrayList()
        for (p in permissions) {
            result = ContextCompat.checkSelfPermission(applicationContext, p)
            if (result != PackageManager.PERMISSION_GRANTED) {
                listPermissionsNeeded.add(p)
            }
        }
        if (listPermissionsNeeded.isNotEmpty()) {
            ActivityCompat.requestPermissions(this, listPermissionsNeeded.toTypedArray(), MULTIPLE_PERMISSIONS)
            return false
        }
        return true
    }
    
    ...

}
```

#
#### Step 4.
You can check all permition are granted or not with function `onRequestPermissionsResult`, if granted you can run your code :

```kotlin
class MainActivity : AppCompatActivity() {
    
    ...

    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //Do Someting ...
                onSuccessCheckPermitions();
            } else {
                val perStr = StringBuilder()
                for (per in permissions) {
                    perStr.append("\n").append(per)
                }
            }
        }
    }
    
    ...

}
```

#
#### Step 5.
Make and call function `onSuccessCheckPermitions` to run your code :

```kotlin
class MainActivity : AppCompatActivity() {
    
    ...
    
    override fun onRequestPermissionsResult(requestCode: Int, permissions: Array<out String>, grantResults: IntArray) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //Do Someting ...
                onSuccessCheckPermitions();
            } else {
                val perStr = StringBuilder()
                for (per in permissions) {
                    perStr.append("\n").append(per)
                }
            }
        }
    }
    
    ...

}
```

#
#### Step 6.
Add action to function `onSuccessCheckPermitions` :

```kotlin
class MainActivity : AppCompatActivity() {
    
    ...

    private fun onSuccessCheckPermitions() {
        Toast.makeText(this, "All Granted", Toast.LENGTH_SHORT).show()
        //letakan action kamu disini
    }

}
```

**notes.** 
  - I always use this method to get permition for all Libray that i made.
  - You can modif body of function `onSuccessCheckPermitions`.
  
#
#### Step 7.
Add function `checkPermissions` in `onCreate` to check permission ever time app running :

```kotlin
class MainActivity : AppCompatActivity() {

    ...

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
  
        ...
        
        if (checkPermissions()) {
            Toast.makeText(this, "Izin sudah diberikan", Toast.LENGTH_SHORT).show();
            onSuccessCheckPermitions();
        } else {
            Toast.makeText(this, "Berikan izin untuk melanjutkan ke tahap berikutnya", Toast.LENGTH_SHORT).show();
        }
    }
    
    ...

}
```

#
#### Step 8.
Fullcode will be like this :

```kotlin
class MainActivity : AppCompatActivity() {

    var permissions = arrayOf<String>(
        Manifest.permission.READ_EXTERNAL_STORAGE,
        Manifest.permission.WRITE_EXTERNAL_STORAGE
    )

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        if (checkPermissions()) {
            Toast.makeText(this, "Izin sudah diberikan", Toast.LENGTH_SHORT).show();
            onSuccessCheckPermitions();
        } else {
            Toast.makeText(this, "Berikan izin untuk melanjutkan ke tahap berikutnya", Toast.LENGTH_SHORT).show();
        }
    }

    var MULTIPLE_PERMISSIONS = 1
    private fun checkPermissions(): Boolean {
        var result: Int
        val listPermissionsNeeded: MutableList<String> = ArrayList()
        for (p in permissions) {
            result = ContextCompat.checkSelfPermission(applicationContext, p)
            if (result != PackageManager.PERMISSION_GRANTED) {
                listPermissionsNeeded.add(p)
            }
        }
        if (listPermissionsNeeded.isNotEmpty()) {
            ActivityCompat.requestPermissions(
                this,
                listPermissionsNeeded.toTypedArray(),
                MULTIPLE_PERMISSIONS
            )
            return false
        }
        return true
    }

    override fun onRequestPermissionsResult(
        requestCode: Int,
        permissions: Array<out String>,
        grantResults: IntArray
    ) {
        if (requestCode == MULTIPLE_PERMISSIONS) {
            if (grantResults.isNotEmpty() && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                //Do Someting ...
                onSuccessCheckPermitions();
            } else {
                val perStr = StringBuilder()
                for (per in permissions) {
                    perStr.append("\n").append(per)
                }
            }
        }
    }

    private fun onSuccessCheckPermitions() {
        Toast.makeText(this, "All Granted", Toast.LENGTH_SHORT).show()
        //letakan action kamu disini
    }
}
```

#
#### Step 9.
Preview :
|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example1.jpg)|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example2.jpg)|![](https://github.com/gzeinnumer/MultiPermition/blob/master/assets/example3.jpg)|
|--|--|--|
|Request Permission 1 |Request Permission 2 |Iff all permission granted Toast `AllGranted` will show |

#
#### FullCode
[Kotlin](https://github.com/gzeinnumer/MultiPermitionkt/blob/master/app/src/main/java/com/gzeinnumer/multipermitionkt/MainActivity.kt)
[Manifest](https://github.com/gzeinnumer/MultiPermitionkt/blob/master/app/src/main/AndroidManifest.xml)

---

```
Copyright 2020 M. Fadli Zein
```
