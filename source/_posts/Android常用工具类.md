---
title: Android常用工具栏
date: 2017-12-19 14:36
categories: android工具
tags: [android工具]
---
### 1、日志工具类

```
import android.util.Log;  
  
/** 
 * Log统一管理类 
 *  
 */  
public class LogUtils  
{  
  
    private LogUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
    }  
  
    public static boolean isDebug = true;// 是否需要打印bug，可以在application的onCreate函数里面初始化  
    private static final String TAG = "LogUtil";  
  
    // 下面四个是默认tag的函数  
    public static void i(String msg)  
    {  
        if (isDebug)  
            Log.i(TAG, msg);  
    }  
  
    public static void d(String msg)  
    {  
        if (isDebug)  
            Log.d(TAG, msg);  
    }  
  
    public static void e(String msg)  
    {  
        if (isDebug)  
            Log.e(TAG, msg);  
    }  
  
    public static void v(String msg)  
    {  
        if (isDebug)  
            Log.v(TAG, msg);  
    }  
  
    // 下面是传入自定义tag的函数  
    public static void i(String tag, String msg)  
    {  
        if (isDebug)  
            Log.i(tag, msg);  
    }  
  
    public static void d(String tag, String msg)  
    {  
        if (isDebug)  
            Log.i(tag, msg);  
    }  
  
    public static void e(String tag, String msg)  
    {  
        if (isDebug)  
            Log.i(tag, msg);  
    }  
  
    public static void v(String tag, String msg)  
    {  
        if (isDebug)  
            Log.i(tag, msg);  
    }  
}  
```

### 2、Toast管理类
<!-- more -->

```
import android.content.Context;  
import android.widget.Toast;  
  
/** 
 * Toast统一管理类 
 *  
 */  
public class ToastUtils  
{  
  
    private ToastUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
    }  
  
    public static boolean isShow = true;  
  
    /** 
     * 短时间显示Toast 
     *  
     * @param context 
     * @param message 
     */  
    public static void showShort(Context context, CharSequence message)  
    {  
        if (isShow)  
            Toast.makeText(context, message, Toast.LENGTH_SHORT).show();  
    }  
  
    /** 
     * 短时间显示Toast 
     *  
     * @param context 
     * @param message 
     */  
    public static void showShort(Context context, int message)  
    {  
        if (isShow)  
            Toast.makeText(context, message, Toast.LENGTH_SHORT).show();  
    }  
  
    /** 
     * 长时间显示Toast 
     *  
     * @param context 
     * @param message 
     */  
    public static void showLong(Context context, CharSequence message)  
    {  
        if (isShow)  
            Toast.makeText(context, message, Toast.LENGTH_LONG).show();  
    }  
  
    /** 
     * 长时间显示Toast 
     *  
     * @param context 
     * @param message 
     */  
    public static void showLong(Context context, int message)  
    {  
        if (isShow)  
            Toast.makeText(context, message, Toast.LENGTH_LONG).show();  
    }  
  
    /** 
     * 自定义显示Toast时间 
     *  
     * @param context 
     * @param message 
     * @param duration 
     */  
    public static void show(Context context, CharSequence message, int duration)  
    {  
        if (isShow)  
            Toast.makeText(context, message, duration).show();  
    }  
  
    /** 
     * 自定义显示Toast时间 
     *  
     * @param context 
     * @param message 
     * @param duration 
     */  
    public static void show(Context context, int message, int duration)  
    {  
        if (isShow)  
            Toast.makeText(context, message, duration).show();  
    }  
  
}  
```

### 3、SharedPreferences封装类

