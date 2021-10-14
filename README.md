### Requirement
- Minimum Android SDK version: 21

### Installation
- Download the  `automate-lock-android-sdk-release.aar` from Github.
- In Android Studio, go to ```File > New > New Module > Import .JAR/.AAR Package```, choose the AAR file, then click `Finish
- Make sure the library is listed at the top of your settings.gradle file, as shown here for library named `bms-android-sdk-realease`:
`include ':app', ':automate-lock-android-sdk-realease'`
- Open the app module's `build.gradle` file and add new lines to the dependencies block as shown in the following snippet:

```gradle
	implementation 'no.nordicsemi.android.support.v18:scanner:1.4.2'
    implementation "com.android.volley:volley:1.1.0+"
    implementation project(':automate-lock-android-sdk-release')
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

    // automate lock sdk dependencies
    implementation 'no.nordicsemi.android.support.v18:scanner:1.4.2'
    implementation "com.android.volley:volley:1.1.0+"
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

        // initiate the SDK with your SDK key
        // status of initiation is returned inside the onInit callback below
        // you can't use other SDK methods until it has been initiated
        automateLockController.initialize(this, "[PASTE_YOUR_SDK_KEY_HERE]");

        // set delegate to be the current activity to receive onConnect and onOpen callbacks
        // it needs to be called after automateLockController.initialize
        automateLockController.setDelegate(this);
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

    // return whether SDK initiation is successful or failed
    @Override
    public void onInit(boolean success) {
        Log.i(PROJECT_NAME, "onInit: " + (success ? "true" : "false"));
    }

    // trigger upon a lock successfully connected or fail
    @Override
    public void onConnect(String s, boolean success) {
        Log.i(PROJECT_NAME, "onConnect: " + (success ? "true" : "false"));

				// you can choose to trigger openLock after this returns true
    }

    // trigger upon a lock successfully opened or fail
    @Override
    public void onOpen(String s, boolean success) {
        Log.i(PROJECT_NAME, "onOpen: " + (success ? "true" : "false"));
    }
}
```
