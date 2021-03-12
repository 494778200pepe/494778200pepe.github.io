---
layout: post
title:  "插件化 - setContentView 源码"
date:   2021-03-09 16:24:00 +0800
categories: Android
tags: Plugin
author: pepe
description: 『 setContentView 源码 』
---

AppCompatActivity:
```
    @Override
    public void setContentView(@LayoutRes int layoutResID) {
        getDelegate().setContentView(layoutResID);
    }
```

AppCompatDelegate:
```
    public abstract void setContentView(@LayoutRes int resId);
```

实现类 AppCompatDelegateImpl:
```
    @Override
    public void setContentView(int resId) {
        ensureSubDecor();
        ViewGroup contentParent = mSubDecor.findViewById(android.R.id.content);
        contentParent.removeAllViews();
		// 重点
        LayoutInflater.from(mContext).inflate(resId, contentParent);
        mAppCompatWindowCallback.getWrapped().onContentChanged();
    }
```

LayoutInflater:
```
    public View inflate(@LayoutRes int resource, @Nullable ViewGroup root) {
        return inflate(resource, root, root != null);
    }
	
    public View inflate(@LayoutRes int resource, @Nullable ViewGroup root, boolean attachToRoot) {
        final Resources res = getContext().getResources();
        if (DEBUG) {
            Log.d(TAG, "INFLATING from resource: \"" + res.getResourceName(resource) + "\" ("
                  + Integer.toHexString(resource) + ")");
        }

        View view = tryInflatePrecompiled(resource, res, root, attachToRoot);
        if (view != null) {
            return view;
        }
        XmlResourceParser parser = res.getLayout(resource);
        try {
			// 重点
            return inflate(parser, root, attachToRoot);
        } finally {
            parser.close();
        }
    }
	
    public View inflate(XmlPullParser parser, @Nullable ViewGroup root, boolean attachToRoot) {
        synchronized (mConstructorArgs) {
            Trace.traceBegin(Trace.TRACE_TAG_VIEW, "inflate");

            final Context inflaterContext = mContext;
            final AttributeSet attrs = Xml.asAttributeSet(parser);
            Context lastContext = (Context) mConstructorArgs[0];
            mConstructorArgs[0] = inflaterContext;
            View result = root;

            try {
                advanceToRootNode(parser);
                final String name = parser.getName();

                if (DEBUG) {
                    System.out.println("**************************");
                    System.out.println("Creating root view: "
                            + name);
                    System.out.println("**************************");
                }

                if (TAG_MERGE.equals(name)) {
                    if (root == null || !attachToRoot) {
                        throw new InflateException("<merge /> can be used only with a valid "
                                + "ViewGroup root and attachToRoot=true");
                    }

                    rInflate(parser, root, inflaterContext, attrs, false);
                } else {
                    // Temp is the root view that was found in the xml
					// 重点
                    final View temp = createViewFromTag(root, name, inflaterContext, attrs);

                    ViewGroup.LayoutParams params = null;

                    if (root != null) {
                        if (DEBUG) {
                            System.out.println("Creating params from root: " +
                                    root);
                        }
                        // Create layout params that match root, if supplied
                        params = root.generateLayoutParams(attrs);
                        if (!attachToRoot) {
                            // Set the layout params for temp if we are not
                            // attaching. (If we are, we use addView, below)
                            temp.setLayoutParams(params);
                        }
                    }

                    if (DEBUG) {
                        System.out.println("-----> start inflating children");
                    }

                    // Inflate all children under temp against its context.
                    rInflateChildren(parser, temp, attrs, true);

                    if (DEBUG) {
                        System.out.println("-----> done inflating children");
                    }

                    // We are supposed to attach all the views we found (int temp)
                    // to root. Do that now.
                    if (root != null && attachToRoot) {
                        root.addView(temp, params);
                    }

                    // Decide whether to return the root that was passed in or the
                    // top view found in xml.
                    if (root == null || !attachToRoot) {
                        result = temp;
                    }
                }

            } catch (XmlPullParserException e) {
                final InflateException ie = new InflateException(e.getMessage(), e);
                ie.setStackTrace(EMPTY_STACK_TRACE);
                throw ie;
            } catch (Exception e) {
                final InflateException ie = new InflateException(
                        getParserStateDescription(inflaterContext, attrs)
                        + ": " + e.getMessage(), e);
                ie.setStackTrace(EMPTY_STACK_TRACE);
                throw ie;
            } finally {
                // Don't retain static reference on context.
                mConstructorArgs[0] = lastContext;
                mConstructorArgs[1] = null;

                Trace.traceEnd(Trace.TRACE_TAG_VIEW);
            }

            return result;
        }
    }
	
    @UnsupportedAppUsage
    private View createViewFromTag(View parent, String name, Context context, AttributeSet attrs) {
        return createViewFromTag(parent, name, context, attrs, false);
    }
	
    View createViewFromTag(View parent, String name, Context context, AttributeSet attrs,
            boolean ignoreThemeAttr) {
        if (name.equals("view")) {
            name = attrs.getAttributeValue(null, "class");
        }

        // Apply a theme wrapper, if allowed and one is specified.
        if (!ignoreThemeAttr) {
            final TypedArray ta = context.obtainStyledAttributes(attrs, ATTRS_THEME);
            final int themeResId = ta.getResourceId(0, 0);
            if (themeResId != 0) {
                context = new ContextThemeWrapper(context, themeResId);
            }
            ta.recycle();
        }

        try {
			// 重点
            View view = tryCreateView(parent, name, context, attrs);

            if (view == null) {
                final Object lastContext = mConstructorArgs[0];
                mConstructorArgs[0] = context;
                try {
                    if (-1 == name.indexOf('.')) {
                        view = onCreateView(context, parent, name, attrs);
                    } else {
                        view = createView(context, name, null, attrs);
                    }
                } finally {
                    mConstructorArgs[0] = lastContext;
                }
            }

            return view;
        } catch (InflateException e) {
            throw e;

        } catch (ClassNotFoundException e) {
            final InflateException ie = new InflateException(
                    getParserStateDescription(context, attrs)
                    + ": Error inflating class " + name, e);
            ie.setStackTrace(EMPTY_STACK_TRACE);
            throw ie;

        } catch (Exception e) {
            final InflateException ie = new InflateException(
                    getParserStateDescription(context, attrs)
                    + ": Error inflating class " + name, e);
            ie.setStackTrace(EMPTY_STACK_TRACE);
            throw ie;
        }
    }
	
    public final View tryCreateView(@Nullable View parent, @NonNull String name,
        @NonNull Context context,
        @NonNull AttributeSet attrs) {
        if (name.equals(TAG_1995)) {
            // Let's party like it's 1995!
            return new BlinkLayout(context, attrs);
        }

        View view;
        if (mFactory2 != null) {
            view = mFactory2.onCreateView(parent, name, context, attrs);
        } else if (mFactory != null) {
            view = mFactory.onCreateView(name, context, attrs);
        } else {
            view = null;
        }

        if (view == null && mPrivateFactory != null) {
            view = mPrivateFactory.onCreateView(parent, name, context, attrs);
        }

        return view;
    }
```

