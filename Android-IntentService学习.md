

```java
/**
 * 开启一个子线程来运行服务,来处理异步的任务请求
 * An {@link IntentService} subclass for handling asynchronous task requests in
 * a service on a separate handler thread.
 * <p/>
 */
public class TestIntentService extends IntentService {
    // TODO: Rename actions, choose action names that describe tasks that this
    // IntentService can perform, e.g. ACTION_FETCH_NEW_ITEMS
    private static final String ACTION_DOWNLOAD = "com.mywjch.learnrxjava.action.FOO";
    private static final String ACTION_BAZ = "com.mywjch.learnrxjava.action.BAZ";
    // TODO: Rename parameters
    private static final String EXTRA_PARAM1 = "com.mywjch.learnrxjava.extra.PARAM1";
    private static final String EXTRA_PARAM2 = "com.mywjch.learnrxjava.extra.PARAM2";
    /**
     * 这是一个帮助方法,你调用这个方法时,会使用ACTION_DOWNLOAD这个Action来启动服务来执行它,
     * 如果服务已经在运行,并且在执行其他任务了,那么就会把这个Action放进队列,接下来执行.
     * Starts this service to perform action Foo with the given parameters. If
     * the service is already performing a task this action will be queued.
     */
    // TODO: Customize helper method
    public static void startActionDownload(Context context, String param1, String param2) {
        Intent intent = new Intent(context, TestIntentService.class);
        intent.setAction(ACTION_DOWNLOAD);
        //把传进来的参数放进Intent,进行处理
        intent.putExtra(EXTRA_PARAM1, param1);
        intent.putExtra(EXTRA_PARAM2, param2);
        //启动服务
        context.startService(intent);
    }
    /**
     * 与上面类似
     * Starts this service to perform action Baz with the given parameters. If
     * the service is already performing a task this action will be queued.
     */
    // TODO: Customize helper method
    public static void startActionBaz(Context context, String param1, String param2) {
        Intent intent = new Intent(context, TestIntentService.class);
        intent.setAction(ACTION_BAZ);
        intent.putExtra(EXTRA_PARAM1, param1);
        intent.putExtra(EXTRA_PARAM2, param2);
        context.startService(intent);
    }
    public TestIntentService() {
        super("TestIntentService");
    }
    /**
     * 在IntentService的实现会回调onHandleIntent方法
     */
    @Override
    protected void onHandleIntent(Intent intent) {
        if (intent != null) {
            final String action = intent.getAction();
            //这里判断是哪一个Action
            if (ACTION_DOWNLOAD.equals(action)) {
                final String param1 = intent.getStringExtra(EXTRA_PARAM1);
                final String param2 = intent.getStringExtra(EXTRA_PARAM2);
                //然后直接随便调用个方法就可以了
                suibiandiaoyonggefangfa(param1, param2);
            } else if (ACTION_BAZ.equals(action)) {
                final String param1 = intent.getStringExtra(EXTRA_PARAM1);
                final String param2 = intent.getStringExtra(EXTRA_PARAM2);
                handleActionBaz(param1, param2);
            }
        }
    }
    /**
     *会在backgroung线程里处理这个方法
     */
    private void suibiandiaoyonggefangfa(String param1, String param2) {
        // TODO: 这里来实现这个方法就可以了
        Thread.sleep(3000);
        //然后发广播返回处理结果
        Intent intent =new Intent(HandlerActivity.UPLOAD_RESULT);
        intent.putExtra(ACTION_PATH, path);
        sendBroadcast(intent);
    }
    /**
     * Handle action Baz in the provided background thread with the provided
     * parameters.
     */
    private void handleActionBaz(String param1, String param2) {
        // TODO: Handle action Baz
        throw new UnsupportedOperationException("Not yet implemented");
    }
    @Override
    public void onDestroy() {
        super.onDestroy();
        //队列中的任务完成后IntentService底层会直接调用stopSelf(msg.arg1);来关闭自己
        Log.e("TAG","OnDeatory");
    }
}
```
