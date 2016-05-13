# Easy Video Player

Easy Video Player is a simple but powerful view that you can plugin to your apps to quickly get
video playback working.

<img src="https://raw.githubusercontent.com/afollestad/easy-video-player/master/art/showcase.png" width="400px" />

#### Features

* *Based on the stock MediaPlayer API.* It will work on all devices and all CPUs, and it works with both local and remote sources.
* *Simple.* Much less code is required than alternative options to get up and running.
* *Very configurable.* There are lots of options available to make the player behave exactly how you want it to behave.
* *Adaptive.* The player use the colors of your (AppCompat) Activity theme automatically.

---

# Gradle Dependency

[ ![jCenter](https://api.bintray.com/packages/drummer-aidan/maven/easy-video-player/images/download.svg) ](https://bintray.com/drummer-aidan/maven/easy-video-player/_latestVersion)
[![Build Status](https://travis-ci.org/afollestad/easy-video-player.svg)](https://travis-ci.org/afollestad/easy-video-player)
[![License](https://img.shields.io/badge/license-Apache%202-4EB1BA.svg?style=flat-square)](https://www.apache.org/licenses/LICENSE-2.0.html)

*Coming soon.*

# Getting Started

#### Configuring a Player Activity

You will need an `Activity` in your app that will hold the `EasyVideoPlayer` view and playback content.
There's only a bit of configuration required.

*First, the Activity needs to use a theme from Google AppCompat. Here's an example from the sample project:*

```xml
<style name="AppTheme" parent="Theme.AppCompat.NoActionBar">

    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primary_dark</item>
    <item name="colorAccent">@color/accent</item>

</style>
```

*Second, the Activity should disable recreation on orientation changes. This allows playback to continue
when the device orientation changes. The player will adapt the aspect ratio accordingly. You just need to
set `android:configChanges` values to your `Activity` in `AndroidManifest.xml`:*

```xml
<activity
    android:name=".MyPlayerActivity"
    android:label="@string/my_player_activity"
    android:configChanges="orientation|keyboardHidden|screenLayout|screenSize"
    android:theme="@style/AppTheme" />   <!-- Don't need to set the theme here if it's set on your <application /> tag already -->
```

#### Layouts

The layout for your player Activity can be very simple. You only need a `EasyVideoPlayer` view,
all the controls and everything else are created by the player view itself.

```xml
<?xml version="1.0" encoding="utf-8"?>
<com.afollestad.easyvideoplayer.EasyVideoPlayer xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/player"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.afollestad.easyvideoplayersample.MainActivity" />
```

#### Code Setup

Since your `Activity` is using an AppCompat theme, make sure it extends `AppCompatActivity`.

Initializing the player is very simple. You just set a callback listener to receive notifications of
important events, and a source.

```java
public class MyPlayerActivity extends AppCompatActivity implements EasyVideoCallback {

    private static final String TEST_URL = "http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4";

    private EasyVideoPlayer player;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_myplayer);

        // Grabs a reference to the player view
        player = (EasyVideoPlayer) findViewById(R.id.player);

        // Sets the callback to this Activity, since it inherits EasyVideoCallback
        player.setCallback(this);

        // Sets the source to the HTTP URL held in the TEST_URL variable.
        // To play files, you can use Uri.fromFile(new File("..."))
        player.setSource(Uri.parse(TEST_URL));

        // From here, the player view will show a progress indicator until the player is prepared.
        // Once it's prepared, the progress indicator goes away and the controls become enabled for the user to begin playback.
    }

    @Override
    public void onPause() {
        super.onPause();
        // Make sure the player stops playing if the user presses the home button.
        player.pause();
    }

    // Methods for the implemented EasyVideoCallback

    @Override
    public void onPreparing(EasyVideoPlayer player) {
        // TODO handle if needed
    }

    @Override
    public void onPrepared(EasyVideoPlayer player) {
        // TODO handle
    }

    @Override
    public void onBuffering(int percent) {
        // TODO handle if needed
    }

    @Override
    public void onError(EasyVideoPlayer player, Exception e) {
        // TODO handle
    }

    @Override
    public void onCompletion(EasyVideoPlayer player) {
        // TODO handle if needed
    }

    @Override
    public void onRetry(EasyVideoPlayer player, Uri source) {
        // TODO handle if used
    }

    @Override
    public void onSubmit(EasyVideoPlayer player, Uri source) {
        // TODO handle if used
    }
}
```

You can see almost identical code in action in the sample project.

---

# Programmatic Control

Here's a list of methods that can be used to control the `EasyVideoPlayer` view programatically.
Methods used to change behavior are discussed in the next section.

```java
EasyVideoPlayer player = // ...

player.setSource(Uri);

player.setCallback(EasyVideoCallback);

player.start();

player.seekTo(int);

player.pause();

player.stop();

player.reset();

player.release();

player.showControls();

player.hideControls().

player.toggleControls();

player.isControlsShown();

player.isPrepared();

player.isPlaying();

player.getCurrentPosition();

player.getDuration();
```

---

# Configuration

There are a few options that can be to change the default behavior of the `EasyVideoPlayer`:

```java
EasyVideoPlayer player = // ...

// EasyVideoPlayer.LEFT_ACTION_NONE:     hides all left actions.
// EasyVideoPlayer.LEFT_ACTION_RESTART:  the default, shows the skip back to beginning button.
// EasyVideoPlayer.LEFT_ACTION_RETRY:    shows a textual 'Retry' button, invokes the onRetry() callback method.
player.setLeftAction(int);

// EasyVideoPlayer.RIGHT_ACTION_NONE:    the default, hides all right actions.
// EasyVideoPlayer.RIGHT_ACTION_SUBMIT:  shows a textual 'Submit' button, invokes the onSubmit() callback method.
player.setRightAction(int);

// Defaults to true. The controls fade out when playback starts.
player.setHideControlsOnPlay(boolean);

// Defaults to false. Immediately starts playback when the player becomes prepared.
player.setAutoPlay(boolean);

// Sets a position that will be skipped to right when the player becomes prepared. Only happens once when set.
player.setInitialPosition(int);
```