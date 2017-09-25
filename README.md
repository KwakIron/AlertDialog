# AlertDialog
```
public class MainActivity extends AppCompatActivity implements View.OnClickListener{
    private Button btn_dialog_one;
    private Button btn_dialog_two;
    private Button btn_dialog_three;
    private Button btn_dialog_four;
    private Button btn_show;
    private View view_custom;

    private Context mContext;
    private boolean[] checkItems;

    private AlertDialog alert = null;
    private AlertDialog.Builder builder = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        mContext = MainActivity.this;
        bindView();




    }

    private void bindView() {
        btn_dialog_one = (Button) findViewById(R.id.btn_dialog_one);
        btn_dialog_two = (Button) findViewById(R.id.btn_dialog_two);
        btn_dialog_three = (Button) findViewById(R.id.btn_dialog_three);
        btn_dialog_four = (Button) findViewById(R.id.btn_dialog_four);
        btn_show = (Button) findViewById(R.id.btn_show);
        btn_show.setOnClickListener(this);
        btn_dialog_one.setOnClickListener(this);
        btn_dialog_two.setOnClickListener(this);
        btn_dialog_three.setOnClickListener(this);
        btn_dialog_four.setOnClickListener(this);
    }

    @Override
    public void onClick(View view) {
        switch (view.getId()){
            case R.id.btn_dialog_one:
                alert = null;
                builder = new AlertDialog.Builder(mContext);
                alert = builder.setIcon(R.mipmap.ic_icon_fish)
                        .setTitle("系统提示：")
                        .setMessage("这是一个最普通的AlertDialog,\n带有三个按钮，分别是取消，中立和确定")
                        .setNegativeButton("取消", new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                Toast.makeText(mContext,"你点击了取消按钮~",Toast.LENGTH_SHORT).show();
                            }
                        })
                        .setPositiveButton("确定", new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                Toast.makeText(mContext,"你点击了确定按钮~",Toast.LENGTH_SHORT).show();
                            }
                        })
                        .setNeutralButton("中立", new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialogInterface, int i) {
                                Toast.makeText(mContext,"你点击了中立按钮~",Toast.LENGTH_SHORT).show();
                            }
                        }).create();
                alert.show();
                break;
            case R.id.btn_dialog_two:
                final String[] lesson = new String[]{"语文", "数学", "英语", "化学", "生物", "物理", "体育"};
                alert = null;
                builder = new AlertDialog.Builder(mContext);
                alert = builder.setIcon(R.mipmap.ic_icon_fish)
                        .setTitle("选择你喜欢的课程")
                        .setItems(lesson, new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialog, int which) {
                                Toast.makeText(getApplicationContext(), "你选择了" + lesson[which], Toast.LENGTH_SHORT).show();
                            }
                        }).create();
                alert.show();
                break;
            //单选列表对话框
            case R.id.btn_dialog_three:
                final String[] fruits = new String[]{"苹果", "雪梨", "香蕉", "葡萄", "西瓜"};
                alert = null;
                builder = new AlertDialog.Builder(mContext);
                alert = builder.setIcon(R.mipmap.ic_icon_fish)
                        .setTitle("选择你喜欢的水果，只能选一个哦~")
                        .setSingleChoiceItems(fruits, 0, new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialog, int which) {
                                Toast.makeText(getApplicationContext(), "你选择了" + fruits[which], Toast.LENGTH_SHORT).show();
                            }
                        }).create();
                alert.show();
                break;
            //多选列表对话框
            case R.id.btn_dialog_four:
                final String[] menu = new String[]{"水煮豆腐", "萝卜牛腩", "酱油鸡", "胡椒猪肚鸡"};
                //定义一个用来记录个列表项状态的boolean数组
                checkItems = new boolean[]{false, false, false, false};
                alert = null;
                builder = new AlertDialog.Builder(mContext);
                alert = builder.setIcon(R.mipmap.ic_icon_fish)
                        .setMultiChoiceItems(menu, checkItems, new DialogInterface.OnMultiChoiceClickListener() {
                            @Override
                            public void onClick(DialogInterface dialog, int which, boolean isChecked) {
                                checkItems[which] = isChecked;
                            }
                        })
                        .setPositiveButton("确定", new DialogInterface.OnClickListener() {
                            @Override
                            public void onClick(DialogInterface dialog, int which) {
                                String result = "";
                                for (int i = 0; i < checkItems.length; i++) {
                                    if (checkItems[i])
                                        result += menu[i] + " ";
                                }
                                Toast.makeText(getApplicationContext(), "客官你点了:" + result, Toast.LENGTH_SHORT).show();
                            }
                        })
                        .create();
                alert.show();
                break;
            case R.id.btn_show:
                alert = null;
                final LayoutInflater inflater = MainActivity.this.getLayoutInflater();
                view_custom = inflater.inflate(R.layout.view_dialog_custom,null,false);
                builder = new AlertDialog.Builder(mContext);
                builder.setView(view_custom);
                builder.setCancelable(false);
                alert = builder.create();
                customView();
                alert.show();
                break;
        }
    }
    private void customView(){
        view_custom.findViewById(R.id.btn_cancle).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                alert.dismiss();
            }
        });
        view_custom.findViewById(R.id.btn_blog).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(getApplicationContext(), "访问博客", Toast.LENGTH_SHORT).show();
                Uri uri = Uri.parse("http://blog.csdn.net/coder_pig");
                Intent intent = new Intent(Intent.ACTION_VIEW, uri);
                startActivity(intent);
                alert.dismiss();
            }
        });
        view_custom.findViewById(R.id.btn_close).setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(getApplicationContext(), "对话框已关闭~", Toast.LENGTH_SHORT).show();
                alert.dismiss();
            }
        });

    }
}