```
import java.lang.reflect.InvocationTargetException;  
import java.lang.reflect.Method;  
import java.util.Map;  
  
import android.content.Context;  
import android.content.SharedPreferences;  
  
public class SPUtils  
{  
    /** 
     * 保存在手机里面的文件名 
     */  
    public static final String FILE_NAME = "share_data";  
  
    /** 
     * 保存数据的方法，我们需要拿到保存数据的具体类型，然后根据类型调用不同的保存方法 
     *  
     * @param context 
     * @param key 
     * @param object 
     */  
    public static void put(Context context, String key, Object object)  
    {  
  
        SharedPreferences sp = context.getSharedPreferences(FILE_NAME,  
                Context.MODE_PRIVATE);  
        SharedPreferences.Editor editor = sp.edit();  
  
        if (object instanceof String)  
        {  
            editor.putString(key, (String) object);  
        } else if (object instanceof Integer)  
        {  
            editor.putInt(key, (Integer) object);  
        } else if (object instanceof Boolean)  
        {  
            editor.putBoolean(key, (Boolean) object);  
        } else if (object instanceof Float)  
        {  
            editor.putFloat(key, (Float) object);  
        } else if (object instanceof Long)  
        {  
            editor.putLong(key, (Long) object);  
        } else  
        {  
            editor.putString(key, object.toString());  
        }  
  
        SharedPreferencesCompat.apply(editor);  
    }  
  
    /** 
     * 得到保存数据的方法，我们根据默认值得到保存的数据的具体类型，然后调用相对于的方法获取值 
     *  
     * @param context 
     * @param key 
     * @param defaultObject 
     * @return 
     */  
    public static Object get(Context context, String key, Object defaultObject)  
    {  
        SharedPreferences sp = context.getSharedPreferences(FILE_NAME,  
                Context.MODE_PRIVATE);  
  
        if (defaultObject instanceof String)  
        {  
            return sp.getString(key, (String) defaultObject);  
        } else if (defaultObject instanceof Integer)  
        {  
            return sp.getInt(key, (Integer) defaultObject);  
        } else if (defaultObject instanceof Boolean)  
        {  
            return sp.getBoolean(key, (Boolean) defaultObject);  
        } else if (defaultObject instanceof Float)  
        {  
            return sp.getFloat(key, (Float) defaultObject);  
        } else if (defaultObject instanceof Long)  
        {  
            return sp.getLong(key, (Long) defaultObject);  
        }  
  
        return null;  
    }  
  
    /** 
     * 移除某个key值已经对应的值 
     * @param context 
     * @param key 
     */  
    public static void remove(Context context, String key)  
    {  
        SharedPreferences sp = context.getSharedPreferences(FILE_NAME,  
                Context.MODE_PRIVATE);  
        SharedPreferences.Editor editor = sp.edit();  
        editor.remove(key);  
        SharedPreferencesCompat.apply(editor);  
    }  
  
    /** 
     * 清除所有数据 
     * @param context 
     */  
    public static void clear(Context context)  
    {  
        SharedPreferences sp = context.getSharedPreferences(FILE_NAME,  
                Context.MODE_PRIVATE);  
        SharedPreferences.Editor editor = sp.edit();  
        editor.clear();  
        SharedPreferencesCompat.apply(editor);  
    }  
  
    /** 
     * 查询某个key是否已经存在 
     * @param context 
     * @param key 
     * @return 
     */  
    public static boolean contains(Context context, String key)  
    {  
        SharedPreferences sp = context.getSharedPreferences(FILE_NAME,  
                Context.MODE_PRIVATE);  
        return sp.contains(key);  
    }  
  
    /** 
     * 返回所有的键值对 
     *  
     * @param context 
     * @return 
     */  
    public static Map<String, ?> getAll(Context context)  
    {  
        SharedPreferences sp = context.getSharedPreferences(FILE_NAME,  
                Context.MODE_PRIVATE);  
        return sp.getAll();  
    }  
  
    /** 
     * 创建一个解决SharedPreferencesCompat.apply方法的一个兼容类 
     *  
     * @author zhy 
     *  
     */  
    private static class SharedPreferencesCompat  
    {  
        private static final Method sApplyMethod = findApplyMethod();  
  
        /** 
         * 反射查找apply的方法 
         *  
         * @return 
         */  
        @SuppressWarnings({ "unchecked", "rawtypes" })  
        private static Method findApplyMethod()  
        {  
            try  
            {  
                Class clz = SharedPreferences.Editor.class;  
                return clz.getMethod("apply");  
            } catch (NoSuchMethodException e)  
            {  
            }  
  
            return null;  
        }  
  
        /** 
         * 如果找到则使用apply执行，否则使用commit 
         *  
         * @param editor 
         */  
        public static void apply(SharedPreferences.Editor editor)  
        {  
            try  
            {  
                if (sApplyMethod != null)  
                {  
                    sApplyMethod.invoke(editor);  
                    return;  
                }  
            } catch (IllegalArgumentException e)  
            {  
            } catch (IllegalAccessException e)  
            {  
            } catch (InvocationTargetException e)  
            {  
            }  
            editor.commit();  
        }  
    }  
}  
```

注意一点，里面所有的commit操作使用了SharedPreferencesCompat.apply进行了替代，目的是尽可能的使用apply代替commit

首先说下为什么，因为commit方法是同步的，并且我们很多时候的commit操作都是UI线程中，毕竟是IO操作，尽可能异步；

所以我们使用apply进行替代，apply异步的进行写入；

但是apply相当于commit来说是new API呢，为了更好的兼容，我们做了适配；

### 4、单位转换类

