# Android-AutoStart-App
[Solved] How to Auto Launch Android App At Boot?


 Android Auto Launch App works according to the boot completed broadcast in Android.
 Whenever the device boots up, it sends out this broadcast, and the apps can register a broadcast receiver to listen to it.
 Once that is done, the app can be launched programmatically, and after the device restarts, it starts automatically.
 The auto-start feature is supported by multiple Android OS versions but is generally available on Android 10 and all the newer versions.
 Some of the manufacturers may have implemented it in the earlier versions as well, but the steps may vary from device to device.

1. Create BroadcastReceiver  
   class BootReceiver : BroadcastReceiver() {

   @SuppressLint("UnsafeProtectedBroadcastReceiver")
   override fun onReceive(context: Context, intent: Intent) {

        val activityIntent = Intent(context, MainActivity::class.java).apply {
            addFlags(Intent.FLAG_ACTIVITY_NEW_TASK)
        }
        context.startActivity(activityIntent)
   }
   }


2. Add receiver to manifest
   <receiver
   android:name=".receiver.BootReceiver"
   android:enabled="false"
   android:exported="true"
   android:permission="android.permission.RECEIVE_BOOT_COMPLETED">
   <intent-filter>
   <action android:name="android.intent.action.BOOT_COMPLETED" />

                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </receiver>

 These permissions are related to device booting and rebooting, allowing the application to receive events related to
 booting and to request a reboot under certain circumstances.

3. Add permissions to manifest

   <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

4. Make Intent for allow user to select permissions
 This code opens the system settings screen where the user can manage overlay permissions for the current application

val intent =
Intent(
Settings.ACTION_MANAGE_OVERLAY_PERMISSION,
Uri.parse("package:$packageName")
)
startActivityForResult(intent, 101)