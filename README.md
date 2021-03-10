### Requirement
- Minimum Android SDK version: 21

### Installation
- Download the  `automate-lock-android-sdk-release.aar` from Github.
- In Android Studio, go to ```File > New > New Module > Import .JAR/.AAR Package```, choose the AAR file, then click `Finish
- Make sure the library is listed at the top of your settings.gradle file, as shown here for library named `bms-android-sdk-realease`:
`include ':app', ':automate-lock-android-sdk-realease'`
- Open the app module's `build.gradle` file and add new lines to the dependencies block as shown in the following snippet:

```gradle
    implementation project(':automate-lock-android-sdk-release')
    implementation "com.android.support:design:27.1.1"
    implementation "com.squareup.picasso:picasso:2.7+"
```

##### The ```dependencies``` block now might look like:
```gradle
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'androidx.appcompat:appcompat:1.0.2'
    implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'androidx.test:runner:1.2.0'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.2.0'

    // another dependencies

    // bms dependencies
    implementation project(':automate-lock-android-sdk-release')
    implementation "com.android.support:design:27.1.1"
    implementation "com.squareup.picasso:picasso:2.7+"
}
```

A message might appear to prompt you to sync the project, click "Sync Now" to proceed.

### Setup
##### Sample setup codes in MainActivity
```java

// implements delegate of SDK
public class MainActivity extends AppCompatActivity implements AutomateLockController.AutomateLockControllerDelegate {
    private static String PROJECT_NAME = "AUTOMATE_LOCK_TEST";
    AutomateLockController automateLockController = new AutomateLockController();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // set delegate to be the current activity to receive onConnect and onOpen callbacks
        automateLockController.setDelegate(this);

        // initiate the SDK with your SDK key and return the status of initiation
        // you can't use other SDK methods until it has been initiated
        boolean initResult = automateLockController.initialize("[PASTE_YOUR_SDK_KEY_HERE]");
    }

    public void connectLock(String macAddress) {
      // return true after the lock connection request has been sent
      // return false if the SDK hasn't been initiated or SDK key is invalid
      boolean connectResult = automateLockController.connectLock(this, macAddress);
    }

    public void openLock(String macAddress, String password) {
      // return true after the lock open request has been sent
      // return false if the SDK hasn't been initiated or SDK key is invalid
      boolean openResult = automateLockController.openLock(macAddress, password);
    }

    // disconnect current connected lock
    public void disconnectLock() {
      // return true after the lock has been disconnected (or if no lock is connected)
      // return false if the SDK hasn't been initiated or SDK key is invalid
      boolean disconnectResult = automateLockController.disconnectLock();
    }

    // trigger upon a lock successfully connected or fail
    @Override
    public void onConnect(String s, boolean success) {
        Log.i(PROJECT_NAME, "onConnect: " + (success ? "true" : "false"));
    }

    // trigger upon a lock successfully opened or fail
    @Override
    public void onOpen(String s, boolean success) {
        Log.i(PROJECT_NAME, "onOpen: " + (success ? "true" : "false"));
    }
}
```