```
import android.content.Context;  
import android.util.TypedValue;  
  
/** 
 * 常用单位转换的辅助类 
 *  
 *  
 *  
 */  
public class DensityUtils  
{  
    private DensityUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
    }  
  
    /** 
     * dp转px 
     *  
     * @param context 
     * @param val 
     * @return 
     */  
    public static int dp2px(Context context, float dpVal)  
    {  
        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,  
                dpVal, context.getResources().getDisplayMetrics());  
    }  
  
    /** 
     * sp转px 
     *  
     * @param context 
     * @param val 
     * @return 
     */  
    public static int sp2px(Context context, float spVal)  
    {  
        return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP,  
                spVal, context.getResources().getDisplayMetrics());  
    }  
  
    /** 
     * px转dp 
     *  
     * @param context 
     * @param pxVal 
     * @return 
     */  
    public static float px2dp(Context context, float pxVal)  
    {  
        final float scale = context.getResources().getDisplayMetrics().density;  
        return (pxVal / scale);  
    }  
  
    /** 
     * px转sp 
     *  
     * @param fontScale 
     * @param pxVal 
     * @return 
     */  
    public static float px2sp(Context context, float pxVal)  
    {  
        return (pxVal / context.getResources().getDisplayMetrics().scaledDensity);  
    }  
  
}  
```

### 5、SD卡相关辅助类

```
import java.io.File;  
  
import android.os.Environment;  
import android.os.StatFs;  
  
/** 
 * SD卡相关的辅助类 
 *  
 *  
 *  
 */  
public class SDCardUtils  
{  
    private SDCardUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
    }  
  
    /** 
     * 判断SDCard是否可用 
     *  
     * @return 
     */  
    public static boolean isSDCardEnable()  
    {  
        return Environment.getExternalStorageState().equals(  
                Environment.MEDIA_MOUNTED);  
  
    }  
  
    /** 
     * 获取SD卡路径 
     *  
     * @return 
     */  
    public static String getSDCardPath()  
    {  
        return Environment.getExternalStorageDirectory().getAbsolutePath()  
                + File.separator;  
    }  
  
    /** 
     * 获取SD卡的剩余容量 单位byte 
     *  
     * @return 
     */  
    public static long getSDCardAllSize()  
    {  
        if (isSDCardEnable())  
        {  
            StatFs stat = new StatFs(getSDCardPath());  
            // 获取空闲的数据块的数量  
            long availableBlocks = (long) stat.getAvailableBlocks() - 4;  
            // 获取单个数据块的大小（byte）  
            long freeBlocks = stat.getAvailableBlocks();  
            return freeBlocks * availableBlocks;  
        }  
        return 0;  
    }  
  
    /** 
     * 获取指定路径所在空间的剩余可用容量字节数，单位byte 
     *  
     * @param filePath 
     * @return 容量字节 SDCard可用空间，内部存储可用空间 
     */  
    public static long getFreeBytes(String filePath)  
    {  
        // 如果是sd卡的下的路径，则获取sd卡可用容量  
        if (filePath.startsWith(getSDCardPath()))  
        {  
            filePath = getSDCardPath();  
        } else  
        {// 如果是内部存储的路径，则获取内存存储的可用容量  
            filePath = Environment.getDataDirectory().getAbsolutePath();  
        }  
        StatFs stat = new StatFs(filePath);  
        long availableBlocks = (long) stat.getAvailableBlocks() - 4;  
        return stat.getBlockSize() * availableBlocks;  
    }  
  
    /** 
     * 获取系统存储路径 
     *  
     * @return 
     */  
    public static String getRootDirectoryPath()  
    {  
        return Environment.getRootDirectory().getAbsolutePath();  
    }  
}  
```

### 6、屏幕相关辅助类

