---
title: Unity安装好SDK JDK 还是无法打包app   2019版本问题
date: 2019-02-22 22:08:17
tags: CSDN迁移
---
  安装好SDK，JDK，打爆安卓软件还是出错。出现版本小了

 ![](https://img-blog.csdnimg.cn/20190222220439791.png)

 ![](https://img-blog.csdnimg.cn/20190222220657210.png)

 弹出错误 CommandInvokationFailure: Failed to create a raw.ap_ package

 
```
 CommandInvokationFailure: Failed to create a raw.ap_ package
H:\Program Files\Unity\Editor\Data\PlaybackEngines\AndroidPlayer/Tools\OpenJDK\Windows\bin\java.exe -Xmx4096M -Dcom.android.sdkmanager.toolsdir="H:/Program Files/android-sdk-windows\tools" -Dfile.encoding=UTF8 -jar "H:\Program Files\Unity\Editor\Data\PlaybackEngines\AndroidPlayer/Tools\sdktools.jar" -

stderr[
Exception in thread "main" java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at SDKMain.main(SDKMain.java:136)
Caused by: java.lang.NoSuchMethodError: com.google.common.util.concurrent.MoreExecutors.directExecutor()Ljava/util/concurrent/Executor;
	at com.android.apkzlib.zip.ZFile.createSources(ZFile.java:1586)
	at com.android.apkzlib.zip.ZFile.makeStoredEntry(ZFile.java:1546)
	at com.android.apkzlib.zip.ZFile.add(ZFile.java:1625)
	at com.android.apkzlib.zip.ZFile.add(ZFile.java:1499)
	at com.android.apkzlib.sign.ManifestGenerationExtension.updateManifest(ManifestGenerationExtension.java:241)
	at com.android.apkzlib.sign.ManifestGenerationExtension.access$000(ManifestGenerationExtension.java:52)
	at com.android.apkzlib.sign.ManifestGenerationExtension$1.lambda$beforeUpdate$0(ManifestGenerationExtension.java:168)
	at com.android.apkzlib.zip.ZFile.notify(ZFile.java:2099)
	at com.android.apkzlib.zip.ZFile.update(ZFile.java:871)
	at com.android.apkzlib.zip.ZFile.close(ZFile.java:1161)
	at com.android.apkzlib.zfile.ApkZFileCreator.close(ApkZFileCreator.java:189)
	at UnityPackageBuilder.CreatePackage(UnityPackageBuilder.java:136)
	at UnityPackageBuilder.<init>(UnityPackageBuilder.java:70)
	at UnityPackageBuilder.main(UnityPackageBuilder.java:27)
	... 5 more
]
stdout[

]
exit code: 1
UnityEditor.Android.AndroidSDKTools.RunCommand (System.String javaExe, System.String sdkToolsDir, System.String[] sdkToolCommand, System.String workingdir, System.String errorMsg, System.Int32 memoryMB) (at <7cbc688ae1af4105929402a46c6a4414>:0)
UnityEditor.Android.AndroidSDKTools.RunSDKToolWithReadLock (System.String[] command, System.String workingdir, System.String errorMsg) (at <7cbc688ae1af4105929402a46c6a4414>:0)
UnityEditor.Android.PostProcessor.Tasks.AAPTPackage.CreatePackage (UnityEditor.Android.PostProcessor.PostProcessorContext context, System.String package, System.String directory, System.Boolean compress) (at <7cbc688ae1af4105929402a46c6a4414>:0)
UnityEditor.Android.PostProcessor.Tasks.AAPTPackage.Pack (UnityEditor.Android.PostProcessor.PostProcessorContext context, System.String package, System.String directory, System.Boolean compress, System.Boolean useAAPT) (at <7cbc688ae1af4105929402a46c6a4414>:0)
UnityEditor.Android.PostProcessor.Tasks.AAPTPackage.Execute (UnityEditor.Android.PostProcessor.PostProcessorContext context) (at <7cbc688ae1af4105929402a46c6a4414>:0)
UnityEditor.Android.PostProcessor.PostProcessRunner.RunAllTasks (UnityEditor.Android.PostProcessor.PostProcessorContext context) (at <7cbc688ae1af4105929402a46c6a4414>:0)
UnityEngine.GUIUtility:ProcessEvent(Int32, IntPtr)

```
 

 这个是版本跟不上结果，要么更新SDK ，要么就是unity有一个use legecy SDK tool

 选上就可以了

 

 ![](https://img-blog.csdnimg.cn/20190222220550732.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQwODcxNDY2,size_16,color_FFFFFF,t_70)

   
 