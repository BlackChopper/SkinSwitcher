# SkinSwitcher
[![](https://img.shields.io/badge/platform-android-orange.svg)](https://github.com/hacknife) [![](https://img.shields.io/badge/language-java-yellow.svg)](https://github.com/hacknife) [![](https://img.shields.io/badge/JCenter-1.0.4-brightgreen.svg)](http://jcenter.bintray.com/com/hacknife/skinswitcher/) [![](https://img.shields.io/badge/build-passing-brightgreen.svg)](https://github.com/hacknife) [![](https://img.shields.io/badge/license-apache--2.0-green.svg)](https://github.com/hacknife) [![](https://img.shields.io/badge/api-14+-green.svg)](https://github.com/hacknife)<br/><br/>
SkinSwitcher是一个动态换肤的开源框架，换肤成功之后不需要重启应用就能达到换肤的效果，同时它支持动态更换布局文件中定义的绝大部分属性。皮肤资源来自于外置的apk文件，而不是app内部的皮肤资源。[English](https://github.com/hacknife/SkinSwitcher/blob/master/README_ENGLISH.md)
<br/>
![Image Text](https://github.com/hacknife/SkinSwitcher/blob/master/skinswitcher.gif)
<br/>
## 使用说明
需要替换的属性资源必须为引用型(@dimen/xxxxx)，直接型（xxdp）是无法进行换肤的。进行换肤的控件对应的属性必须有相应的方法。如果没有，开发者也可以实现相应的方法。
### 代码示例
```Java
public class BaseActivity extends Activity{
    protected SkinFactory factory;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        factory = new SkinFactory() {
            /**
             * 属性过滤器
             * @param attrName textColor,background,drawable 等.
             * @return 如果为真，表示我们需要替换该属性，同时该属性对应的View也会通过onSwitcher方法
             */
            @Override
            public boolean onAttributeFilter(String attrName) {
                return attrName.contains("background");
            }

            /**
             * 皮肤切换器，当我们设置皮肤包以后，这两个方法（onAttributeFilter，onSwitcher）才会调用。
             * @param view 如果当前Activity包含多个需要换肤的控件，那么此方法也会对应执行相应的次数，使用者需要自行判断view的类型。
             * @param attrType 此属性用于辅助使用者判断当前view需要替换的资源类型。（string,mipmap,drawable,dimen,color等等）
             * @param attrName 属性名称。此属性是通过onAttributeFilter方法过滤。使用者可以通过属性名称，调用相应的方法设置View的属性，以达到换肤的目的。（textColor,background,drawable 等）
             * @param attrValue 属性应用值（@dimen/xxx,@color/xxxx,@mimap/xxxx and so on）
             * @param attrObj 皮肤包中读取的属性值.
             */
            @Override
            public void onSwitcher(View view, ResourceType attrType, String attrName, String attrValue, Object attrObj) {
            
                view.setBackgroundColor((Integer) attrObj);
            }
        };
        LayoutInflaterCompat.setFactory2(getLayoutInflater(), factory);
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        factory.apply();
    }
}
```
```Java
public class BaseActivity extends Activity implements SkinFactory2.OnSkinFactory {
    protected SkinFactory2 factory;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        factory=new SkinFactory2();
        factory.setOnSkinFactory(this);
        LayoutInflaterCompat.setFactory2(getLayoutInflater(), factory);
    }

    @Override
    protected void onRestart() {
        super.onRestart();
        factory.apply();
    }

    @Override
    public boolean onAttributeFilter(String attrName) {
        return attrName.contains("background");
    }

    @Override
    public void onSwitcher(View view, ResourceType attrType, String attrName, String attrValue, Object attrObj) {
        view.setBackgroundColor((Integer) attrObj);
    }
}

```
### 皮肤包制作
皮肤包就是apk包，apk中只需要包含使用到的资源即可。资源的类型，以及名称不能改变。
```Java
      SkinFactory.initSkinFactory(getApplicationContext());
      SkinFactory.loadSkinPackage("/sdcard/app-debug.apk");
```
## 快速引入项目
合并以下代码到需要使用Module的dependencies中。
```Java
	dependencies {
                ...
	        compile 'com.hacknife:skinswitcher:1.0.4'
	}
```
## 感谢浏览
如果你有任何疑问，请加入QQ群，我将竭诚为你解答。欢迎Star和Fork本仓库，当然也欢迎你关注我。
<br/>
![Image Text](https://github.com/hacknife/CarouselBanner/blob/master/qq_group.png)