```
import android.app.Activity;  
import android.content.Context;  
import android.graphics.Bitmap;  
import android.graphics.Rect;  
import android.util.DisplayMetrics;  
import android.view.View;  
import android.view.WindowManager;  
  
/** 
 * 获得屏幕相关的辅助类 
 *  
 *  
 *  
 */  
public class ScreenUtils  
{  
    private ScreenUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
    }  
  
    /** 
     * 获得屏幕高度 
     *  
     * @param context 
     * @return 
     */  
    public static int getScreenWidth(Context context)  
    {  
        WindowManager wm = (WindowManager) context  
                .getSystemService(Context.WINDOW_SERVICE);  
        DisplayMetrics outMetrics = new DisplayMetrics();  
        wm.getDefaultDisplay().getMetrics(outMetrics);  
        return outMetrics.widthPixels;  
    }  
  
    /** 
     * 获得屏幕宽度 
     *  
     * @param context 
     * @return 
     */  
    public static int getScreenHeight(Context context)  
    {  
        WindowManager wm = (WindowManager) context  
                .getSystemService(Context.WINDOW_SERVICE);  
        DisplayMetrics outMetrics = new DisplayMetrics();  
        wm.getDefaultDisplay().getMetrics(outMetrics);  
        return outMetrics.heightPixels;  
    }  
  
    /** 
     * 获得状态栏的高度 
     *  
     * @param context 
     * @return 
     */  
    public static int getStatusHeight(Context context)  
    {  
  
        int statusHeight = -1;  
        try  
        {  
            Class<?> clazz = Class.forName("com.android.internal.R$dimen");  
            Object object = clazz.newInstance();  
            int height = Integer.parseInt(clazz.getField("status_bar_height")  
                    .get(object).toString());  
            statusHeight = context.getResources().getDimensionPixelSize(height);  
        } catch (Exception e)  
        {  
            e.printStackTrace();  
        }  
        return statusHeight;  
    }  
  
    /** 
     * 获取当前屏幕截图，包含状态栏 
     *  
     * @param activity 
     * @return 
     */  
    public static Bitmap snapShotWithStatusBar(Activity activity)  
    {  
        View view = activity.getWindow().getDecorView();  
        view.setDrawingCacheEnabled(true);  
        view.buildDrawingCache();  
        Bitmap bmp = view.getDrawingCache();  
        int width = getScreenWidth(activity);  
        int height = getScreenHeight(activity);  
        Bitmap bp = null;  
        bp = Bitmap.createBitmap(bmp, 0, 0, width, height);  
        view.destroyDrawingCache();  
        return bp;  
  
    }  
  
    /** 
     * 获取当前屏幕截图，不包含状态栏 
     *  
     * @param activity 
     * @return 
     */  
    public static Bitmap snapShotWithoutStatusBar(Activity activity)  
    {  
        View view = activity.getWindow().getDecorView();  
        view.setDrawingCacheEnabled(true);  
        view.buildDrawingCache();  
        Bitmap bmp = view.getDrawingCache();  
        Rect frame = new Rect();  
        activity.getWindow().getDecorView().getWindowVisibleDisplayFrame(frame);  
        int statusBarHeight = frame.top;  
  
        int width = getScreenWidth(activity);  
        int height = getScreenHeight(activity);  
        Bitmap bp = null;  
        bp = Bitmap.createBitmap(bmp, 0, statusBarHeight, width, height  
                - statusBarHeight);  
        view.destroyDrawingCache();  
        return bp;  
  
    }  
  
}  
```

### 7、App相关辅助类

```
import android.content.Context;  
import android.content.pm.PackageInfo;  
import android.content.pm.PackageManager;  
import android.content.pm.PackageManager.NameNotFoundException;  
  
/** 
 * 跟App相关的辅助类 
 *  
 *  
 *  
 */  
public class AppUtils  
{  
  
    private AppUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
  
    }  
  
    /** 
     * 获取应用程序名称 
     */  
    public static String getAppName(Context context)  
    {  
        try  
        {  
            PackageManager packageManager = context.getPackageManager();  
            PackageInfo packageInfo = packageManager.getPackageInfo(  
                    context.getPackageName(), 0);  
            int labelRes = packageInfo.applicationInfo.labelRes;  
            return context.getResources().getString(labelRes);  
        } catch (NameNotFoundException e)  
        {  
            e.printStackTrace();  
        }  
        return null;  
    }  
  
    /** 
     * [获取应用程序版本名称信息] 
     *  
     * @param context 
     * @return 当前应用的版本名称 
     */  
    public static String getVersionName(Context context)  
    {  
        try  
        {  
            PackageManager packageManager = context.getPackageManager();  
            PackageInfo packageInfo = packageManager.getPackageInfo(  
                    context.getPackageName(), 0);  
            return packageInfo.versionName;  
  
        } catch (NameNotFoundException e)  
        {  
            e.printStackTrace();  
        }  
        return null;  
    }  
  
}  
```

### 8、软键盘相关辅助类

