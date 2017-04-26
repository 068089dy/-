刚开始以为用videoview就能搞定：
{% highlight java %}
public class MainActivity extends AppCompatActivity {
    String url = "http://hdl1a.douyucdn.cn/live/126493r9JT3vr55u.flv?wsAuth=1f3918b0bc3f31fda18f930b8eeb8c67&token=cpn-dotamax-0-126493-6a8137f04e260cbaee45fc1c945ada74&logo=0&expire=0&wshc_tag=0&wsts_tag=58a65e3e&wsid_tag=7d3e14a4&wsiphost=ipdbm";
    private VideoView mVideoView;

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        setContentView(R.layout.activity_main);

        mVideoView = (VideoView) findViewById(R.id.my_video_view);

        MediaController mediaController = new MediaController(this);
        mVideoView.setMediaController(mediaController);
        mediaController.setAnchorView(mVideoView);
        mVideoView.setVideoURI(Uri.parse(url));

        mVideoView.requestFocus();
        mVideoView.setOnPreparedListener(new MediaPlayer.OnPreparedListener() {
            @Override
            public void onPrepared(MediaPlayer mp) {
                mVideoView.start();
            }
        });
    }
}
{%endhuighlight%}

然而不行，可能是videoview支持的视频格式太少，或者不能播放流媒体。于是就在github上找到一个流媒体播放器，

导入moudle：
修改工程的build.gradle,
ext {
    compileSdkVersion = 23
    buildToolsVersion = "23.0.0"

    targetSdkVersion = 23

    versionCode = 405001
    versionName = "0.4.5.1"
}