追踪到这里，那么问题来了，mFactory2 是哪儿来的呢？

LayoutInflater：
```
	// 追踪 mFactory2 的初始化
    public void setFactory2(Factory2 factory) {
        if (mFactorySet) {
            throw new IllegalStateException("A factory has already been set on this LayoutInflater");
        }
        if (factory == null) {
            throw new NullPointerException("Given factory can not be null");
        }
        mFactorySet = true;
		// 重点
        if (mFactory == null) {
            mFactory = mFactory2 = factory;
        } else {
            mFactory = mFactory2 = new FactoryMerger(factory, factory, mFactory, mFactory2);
        }
    }
```
LayoutInflaterCompat:
```
    public static void setFactory2(
            @NonNull LayoutInflater inflater, @NonNull LayoutInflater.Factory2 factory) {
		// 重点
        inflater.setFactory2(factory);

        if (Build.VERSION.SDK_INT < 21) {
            final LayoutInflater.Factory f = inflater.getFactory();
            if (f instanceof LayoutInflater.Factory2) {
                // The merged factory is now set to getFactory(), but not getFactory2() (pre-v21).
                // We will now try and force set the merged factory to mFactory2
                forceSetFactory2(inflater, (LayoutInflater.Factory2) f);
            } else {
                // Else, we will force set the original wrapped Factory2
                forceSetFactory2(inflater, factory);
            }
        }
    }
```
AppCompatDelegateImpl:
```
    @Override
    public void installViewFactory() {
        LayoutInflater layoutInflater = LayoutInflater.from(mContext);
        if (layoutInflater.getFactory() == null) {
			// 重点，注意这个 this
            LayoutInflaterCompat.setFactory2(layoutInflater, this);
        } else {
            if (!(layoutInflater.getFactory2() instanceof AppCompatDelegateImpl)) {
                Log.i(TAG, "The Activity's LayoutInflater already has a Factory installed"
                        + " so we can not install AppCompat's");
            }
        }
    }
```
AppCompatActivity:
```
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        final AppCompatDelegate delegate = getDelegate();
        delegate.installViewFactory();
        delegate.onCreate(savedInstanceState);
        super.onCreate(savedInstanceState);
    }
```
至此，我们知道了 mFactory2 的初始化流程：LayoutInflaterCompat.setFactory2(layoutInflater, this)，继续来看这个 this：

