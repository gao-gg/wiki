精确获取屏幕尺寸（例如：3.5、4.0、5.0寸屏幕）
```java
public static double getScreenPhysicalSize(Activity ctx) {
        DisplayMetrics dm = new DisplayMetrics();
        ctx.getWindowManager().getDefaultDisplay().getMetrics(dm);
        double diagonalPixels = Math.sqrt(Math.pow(dm.widthPixels, 2) + Math.pow(dm.heightPixels, 2));
        return diagonalPixels / (160 * dm.density);
    }
```

          
判断是否是平板（官方用法）,一般是7寸以上是平板
```java
public static boolean isTablet(Context context) {
        return (context.getResources().getConfiguration().screenLayout & Configuration.SCREENLAYOUT_SIZE_MASK) >= Configuration.SCREENLAYOUT_SIZE_LARGE;
    }
```

文字根据状态更改颜色 android:textColor , 放在res/color/目录下
```java
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:color="#53c1bd" android:state_selected="true"/>
    <item android:color="#53c1bd" android:state_focused="true"/>
    <item android:color="#53c1bd" android:state_pressed="true"/>
    <item android:color="#777777"/>
</selector>
```

背景色根据状态更改颜色 android:backgroup,如果直接给背景色color会报错。
```java
<selector xmlns:android="http://schemas.android.com/apk/res/android">

    <item android:state_selected="true"><shape>            <gradient android:angle="0" android:centerColor="#00a59f" android:endColor="#00a59f" android:startColor="#00a59f" />
        </shape></item>
    <item android:state_focused="true"><shape>
            <gradient android:angle="0" android:centerColor="#00a59f" android:endColor="#00a59f" android:startColor="#00a59f" />
        </shape></item>
    <item android:state_pressed="true"><shape>
            <gradient android:angle="0" android:centerColor="#00a59f" android:endColor="#00a59f" android:startColor="#00a59f" />
        </shape></item>
    <item><shape>
            <gradient android:angle="0" android:centerColor="#00ff00" android:endColor="00ff00" android:startColor="00ff00" />
        </shape></item>

</selector>
```

启动APK的默认Activity
```java
public static void startApkActivity(final Context ctx, String packageName) {
        PackageManager pm = ctx.getPackageManager();
        PackageInfo pi;
        try {
            pi = pm.getPackageInfo(packageName, 0);
            Intent intent = new Intent(Intent.ACTION_MAIN, null);
            intent.addCategory(Intent.CATEGORY_LAUNCHER);
            intent.setPackage(pi.packageName);

            List<ResolveInfo> apps = pm.queryIntentActivities(intent, 0);

            ResolveInfo ri = apps.iterator().next();
            if (ri != null) {
                String className = ri.activityInfo.name;
                intent.setComponent(new ComponentName(packageName, className));
                ctx.startActivity(intent);
            }
        } catch (NameNotFoundException e) {
            Log.e("startActivity", e);
        }
    }
```

计算字宽
```java
public static float GetTextWidth(String text, float Size) {
        TextPaint FontPaint = new TextPaint();
        FontPaint.setTextSize(Size);
        return FontPaint.measureText(text);
    }
```

获取应用程序下所有Activity 
```java
public static ArrayList<String> getActivities(Context ctx) {
      ArrayList<String> result = new ArrayList<String>();
      Intent intent = new Intent(Intent.ACTION_MAIN, null);
      intent.setPackage(ctx.getPackageName());
      for (ResolveInfo info : ctx.getPackageManager().queryIntentActivities(intent, 0)) {
          result.add(info.activityInfo.name);
      }
      return result;
  }
```

检测字符串中是否包含汉字
```java
public static boolean checkChinese(String sequence) {
        final String format = "[\\u4E00-\\u9FA5\\uF900-\\uFA2D]";
        boolean result = false;
        Pattern pattern = Pattern.compile(format);
        Matcher matcher = pattern.matcher(sequence);
        result = matcher.find();
        return result;
    }
```

检查有没有应用程序来接受处理你发出的intent
```java
public static boolean isIntentAvailable(Context context, String action) {
        final PackageManager packageManager = context.getPackageManager();
        final Intent intent = new Intent(action);
        List<ResolveInfo> list = packageManager.queryIntentActivities(intent, PackageManager.MATCH_DEFAULT_ONLY);
        return list.size() > 0;
    }
```


使用TransitionDrawable实现渐变效果 ,比使用AlphaAnimation效果要好，可避免出现闪烁问题。
```java
private void setImageBitmap(ImageView imageView, Bitmap bitmap) {
        // Use TransitionDrawable to fade in.
        final TransitionDrawable td = new TransitionDrawable(new Drawable[] { new ColorDrawable(android.R.color.transparent), new BitmapDrawable(mContext.getResources(), bitmap) });
        //noinspection deprecation
            imageView.setBackgroundDrawable(imageView.getDrawable());
        imageView.setImageDrawable(td);
        td.startTransition(200);
    }
```


扫描指定的文件,用途：从本软件新增、修改、删除图片、文件某一个文件（音频、视频）需要更新系统媒体库时使用，不必扫描整个SD卡
```java
     sendBroadcast(new Intent(Intent.ACTION_MEDIA_SCANNER_SCAN_FILE, uri));
```


Dip转px
```java
public static int dipToPX(final Context ctx, float dip) {
        return (int)TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dip, ctx.getResources().getDisplayMetrics());
    }

```
