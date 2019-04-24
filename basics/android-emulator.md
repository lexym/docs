# Android Emulator

In case you do not own an Android device on which you can run our Sandbox app for end-to-end testing, you can set up an emulator to run the bunq Sandbox app for Android.

#### Things you will need

* The [bunq Sandbox App APK](https://appstore.bunq.com/api/android/builds/bunq-android-sandbox-master.apk) that's optimised for emulating;
* [Android Studio](https://developer.android.com/studio/index.html).

#### Starting the Android Virtual Device \(AVD\) Manager

1. Open Android Studio.
2. From the top menu, select “Tools” &gt; "Android" &gt; "AVD Manager".

### Setting up a new virtual device

1. Start the wizard by clicking on "+ Create Virtual Device".
2. Select a device \(recommendation: "Pixel 5.0" or "Nexus 6"\) and press "Next".
3. Select an x86 system image \(recommendation: Nougat, API Level 25, Android 7.1.1 with Google APIs\) and press "Next". The image needs to have Google Play Services 10.0.1 or higher.
4. In the bottom left corner, select "Show Advanced Settings".
5. Scroll to "Memory and Storage".
6. Change "Internal Storage" to "2048 MB".
7. Change "SD card" to "200 MB".
8. Press "Finish".

### Starting the virtual device

1. On the right side under "Actions", select the green "Play" button.
2. Wait for the device to boot, this may take a few minutes.

### Installing the bunq Sandbox App APK

1. Open the command line.
2. Navigate to your Android SDK platform tools directory \(e.g. `cd ~/Library/Android/sdk/platform-tools` on macOS\).
3. Make sure that the virtual device is started and has fully booted.
4. Run `./adb install ~/Downloads/bunq-android-sandboxEmulator-public-api.apk`, this may take a few minutes, and should finish with "Success".

### Creating an account or logging in

* The first time you open the app you will be asked to verify your phonenumber. Sandbox however does not send actual SMS messages. Enter any valid phonenumber and use the default verification code `123456`. This will work for all numbers.
* Get [tinker](https://bunq.com/api/) for the language of your choice.
* Once installed, run `tinker/user-overview`, this will create an account for you when necessary.
* The output of the command above will show you the login credentials for your sandbox account.
* It is **not** possible to create accounts using the regular signup in the app, bunq is not reviewing Sandbox applications.

### Creating a new API key

To create additional API keys for the sandbox environment, log in to the sandbox app for Android as either a UserPerson or UserCompany. Navigate to Profile &gt; Security &gt; API keys and click the '+' button. Please be aware that the API key can only be assigned to an IP within 4 hours after its creation. After the 4 hours, it will become invalid if not assigned. API keys that are created via the sandbox app are wiped with each sandbox reset.

