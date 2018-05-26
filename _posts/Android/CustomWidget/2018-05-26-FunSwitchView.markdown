---
layout: post
title:  "FunSwitchView"
date:   2018-05-26 16:29:00 +0800
categories: Android
tags: CustomWidget
author: pepe
description: 『 FunSwitchView 』
---

```
/**
 * Created on 2017/10/11.
 * @author wang
 */
public class FunSwitchView extends View implements View.OnClickListener {
    /**
     * 开关
     */
    boolean isLaugh = false;
    /**
     * 背景画笔
     */
    Paint mBackgroundPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
    /**
     * 面部画笔
     */
    Paint mFacePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
    int left, top;
    int mWidth, mHeight;
    int mRadius;
    int mStrokeWidth = 2;
    Point mCenterPoint = new Point();

    int color1 = Color.GRAY;
    int color2 = Color.GREEN;
    int mainColor = color1;

    int mFaceOffset = 0;
    int mouthHeight = 50;
    int mMouthHeight = 0;

    public FunSwitchView(Context context) {
        super(context);
    }

    public FunSwitchView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public FunSwitchView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    {
        //非静态代码块
        mFacePaint.setStrokeWidth(mStrokeWidth);
    }

    @Override
    protected void onAttachedToWindow() {
        super.onAttachedToWindow();
        setOnClickListener(this);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        left = getLeft();
        top = getTop();
        int width = getWidth();
        int height = getHeight();
        if (width >= 2 * height) {
            mWidth = 2 * height;
        } else {
            mWidth = width;
        }
        mHeight = mWidth / 2;
        mRadius = mHeight / 2;
        Log.d("pepe", " left = " + left);
        Log.d("pepe", " top = " + top);
        Log.d("pepe", " mWidth = " + mWidth);
        Log.d("pepe", " mHeight = " + mHeight);
        Log.d("pepe", " mRadius = " + mRadius);
        drawBackground(canvas);
        drawFace(canvas);
    }

    /**
     * 画背景
     * @param canvas 画布
     */
    private void drawBackground(Canvas canvas) {
        mBackgroundPaint.setStyle(Paint.Style.FILL);
        mBackgroundPaint.setColor(mainColor);
        canvas.drawArc(new RectF(left, top, left + mHeight, top + mHeight), 90, 180, true, mBackgroundPaint);
        canvas.drawRect(new RectF(left + mRadius, top, left + mRadius + mHeight, top + mHeight), mBackgroundPaint);
        canvas.drawArc(new RectF(left + mHeight, top, left + mHeight * 2, top + mHeight), -90, 180, true, mBackgroundPaint);
    }

    /**
     * 绘制面部信息
     * @param canvas 画布
     */
    private void drawFace(Canvas canvas) {
        mCenterPoint.x = left + mRadius + mFaceOffset;
        mCenterPoint.y = top + mRadius;

        mFacePaint.setStyle(Paint.Style.FILL);
        mFacePaint.setColor(Color.WHITE);
        canvas.drawCircle(mCenterPoint.x, mCenterPoint.y, mRadius - mStrokeWidth, mFacePaint);

        mFacePaint.setColor(mainColor);
        int mouthLength = mRadius / 3;
        mFacePaint.setStrokeWidth(5);

        int beishu = 5;
        int bb = 3;
        if (mOffset <= 0.25) {
            Camera camera = new Camera();
            canvas.save();
            camera.save();
            camera.setLocation(0, 0, -12);
            canvas.translate(mRadius + mRadius * mOffset, mRadius);
            camera.rotateY(90 * mOffset * 2);
            camera.applyToCanvas(canvas);
            canvas.translate(-mRadius - mRadius * mOffset, -mRadius);
            camera.restore();

            canvas.drawLine(mCenterPoint.x - mouthLength + mRadius * mOffset * beishu, mCenterPoint.y + mouthLength, mCenterPoint.x + mouthLength + mRadius * mOffset * beishu, mCenterPoint.y + mouthLength, mFacePaint);
            int eyeWidth = mRadius / 4, eyeHeight = mRadius / 3;
            canvas.drawArc(new RectF(mCenterPoint.x - mouthLength - eyeWidth / 2 + mRadius * mOffset * beishu, mCenterPoint.y - mouthLength - eyeHeight / 2, mCenterPoint.x - mouthLength + eyeWidth / 2 + mRadius * mOffset * beishu, mCenterPoint.y - mouthLength + eyeHeight / 2), 0, 360, true, mFacePaint);

            canvas.drawArc(new RectF(mCenterPoint.x + mouthLength - eyeWidth / 2 + mRadius * mOffset * beishu, mCenterPoint.y - mouthLength - eyeHeight / 2, mCenterPoint.x + mouthLength + eyeWidth / 2 + mRadius * mOffset * beishu, mCenterPoint.y - mouthLength + eyeHeight / 2), 0, 360, true, mFacePaint);
            canvas.restore();
        } else if (mOffset >= 0.75) {
            Camera camera = new Camera();
            canvas.save();
            camera.save();
            camera.setLocation(0, 0, -12);
            canvas.translate(3 * mRadius, mRadius);
            camera.rotateY(-90 + 90 * (mOffset - 0.5f) * 2);
            camera.applyToCanvas(canvas);
            canvas.translate(-3 * mRadius, -mRadius);
            camera.restore();
            float eyeWidth = mRadius / 4, eyeHeight = mRadius / 3;


            float mouthOffset = mOffset;
            float eyeOffset = mOffset;
            if (mOffset > 1.02) {
                mouthOffset = 1.02f;
                eyeHeight = eyeHeight - eyeHeight * (mOffset - 1) * beishu;
                eyeWidth = eyeWidth + eyeHeight * (mOffset - 1) * bb;
                eyeOffset = 1.02f;
            }
            Path path = new Path();
            path.moveTo(mCenterPoint.x - mouthLength - 2 * (mRadius - (mouthOffset - 0.5f) * 2 * mRadius), mCenterPoint.y + mouthLength);
            path.quadTo(mCenterPoint.x - 2 * (mRadius - (mouthOffset - 0.5f) * 2 * mRadius), mCenterPoint.y + mouthLength + mMouthHeight, mCenterPoint.x + mouthLength - bb * (mRadius - (mouthOffset - 0.5f) * 2 * mRadius), mCenterPoint.y + mouthLength);
            path.close();
            canvas.drawPath(path, mFacePaint);

            canvas.drawArc(new RectF(mCenterPoint.x - mouthLength - eyeWidth / 2 - bb * (mRadius - (eyeOffset - 0.5f) * 2 * mRadius), mCenterPoint.y - mouthLength - eyeHeight / 2, mCenterPoint.x - mouthLength + eyeWidth / 2 - bb * (mRadius - (eyeOffset - 0.5f) * 2 * mRadius), mCenterPoint.y - mouthLength + eyeHeight / 2), 0, 360, true, mFacePaint);

            canvas.drawArc(new RectF(mCenterPoint.x + mouthLength - eyeWidth / 2 - bb * (mRadius - (eyeOffset - 0.5f) * 2 * mRadius), mCenterPoint.y - mouthLength - eyeHeight / 2, mCenterPoint.x + mouthLength + eyeWidth / 2 - bb * (mRadius - (eyeOffset - 0.5f) * 2 * mRadius), mCenterPoint.y - mouthLength + eyeHeight / 2), 0, 360, true, mFacePaint);
            canvas.restore();
        }
    }

    /**
     * 改变背景颜色
     */
    private void changeBackgroundColor() {
        ObjectAnimator animator;
        if (isLaugh) {
            animator = ObjectAnimator.ofInt(this, "color", color2, color1);
        } else {
            animator = ObjectAnimator.ofInt(this, "color", color1, color2);
        }
        animator.setEvaluator(new ArgbEvaluator());
        animator.setDuration(700);
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                int color = (int) animation.getAnimatedValue();
                mainColor = color;
            }
        });
        animator.start();
    }

    private void changeFaceLocation() {
        ObjectAnimator animator;
        if (isLaugh) {
            animator = ObjectAnimator.ofFloat(this, "offset", 1, 0);
            animator.setInterpolator(null);
            animator.setDuration(500);
        } else {
            animator = ObjectAnimator.ofFloat(this, "offset", 0, 1);
            animator.setInterpolator(new OvershootInterpolator());
            animator.setDuration(700);
        }
        animator.setEvaluator(new FloatEvaluator());

        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
            @Override
            public void onAnimationUpdate(ValueAnimator animation) {
                float offset = (float) animation.getAnimatedValue();
                mOffset = offset;
                mFaceOffset = (int) (offset * 2 * mRadius);
                if (offset >= 0.5) {
                    if (offset > 1) {
                        mMouthHeight = (int) ((1 - 0.5) * 2 * mouthHeight);
                    } else {
                        mMouthHeight = (int) ((offset - 0.5) * 2 * mouthHeight);
                    }
                } else {
                    mMouthHeight = 0;
                }
                invalidate();
            }
        });
        animator.start();
    }

    @Override
    public void onClick(View v) {
        changeBackgroundColor();
        changeFaceLocation();
        isLaugh = !isLaugh;
    }

    private float mOffset;

    public void setOffset(float offset) {
        mOffset = offset;
    }

    public float getOffset() {
        return mOffset;
    }

    private int mColor;

    public void setColor(int color) {
        mColor = color;
    }

    public int getColor() {
        return mColor;
    }
}

```
参考：

[自定义Switch过程详解 - CSDN博客](http://blog.csdn.net/u012422440/article/details/51921498) 

