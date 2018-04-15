- **前言**

RecyclerView is a more advanced and flexible version of ListView. This widget is a container for large sets of views that can be recycled and scrolled very efficiently. Use the RecyclerView widget when you have lists with elements that change dynamically. 
RecyclerView是谷歌V7包下新增的控件,用来替代ListView的使用，更加先进和灵活，居然google这样说了，我们肯定得要了解下它和listview区别以及优势。

- **实现**

在gradle文件中添加包的引用（配合cardview使用效果更好）：

```
  compile 'com.android.support:cardview-v7:25.3.0'
  compile 'com.android.support:recyclerview-v7:25.3.0'
```
在XML文件用使用它：

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.RecyclerView
    android:id="@+id/linear_recycler_view"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```
这个时候不得不和listView的使用比较下了：

```
<ListView
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:cacheColorHint="#00000000" // 每次基本都需要自己设置下，功能很鸡肋
    android:divider="#00000000"     
    android:dividerHeight="0dp" 
    android:listSelector="#00000000" />
```

接下来就是它的使用了，它也需要的设置Adapter，但会多一个设置LayoutManager，默认提供的 RecyclerView 就能支持线性布局、网格布局、瀑布流布局三种，而且同时还能够控制横向还是纵向滚动，相比listview要灵活多了，代码如下所示：

```
public class MainActivity extends ActionBarActivity {
    @InjectView(R.id.recycler_view)
    RecyclerView mRecyclerView;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        ButterKnife.inject(this);
 
        mRecyclerView.setLayoutManager(new LinearLayoutManager(this));//这里用线性显示 类似于listview
//        mRecyclerView.setLayoutManager(new GridLayoutManager(this, 2));//这里用线性宫格显示 类似于grid view
//        mRecyclerView.setLayoutManager(new StaggeredGridLayoutManager(2, OrientationHelper.VERTICAL));//这里用线性宫格显示 类似于瀑布流
        mRecyclerView.setAdapter(new NormalRecyclerViewAdapter(this));
    }
}
```

以下是Adapter的写法：

```
public class NormalRecyclerViewAdapter extends RecyclerView.Adapter<NormalRecyclerViewAdapter.NormalTextViewHolder> {
    private final LayoutInflater mLayoutInflater;
    private final Context mContext;
    private String[] mTitles;
 
    public NormalRecyclerViewAdapter(Context context) {
        mTitles = context.getResources().getStringArray(R.array.titles);
        mContext = context;
        mLayoutInflater =LayoutInflater.from(context);
    }
 
    @Override
    public NormalTextViewHolder onCreateViewHolder(ViewGroup parent, int viewType) { // 需要实现的新方法
        return new NormalTextViewHolder(mLayoutInflater.inflate(R.layout.item_text, parent, false));
    }
 
    @Override
    public void onBindViewHolder(NormalTextViewHolder holder, int position) { // 需要实现的新方法
        holder.mTextView.setText(mTitles[position]);
    } @Override
    public int getItemCount() {
        return mTitles == null ? 0 : mTitles.length;
    }
 
    public static class NormalTextViewHolder extends RecyclerView.ViewHolder { // 添加ViewHolder类
        @InjectView(R.id.text_view)
        TextView mTextView;
 
        NormalTextViewHolder(View view) {
            super(view);
            ButterKnife.inject(this, view);
            view.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Log.d("NormalTextViewHolder", "onClick--> position = " + getPosition());
                }
            });
        }
    }
}
```
这个时候又需要和listView做下比较了, ListView也需要写ViewHolder，在getView()中需要自己写复用view的代码，而RecyclerView已经有系统帮我们做了，代码显得更加简洁了：

```
public View getView(final int position, View view, ViewGroup arg2) {
    ViewHolder viewHolder = null;
    if (view == null) {
        viewHolder = new ViewHolder();
        view = LayoutInflater.from(mContext).inflate(R.layout.school_item, null);
        viewHolder.tvoninfo = (TextView) view.findViewById(R.id.tvoninfo);
        view.setTag(viewHolder);
    } else {
        viewHolder = (ViewHolder) view.getTag();
    }
    viewHolder.tvoninfo.setText(this.list.get(position).getName());
    return view;
 
}

