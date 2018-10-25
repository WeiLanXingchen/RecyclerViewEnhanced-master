# RecyclerViewEnhanced
Android RecyclerView库提供滑动、点击等其他功能


原链接地址：https://github.com/nikhilpanju/RecyclerViewEnhanced
## 用法

将其添加到build.gradle文件中

```
dependencies {
  compile 'com.nikhilpanju.recyclerviewenhanced:recyclerviewenhanced:1.1.0'
}
```

## 特征
* 支持API 14+（未经过测试的早期API）
* 支持“滑动选项”的任何视图
* 不需要任何新适配器或新视图。适用于任何现有的RecyclerViews。
* 需要添加 `OnItemTouchListener` 到 RecyclerView
* 支持单击和滑动功能。
* 支持禁用特定项目/行的单击和滑动。
* 支持 `independentViews` 您的item/行（有关详细信息，请参阅下文）
* 支持 `fadeViews` 您的item/行（有关详细信息，请参阅下文）

## 演示
构建示例应用程序以尝试 RecyclerViewEnhanced
![alt text](https://github.com/nikhilpanju/RecyclerViewEnhanced/blob/master/sample/src/common/images/Demo.gif "Demo")

## 配置
* #### 创建一个实例 `RecyclerTouchListener`
  `onTouchListener = new RecyclerTouchListener(this, mRecyclerView);`
  
* #### 设置 `IndependentViews` 和 `FadeViews` (如果需要的话)
  `IndependentViews` 是可以从整行单独单击的视图。他们的点击与行点击具有不同的功能。 `FadeViews` 当行被分别打开关闭和打开时，是淡入和淡出的视图。
  
  ```
  onTouchListener.setIndependentViews(R.id.rowButton)
                 .setViewsToFade(R.id.rowButton)               
  ```
  
* #### Implement `OnRowClickListener` 使用 `setClickable()`
  `setClickable()` 可以点击recyclerView的item和 `IndependentViews`
  
  ```
  .setClickable(new RecyclerTouchListener.OnRowClickListener() {
            @Override
            public void onRowClicked(int position) {
                // Do something
            }

            @Override
            public void onIndependentViewClicked(int independentViewID, int position) {
                // Do something
            }
        })               
  ```
  
* #### 启用滑动功能

  设置需要单击侦听器的视图，并使用启用滑动`setSwipeable()`
  ```
  .setSwipeOptionViews(R.id.add, R.id.edit, R.id.change)
  .setSwipeable(R.id.rowFG, R.id.rowBG, new RecyclerTouchListener.OnSwipeOptionsClickListener() {
            @Override
            public void onSwipeOptionClicked(int viewID, int position) {
                if (viewID == R.id.add) {
                    // Do something
                } else if (viewID == R.id.edit) {
                    // Do something
                } else if (viewID == R.id.change) {
                    // Do something
                }
           }
       });
  ```
  
* #### 将侦听器添加到 RecyclerView

  在 `onResume()` 添加监听器: 
  ```
  mRecyclerView.addOnItemTouchListener(onTouchListener);
  ```
  在 `onPause()` 删除监听: 
  ```
  mRecyclerView.removeOnItemTouchListener(onTouchListener);
  ```
       
## 附加功能
* 使用 `onRowLongClickListener` 接收长按事件
  ```
  .setLongClickable(true, new RecyclerTouchListener.OnRowLongClickListener() {
                    @Override
                    public void onRowLongClicked(int position) {
                        ToastUtil.makeToast(getApplicationContext(), "Row " + (position + 1) + " long clicked!");
                    }
                })
  ```
  
* 使用 `setUnSwipeableRows()` 从刷卡禁用某些行。在尝试滑动不可擦除的行时，使用此选项还会显示“难以滑动”的动画。
* 使用 `setUnClickableRows()` 禁用点击某些行的行动。（注意：这也可以防止单击independentViews）。
* `openSwipeOptions()` 打开特定行的滑动选项。
* `closeVisibleBG()`关闭所有打开选项。
* Implement `OnSwipeListener` 获取 `onSwipeOptionsClosed()` 和 `onSwipeOptionsOpened()` 事件.

  
### 单击recyclelerView外部的任何位置时关闭滑动选项：
* 让您的Activity implement `RecyclerTouchListener.RecyclerTouchListenerHelper` 并存储touchListener
```
private OnActivityTouchListener touchListener;

@Override
public void setOnActivityTouchListener(OnActivityTouchListener listener) {
    this.touchListener = listener;
}
```
* Override `dispatchTouchEvent()` 您的Activity并将 `MotionEvent` 变量传递给`touchListener`
```
@Override
public boolean dispatchTouchEvent(MotionEvent ev) {
    if (touchListener != null) touchListener.getTouchCoordinates(ev);
        return super.dispatchTouchEvent(ev);
}
```
## 作者
* Nikhil Panju ([Github](https://github.com/nikhilpanju))


## 执照
版权所有2016 Nikhil Panju

根据Apache许可证2.0版（“许可证”）获得许可; 除非符合许可，否则您不得使用此文件。您可以在以下位置获取许可证副本

([http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0))

除非适用法律要求或书面同意，否则根据许可证分发的软件按“原样”分发，不附带任何明示或暗示的担保或条件。有关管理许可下的权限和限制的特定语言，请参阅许可证。
