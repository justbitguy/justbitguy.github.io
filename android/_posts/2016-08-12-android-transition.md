---
layout: post
category: android
title: 'Android Transition'
date: 2016-08-12 16:46
---

Android Transition框架主要运用在以下三种情况：

- Activity之间或者Fragment之间的过渡动画。
- Activity之间或者Fragment之间共享元素的过渡动画。
- Activity中布局元素的过渡动画。

## Scene
定义: 记录了view hierarchy的状态；  
作用：布局之间的切换:

```java
	ViewGroup sceneRoot = (ViewGroup) findViewById(R.id.scene_root);
	scene1 = Scene.getSceneForLayout(sceneRoot, R.layout.activity_animations_scene1, this);
	View button1 = findViewById(R.id.sample3_button1);
	button1.setOnClickListener(new View.OnClickListener() {
	    @Override
	    public void onClick(View v) {
	        TransitionManager.go(scene1, new ChangeBounds());
	    }
	});
```

## Transition
当同一个布局页面下的view hierarchy发生改变的时候，可以通过 transaction指定过渡动画，并且不需要指定scene.

```java
private TextView mLabelText;
private Fade mFade;
private ViewGroup mRootView;
...

// Load the layout
this.setContentView(R.layout.activity_main);
...

// Create a new TextView and set some View properties
mLabelText = new TextView();
mLabelText.setText("Label").setId("1");

// Get the root view and create a transition
mRootView = (ViewGroup) findViewById(R.id.mainLayout);
mFade = new Fade(IN);

// Start recording changes to the view hierarchy
TransitionManager.beginDelayedTransition(mRootView, mFade);

// Add the new TextView to the view hierarchy
mRootView.addView(mLabelText);

// When the system redraws the screen to show this update,
// the framework will animate the addition as a fade in
```

自定义Transition:

```java
public class CustomTransition extends Transition {

    @Override
    public void captureStartValues(TransitionValues values) {}

    @Override
    public void captureEndValues(TransitionValues values) {}

    @Override
    public Animator createAnimator(ViewGroup sceneRoot,
                                   TransitionValues startValues,
                                   TransitionValues endValues) {}
}
```

参考

-  [Animating Views Using Scenes and Transitions](https://developer.android.com/training/transitions/index.html)
- [Android Transition Animation介绍](http://www.in-droid.com/2016/04/28/Android-Transition-Animation%E4%BB%8B%E7%BB%8D/)
