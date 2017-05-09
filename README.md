## Welcome to Android snip Code from every Where

> Tools:


 * Widgets
 * Activity
 * Tools
 * Connection
 * Intent
 * Others


### Widgets
- Fonts
- General

![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/from_html.png)

```
<h1>Hello World</h1>
Here is an
<img src="octopus"><i>octopus</i>.<br>
And here is a
<a href="http://d.android.com">
link</a>.
```



```
<string name="from_html_text">
<![CDATA[
<h1>Hello World</h1>
Here is an
<img src="octopus"><i>octopus</i>.<br>
And here is a
<a href="http://d.android.com">
link</a>.
]]>
</string>
```



```
String html = getString(R.string.from_html_text);
textView.setMovementMethod(
    LinkMovementMethod.getInstance());
textView.setText(Html.fromHtml(
    html, new ResourceImageGetter(this), null));
    ```
    
    
    

```
private static class ResourceImageGetter
    implements Html.ImageGetter {
  // Constructor takes a Context
  public Drawable getDrawable(String source) {
    int path = context.getResources().getIdentifier(
        source, "drawable", context.getPackageName());
    Drawable drawable = ContextCompat.getDrawable(context, path);
    drawable.setBounds(0, 0,
       drawable.getIntrinsicWidth(),
       drawable.getIntrinsicHeight());
    return drawable;
  }
}
```



____________________________________________



![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/fraction_equation.png)



```
@TargetApi(LOLLIPOP)
<string name="fraction_text">
<![CDATA[
1<afrc>1/2</afrc> + <afrc>11/16</afrc>
= 2<afrc>3/16</afrc>
]]>
</string>
```




```
String html = getString(
  R.string.fraction_text);
textView.setText(Html.fromHtml(
    html, null,
    new FractionTagHandler()))
    ```
    
    
    
    
    
```
private static class FractionTagHandler implements Html.TagHandler {
  public void handleTag(boolean opening,
      String tag, Editable output, XMLReader xmlReader) {
  if (!"afrc".equalsIgnoreCase(tag)) return;
  int len = output.length();
  if (opening) {
    output.setSpan(new FractionSpan(), len, len,
        Spannable.SPAN_MARK_MARK);
  } else {
    Object obj = getLast(output, FractionSpan.class);
    int where = output.getSpanStart(obj);
    output.removeSpan(obj);
    if (where != len) {
      output.setSpan(new FractionSpan(), where, len, 0);
    }
  }
}
```





```
private Object getLast(Editable text, Class kind) {
  Object[] objs = text.getSpans(0, text.length(), kind);
  if (objs.length == 0) return null;
  for (int i = objs.length - 1; i >= 0; --i) {
    if(text.getSpanFlags(objs[i]) == Spannable.SPAN_MARK_MARK) {
      return objs[i];
    }
  }
  return null;
}
```




```
private static class FractionSpan extends MetricAffectingSpan {
  public void updateMeasureState(TextPaint textPaint) {
    textPaint.setFontFeatureSettings("afrc");
  }
  public void updateDrawState(TextPaint textPaint) {
    textPaint.setFontFeatureSettings("afrc");
  }
}
```



##### Styled string



```
static SpannableString formatString(
    Context context,
    int textId, int styleId) {
  String text = context.getString(textId);
  SpannableString spannableString
    = new SpannableString(text);
  spannableString.setSpan(
      new TextAppearanceSpan(
        context, styleId),
      0, text.length(), 0);
  return spannableString;
}
```




```
SpannableStringBuilder builder
    = new SpannableStringBuilder()
  .append(formatString(
      this, R.string.big_red,
      R.style.BigRedTextAppearance))
  .append("\n")
  .append(formatString(
      this, R.string.medium_green,
      R.style.MediumGreenTextAppearance))
  .append("\n")
  .append(formatString(
      this, R.string.small_blue,
      R.style.SmallBlueTextAppearance));
      ```
      
      
     
      
      
      
 ```
 textView.setText(
  builder.subSequence(
    0, builder.length()));
    ```
    
    
    
    
```
<style name="BigRedTextAppearance"
    parent="@android:style/TextAppearance">
  <item name="android:textSize">
    56sp</item>
  <item name="android:textColor">
    #c00</item>
</style>
```



##### AlignmentSpan




```
public void click(View button) {
  String text
      = editText.getText().toString();
  Layout.Alignment align =
    button.getId() ==
        R.id.add_to_right_button ?
      Layout.Alignment.ALIGN_OPPOSITE :
      Layout.Alignment.ALIGN_NORMAL;
  appendText(text, align);
  editText.setText(null);
}
```