AppCompatDelegateImpl：
```
    public final View onCreateView(View parent, String name, Context context, AttributeSet attrs) {
        return createView(parent, name, context, attrs);
    }
	
    @Override
    public View createView(View parent, final String name, @NonNull Context context,
            @NonNull AttributeSet attrs) {
        if (mAppCompatViewInflater == null) {
            TypedArray a = mContext.obtainStyledAttributes(R.styleable.AppCompatTheme);
            String viewInflaterClassName =
                    a.getString(R.styleable.AppCompatTheme_viewInflaterClass);
            if (viewInflaterClassName == null) {
                // Set to null (the default in all AppCompat themes). Create the base inflater
                // (no reflection)
                mAppCompatViewInflater = new AppCompatViewInflater();
            } else {
                try {
                    Class<?> viewInflaterClass = Class.forName(viewInflaterClassName);
                    mAppCompatViewInflater =
                            (AppCompatViewInflater) viewInflaterClass.getDeclaredConstructor()
                                    .newInstance();
                } catch (Throwable t) {
                    Log.i(TAG, "Failed to instantiate custom view inflater "
                            + viewInflaterClassName + ". Falling back to default.", t);
                    mAppCompatViewInflater = new AppCompatViewInflater();
                }
            }
        }

        boolean inheritContext = false;
        if (IS_PRE_LOLLIPOP) {
            inheritContext = (attrs instanceof XmlPullParser)
                    // If we have a XmlPullParser, we can detect where we are in the layout
                    ? ((XmlPullParser) attrs).getDepth() > 1
                    // Otherwise we have to use the old heuristic
                    : shouldInheritContext((ViewParent) parent);
        }
		// 重点
        return mAppCompatViewInflater.createView(parent, name, context, attrs, inheritContext,
                IS_PRE_LOLLIPOP, /* Only read android:theme pre-L (L+ handles this anyway) */
                true, /* Read read app:theme as a fallback at all times for legacy reasons */
                VectorEnabledTintResources.shouldBeUsed() /* Only tint wrap the context if enabled */
        );
    }
```
AppCompatViewInflater:
```
    final View createView(View parent, final String name, @NonNull Context context,
            @NonNull AttributeSet attrs, boolean inheritContext,
            boolean readAndroidTheme, boolean readAppTheme, boolean wrapContext) {
        final Context originalContext = context;

        // We can emulate Lollipop's android:theme attribute propagating down the view hierarchy
        // by using the parent's context
        if (inheritContext && parent != null) {
            context = parent.getContext();
        }
        if (readAndroidTheme || readAppTheme) {
            // We then apply the theme on the context, if specified
            context = themifyContext(context, attrs, readAndroidTheme, readAppTheme);
        }
        if (wrapContext) {
            context = TintContextWrapper.wrap(context);
        }

		// 重点
        View view = null;

        // We need to 'inject' our tint aware Views in place of the standard framework versions
        switch (name) {
            case "TextView":
                view = createTextView(context, attrs);
                verifyNotNull(view, name);
                break;
            case "ImageView":
                view = createImageView(context, attrs);
                verifyNotNull(view, name);
                break;
            case "Button":
                view = createButton(context, attrs);
                verifyNotNull(view, name);
                break;
            case "EditText":
                view = createEditText(context, attrs);
                verifyNotNull(view, name);
                break;
            case "Spinner":
                view = createSpinner(context, attrs);
                verifyNotNull(view, name);
                break;
            case "ImageButton":
                view = createImageButton(context, attrs);
                verifyNotNull(view, name);
                break;
            case "CheckBox":
                view = createCheckBox(context, attrs);
                verifyNotNull(view, name);
                break;
            case "RadioButton":
                view = createRadioButton(context, attrs);
                verifyNotNull(view, name);
                break;
            case "CheckedTextView":
                view = createCheckedTextView(context, attrs);
                verifyNotNull(view, name);
                break;
            case "AutoCompleteTextView":
                view = createAutoCompleteTextView(context, attrs);
                verifyNotNull(view, name);
                break;
            case "MultiAutoCompleteTextView":
                view = createMultiAutoCompleteTextView(context, attrs);
                verifyNotNull(view, name);
                break;
            case "RatingBar":
                view = createRatingBar(context, attrs);
                verifyNotNull(view, name);
                break;
            case "SeekBar":
                view = createSeekBar(context, attrs);
                verifyNotNull(view, name);
                break;
            case "ToggleButton":
                view = createToggleButton(context, attrs);
                verifyNotNull(view, name);
                break;
            default:
                // The fallback that allows extending class to take over view inflation
                // for other tags. Note that we don't check that the result is not-null.
                // That allows the custom inflater path to fall back on the default one
                // later in this method.
                view = createView(context, name, attrs);
        }

        if (view == null && originalContext != context) {
            // If the original context does not equal our themed context, then we need to manually
            // inflate it using the name so that android:theme takes effect.
            view = createViewFromTag(context, name, attrs);
        }

        if (view != null) {
            // If we have created a view, check its android:onClick
            checkOnClickListener(view, attrs);
        }

        return view;
    }
	
    @NonNull
    protected AppCompatTextView createTextView(Context context, AttributeSet attrs) {
        return new AppCompatTextView(context, attrs);
    }
	
```

到现在也就解释了整个创建流程，并且获知了 为什么 xml 中的 TextView 会被创建为 AppCompatTextView 了。



















