---
layout: single
title: "[Android] RecyclerView 스크롤 연동"
excerpt: "2개의 RecyclerView 스크롤 연동"
tag: [Android]
categories: android
commnets: true
author_profile: true
published: true
---


# 2개의 RecyclerView를 연동시키는 방법
내가 생각하는 가장 단순한 방법은 한 RecyclerView에서 Scroll 이벤트가 발생하면 다른 RecyclerView에 scrollBy(int x, int y)를 실행하는 방법이다.


<br/>

---

## 문제점
그러나 이 방법에는 문제가 하나 있다. <br />
RecyclerView에 scrollBy를 실행 하면 다른 RecyclerView의 Listener도 트리거 되므로 루프에 빠지게 된다.

<br />

---
## OnScrollListener 구현
이 문제를 해결하려면 Scroll Event가 발생한 RecyclerView를 제외한 다른 RecyclerView의 Scroll Event를 제거하면 된다.

```java
public class SelfRemovingOnScrollListener extends RecyclerView.OnScrollListener {

    @Override
    public final void onScrollStateChanged(@NonNull final RecyclerView recyclerView, final int newState) {
        super.onScrollStateChanged(recyclerView, newState);
        if (newState == RecyclerView.SCROLL_STATE_IDLE) {
            recyclerView.removeOnScrollListener(this);
        }
    }
}
```

다음은 2개의 OnScrollListener 구현이다.
```java
private final RecyclerView.OnScrollListener mLeftOSL = new SelfRemovingOnScrollListener() {
    @Override
    public void onScrolled(@NonNull final RecyclerView recyclerView, final int dx, final int dy) {
        super.onScrolled(recyclerView, dx, dy);
        mRightRecyclerView.scrollBy(dx, dy);
    }
}, mRightOSL = new SelfRemovingOnScrollListener() {

    @Override
    public void onScrolled(@NonNull final RecyclerView recyclerView, final int dx, final int dy) {
        super.onScrolled(recyclerView, dx, dy);
        mLeftRecyclerView.scrollBy(dx, dy);
    }
};
```
Left RecyclerView에 스크롤 이벤트가 발생하면 Right RecyclerView의 scrollBy()를, Right RecyclerView에 스크롤 이벤트가 발생하면 Left RecyclerView의 scrollBy를 실행한다.

<br/>

---

## RecyclerView에 OnItemTouchListener 설정
Left RecyclerView, Right RecyclerView에 OnItemTouchListener를 구현, 설정 해준다.
```java
mLeftRecyclerView.addOnItemTouchListener(new RecyclerView.OnItemTouchListener() {

        private int mLastY;

        @Override
        public boolean onInterceptTouchEvent(@NonNull final RecyclerView rv, @NonNull final
        MotionEvent e) {
            Log.d("debug", "LEFT: onInterceptTouchEvent");

            final Boolean ret = rv.getScrollState() != RecyclerView.SCROLL_STATE_IDLE;
            if (!ret) {
                onTouchEvent(rv, e);
            }
            return Boolean.FALSE;
        }

        @Override
        public void onTouchEvent(@NonNull final RecyclerView rv, @NonNull final MotionEvent e) {
            Log.d("debug", "LEFT: onTouchEvent");

            final int action;
            if ((action = e.getAction()) == MotionEvent.ACTION_DOWN && mRightRecyclerView
                    .getScrollState() == RecyclerView.SCROLL_STATE_IDLE) {
                mLastY = rv.getScrollY();
                rv.addOnScrollListener(mLeftOSL);
            }
            else {
                if (action == MotionEvent.ACTION_UP && rv.getScrollY() == mLastY) {
                    rv.removeOnScrollListener(mLeftOSL);
                }
            }
        }

        @Override
        public void onRequestDisallowInterceptTouchEvent(final boolean disallowIntercept) {
            Log.d("debug", "LEFT: onRequestDisallowInterceptTouchEvent");
        }
    });

    mRightRecyclerView.addOnItemTouchListener(new RecyclerView.OnItemTouchListener() {

        private int mLastY;

        @Override
        public boolean onInterceptTouchEvent(@NonNull final RecyclerView rv, @NonNull final
        MotionEvent e) {
            Log.d("debug", "RIGHT: onInterceptTouchEvent");

            final Boolean ret = rv.getScrollState() != RecyclerView.SCROLL_STATE_IDLE;
            if (!ret) {
                onTouchEvent(rv, e);
            }
            return Boolean.FALSE;
        }

        @Override
        public void onTouchEvent(@NonNull final RecyclerView rv, @NonNull final MotionEvent e) {
            Log.d("debug", "RIGHT: onTouchEvent");

            final int action;
            if ((action = e.getAction()) == MotionEvent.ACTION_DOWN && mLeftRecyclerView
                    .getScrollState
                            () == RecyclerView.SCROLL_STATE_IDLE) {
                mLastY = rv.getScrollY();
                rv.addOnScrollListener(mRightOSL);
            }
            else {
                if (action == MotionEvent.ACTION_UP && rv.getScrollY() == mLastY) {
                    rv.removeOnScrollListener(mRightOSL);
                }
            }
        }

        @Override
        public void onRequestDisallowInterceptTouchEvent(final boolean disallowIntercept) {
            Log.d("debug", "RIGHT: onRequestDisallowInterceptTouchEvent");
        }
    });
}
```

만약 사용자가 두 RecyclerView를 동시에 스크롤 하려고 하면 충돌이 발생하게 되므로 두 RecyclerView를 감싼 부모에 다음 코드를 추가해준다
```xml
android:splitMotionEvents="false"
```

<br />

출처 : [https://stackoverflow.com/questions/30702726/sync-scrolling-of-multiple-recyclerviews](https://stackoverflow.com/questions/30702726/sync-scrolling-of-multiple-recyclerviews)