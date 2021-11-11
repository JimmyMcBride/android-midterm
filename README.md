# Midterm Exam

## Android

---

### Activity

> An activity is a single, focused thing that the user can do. Almost all activities interact with the user, so the Activity class takes care of creating a window for you in which you can place your UI with setContentView(View). While activities are often presented to the user as full-screen windows, they can also be used in other ways: as floating windows (via a theme with R.attr.windowIsFloating set), Multi-Window mode or embedded into other windows. - [Source](https://developer.android.com/reference/android/app/Activity)

#### Activity Lifecycle

```java
public class Activity extends ApplicationContext {
  /**
  Called when the activity is first created. This is where you should do all of your normal static set up: create views, bind data to lists, etc. This method also provides you with a Bundle containing the activity's previously frozen state, if there was one.
  Always followed by onStart().
  */
  protected void onCreate(Bundle savedInstanceState);

  /**
  Called after your activity has been stopped, prior to it being started again.
  Always followed by onStart()
  */
  protected void onRestart();

  /**
  Called when the activity is becoming visible to the user.
  Followed by onResume() if the activity comes to the foreground, or onStop() if it becomes hidden.
  */
  protected void onStart();

  /**
  Called when the activity will start interacting with the user. At this point your activity is at the top of its activity stack, with user input going to it.
  Always followed by onPause().
  */
  protected void onResume();

  /**
  Called when the activity loses foreground state, is no longer focusable or before transition to stopped/hidden or destroyed state. The activity is still visible to user, so it's recommended to keep it visually active and continue updating the UI. Implementations of this method must be very quick because the next activity will not be resumed until this method returns.
  Followed by either onResume() if the activity returns back to the front, or onStop() if it becomes invisible to the user.
  */
  protected void onPause();

  /**
  Called when the activity is no longer visible to the user. This may happen either because a new activity is being started on top, an existing one is being brought in front of this one, or this one is being destroyed. This is typically used to stop animations and refreshing the UI, etc.
  Followed by either onRestart() if this activity is coming back to interact with the user, or onDestroy() if this activity is going away.
  */
  protected void onStop();

  /**
  The final call you receive before your activity is destroyed. This can happen either because the activity is finishing (someone called Activity#finish on it), or because the system is temporarily destroying this instance of the activity to save space. You can distinguish between these two scenarios with the isFinishing() method.
  */
  protected void onDestroy();
}
```

![Android Lifecycle Model](https://developer.android.com/images/activity_lifecycle.png)

### Fragment

> A Fragment represents a reusable portion of your app's UI. A fragment defines and manages its own layout, has its own lifecycle, and can handle its own input events. Fragments cannot live on their own--they must be hosted by an activity or another fragment. The fragment’s view hierarchy becomes part of, or attaches to, the host’s view hierarchy. -
> [Source](https://developer.android.com/guide/fragments)

#### Modularity

> Fragments introduce modularity and reusability into your activity’s UI by allowing you to divide the UI into discrete chunks. Activities are an ideal place to put global elements around your app's user interface, such as a navigation drawer. Conversely, fragments are better suited to define and manage the UI of a single screen or portion of a screen.
>
> Consider an app that responds to various screen sizes. On larger screens, the app should display a static navigation drawer and a list in a grid layout. On smaller screens, the app should display a bottom navigation bar and a list in a linear layout. Managing all of these variations in the activity can be unwieldy. Separating the navigation elements from the content can make this process more manageable. The activity is then responsible for displaying the correct navigation UI while the fragment displays the list with the proper layout. - [Source](https://developer.android.com/guide/fragments#modularity)

![Fragment Screen Example](https://developer.android.com/images/guide/fragments/fragment-screen-sizes.png)

> Figure 1. Two versions of the same screen on different screen sizes. On the left, a large screen contains a navigation drawer that is controlled by the activity and a grid list that is controlled by the fragment. On the right, a small screen contains a bottom navigation bar that is controlled by the activity and a linear list that is controlled by the fragment.
>
> Dividing your UI into fragments makes it easier to modify your activity's appearance at runtime. While your activity is in the STARTED lifecycle state or higher, fragments can be added, replaced, or removed. You can keep a record of these changes in a back stack that is managed by the activity, allowing the changes to be reversed.
>
> You can use multiple instances of the same fragment class within the same activity, in multiple activities, or even as a child of another fragment. With this in mind, you should only provide a fragment with the logic necessary to manage its own UI. You should avoid depending on or manipulating one fragment from another. - [Source](https://developer.android.com/guide/fragments#modularity)

### Intent

> The startActivity(Intent) method is used to start a new activity, which will be placed at the top of the activity stack. It takes a single argument, an Intent, which describes the activity to be executed.
>
> Sometimes you want to get a result back from an activity when it ends. For example, you may start an activity that lets the user pick a person in a list of contacts; when it ends, it returns the person that was selected. To do this, you call the startActivityForResult(Intent, int) version with a second integer parameter identifying the call. The result will come back through your onActivityResult(int, int, Intent) method.
>
> When an activity exits, it can call setResult(int) to return data back to its parent. It must always supply a result code, which can be the standard results RESULT_CANCELED, RESULT_OK, or any custom values starting at RESULT_FIRST_USER. In addition, it can optionally return back an Intent containing any additional data it wants. All of this information appears back on the parent's Activity.onActivityResult(), along with the integer identifier it originally supplied.
>
> If a child activity fails for any reason (such as crashing), the parent activity will receive a result with the code RESULT_CANCELED. - [Source](https://developer.android.com/reference/android/app/Activity#starting-activities-and-getting-results)

```java
 public class MyActivity extends Activity {
  ...

  static final int PICK_CONTACT_REQUEST = 0;

  public boolean onKeyDown(int keyCode, KeyEvent event) {
    if (keyCode == KeyEvent.KEYCODE_DPAD_CENTER) {
      // When the user center presses, let them pick a contact.
      startActivityForResult(
        new Intent(Intent.ACTION_PICK,
        new Uri("content://contacts")), PICK_CONTACT_REQUEST);
      return true;
    }
    return false;
  }

  protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    if (requestCode == PICK_CONTACT_REQUEST) {
      if (resultCode == RESULT_OK) {
        // A contact was picked.  Here we will just display it
        // to the user.
        startActivity(new Intent(Intent.ACTION_VIEW, data));
      }
    }
  }
}

```

### Gradle

> Gradle is a build system (open source) that is used to automate building, testing, deployment, etc. “build.gradle” are scripts where one can automate the tasks. For example, the simple task to copy some files from one directory to another can be performed by the Gradle build script before the actual build process happens. - [Source](https://www.geeksforgeeks.org/android-build-gradle/)

Docs:

> The Android Studio build system is based on Gradle, and the Android Gradle plugin adds several features that are specific to building Android apps. Although the Android plugin is typically updated in lock-step with Android Studio, the plugin (and the rest of the Gradle system) can run independent of Android Studio and be updated separately. - [Source](https://developer.android.com/studio/releases/gradle-plugin)

### Res

> Class for accessing an application's resources. This sits on top of the asset manager of the application (accessible through getAssets()) and provides a high-level API for getting typed data from the assets.
>
> The Android resource system keeps track of all non-code assets associated with an application. You can use this class to access your application's resources. You can generally acquire the Resources instance associated with your application with getResources(). - [Source](https://developer.android.com/studio/releases/gradle-plugin)

### ViewBinding

> View binding is a feature that allows you to more easily write code that interacts with views. Once view binding is enabled in a module, it generates a binding class for each XML layout file present in that module. An instance of a binding class contains direct references to all views that have an ID in the corresponding layout.
>
> In most cases, view binding replaces findViewById. - [Source](https://developer.android.com/topic/libraries/view-binding)

#### Setup - [Source](https://developer.android.com/topic/libraries/view-binding#setup)

```groovy
android {
  ...
  buildFeatures {
    viewBinding = true
  }
}
```

#### Use view binding in activities - [Source](https://developer.android.com/topic/libraries/view-binding#activities)

```kotlin
private lateinit var binding: ResultProfileBinding

override fun onCreate(savedInstanceState: Bundle?) {
  super.onCreate(savedInstanceState)
  binding = ResultProfileBinding.inflate(layoutInflater)
  val view = binding.root
  setContentView(view)

  binding.name.text = viewModel.name
  binding.button.setOnClickListener { viewModel.userClicked() }
}
```

### Multi-threading

> Working on multiple tasks at the same time is Multitasking. In the same way, multiple threads running at the same time in a machine is called Multi-Threading. Technically, a thread is a unit of a process. Multiple such threads combine to form a process. This means when a process is broken, the equivalent number of threads are available.
>
> For example, Autocorrect is the process where the software looks for the mistakes in the current word being typed. Endlessly checking for the mistake and providing suggestions at the same time is an example of a Multi-Threaded process. - [Source](https://www.geeksforgeeks.org/multithreading-in-android-with-examples/)

### RecyclerView

> [RecyclerView](https://developer.android.com/reference/androidx/recyclerview/widget/RecyclerView) makes it easy to efficiently display large sets of data. You supply the data and define how each item looks, and the RecyclerView library dynamically creates the elements when they're needed.
>
> As the name implies, RecyclerView recycles those individual elements. When an item scrolls off the screen, RecyclerView doesn't destroy its view. Instead, RecyclerView reuses the view for new items that have scrolled onscreen. This reuse vastly improves performance, improving your app's responsiveness and reducing power consumption. - [Source](https://developer.android.com/guide/topics/ui/layout/recyclerview)

In order for a recycler to work, you need to set up an adapter. For more information on that, see the source linked above.

### Linear, Relative, Constraint Layout and other Widgets

#### LinearLayout

> LinearLayout is a type of view group which is responsible for holding views in it either Horizontally or vertically. It is a type of Layout where one can arrange groups either Horizontally or Vertically. - [Source](https://www.geeksforgeeks.org/difference-between-linearlayout-and-relativelayout-in-android/)

#### RelativeLayout

> RelativeLayout is a layout in which we can arrange views/widgets according to the position of other view/widgets. It is independent of horizontal and vertical view and we can arrange it according to one’s satisfaction. - [Source](https://www.geeksforgeeks.org/difference-between-linearlayout-and-relativelayout-in-android/)

#### ConstraintLayout

> LinearLayout is a type of view group which is responsible for holding views in it either Horizontally or vertically. It is a type of Layout where one can arrange groups either Horizontally or Vertically. - [Source](https://www.geeksforgeeks.org/difference-between-linearlayout-and-relativelayout-in-android/)

### Manifest

### Retrofit

### Moshi

### Navigation Component

### Glide

## Java

---

### OOP

### Design Patterns

### Static

### Implements

### Extends

## Kotlin

---

### Val vs Var vs Const Val

### Lateinit

### Lazy

### Null Safety

### Data Class

### Object

### Companion Object

### Scope Functions

### Extension Functions