final static class ViewHolder {
    TextView  tvoninfo;
}
```
完成这一步，简单的RecyclerView就实现了，是不是so easy~！~！

接下来就会想想ListView还有哪些常用的功能，必不可少的当然是item的点击事件，ListView直接调用setOnItemClickListener()即可，第一感觉RecyclerView肯定也会有，输入后找不到，然后以为google改了名称，然后就查找API，我靠，google居(就)然(是)没(这)有(么)提(任)供(性)，那没有办法，只能自己实现了，其实也不是很麻烦，以下是实现代码：

```
public interface OnItemClickListener {
    void onItemClick(int position);
}
  
public void setOnItemClickListener(OnItemClickListener listener) {
    mListener = listener;
}
 
public static class GridViewHolder extends RecyclerView.ViewHolder {
 
    @BindView(R.id.tv_name)
    TextView nameTv;
 
    public GridViewHolder(View itemView) {
        super(itemView);
        ButterKnife.bind(this, itemView);
        itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(mContext, "OnClick ---> " + getPosition(), Toast.LENGTH_SHORT).show();;
                if (mListener != null) {
                    mListener.onItemClick(getAdapterPosition());
                }
            }
        });
    }
}
```

接下来我们一般常用的还有添加HeaderView和FooterView，结果搜索一番，又无功而返，google又没有提供，没办法，只能自己实现了，其实根据ListView的添加HeaderView和FooterView的经验可以得知，它们也是作为一个Item的，以下是实现代码：


```
@Override
public int getItemViewType(int position) {
    if (mHeaderView == null && mFooterView == null) {
        return TYPE_ITEM;
    }
 
    if (mHeaderView != null && 0 == position) { // 添加header
        return TYPE_HEADER;
    }
 
    if (mFooterView != null && getItemCount() - 1 == position) { // 添加footer
        return TYPE_FOOTER;
    }
 
    return TYPE_ITEM;
}
 
@Override
public int getItemCount() {
    if (mNames == null) {
        return 0;
    }
    if (mHeaderView == null && mFooterView == null) {
        return mNames.size();
    }
    if (mHeaderView != null && mFooterView != null) {
        return mNames.size() + 2;
    }
    return mNames.size() + 1;
}
  
public class HeaderViewHolder extends RecyclerView.ViewHolder {
 
    @BindView(R.id.iv_header)
    ImageView headerIv;
 
    public HeaderViewHolder(View itemView) {
        super(itemView);
        ButterKnife.bind(this, itemView);
        headerIv.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(mContext, "OnClick ---> " + getAdapterPosition(), Toast.LENGTH_SHORT).show();
            }
        });
    }
}
 
public class FooterViewHolder extends RecyclerView.ViewHolder {
 
    public FooterViewHolder(View itemView) {
        super(itemView);
        itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(mContext, "OnClick ---> Footer" + getAdapterPosition(), Toast.LENGTH_SHORT).show();
            }
        });
    }
}
  
public void setHeaderView(View headerView) {
    mHeaderView = headerView;
    notifyDataSetChanged();
}
 
public void setFooterView(View footerView) {
    mFooterView = footerView;
    notifyDataSetChanged();
}

```

接下来再介绍下RecyclerView的添加和删除，特别让我们眼前一亮的是，提供默认的删除效果，你只需这样调用：


```
public void addItem(int position) {
    if (mNames != null) {
        mNames.add(position, "Jam");
        notifyItemInserted(position);
    }
}
 