```
private void appendText(
    CharSequence text,
    Layout.Alignment align) {
  AlignmentSpan span
    = new AlignmentSpan.Standard(align);
  SpannableString spannableString
    = new SpannableString(text);
  spannableString.setSpan(
    span, 0, text.length(), 0);
  if (textView.length() > 0) {
    textView.append("\n\n");
  }
  textView.append(spannableString);
}
```






##### Animated rainbow span



![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/animated_rainbow.gif)




```
ObjectAnimator objectAnimator 
  = ObjectAnimator.ofFloat(
    span, 
    ANIMATED_COLOR_SPAN_FLOAT_PROPERTY, 
    0, 100);
objectAnimator.setEvaluator(
  new FloatEvaluator());
objectAnimator.addUpdateListener(
    new ValueAnimator.AnimatorUpdateListener() {
  public void onAnimationUpdate(
      ValueAnimator animation) {
    textView.setText(spannableString);
  }
});
objectAnimator.setInterpolator(
  new LinearInterpolator());
objectAnimator.setDuration(
  DateUtils.MINUTE_IN_MILLIS * 3);
objectAnimator.setRepeatCount(
  ValueAnimator.INFINITE);
objectAnimator.start();
```





```
private static final 
  Property<AnimatedColorSpan, Float>
    ANIMATED_COLOR_SPAN_FLOAT_PROPERTY
    = new Property<AnimatedColorSpan, Float>(
        Float.class, 
        "ANIMATED_COLOR_SPAN_FLOAT_PROPERTY") {
  public void set(
      AnimatedColorSpan span, Float value) {
    span.setTranslateXPercentage(value);
  }
  public Float get(AnimatedColorSpan span) {
    return span.getTranslateXPercentage();
  }
};
```





```
public void setTranslateXPercentage(
    float value) {
  translateXPercentage = value;
}
public float getTranslateXPercentage() {
  return translateXPercentage;
}
```





```
public void updateDrawState(
    TextPaint paint) {
  paint.setStyle(Paint.Style.FILL);
  float width 
    = paint.getTextSize() * colors.length;
  if (this.shader == null) {
    this.shader = new LinearGradient(
        0, 0, 0, width, colors, null, 
        Shader.TileMode.MIRROR);
  }
  this.matrix.reset();
  this.matrix.setRotate(90);
  this.matrix.postTranslate(
    width * translateXPercentage, 0);
  this.shader.setLocalMatrix(this.matrix);
  paint.setShader(this.shader);
}
```




##### ClickableSpan


![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/clickable_span.gif)




```
String text = textView.getText().toString();

String goToSettings = getString(R.string.go_to_settings);
int start = text.indexOf(goToSettings);
int end = start + goToSettings.length();

SpannableString spannableString = new SpannableString(text);
spannableString.setSpan(new GoToSettingsSpan(), start, end, 0);
textView.setText(spannableString);

textView.setMovementMethod(new LinkMovementMethod());
```




```
private static class GoToSettingsSpan extends ClickableSpan {
  public void onClick(View view) {
    view.getContext().startActivity(
      new Intent(android.provider.Settings.ACTION_SETTINGS));
  }
}
```





```
<TextView
  android:layout_width="match_parent"
  android:layout_height="match_parent"
  android:text="@string/clickable_span_text"
  android:textColorLink="@color/go_to_settings"
  android:textColorHighlight="@color/light_green"/>
  ```
  
  
  
  ##### Lined paper
  
  
  
  
  ![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/lined_paper.gif)
  
  
  ```
  public class LinedEditText 
    extends EditText {
  private void init() {
    this.paint = new Paint();
    paint.setStyle(Paint.Style.STROKE);
    paint.setColor(
      getResources().getColor(
        R.color.paper_line));
    paint.setStrokeWidth(
        getLineHeight() / 10);
    paint.setStrokeCap(Paint.Cap.ROUND);
  }
}
```




```
protected void onDraw(Canvas canvas) {
  float startX = getPaddingLeft();
  float stopX 
    = getWidth() - getPaddingRight();
  float offsetY = getPaddingTop()
     - getPaint().getFontMetrics().top
     + paint.getStrokeWidth() / 2;
  for (int i = 0; i < getLineCount(); ++i) {
    float y = offsetY + getLineHeight() * i;
    canvas.drawLine(
      startX, y, stopX, y, paint);
  }

  super.onDraw(canvas);
}
```




###### Emoji && Unicode with system font


![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/emoji_raw.png)

![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/emoji.png)

![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/emoji_raw_galaxy_nexus.png)

