##DropDownMenu
筛选器. 尽管之前有很多人写,站在别人基础上重新写了一版.因为公司筛选器逻辑十分复杂,而且各种数据model不一样.
现在的代码已经很清晰明了,模块分明.

##特点
1. 使用 Adapter模式 添加筛选器条目.使代码清晰可见,便于维护.
2. 提供三种泛型View类, 单列ListView,双列ListView 和 单个GridView, sample中还提供了两个GridView的示例.(用泛型是因为公司项目多个地方的筛选器model不一样,泪奔)
3. 自己写FilterCheckedView,支持checked属性,使用selector就可配置选中样式.配合AbsListView.setChecked()使用
4. 使用FilterUrl作为中介,toString()方法获取到所拼接url,隔离了数据和View,使代码更清晰.

##ScreenShot
![DropDownMenu](images/dropDownMenu.gif "Gif Example")



##使用
布局文件
```
    <com.baiiu.filter.DropDownMenu
        android:id="@+id/filterDropDownView"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <TextView
            android:id="@id/mFilterContentView" //mFilterContentView标识为其内部内容,可换为RecyclerView等. id必填.
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:gravity="center_vertical"
            android:textSize="22sp" />
    </com.baiiu.filter.DropDownMenu>
```

代码中:
```
    //代码中设置Adapter.
    dropDownView.setMenuAdapter(new DropMenuAdapter(this, titleList));
```

添加SingleListView
```
    SingleListView<String> singleListView = new SingleListView<String>(mContext)
            .adapter(new SimpleTextAdapter<String>(null, mContext) {
                @Override
                public String provideText(String string) {
                    return string;
                }
            })
            .onItemClick(new OnFilterItemClickListener<String>() {
                @Override
                public void onItemClick(String item) {
                    FilterUrl.instance().singleListPosition = item;

                    FilterUrl.instance().position = 0;
                    FilterUrl.instance().positionTitle = item;

                    if (onFilterDoneListener != null) {
                        onFilterDoneListener.onFilterDone(0, "", "");
                    }
                }
            });
            
    //初始化数据
    singleListView.setList(list, -1);
```



