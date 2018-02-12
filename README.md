# droidbox-problem-solving-progress

### JH 問題描述
```
JH: I want to get more apk logs(network, SMS, leakage, read/write behavior) by executing ./droidbox.sh XXX.apk, 
    but the terminal always shown "collect 0 sandbox" and get only static result.
    (such as hash value, Broadcast receivers, Enforced permissions) 

KYC: 
    執行 ./droidbox.sh *.apk 後只得到靜態分析結果。

JH: The droidbox.sh will execute droidbox.py, 
    so I trace the code of droidbox.py and figure out in line 267 "logcatInput = sys.stdin.readline()", 
    which is an empty value ....

KYC: 
    droidbox.sh -> droidbox.py -> line 267
    可以把 droidbox.py 裡面 你鎖定到 line 267 的過程詳細描述嗎？

JH: The step I've executed as follows: 
    1. launch the emulator by executing emulator -avd test (test is name of emulator) 
    2. Then typing ./droidbox.sh com.kingroot.kinguser.apk to launch the app and monkeyrunner
    
KYC: 
    有無可能 emulator 太舊或太新，不支援 APK 版本？
```

```
 [Info]
 ------
    File name:  com.kingroot.kinguser.apk
    MD5:        9a19974cbfb072f7cab0c13877b5fdd0
    SHA1:       88c834f6661d82a3c98405ccaa14a02f952e7010
    SHA256:     616df8b1da40a00e6e9e037dcc058b858833e65e1fd2ed7fa4a438be333d691d
    Duration:   5.13150787354s


 [File activities]
 -----------------

    [Read operations]
    -----------------

    [Write operations]
    ------------------

 [Crypto API activities]
 -----------------------

 [Network activity]
 ------------------

    [Opened connections]
    --------------------

    [Outgoing traffic]
    ------------------


    [Incoming traffic]
    ------------------

 [DexClassLoader]
 -----------------
 [Broadcast receivers]
 ---------------------
       com.kingroot.kinguser.receiver.DeviceOwnerReceiver            Action: android.app.action.DEVICE_ADMIN_ENABLED

       com.kingroot.common.framework.main.MainExitReceiver           Action: com.kingroot.master.action.MAIN_EXIT_CHECK

       com.kingroot.common.framework.broadcast.KSysBroadcastReceiver             Action: com.kingroot.kinguser.gamebox.ACTION_GAME_DELETED

       com.toprange.pluginmaster.base.ActionViewBroadcastReceiver            Action: com.toprange.plugin.action.UNINSTALL_SHORTCUT

       com.kingroot.kinguser.receiver.SuRequestReceiver          Action: com.kingroot.kinguser.SU_REQUEST

       com.kingroot.kinguser.receiver.AntiInjectLogDeleteReceiver            Action: com.kingroot.kinguser.ANTILOG_DELETE

 [Started services]
 ------------------
 [Enforced permissions]
 ----------------------
       com.kingroot.kinguser.INNER_BROADCAST
       com.kingroot.kinguser.permission.REQUEST
       com.kingroot.kinguser.permission.activityCalled
       com.kingroot.kinguser.permission.RootShell

 [Permissions bypassed]
 ----------------------

 [Information leakage]
 ---------------------

 [Sent SMS]
 ----------

 [Phone calls]
 -------------
```
