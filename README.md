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
JH: 我將原先line 247 的 call(['adb', 'logcat', '-c'])
    更改為 adb = Popen(["adb", "logcat", "ActivityManager:I *:S"], stdin=subprocess.PIPE, stdout=subprocess.PIPE)
    將原先 line 267 的 logcatInput = sys.stdin.readline()
    更改為 logcatInput = adb.stdout.readline()
    讓這個shell所產生出來的log可以藉由adb.stdout產生出來，提供 line 267 讀取每一行log
    於 line 270 的 boxlog = logcatInput.split('DroidBox:') 取出 DroidBox 啟動後的所有log資訊，並針對所產生出來的log進行分類
    網路上有使用droidbox的討論都是以此手法進行，但我發現這跑出來的log都不是我預期的資料，亦即原程式抓不到需要分類的項目名稱        (DexClassLoader,ServiceStart,RecvNet,FdAccess,FileRW,OpenNet,CloseNet,SendNet,DataLeak,SendSMS,PhoneCall,CryptoUsage)
    因此導致我輸入ctrl+C 結束程式時，上述這些項目類型的資料皆為空值，僅顯示（Info，Broadcast receivers，Enforced permissions）等靜態資料

JH: The step I've executed as follows: 
    1. launch the emulator by executing emulator -avd test (test is name of emulator) 
    2. Then typing ./droidbox.sh com.kingroot.kinguser.apk to launch the app and monkeyrunner
    
KYC: 
    有無可能 emulator 太舊或太新，不支援 APK 版本？
``` 不同的 APK 需要使用可支援其規格的 emulator 進行運作，目前都是使用我開啟的 emulator 可以順利安裝上來的 APK 進行分析，
    使用 adb install *.apk 皆可以正確的將 APK 安裝到 emulator 上， 也能在 emulator 上對此 APK 進行操作 （更新，下載東西等資料存取或網路行為）
    若我開始操作 APK 並且在 terminal 同時執行 adb logcat， 會持續不斷的跳出 log， 但這些 log 都不是我目前需要的資訊（需要抓到 DroidBox 的 log ） 
    因此初步判定應該不是 emulator 版本問題， 對此我感到疑惑， 為何抓不到 DroidBox 的 log 資訊
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