public void deleteItem(int position) {
    if (mNames != null && mNames.size() > position) {
        mNames.remove(position);
        notifyItemRemoved(position);
    }
}
```
居然有了删除效果，大家在这个时候肯定会怀念左滑或右滑这种高级的删除功能，那么用RecyclerView该如何实现了？你只需要使用ItemTouchHelper，实现起来真不要太容易哦：

```
new ItemTouchHelper(new ItemTouchHelper.Callback() {
    @Override
    public int getMovementFlags(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {
        int dragFlags = ItemTouchHelper.UP | ItemTouchHelper.DOWN; // 允许上下拖动
        int swipeFlags = ItemTouchHelper.LEFT; // 允许左滑
        return makeMovementFlags(dragFlags, swipeFlags);
    }
 
    @Override
    public boolean onMove(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder, RecyclerView.ViewHolder target) {
        mLinearRecyclerAdapter.onItemMove(viewHolder.getAdapterPosition(), target.getAdapterPosition());
        return true;
    }
 
    @Override
    public void onSwiped(RecyclerView.ViewHolder viewHolder, int direction) {
        mLinearRecyclerAdapter.onItemDissmiss(viewHolder.getAdapterPosition());
    }
}).attachToRecyclerView(mLinearRecyclerView);
```
接下来介绍一下ItemDecoration，它是一个item的一个装饰效果，以下是实现item垂直和水平分隔线的代码：

```
public class DefaultItemDecoration extends RecyclerView.ItemDecoration {
    private final Drawable mDivider;
    private final int mSize;
    private final int mOrientation;
 
    public DefaultItemDecoration(Resources res, @ColorRes int color,
                                 @DimenRes int size, int orientation) {
        mDivider = new ColorDrawable(res.getColor(color));
        mSize = res.getDimensionPixelSize(size);
        mOrientation = orientation;
    }
 
    @Override
    public void onDrawOver(Canvas c, RecyclerView parent, RecyclerView.State state) {
        int left;
        int right;
        int top;
        int bottom;
        if (mOrientation == LinearLayoutManager.HORIZONTAL) {
            top = parent.getPaddingTop();
            bottom = parent.getHeight() - parent.getPaddingBottom();
            final int childCount = parent.getChildCount();
            for (int i = 0; i < childCount - 1; i++) {
                final View child = parent.getChildAt(i);
                final RecyclerView.LayoutParams params =
                        (RecyclerView.LayoutParams) child.getLayoutParams();
                left = child.getRight() + params.rightMargin;
                right = left + mSize;
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(c);
            }
        } else {
            left = parent.getPaddingLeft();
            right = parent.getWidth() - parent.getPaddingRight();
            final int childCount = parent.getChildCount();
            for (int i = 0; i < childCount - 1; i++) {
                final View child = parent.getChildAt(i);
                final RecyclerView.LayoutParams params =
                        (RecyclerView.LayoutParams) child.getLayoutParams();
                top = child.getBottom() + params.bottomMargin;
                bottom = top + mSize;
                mDivider.setBounds(left, top, right, bottom);
                mDivider.draw(c);
            }
        }
    }
 
    @Override
    public void getItemOffsets(Rect outRect, View view, RecyclerView parent,
                               RecyclerView.State state) {
        if (mOrientation == LinearLayoutManager.HORIZONTAL) {
            outRect.set(0, 0, mSize, 0);
        } else {
            outRect.set(0, 0, 0, mSize);
        }
    }

```    
最后对于有些App，可能需要做一些酷炫的效果，那么你就可以尝试使用ItemAnimator，recyclerview-animators这个是一个不错的开源库，可以借鉴和参考。



- **总结**
  
  本文主要介绍了一下，RecyclerView的创建和使用，以及和ListView进行了比较，显示出了RecyclerView更加灵活和高效，RecyclerView本身它是不关心视图相关的问题的，由于ListView的紧耦合的问题，google的改进就是RecyclerView本身不参与任何视图相关的问题。它不关心如何将子View放在合适的位置，也不关心如何分割这些子View，更不关心每个子View各自的外观，更进一步来说就是RecyclerView它只负责回收和重用的工作。通过和ItemTouchHelper、ItemDecoration、ItemAnimator的结合可以做出各种各样的酷炫效果，让我们赶紧开始使用RecyclerView吧。