# DavidTimerDown
【Android】Android开发小功能，倒计时的实现。时间计时器倒计时功能。
作者：程序员小冰，GitHub主页：https://github.com/QQ986945193 
新浪微博：http://weibo.com/mcxiaobing 
首先给大家看一下我们今天这个最终实现的效果图： 
![这里写图片描述](http://img.blog.csdn.net/20160910161033456)

界面效果有点丑，不过功能齐全。大家如果需要，可以集成到自己的项目中。
首先说明，其实这个很简单的，利用了一个定时器而已。不过我这里进行了
输入的时间进行判断，比如不是Int类型均不可以进行倒计时。还有防止
多次点击开始计时，最后就是进行了回收对象。好了给大家看一下代码吧。
xml布局比较简单，几个button与textview，edittext实现的。

```
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <EditText
        android:id="@+id/et_timer"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入计时时间" />

    <Button
        android:id="@+id/btn_get"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="获取计时时间" />

    <TextView
        android:id="@+id/tv_time"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="计时时间" />

    <Button
        android:id="@+id/btn_go"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="开始计时" />

    <Button
        android:id="@+id/btn_stop"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="停止计时" />


</LinearLayout>

```

下面是java类中的代码：

```
package davidtimerdown.qq986945193.com.davidtimerdown;


import java.util.Timer;
import java.util.TimerTask;

import android.app.Activity;
import android.os.Bundle;
import android.os.Handler;
import android.os.Message;
import android.view.View;
import android.view.View.OnClickListener;
import android.widget.Button;
import android.widget.EditText;
import android.widget.TextView;
import android.widget.Toast;

/**
 * @author ：程序员小冰
 * @新浪微博 ：http://weibo.com/mcxiaobing
 * @GitHub: https://github.com/QQ986945193
 * @CSDN博客: http://blog.csdn.net/qq_21376985
 * @码云OsChina ：http://git.oschina.net/MCXIAOBING
 */
public class MainActivity extends Activity implements OnClickListener {
    protected static final int MSG_WHAT = 0;
    //开始倒计时
    private Button btn_go;
    //获取到输入的时间
    private Button btn_get;
    //停止倒计时
    private Button btn_stop;
    //输入的时间
    private static EditText et_time;
    //时间
    private static TextView tv_time;

    private Timer timer;
    private static int time;

    private Handler mHandler = new Handler() {
        public void handleMessage(Message msg) {
            tv_time.setText(time + "");
            switch (msg.what) {
                case MSG_WHAT:
                    if (time > 0) {
                        time--;
                    } else {
                        Toast.makeText(MainActivity.this, "倒计时完成", Toast.LENGTH_SHORT).show();
                        if (timer != null) {

                            timer.cancel();
                            timer = null;
                        }
                    }
                    break;

                default:
                    break;
            }
        }

        ;
    };


    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        btn_go = (Button) findViewById(R.id.btn_go);
        btn_get = (Button) findViewById(R.id.btn_get);
        btn_stop = (Button) findViewById(R.id.btn_stop);
        et_time = (EditText) findViewById(R.id.et_timer);
        tv_time = (TextView) findViewById(R.id.tv_time);
        btn_go.setOnClickListener(this);
        btn_get.setOnClickListener(this);
        btn_stop.setOnClickListener(this);

    }

    @Override
    public void onClick(View v) {
        // TODO Auto-generated method stub
        switch (v.getId()) {
            //获得倒计时间
            case R.id.btn_get:
                if (et_time.getText().toString().trim() != null && !(et_time.getText().toString().trim().equals(""))) {
                    try {
                        time = Integer.parseInt(et_time.getText().toString());
                        tv_time.setText(et_time.getText().toString());
                    } catch (NumberFormatException e) {
                        e.printStackTrace();
                        Toast.makeText(MainActivity.this, "时间格式不正确", Toast.LENGTH_SHORT).show();
                        return;
                    }
                }
                break;
            //开始倒计时
            case R.id.btn_go:
                if (timer == null) {
                    timer = new Timer();
                    timer.schedule(new TimerTask() {

                        @Override
                        public void run() {
                            // TODO Auto-generated method stub
                            mHandler.sendEmptyMessage(MSG_WHAT);

                        }
                    }, 0, 1000);
                } else {
                    Toast.makeText(MainActivity.this, "请勿多次点击", Toast.LENGTH_SHORT).show();
                    return;
                }

                break;
            //停止倒计时
            case R.id.btn_stop:
                if (timer != null) {

                    timer.cancel();
                    timer = null;
                }
                break;

            default:
                break;
        }
    }

}

```
好了，介绍到此结束。如果此Demo帮到了你，欢迎fork，star。
