

#### 基础

首先在Modlue项目工程build配置文件里添加RecyclerView远程依赖库，如下:

```
 implementation 'com.android.support:recyclerview-v7:28.0.0'
```

##### RecylerView基本使用流程


- 初始化控件
- 设置布局管理器 LayoutManager
- 设置 adapter、ViewHolder
- 添加分割线 ItemDecoration
- 设置Item的增删动画 ItemAnimator
- 添加Item的点击、长按事件 （需要自己实现）



```java
//初始化控件
mRecyclerView = findView(R.id.id_recyclerview);
//设置布局管理器
mRecyclerView.setLayoutManager(layout);
//设置adapter
mRecyclerView.setAdapter(adapter)
//设置Item增加、移除动画
mRecyclerView.setItemAnimator(new DefaultItemAnimator());
//添加分割线
mRecyclerView.addItemDecoration(new DividerItemDecoration(
                getActivity(), DividerItemDecoration.HORIZONTAL_LIST));

```


##### 布局管理器

- LinearLayoutManager
- GridLayoutManager
- StaggeredGridLayoutManager

###### LinearLayoutManager基本使用


###### GridLayoutManager基本使用


###### LayoutManager自定义


##### 分割线ItemDecoration


##### Item的增删动画ItemAnimator


##### Item的点击事件


#### 进阶