```
import android.content.Context;  
import android.view.inputmethod.InputMethodManager;  
import android.widget.EditText;  
  
/** 
 * 打开或关闭软键盘 
 *  
 * @author zhy 
 *  
 */  
public class KeyBoardUtils  
{  
    /** 
     * 打卡软键盘 
     *  
     * @param mEditText 
     *            输入框 
     * @param mContext 
     *            上下文 
     */  
    public static void openKeybord(EditText mEditText, Context mContext)  
    {  
        InputMethodManager imm = (InputMethodManager) mContext  
                .getSystemService(Context.INPUT_METHOD_SERVICE);  
        imm.showSoftInput(mEditText, InputMethodManager.RESULT_SHOWN);  
        imm.toggleSoftInput(InputMethodManager.SHOW_FORCED,  
                InputMethodManager.HIDE_IMPLICIT_ONLY);  
    }  
  
    /** 
     * 关闭软键盘 
     *  
     * @param mEditText 
     *            输入框 
     * @param mContext 
     *            上下文 
     */  
    public static void closeKeybord(EditText mEditText, Context mContext)  
    {  
        InputMethodManager imm = (InputMethodManager) mContext  
                .getSystemService(Context.INPUT_METHOD_SERVICE);  
  
        imm.hideSoftInputFromWindow(mEditText.getWindowToken(), 0);  
    }  
}  
```

### 9、网络相关辅助类

```
import android.app.Activity;  
import android.content.ComponentName;  
import android.content.Context;  
import android.content.Intent;  
import android.net.ConnectivityManager;  
import android.net.NetworkInfo;  
  
/** 
 * 跟网络相关的工具类 
 *  
 *  
 *  
 */  
public class NetUtils  
{  
    private NetUtils()  
    {  
        /* cannot be instantiated */  
        throw new UnsupportedOperationException("cannot be instantiated");  
    }  
  
    /** 
     * 判断网络是否连接 
     *  
     * @param context 
     * @return 
     */  
    public static boolean isConnected(Context context)  
    {  
  
        ConnectivityManager connectivity = (ConnectivityManager) context  
                .getSystemService(Context.CONNECTIVITY_SERVICE);  
  
        if (null != connectivity)  
        {  
  
            NetworkInfo info = connectivity.getActiveNetworkInfo();  
            if (null != info && info.isConnected())  
            {  
                if (info.getState() == NetworkInfo.State.CONNECTED)  
                {  
                    return true;  
                }  
            }  
        }  
        return false;  
    }  
  
    /** 
     * 判断是否是wifi连接 
     */  
    public static boolean isWifi(Context context)  
    {  
        ConnectivityManager cm = (ConnectivityManager) context  
                .getSystemService(Context.CONNECTIVITY_SERVICE);  
  
        if (cm == null)  
            return false;  
        return cm.getActiveNetworkInfo().getType() == ConnectivityManager.TYPE_WIFI;  
  
    }  
  
    /** 
     * 打开网络设置界面 
     */  
    public static void openSetting(Activity activity)  
    {  
        Intent intent = new Intent("/");  
        ComponentName cm = new ComponentName("com.android.settings",  
                "com.android.settings.WirelessSettings");  
        intent.setComponent(cm);  
        intent.setAction("android.intent.action.VIEW");  
        activity.startActivityForResult(intent, 0);  
    }  
  
}  
```

### 10、Http相关辅助类

