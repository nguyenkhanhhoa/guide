https://xdaforums.com/t/apk-google-play-store-v19-5-15-android-tv-2020-04-14.3942565/ \
su \
mount -o remount,rw /system \
rm /data/dalvik-cache/arm/system@priv-app@Phonesky@Phonesky.apk@classes.dex \
rm /data/dalvik-cache/profiles/com.android.vending \
rm /system/priv-app/Phonesky/Phonesky.apk \
mount -o remount,ro /system \
reboot \
(after reboot) \
su \
mount -o remount,rw /system \
cp /sdcard/Download/Phonesky.apk /system/priv-app/Phonesky/Phonesky.apk \
chmod 644 /system/priv-app/Phonesky/Phonesky.apk \
mount -o remount,ro /system \
reboot


#https://github.com/kitadai31/revanced-patches-android6-7/wiki/How-to-build#method-5-using-auto-cli-pc