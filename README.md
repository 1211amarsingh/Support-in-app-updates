# Support-in-app-updates   
<b>Inbuilt App Update Check</b><br>
Keeping your app up-to-date on your users’ devices enables them to try new features, as well as benefit from performance improvements and bug fixes. Although some users enable background updates when their device is connected to an unmetered connection, other users may need to be reminded to update. In-app updates is a Play Core library feature that introduces a new request flow to prompt active users to update your app.

## Info

* Minimum Sdk Version <b>Android 5.0 (API level 21) </b>or higher

* Work only in <b>Release</b>

## Update Flow Type
* Flexible (Small Dialog)
* Immediate (Full Screen)
     
## Dependency
       implementation 'com.google.android.play:core:1.6.1'
## How To Implement 

* <b> Copy & Paste Step 1 to Step 5 and Call checkUpdate(context) Method</b>

* Step 1 init variable
```
    private static final int MY_REQUEST_CODE = 17300;
    AppUpdateManager appUpdateManager;
```
* Step 2  Check for update availability
```
public void checkUpdate(Context context) {
        appUpdateManager = AppUpdateManagerFactory.create(context);
        Task<AppUpdateInfo> appUpdateInfoTask = appUpdateManager.getAppUpdateInfo();

        appUpdateInfoTask.addOnSuccessListener(new OnSuccessListener<AppUpdateInfo>() {
            @Override
            public void onSuccess(AppUpdateInfo appUpdateInfo) {
                if (appUpdateInfo.updateAvailability() == UpdateAvailability.UPDATE_AVAILABLE) {
                
                    Log.d("Support in-app-update", "UPDATE_AVAILABLE");
                   
                    if (appUpdateInfo.isUpdateTypeAllowed(AppUpdateType.IMMEDIATE)) {
                        requestUpdate(appUpdateInfo, AppUpdateType.IMMEDIATE);
                    } else { //AppUpdateType.FLEXIBLE
                        requestUpdate(appUpdateInfo, AppUpdateType.FLEXIBLE);
                    }

                } else {
                    Log.d("Support in-app-update", "UPDATE_NOT_AVAILABLE");
                }
            }
        });
}
```
* Step 3  requestForUpdate
```    
private void requestUpdate(AppUpdateInfo appUpdateInfo, int flow_type) {
        try {
            appUpdateManager.startUpdateFlowForResult(appUpdateInfo,
                    flow_type,
                    this,
                    MY_REQUEST_CODE);

        } catch (IntentSender.SendIntentException e) {
            e.printStackTrace();
        }
}
```
* Step 4 resultOfRequest
```
 @Override
  protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);

        if (requestCode == MY_REQUEST_CODE) {
            if (resultCode != RESULT_OK) {
                log("Update flow failed! Result code: " + resultCode);
                // If the update is cancelled or fails,
                // you can request to start the update again.
            }
        }
}
```
## Output 
<p><strong>Immediate:</strong><br>
<img src="https://developer.android.com/images/app-bundle/immediate_flow.png" alt="" width="400"></p>

<p><strong>Flexible:</strong><br>
<img src="https://developer.android.com/images/app-bundle/flexible_flow.png" alt="" width="600"></p>

## Demo
Feel free to clone this project and run in your IDE to see how can be implemented :).

## Reference
https://developer.android.com/guide/app-bundle/in-app-updates

---------------------------------------------------------
We're always excited to hear from you! If you have any request, suggestions, feedback, questions, or concerns, please email us at:

 <a href="mailto:1211AMARSINGH@GMAIL.COM" >1211AMARSINGH@GMAIL.COM</a>