```
import java.io.BufferedReader;  
import java.io.ByteArrayOutputStream;  
import java.io.IOException;  
import java.io.InputStream;  
import java.io.InputStreamReader;  
import java.io.PrintWriter;  
import java.net.HttpURLConnection;  
import java.net.URL;  
  
/** 
 * Http请求的工具类 
 *  
 * @author zhy 
 *  
 */  
public class HttpUtils  
{  
  
    private static final int TIMEOUT_IN_MILLIONS = 5000;  
  
    public interface CallBack  
    {  
        void onRequestComplete(String result);  
    }  
  
  
    /** 
     * 异步的Get请求 
     *  
     * @param urlStr 
     * @param callBack 
     */  
    public static void doGetAsyn(final String urlStr, final CallBack callBack)  
    {  
        new Thread()  
        {  
            public void run()  
            {  
                try  
                {  
                    String result = doGet(urlStr);  
                    if (callBack != null)  
                    {  
                        callBack.onRequestComplete(result);  
                    }  
                } catch (Exception e)  
                {  
                    e.printStackTrace();  
                }  
  
            };  
        }.start();  
    }  
  
    /** 
     * 异步的Post请求 
     * @param urlStr 
     * @param params 
     * @param callBack 
     * @throws Exception 
     */  
    public static void doPostAsyn(final String urlStr, final String params,  
            final CallBack callBack) throws Exception  
    {  
        new Thread()  
        {  
            public void run()  
            {  
                try  
                {  
                    String result = doPost(urlStr, params);  
                    if (callBack != null)  
                    {  
                        callBack.onRequestComplete(result);  
                    }  
                } catch (Exception e)  
                {  
                    e.printStackTrace();  
                }  
  
            };  
        }.start();  
  
    }  
  
    /** 
     * Get请求，获得返回数据 
     *  
     * @param urlStr 
     * @return 
     * @throws Exception 
     */  
    public static String doGet(String urlStr)   
    {  
        URL url = null;  
        HttpURLConnection conn = null;  
        InputStream is = null;  
        ByteArrayOutputStream baos = null;  
        try  
        {  
            url = new URL(urlStr);  
            conn = (HttpURLConnection) url.openConnection();  
            conn.setReadTimeout(TIMEOUT_IN_MILLIONS);  
            conn.setConnectTimeout(TIMEOUT_IN_MILLIONS);  
            conn.setRequestMethod("GET");  
            conn.setRequestProperty("accept", "*/*");  
            conn.setRequestProperty("connection", "Keep-Alive");  
            if (conn.getResponseCode() == 200)  
            {  
                is = conn.getInputStream();  
                baos = new ByteArrayOutputStream();  
                int len = -1;  
                byte[] buf = new byte[128];  
  
                while ((len = is.read(buf)) != -1)  
                {  
                    baos.write(buf, 0, len);  
                }  
                baos.flush();  
                return baos.toString();  
            } else  
            {  
                throw new RuntimeException(" responseCode is not 200 ... ");  
            }  
  
        } catch (Exception e)  
        {  
            e.printStackTrace();  
        } finally  
        {  
            try  
            {  
                if (is != null)  
                    is.close();  
            } catch (IOException e)  
            {  
            }  
            try  
            {  
                if (baos != null)  
                    baos.close();  
            } catch (IOException e)  
            {  
            }  
            conn.disconnect();  
        }  
          
        return null ;  
  
    }  
  
    /**  
     * 向指定 URL 发送POST方法的请求  
     *   
     * @param url  
     *            发送请求的 URL  
     * @param param  
     *            请求参数，请求参数应该是 name1=value1&name2=value2 的形式。  
     * @return 所代表远程资源的响应结果  
     * @throws Exception  
     */  
    public static String doPost(String url, String param)   
    {  
        PrintWriter out = null;  
        BufferedReader in = null;  
        String result = "";  
        try  
        {  
            URL realUrl = new URL(url);  
            // 打开和URL之间的连接  
            HttpURLConnection conn = (HttpURLConnection) realUrl  
                    .openConnection();  
            // 设置通用的请求属性  
            conn.setRequestProperty("accept", "*/*");  
            conn.setRequestProperty("connection", "Keep-Alive");  
            conn.setRequestMethod("POST");  
            conn.setRequestProperty("Content-Type",  
                    "application/x-www-form-urlencoded");  
            conn.setRequestProperty("charset", "utf-8");  
            conn.setUseCaches(false);  
            // 发送POST请求必须设置如下两行  
            conn.setDoOutput(true);  
            conn.setDoInput(true);  
            conn.setReadTimeout(TIMEOUT_IN_MILLIONS);  
            conn.setConnectTimeout(TIMEOUT_IN_MILLIONS);  
  
            if (param != null && !param.trim().equals(""))  
            {  
                // 获取URLConnection对象对应的输出流  
                out = new PrintWriter(conn.getOutputStream());  
                // 发送请求参数  
                out.print(param);  
                // flush输出流的缓冲  
                out.flush();  
            }  
            // 定义BufferedReader输入流来读取URL的响应  
            in = new BufferedReader(  
                    new InputStreamReader(conn.getInputStream()));  
            String line;  
            while ((line = in.readLine()) != null)  
            {  
                result += line;  
            }  
        } catch (Exception e)  
        {  
            e.printStackTrace();  
        }  
        // 使用finally块来关闭输出流、输入流  
        finally  
        {  
            try  
            {  
                if (out != null)  
                {  
                    out.close();  
                }  
                if (in != null)  
                {  
                    in.close();  
                }  
            } catch (IOException ex)  
            {  
                ex.printStackTrace();  
            }  
        }  
        return result;  
    }  
}  
```

### 11、对话框工具类

```
/**
 *
 * 使用观察者模式来实现确定结果回调
 *
 * 调用方式：
 * AlertDialogUtil dialogUtil = new AlertDialogUtil(context);
 * dialogUtil.showDialog("确定删除已上传的图片？");
 * dialogUtil.setDialogPositiveButtonListener(new AlertDialogUtil.DialogPositiveButtonListener() {
 *
 *      @Override
 *     public void setDialogPositiveButtonListener() {
 *
 *    }
 * });

 */
public class AlertDialogUtil {
    public Context context;
    private DialogPositiveButtonListener listener;

    public AlertDialogUtil(Context context) {
        this.context = context;
    }

    public void showDialog(String message) {
        AlertDialog.Builder dialog = new AlertDialog.Builder(context);
        dialog.setMessage(message);
        dialog.setCancelable(false);//点击框外取消
        dialog.setPositiveButton("确定", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialogInterface, int i) {
                if (listener != null) {
                    listener.setDialogPositiveButtonListener();
                }
            }

        });
        dialog.setNegativeButton("取消", null);
        dialog.show();
    }

    public void setDialogPositiveButtonListener(DialogPositiveButtonListener listener) {
        this.listener = listener;
    }

    public interface DialogPositiveButtonListener {
        void setDialogPositiveButtonListener();
    }
}
```