![alt text](https://raw.githubusercontent.com/SamiMohsin/snip/master/emoji_raw (1).png)


```
String text 
  = textView.getText().toString();
SpannableString spannableString 
  = new SpannableString(text);
IconFontSpan iconFontSpan
  = new IconFontSpan(
    textView.getContext());
Pattern pattern 
  = Pattern.compile("\u26F7");  // skier
Matcher matcher = pattern.matcher(text);
while (matcher.find()) {
  spannableString.setSpan(iconFontSpan,
    matcher.start(), matcher.end(), 0);
}
```




```
private static class IconFontSpan
    extends MetricAffectingSpan {
  private static Typeface typeface = null;
  public IconFontSpan(Context context) {
    if (typeface == null) {
      typeface = Typeface.createFromAsset(
        context.getAssets(), "icomoon.ttf");
      }
    }
  public void updateMeasureState(
      TextPaint textPaint) {
    textPaint.setTypeface(typeface);
  }
  public void updateDrawState(
      TextPaint textPaint) {
    textPaint.setTypeface(typeface);
  }
}
```




```
Pattern pattern = Pattern.compile(":octopus:");
Matcher matcher = pattern.matcher(text);
Bitmap octopus = null;
int size = (int) (-textView.getPaint().ascent());
while (matcher.find()) {
  if (octopus == null) {
    Bitmap bitmap = BitmapFactory.decodeResource(
        getResources(), R.drawable.octopus);
    octopus = Bitmap.createScaledBitmap(
        bitmap, size, size, true);
    bitmap.recycle();
  }
  ImageSpan span = new ImageSpan(
      this, octopus, ImageSpan.ALIGN_BASELINE);
  spannableString.setSpan(
      span, matcher.start(), matcher.end(), 0);
}
```




```
Pattern pattern = Pattern.compile(":octopus:");
Matcher matcher = pattern.matcher(text);
Bitmap octopus = null;
int size = (int) (-textView.getPaint().ascent());
while (matcher.find()) {
  if (octopus == null) {
    Bitmap bitmap = BitmapFactory.decodeResource(
        getResources(), R.drawable.octopus);
    octopus = Bitmap.createScaledBitmap(
        bitmap, size, size, true);
    bitmap.recycle();
  }
  ImageSpan span = new ImageSpan(
      this, octopus, ImageSpan.ALIGN_BASELINE);
  spannableString.setSpan(
      span, matcher.start(), matcher.end(), 0);
}
```




```
// :speed_50: :speed_110:
Pattern pattern 
  = Pattern.compile(":speed_(\\d\\d\\d?):");
Pattern matcher = pattern.matcher(text);
while (matcher.find()) {
  SpeedSignDrawable drawable 
    = new SpeedSignDrawable(
      textView, matcher.group(1));
  ImageSpan span = new ImageSpan(
      drawable, ImageSpan.ALIGN_BASELINE);
  spannableString.setSpan(
      span, matcher.start(), matcher.end(), 0);
}
```




```
private static class SpeedSignDrawable 
    extends Drawable {
  public SpeedSignDrawable(
      TextView textView, String number) {
    this.ascent 
      = textView.getPaint().ascent();
    this.descent 
      = textView.getPaint().descent();
    this.textSize = textView.getTextSize();
    this.strokeWidth 
      = textView.getPaint().getStrokeWidth();
    this.number = number;
    int size = (int) -ascent;
    this.setBounds(0, 0, size, size);
  }
  ```
  
  
  
  
  
  ```
  public void draw(Canvas canvas) {
  drawYellowCircle(canvas);
  drawRedRing(canvas);
  drawBlackNumber(canvas);
}

private void drawYellowCircle(Canvas canvas) {
  int size = (int) -ascent;
  Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
  paint.setStyle(Paint.Style.FILL);
  paint.setColor(Color.YELLOW);
  canvas.drawCircle(
    size/2, size/2, size/2, paint);
}
```





```
private void drawRedRing(Canvas canvas) {
  Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
  paint.setStyle(Paint.Style.STROKE);
  paint.setColor(Color.RED);
  float ringWidth = size / 10;
  paint.setStrokeWidth(ringWidth);
  canvas.drawCircle(
    size/2, size/2, size/2 - ringWidth/2, paint);
}
```




```
private void drawBlackNumber(Canvas canvas) {
  float ratio = 0.4f;
  Paint paint = new Paint(
    Paint.ANTI_ALIAS_FLAG);
  paint.setStyle(Paint.Style.FILL);
  paint.setColor(Color.BLACK);
  paint.setTextAlign(Paint.Align.CENTER);
  paint.setTextSize(textSize * ratio);
  paint.setStrokeWidth(strokeWidth);
  paint.setTypeface(
    Typeface.defaultFromStyle(Typeface.BOLD));
  int x = size / 2;
  int y = (int) (size/2 
    - ((descent + ascent)/2) * ratio);
  canvas.drawText(number, x, y, paint);
}
```




Reference from above code -> https://chiuki.github.io/advanced-android-textview






