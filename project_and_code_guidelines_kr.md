# 1. 프로젝트 가이드 라인

## 1.1 프로젝트 구조

새로운 프로젝트는 Android Gradle 프로젝트 구조에 따라야합니다 [Android Gradle plugin user guide]
(http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure). [ribot Boilerplate](https://github.com/ribot/android-boilerplate) 프로젝트는 프로젝트를 시작할때 좋은 참고 자료입니다.

## 1.2 파일 이름 지정

### 1.2.1 Class 파일
Class 이름은 [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase)로 작성합니다.

안드로이드의 컴퍼넌트를 상속받은 클래스들, 
Android 구성 요소를 확장하는 클래스의 경우 클래스 이름은 구성 요소의 이름으로 끝나야합니다. (ex : `SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`)

### 1.2.2 Resources 파일

Resources 파일 이름은 __lowercase_underscore__로 작성합니다.

#### 1.2.2.1 Drawable 파일

Drawable 이름 규약:


| Asset Type   | Prefix            |		Example               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	            | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	            | `ic_star.png`               |
| Menu         | `menu_	`           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	| `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

Icons 이름 규칙
 (taken from [Android iconography guidelines](http://developer.android.com/design/style/iconography.html)):

| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

selector states 이름 규칙:

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 Layout 파일

레이아웃 파일은 생성한 컴포넌트의 이름과 일치해야하지만 최상위 컴포넌트 이름은 처음으로 해야합니다. 예를 들어,`SignInActivity`를위한 레이아웃을 생성한다면, 레이아웃 파일의 이름은`activity_sign_in.xml`이어야합니다.


| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |


`Adapter`에 의해서 사용되는 레이아웃 파일을 생성할 경우엔 약간의 차이가 있습니다(ex:`ListView`에서 사용되는 경우). 이 경우, 레이아웃의 이름은`item_`로 시작해야합니다.

이러한 규칙을 적용 할 수없는 경우가 있습니다. 예를 들어, 다른 레이아웃의 일부로 의도 된 레이아웃 파일을 만들 때. 이 경우 접두어`partial_`을 사용해야합니다.

#### 1.2.2.3 Menu 파일

레이아웃 파일과 마찬가지로 메뉴 파일은 구성 요소의 이름과 일치해야합니다. 예를 들어,`UserActivity`에서 사용될 메뉴 파일을 정의한다면 파일의 이름은`activity_user.xml`이어야합니다.

좋은 예는`menu`라는 단어를`menu` 디렉토리에 이미 있기 때문에 이름의 일부로`menu`라는 단어를 포함하지 않는 것입니다.

#### 1.2.2.4 Values 파일

값 폴더의 리소스 파일은 __복수__여야합니다. (예 : `strings.xml`,`styles.xml`,`colors.xml`,`dimens.xml`,`attrs.xml`

# 2 Code guidelines

## 2.1 Java language rules

### 2.1.1 예외를 무시하지 말 것

__*Don't*__:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) { }
}
```

_코드가 이 오류 조건을 결코 겪지 않을 것이라고 생각하거나 그것을 처리하는 것이 중요하지 않다고 생각할 수 있습니다. 위와 같은 예외를 무시하면 다른 사람이 언젠가는 넘어야 할 코드 광산이 생깁니다. 코드에서 모든 예외 사항을 원칙적으로 처리해야합니다. 구체적인 취급 방법은 케이스에 따라 다릅니다._ - ([Android code style guidelines](https://source.android.com/source/code-style.html))

See alternatives [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions).

### 2.1.2 일반 예외로 잡지 말 것

__*Don't*__:

```java
try {
    someComplicatedIOFunction();        // throw IOException
    someComplicatedParsingFunction();   // throw ParsingException
    someComplicatedSecurityFunction();  // throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // 모든 예외상황을 받고
    handleError();                      // 같은 방법으로 처리를 한다.
}
```

[자세한 내용](https://source.android.com/source/code-style.html#dont-catch-generic-exception)

간단 요약 : 예상하지 못한 런타임 오류도 모두 잡아서 처리한다.

### 2.1.3 finalizers를 사용하지 말 것

_우리는 파이널 라이저를 사용하지 않습니다. 파이널 라이저가 호출될지 아니면 전부 다 호출 될 수 있을지 보장 할 수 없습니다. 대부분의 경우 훌륭한 예외 처리 기능을 사용하여 파이널 라이저에서 필요한 것을 할 수 있습니다. 꼭 필요한 경우 close () 메소드 (또는 비슷한 것)를 정의하고 메소드가 호출되어야 할 때 정확하게 문서화하십시오. `InputStream`을 보면. 이 경우 로그를 넘치게 하지 않는 한 종료 자에서 짧은 로그 메시지를 인쇄하는 것이 적절하지만 반드시 필요하지는 않습니다._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))

(_We don't use finalizers. There are no guarantees as to when a finalizer will be called, or even that it will be called at all. In most cases, you can do what you need from a finalizer with good exception handling. If you absolutely need it, define a `close()` method (or the like) and document exactly when that method needs to be called. See `InputStream` for an example. In this case it is appropriate but not required to print a short log message from the finalizer, as long as it is not expected to flood the logs._ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers)))

### 2.1.4 Fully qualify imports

__*Don't*__: `import foo.*;`

__*Do*__: `import foo.Bar;`

[자세한 내용](https://source.android.com/source/code-style.html#fully-qualify-imports)

## 2.2 자바 스타일 규칙

### 2.2.1 필드 정의 및 이름 지정

필드는 파일의 __맨 위에__ 정의되어야하며 아래에 나열된 이름 지정 규칙을 따라야합니다.

* Private, non-static 필드의 이름은 다음으로 시작됩니다 __m__.
* Private, static 필드의 이름은 다음으로 시작됩니다 __s__.
* 다른 필드는 소문자로 시작합니다.
* Static final fields (상수)는 ALL_CAPS_WITH_UNDERSCORES입니다.

Example:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

### 2.2.3 두문자어를 단어로 취급하십시오.

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.4 들여 쓰기에 공백 사용

블록에 __4 space__를 사용하십시오:

```java
if (x == 1) {
    x++;
}
```

줄 바꿈에 __8 space__ 사용 :

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.5 표준 중괄호 스타일 사용

중괄호는 앞에있는 코드와 같은 줄에 있습니다.

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

조건과 본문이 한 줄에 들어 가지 않으면 명령문 주위의 중괄호가 필요합니다.

조건과 본문이 한 줄에 있고 그 줄이 최대 줄 길이보다 짧으면 중괄호는 필요하지 않습니다, e.g.

```java
if (condition) body();
```

This is __bad__:

```java
if (condition)
    body();  // bad!
```

### 2.2.6 Annotations

#### 2.2.6.1 Annotations 관행

Android 코드 스타일 가이드에 따르면 Java에서 사전 정의 된 주석의 표준 사례는 다음과 같습니다:

* `@Override`: The @Override annotation __must be used__ whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.

@Override annotation은 메소드가 수퍼 클래스의 선언이나 구현을 오버라이드 __할 때마다__ 사용해야합니다. 예를 들어 @inheritdocs Javadoc 태그를 사용하고 클래스가 아니라 인터페이스에서 파생 된 경우, 당신은 또한 그 메소드가 부모 클래스의 메소드를`@Overrides`하는 것에 주석을 달아 주어야한다.

* `@SuppressWarnings`: @SuppressWarnings 주석은 경고를 제거 할 수없는 경우에만 사용해야합니다. 경고가 "제거 불가능"테스트를 통과하면 모든 경고가 코드의 실제 문제를 반영하도록 @SuppressWarnings 주석을 사용해야합니다.

주석 지침에 대한 자세한 내용은 [여기](http://source.android.com/source/code-style.html#use-standard-java-annotations)에서 확인할 수 있습니다.

#### 2.2.6.2 Annotations 스타일

__Classes, Methods and Constructors__

주석은 class, method 또는 constructor에 적용될 때 문서 블록 다음에 나열되며 __한 줄에 하나의 주석으로 표시__되어야합니다.

```java
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
public class MyAnnotatedClass { }
```

__Fields__

fields에 적용되는 Annotations는 줄이 최대 줄 길이(100)에 도달하지 않는 한 __같은 줄__에 나열되어야합니다.

```java
@Nullable @Mock DataManager mDataManager;
```

### 2.2.7 변수 범위 제한

_지역 변수의 범위는 최소한으로 유지되어야한다 (Effective Java Item 29). 이렇게하면 코드의 가독성과 유지 보수성이 향상되고 오류가 발생할 가능성이 줄어 듭니다. 각 변수는 변수의 사용되는 가장 안쪽의 블록에서 선언되어야합니다._

_지역 변수는 처음 사용한 시점에서 선언해야합니다. 거의 모든 지역 변수 선언은 이니셜 라이저를 포함해야합니다. 변수를 현명하게 초기화하는 데 필요한 정보가 충분하지 않은 경우에는 선언을 연기해야합니다._- ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))

### 2.2.8 import statements 순서

Android Studio와 같은 IDE를 사용하는 경우 IDE가 이미 이러한 규칙을 따르고 있으므로 걱정할 필요가 없습니다. 그렇지 않다면 아래를보십시오.

import statements의 순서 :

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax
4. Same project imports

IDE 설정을 정확히 일치 시키려면 imports가 다음과 같아야합니다:

* 각 그룹 내에서 알파벳순으로 정렬되며 소문자 앞에 대문자가옵니다 (예 : a 이전의 Z).
* 각 주요 그룹 (android, com, junit, net, org, java, javax) 사이에는 빈 줄이 있어야합니다.

[여기에 대한 자세한 정보](https://source.android.com/source/code-style.html#limit-variable-scope)

### 2.2.9 Logging guidelines

`Log` 클래스가 제공하는 로깅 메소드를 사용하여 개발자가 문제를 식별하는 데 유용한 오류 메시지 또는 기타 정보를 출력하십시오.

* `Log.v(String tag, String msg)` (verbose)
* `Log.d(String tag, String msg)` (debug)
* `Log.i(String tag, String msg)` (information)
* `Log.w(String tag, String msg)` (warning)
* `Log.e(String tag, String msg)` (error)

일반적으로 우리는 클래스 이름을 태그로 사용하고 파일의 맨 위에 `static final` 필드로 정의합니다. 예 :

```java
public class MyClass {
    private static final String TAG = MyClass.class.getSimpleName();

    public myMethod() {
        Log.e(TAG, "My error message");
    }
}
```

릴리즈 빌드에서는 VERBOSE 및 DEBUG 로그를 비활성화해야합니다. 또한 INFORMATION, WARNING 및 ERROR 로그를 비활성화하는 것이 좋지만 릴리스 빌드에서 문제를 식별하는 것이 유용 할 수 있다고 생각되는 경우 계속 사용하려고 할 수 있습니다. 사용하도록 설정 한 경우 이메일 주소, 사용자 ID 등과 같은 개인 정보가 누출되지 않도록해야합니다.

디버그 빌드의 로그만 표시하려면:

```java
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x);
```

### 2.2.10 Class member 순서

이에 대한 정확한 해결책은 하나도 없지만 __논리적__이고 __일관된 순서__를 사용하면 코드 학습 가능성과 가독성이 향상됩니다. 다음 순서를 따르는 것이 좋습니다:

1. Constants
2. Fields
3. Constructors
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

Example:

```java
public class MainActivity extends Activity {

    private static final String TAG = MainActivity.class.getSimpleName();

    private String mTitle;
    private TextView mTextViewTitle;

    @Override
    public void onCreate() {
        ...
    }

    public void setTitle(String title) {
    	mTitle = title;
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {

    }

}
```

If your class is extending an __Android component__ such as an Activity or a Fragment, it is a good practice to order the override methods so that they __match the component's lifecycle__. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

If your class is extending an Android component such as an Activity or a Fragment, it is a good practice to order the override methods so that they match the component's lifecycle. For example, if you have an Activity that implements `onCreate()`, `onDestroy()`, `onPause()` and `onResume()`, then the correct order is:

클래스가 Activity 또는 Fragment와 같은 __Android 구성 요소__를 extend하는 경우 override 메소드가 구성 요소의 __생명주기와 일치__하도록 정렬하는 것이 좋습니다. 예를 들어, `onCreate()`, `onDestroy()`, `onPause()` 및 `onResume()`을 구현 한 Activity가있는 경우 올바른 순서는 다음과 같습니다:

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.11 메소드의 매개 변수 순서

안드로이드를 프로그래밍 할 때는`Context`를 취하는 메소드를 정의하는 것이 일반적입니다. 이와 같은 메소드를 작성하는 경우 __Context__가 __첫 번째__ 매개 변수 여야합니다.

반대의 경우로 항상 __마지막__에 와야하는 매개 변수는 __callback__ 인터페이스입니다.

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.13 String 상수, 이름 지정 및 값

`SharedPreferences`,`Bundle`,`Intent`와 같은 안드로이드 SDK의 많은 요소들은 키 - 값 쌍 접근 방식을 사용하기 때문에 작은 응용 프로그램이라 할지라도 많은 문자열 상수를 작성해야 할 가능성이 높습니다.

When using one of these components, you __must__ define the keys as a `static final` fields and they should be prefixed as indicated below.

이 컴포넌트들 중 하나를 사용할 때, 키를 __반드시__ `static final` 필드로 정의해야하며, 다음과 같이 접두어를 붙여야합니다.

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

Fragment -`Fragment.getArguments()`의 인수는 또한 번들입니다. 그러나 이 번들은 매우 일반적인 용도이므로 다른 접두사를 정의합니다.

Example:

```java
// 필드의 값은 중복 문제를 피하기 위해 다음과 같습니다.
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent와 관련된 항목은 전체 패키지 이름을 값으로 사용합니다.
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 Arguments in Fragments and Activities

데이터가 `Intent` 또는 `Bundle`을 통해 `Activity` 또는 `Fragment`에 전달되면, 다른 값의 키는 위 섹션에서 설명 된 규칙을 __무조건__ 따라야합니다.

`Activity` 나 `Fragment`가 인자를 기대할 때 `Intent`나 `Fragment`를 쉽게 생성 할 수있는 public static 메소드를 제공해야합니다.

액티비티의 경우이 메소드는 일반적으로`getStartIntent()`라고 불린다:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

Fragment의 경우, 이름은 `newInstance()`이며 오른쪽 인수를 사용하여 Fragment의 생성을 처리합니다:

```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment();
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: 이 메소드들은`onCreate()`전에 클래스 최상단에 위치해야합니다.

__Note 2__: 위에서 설명한 메소드를 제공하면 extras 및 arguments의 키는 클래스 외부에 노출 될 필요가 없기 때문에 `private`이어야합니다.

### 2.2.15 Line length limit

코드 행은 __100자__를 넘지 않아야합니다. 행이이 제한보다 길면 길이를 줄이기 위해 일반적으로 두 가지 옵션이 있습니다.

* 지역 변수 또는 메소드를 추출합니다 (권장).
* 줄 바꿈을 적용하여 한 줄을 여러 줄로 나눕니다.

100보다 긴 행을 가질 수있는 두 가지 __예외__가 있습니다:

* 분할 할 수없는 행 (예 : 댓글의 긴 URL)
* `package`와 `import` 문.

#### 2.2.15.1 줄 바꿈 전략

라인 랩 (line-wrap) 방법을 설명하는 정확한 수식이 없으며 종종 다른 솔루션이 유효합니다. 그러나 일반적인 경우에 적용 할 수있는 몇 가지 규칙이 있습니다.

__연산자 줄바꿈__

연산자에서 라인이 끊어지면 연산자보다 __먼저__ 줄바꿈이 발생합니다. 예 :

```java
int longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne;
```

__대입 연산자 예외__

`break at operators` 규칙의 예외는 대입 연산자`=`입니다. 여기서 줄 바꿈은 연산자 __다음__에 일어나야합니다.


```java
int longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne;
```

__메소드 체인 case__

예를 들어 빌더를 사용할 때 같은 메소드에서 여러 메소드가 연결될 때 메소드의 모든 호출은 그 자체의 라인에 들어가야하고`.` 앞에 라인을 끊어야한다.

```java
//Don't
Picasso.with(context).load("http://ribot.co.uk/images/sexyjoe.jpg").into(imageView);
```

```java
//DO
Picasso.with(context)
        .load("http://ribot.co.uk/images/sexyjoe.jpg")
        .into(imageView);
```

__긴 매개변수 case__

메소드에 매개 변수가 많거나 매개 변수가 너무 길면 쉼표`,`

```java
//Don't
loadPicture(context, "http://ribot.co.uk/images/sexyjoe.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
//DO
loadPicture(context,
        "http://ribot.co.uk/images/sexyjoe.jpg",
        mImageViewProfilePicture,
        clickListener,
        "Title of the picture");
```

### 2.2.16 RxJava chains styling

Rx 체인 연산자는 줄 바꿈이 필요합니다. 모든 연산자는 새 행을 가져와야하며 행은`.` 앞에 분리되어야합니다.

```java
public Observable<Location> syncLocations() {
    return mDatabaseHelper.getAllLocations()
            .concatMap(new Func1<Location, Observable<? extends Location>>() {
                @Override
                 public Observable<? extends Location> call(Location location) {
                     return mRetrofitService.getLocation(location.id);
                 }
            })
            .retry(new Func2<Integer, Throwable, Boolean>() {
                 @Override
                 public Boolean call(Integer numRetries, Throwable throwable) {
                     return throwable instanceof RetrofitError;
                 }
            });
}
```

## 2.3 XML 스타일 rules

### 2.3.1 자체 닫기 태그 사용

XML 요소에 내용이 없으면 __무조건__ 자체 닫기 태그를 사용해야합니다.

This is good:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 리소스 네이밍

리소스 ID와 이름은 __lowercase_underscore__로 작성됩니다

#### 2.3.2.1 ID 네이밍

ID 앞에는 소문자와 언더스코어로 된 요소의 접두어가 붙어야합니다. 예 :

| Element            | Prefix            |
| -----------------  | ----------------- |
| `TextView`           | `text_`             |
| `ImageView`          | `image_`            |
| `Button`             | `button_`           |
| `Menu`               | `menu_`             |

ImageView 예:

```xml
<ImageView
    android:id="@+id/image_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu 예:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Strings

문자열 이름은 자신이 속한 섹션을 식별하는 접두사로 시작합니다. 예 :`registration_email_hint` 또는`registration_name_hint`. 문자열이 섹션에 속하지 않으면 아래 규칙을 따라야합니다.


| Prefix             | Description                           |
| -----------------  | --------------------------------------|
| `error_`             | An error message                      |
| `msg_`               | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| `action_`            | An action such as "Save" or "Create"  |


#### 2.3.2.3 Styles and Themes

다른 리소스와 달리 스타일 이름은 __UpperCamelCase__로 작성됩니다.

### 2.3.3 Attributes 순서

일반적으로 비슷한 속성을 그룹화해야합니다. 가장 일반적인 속성을 정렬하는 좋은 방법은 다음과 같습니다:

1. View Id
2. Style
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

## 2.4 테스트 스타일 규칙

### 2.4.1 단위 테스트

테스트 클래스는 테스트 대상 클래스의 이름과 일치해야하며 그 뒤에`Test`가옵니다. 예를 들어,`DatabaseHelper`에 대한 테스트를 포함하는 테스트 클래스를 만들면, 이름을`DatabaseHelperTest`로 지정해야합니다.

테스트 메소드에는`@Test` 주석이 달려 있으며 일반적으로 테스트 할 메소드의 이름으로 시작해야하며 그 다음에 전제 조건 및/또는 예상되는 동작이 따라옵니다.
(Test methods are annotated with `@Test` and should generally start with the name of the method that is being tested, followed by a precondition and/or expected behaviour.)

* Template: `@Test void methodNamePreconditionExpectedBehaviour()`
* Example: `@Test void signInWithEmptyEmailFails()`

시험이 없는 경우 사전 조건 및/또는 예상되는 동작이 항상 필요한 것은 아닙니다.
(Precondition and/or expected behaviour may not always be required if the test is clear enough without them.)


때때로 클래스에는 많은 양의 메소드가 포함될 수 있으며, 동시에 각 메소드에 대해 여러 테스트가 필요합니다. 이 경우 테스트 클래스를 여러 클래스로 분할하는 것이 좋습니다. 예를 들어,`DataManager`에 많은 메소드가 포함되어 있다면, `DataManagerSignInTest`,`DataManagerLoadUsersTest` 등으로 나누고 싶을 것입니다. 일반적으로 공통된 [test fixtures](https://en.wikipedia.org/wiki/Test_fixture)를 가지고 있기 때문에 어떤 테스트가 속하는지 볼 수 있습니다.

### 2.4.2 Espresso 테스트

모든 에스프레소 테스트 클래스는 일반적으로 액티비티를 대상으로하므로, 이름은 타겟 액티비티의 이름과 `Test`가 뒤따라야합니다. 예 : `SignInActivityTest`

Espresso API를 사용하는 경우 새 행에 연결된 메소드를 배치하는 것이 일반적입니다.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```

# License

```
Copyright 2015 Ribot Ltd.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