### 12、文件相关

```
public class FileUtils {

    public static String SDPATH = Environment.getExternalStorageDirectory() + "/formats/";// 获取文件夹
    // 保存图片

    public static boolean saveBitmap(Bitmap mBitmap, String path, String imgName) {
        String sdStatus = Environment.getExternalStorageState();
        if (!sdStatus.equals(Environment.MEDIA_MOUNTED)) { // 检测sd是否可用
            return false;
        }
        FileOutputStream b = null;
        File file = new File(path);
        file.mkdirs();// 创建文件夹
        String fileName = path + imgName;
//        delFile(path, imgName);//删除本地旧图
        try {
            b = new FileOutputStream(fileName);
            mBitmap.compress(Bitmap.CompressFormat.JPEG, 100, b);// 把数据写入文件
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } finally {
            try {
                    b.flush();
                    b.close();

            } catch (IOException e) {
                e.printStackTrace();
            }

        }
        return true;
    }

    public static File createSDDir(String dirName) throws IOException {
        File dir = new File(SDPATH + dirName);
        if (Environment.getExternalStorageState().equals(Environment.MEDIA_MOUNTED)) {

            System.out.println("createSDDir:" + dir.getAbsolutePath());
            System.out.println("createSDDir:" + dir.mkdir());
        }
        return dir;
    }

    public static boolean isFileExist(String fileName) {
        File file = new File(SDPATH + fileName);
        file.isFile();
        return file.exists();
    }

    // 删除文件
    public static void delFile(String path, String fileName) {
        File file = new File(path + fileName);
        if (file.isFile()) {
            file.delete();
        }
        file.exists();
    }

    // 删除文件夹和文件夹里面的文件
    public static void deleteDir() {
        File dir = new File(SDPATH);
        if (dir == null || !dir.exists() || !dir.isDirectory())
            return;

        for (File file : dir.listFiles()) {
            if (file.isFile())
                file.delete(); // 删除所有文件
            else if (file.isDirectory())
                deleteDir(); // 递规的方式删除文件夹
        }
        dir.delete();// 删除目录本身
    }

    public static boolean fileIsExists(String path) {
        try {
            File f = new File(path);
            if (!f.exists()) {
                return false;
            }
        } catch (Exception e) {

            return false;
        }
        return true;
    }

}
```

### 13、时间戳相关

```
public class DateUtils {

    private static SimpleDateFormat sf;
    private static SimpleDateFormat sdf;

    /**
     * 获取系统时间 格式为："yyyy/MM/dd "
     **/
    public static String getCurrentDate() {
        Date d = new Date();
        sf = new SimpleDateFormat("yyyy年MM月dd日");
        return sf.format(d);
    }

    /**
     * 获取系统时间 格式为："yyyy "
     **/
    public static String getCurrentYear() {
        Date d = new Date();
        sf = new SimpleDateFormat("yyyy");
        return sf.format(d);
    }

    /**
     * 获取系统时间 格式为："MM"
     **/
    public static String getCurrentMonth() {
        Date d = new Date();
        sf = new SimpleDateFormat("MM");
        return sf.format(d);
    }

    /**
     * 获取系统时间 格式为："dd"
     **/
    public static String getCurrentDay() {
        Date d = new Date();
        sf = new SimpleDateFormat("dd");
        return sf.format(d);
    }

    /**
     * 获取当前时间戳
     *
     * @return
     */
    public static long getCurrentTime() {
        long d = new Date().getTime() / 1000;
        return d;
    }

    /**
     * 时间戳转换成字符窜
     */
    public static String getDateToString(long time) {
        Date d = new Date(time * 1000);
        sf = new SimpleDateFormat("yyyy年MM月dd日");
        return sf.format(d);
    }

    /**
     * 时间戳中获取年
     */
    public static String getYearFromTime(long time) {
        Date d = new Date(time * 1000);
        sf = new SimpleDateFormat("yyyy");
        return sf.format(d);
    }

    /**
     * 时间戳中获取月
     */
    public static String getMonthFromTime(long time) {
        Date d = new Date(time * 1000);
        sf = new SimpleDateFormat("MM");
        return sf.format(d);
    }

    /**
     * 时间戳中获取日
     */
    public static String getDayFromTime(long time) {
        Date d = new Date(time * 1000);
        sf = new SimpleDateFormat("dd");
        return sf.format(d);
    }

    /**
     * 将字符串转为时间戳
     */
    public static long getStringToDate(String time) {
        sdf = new SimpleDateFormat("yyyy年MM月dd日");
        Date date = new Date();
        try {
            date = sdf.parse(time);
        } catch (ParseException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
        return date.getTime();
    }
}
```

