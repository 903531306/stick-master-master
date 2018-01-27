PtrScrollView 自定义 ScrollView 加上拉刷次下拉刷新头部悬浮 
 scrollStickView.setChanged(this);
        scrollStickView.getViewTreeObserver().addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {

            @Override
            public void onGlobalLayout() {
                if (isFirst)
                    onSrollChanged(scrollStickView.getScrollY());

            }
        });
    
        ptrScrollView.setOnRefreshListener(new PullToRefreshBase.OnRefreshListener2<ScrollStickView>() {
            @Override
            public void onPullDownToRefresh(PullToRefreshBase<ScrollStickView> refreshView) {
                ptrScrollView.onRefreshComplete();
            }

            @Override
            public void onPullUpToRefresh(PullToRefreshBase<ScrollStickView> refreshView) {
                ptrScrollView.onRefreshComplete();
            }
        });
        
         //设置下拉刷新模式 此模式不设置 没有上拉
         
        ptrScrollView.setMode(PullToRefreshBase.Mode.BOTH);
        ptrScrollView.setCallBack(new PtrScrollView.CallBack() {
            @Override
            public void ScrollChanged(int t) {
                if (t > 0) {//用一个相同的吸顶布局放置在PullToRefresh同级放在上拉加载时候 吸顶布局闪动
//                    int scrollY = lastScrlll + t;
//                    if (lastScrlll != 0)
//                        llStick.setTranslationY(scrollY);
                    findViewById(R.id.lldd).setVisibility(View.VISIBLE);
                } else {
//                    llStick.setTranslationY(Math.max(linearLayout.getTop(), lastScrlll));
                    findViewById(R.id.lldd).setVisibility(View.GONE);
                }
            }
        });
    }
    
      //自动刷新 此处要加在初始化完成之后否则没有效果
      
      
      new Handler().postDelayed(new Runnable() {
            public void run() {
                ptrScrollView.setRefreshing();
            }
        }, 500);
        
        
        
    /**
     * 显示滚动记录
     *
     */
    public void showRemenber(String type){
        if(type.equals("0")){//0:代表第一个tab；1：代表第二个tab
            scrollStickView.smoothScrollTo(0, lasttab1);
        }else{
            scrollStickView.smoothScrollTo(0, lasttab2);
        }
    }
    private void initView() {
        ptrScrollView = (PtrScrollView) findViewById(R.id.totop_scrollview);
        scrollStickView = ptrScrollView.getRefreshableView();
        linearLayout = (LinearLayout) findViewById(R.id.ll);
        llStick = (RadioGroup) findViewById(R.id.stick);
    }

    @Override
    public void onSrollChanged(int t) {
        if (isFirst) {
            if (scrollStickView.getScrollY() < linearLayout.getTop()) {
                isStick = false;
                lasttab1 = lastScrlll;
                lasttab2 = lastScrlll;
            } else {
                if (!isStick) {
                    isStick = true;
                    lasttab1 = linearLayout.getTop();
                    lasttab2 = linearLayout.getTop();
                }
            }
            lastScrlll = t;
            llStick.setTranslationY(Math.max(lastScrlll, linearLayout.getTop()));
        } else {
            isFirst = true;
            scrollStickView.smoothScrollTo(0, 0);
        }

    }
 
不多说了  具体去看代码 不足之处请见谅。
