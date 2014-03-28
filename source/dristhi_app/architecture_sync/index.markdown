---
layout: page
---

# Dristhi Sync

## Login Scenario

![][sync_login_diagram]

The above mentioned components can be found in code as below:

* Login block refers to LoginActitity and UserService class
* Scheduler is implemented as DrishtiSyncScheduler class
* [Android Alarm Manager][] is an Android API that allows access to system alarm service.
* Receiver is implemented as SyncBroadcastReceiver class
* Dristhi Service refers to the Dristhi-web service, currently hosted at <https://smartregistries.org>

More information about the sequence diagram messages below:

1. When the user logs in successfully, LoginActivity calls UserService to do post login tasks
2. UserService calls DrishtiSyncScheduler to start a sync schedule
3. DrishtiSyncScheduler uses AlarmManager to create a alarm which gives the app a callback every 'X' minutes. As of now X = 2 minutes. When creating the alarm, SyncBroadcastReceiver is set as the callback receiver/handler
4. Hence forth, till the alarm is stopped, AlarmManager will invoke the SyncBroadcastReceiver every 'X' minutes
5. When SyncBroadcastReceiver is invoked by AlarmManager, it POSTs any new form submission that to Dristhi Service using UpdateActionsTask class. UpdateActionsTask uses Android AsyncTask API to sync from Dristhi Service in the background
6. SyncBroadcastReceiver then tries to download any new data from Dristhi Service. New data is identified by using a token. First time sync runs with token as 0. Subsequently, sync stores the version of the last form submission and action that it downloads as token and this token is then used in the next sync to get new data
    1. It requests for new data since last sync by using */form-submissions* and */actions* API. New data includes any of Form Submissions, Reminders, Reports, etc
    2. Dristhi service responds to this request by sending back any new data in response
7. SyncBroadcastReceiver stores any newly downloaded data and handles them appropriately, i.e. form submissions are used to create or update entities (EC, ANC, PNC, Child), reminders updated on smart registers and repots are updated on the report screens


## Internet Connectivity Status Changed Scenario

![][sync_connectivity_status_changed_diagram]

The above mentioned components can be found in code as below:

* Change Connectivity Receiver refers to ConnectivityChangeReceiver class. Android has [ConnectivityManager][] which broadcasts connectivity status by using the android.net.conn.CONNECTIVITY_CHANGE intent. The receiver uses this intent to track the connectivity to internet
* Scheduler is implemented as DrishtiSyncScheduler class
* [Android Alarm Manager][] is an Android API that allows access to system alarm service.
* Receiver is implemented as SyncBroadcastReceiver class
* Dristhi Service refers to the Dristhi-web service, currently hosted at <https://smartregistries.org>

More information about the sequence diagram messages below:

1. ConnectivityChangeReceiver is invoked by ConnectivityManager when there is a change in Internet Connectivity Status
2. ConnectivityChangeReceiver checks the status
    1. When status is *Connection restored*
        1. When status is Connected, receiver calls Start Sync on DrishtiSyncScheduler
        2. DrishtiSyncScheduler uses AlarmManager to create a alarm which gives the app a callback every 'X' minutes. As of now X = 2 minutes. When creating the alarm, SyncBroadcastReceiver is set as the callback receiver/handler
        3. Hence forth, till the alarm is stopped, AlarmManager will invoke the SyncBroadcastReceiver every 'X' minutes
        4. When SyncBroadcastReceiver is invoked by AlarmManager, it POSTs any new form submission that to Dristhi Service using UpdateActionsTask class. UpdateActionsTask uses Android AsyncTask API to sync from Dristhi Service in the background
        5. SyncBroadcastReceiver then tries to download any new data from Dristhi Service. New data is identified by using a token. First time sync runs with token as 0. Subsequently, sync stores the version of the last form submission and action that it downloads as token and this token is then used in the next sync to get new data
            1. It requests for new data since last sync by using */form-submissions* and */actions* API. New data includes any of Form Submissions, Reminders, Reports, etc
            2. Dristhi service responds to this request by sending back any new data in response
        6. SyncBroadcastReceiver stores any newly downloaded data and handles them appropriately, i.e. form submissions are used to create or update entities (EC, ANC, PNC, Child), reminders updated on smart registers and repots are updated on the report screens
    2. When status is *Connection lost*
        1. ConnectivityChangeReceiver calls DrishtiSyncScheduler to stop sync schedule
        2. DrishtiSyncScheduler uses AlarmManager to stop the alarm that was created to sync. Receiver is not called anymore


[Android Alarm Manager]: https://developer.android.com/reference/android/app/AlarmManager.html
[ConnectivityManager]: https://developer.android.com/reference/android/net/ConnectivityManager.html
[sync_login_diagram]: /images/custom/dristhi_app/sync_login.png "Dristhi Sync Sequence Diagram for Login Scenario"
[sync_connectivity_status_changed_diagram]: /images/custom/dristhi_app/sync_connectivity_status_changed.png "Dristhi Sync Sequence Diagram for Internet Connectivity Changed Scenario"