### 14、比较常用的正则表达式验证

```
public class RegularUtils {

    private RegularUtils() {
        throw new UnsupportedOperationException("u can't fuck me...");
    }

    /**
     * 验证手机号（简单）
     */
    private static final String REGEX_MOBILE_SIMPLE = "^[1]\\d{10}$";
    /**
     * 验证手机号（精确）
     * <p>
     * <p>移动：134(0-8)、135、136、137、138、139、147、150、151、152、157、158、159、178、182、183、184、187、188
     * <p>联通：130、131、132、145、155、156、175、176、185、186
     * <p>电信：133、153、173、177、180、181、189
     * <p>全球星：1349
     * <p>虚拟运营商：170
     */
    private static final String REGEX_MOBILE_EXACT = "^((13[0-9])|(14[5,7])|(15[0-3,5-8])|(17[0,3,5-8])|(18[0-9])|(147))\\d{8}$";
    /**
     * 验证座机号,正确格式：xxx/xxxx-xxxxxxx/xxxxxxxx/
     */
    private static final String REGEX_TEL = "^0\\d{2,3}[- ]?\\d{7,8}";
    /**
     * 验证邮箱
     */
    private static final String REGEX_EMAIL = "^\\w+([-+.]\\w+)*@\\w+([-.]\\w+)*\\.\\w+([-.]\\w+)*$";
    /**
     * 验证url
     */
    private static final String REGEX_URL = "http(s)?://([\\w-]+\\.)+[\\w-]+(/[\\w-./?%&=]*)?";
    /**
     * 验证汉字
     */
    private static final String REGEX_CHZ = "^[\\u4e00-\\u9fa5]+$";
    /**
     * 验证用户名,取值范围为a-z,A-Z,0-9,"_",汉字，不能以"_"结尾,用户名必须是6-20位
     */
    private static final String REGEX_USERNAME = "^[\\w\\u4e00-\\u9fa5]{6,20}(?<!_)$";
    /**
     * 验证IP地址
     */
    private static final String REGEX_IP = "((2[0-4]\\d|25[0-5]|[01]?\\d\\d?)\\.){3}(2[0-4]\\d|25[0-5]|[01]?\\d\\d?)";

    //If u want more please visit http://toutiao.com/i6231678548520731137/

    /**
     * @param string 待验证文本
     * @return 是否符合手机号（简单）格式
     */
    public static boolean isMobileSimple(String string) {
        return isMatch(REGEX_MOBILE_SIMPLE, string);
    }

    /**
     * @param string 待验证文本
     * @return 是否符合手机号（精确）格式
     */
    public static boolean isMobileExact(String string) {
        return isMatch(REGEX_MOBILE_EXACT, string);
    }

    /**
     * @param string 待验证文本
     * @return 是否符合座机号码格式
     */
    public static boolean isTel(String string) {
        return isMatch(REGEX_TEL, string);
    }

    /**
     * @param string 待验证文本
     * @return 是否符合邮箱格式
     */
    public static boolean isEmail(String string) {
        return isMatch(REGEX_EMAIL, string);
    }

    /**
     * @param string 待验证文本
     * @return 是否符合网址格式
     */
    public static boolean isURL(String string) {
        return isMatch(REGEX_URL, string);
    }

    /**
     * @param string 待验证文本
     * @return 是否符合汉字
     */
    public static boolean isChz(String string) {
        return isMatch(REGEX_CHZ, string);
    }

    /**
     * @param string 待验证文本
     * @return 是否符合用户名
     */
    public static boolean isUsername(String string) {
        return isMatch(REGEX_USERNAME, string);
    }

    /**
     * @param regex  正则表达式字符串
     * @param string 要匹配的字符串
     * @return 如果str 符合 regex的正则表达式格式,返回true, 否则返回 false;
     */
    public static boolean isMatch(String regex, String string) {
        return !TextUtils.isEmpty(string) && Pattern.matches(regex, string);
    }
}
```




