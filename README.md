**Themed Battery Meter Example**
============================
The day has finally come when theme designers can once again theme the look of the status bar's battery meter.  The battery meter has been re-designed to make use of Android's native support for vector based drawables, allowing theme designers to overlay a handful of XML files to give the battery a fresh new look.

**How it works**
------------
Each battery type is made up of three layers that are composed into the final battery.  These layers are:

 1. Battery frame - The batteries background
 2. Battery level indicator - The drawable that is dynamically drawn based on the battery level and an associated animation which determines how much of the level to draw
 3. Charge indicator - A charging indicator drawn on top of the battery level when the device is charging.  This drawable is shared among all types of battery meters.

In addition to the three battery layers, there are three other XML files that define the how to compose the final battery meter.

 1. A layer-list - Defines how the three drawables that make up the battery are layered and positioned on the screen
 2. An animated-vector - This drawable specifies which drawable to animate for the battery level as well as the animation to use for determining how much and/or what parts of the level to draw based on the current battery level.
 3. An objectAnimator - This animation defines what properties will be animated for the battery level

Let's take a look at how this all comes together by looking at the circle battery meter XMLs

Battery frame - *drawable/ic_battery_circle_frame.xml*
-------------
    <?xml version="1.0" encoding="utf-8"?>
    <vector xmlns:android="http://schemas.android.com/apk/res/android"
        android:width="24dp"
        android:height="24dp"
        android:viewportWidth="24"
        android:viewportHeight="24">
    
        <path
            android:name="frame"
            android:strokeColor="@color/batterymeter_frame_color"
            android:strokeLineJoin="round"
            android:strokeWidth="3"
            android:pathData="@string/battery_circle_path"/>
    
    </vector>

Battery level - *drawable/ic_battery_circle_fill.xml*
-------------

    <?xml version="1.0" encoding="utf-8"?>
    <vector xmlns:android="http://schemas.android.com/apk/res/android"
        android:width="24dp"
        android:height="24dp"
        android:viewportWidth="24"
        android:viewportHeight="24">
    
        <!-- Path will be tinted based on battery level -->
        <path
            android:name="battery_level"
            android:strokeColor="#000000"
            android:strokeLineJoin="round"
            android:strokeWidth="3"
            android:pathData="@string/battery_circle_path" />
    </vector>

Charging indicator - *drawable/ic_battery_bolt.xml*
------------------

    <?xml version="1.0" encoding="utf-8"?>
    <vector xmlns:android="http://schemas.android.com/apk/res/android"
        android:width="24dp"
        android:height="24dp"
        android:viewportWidth="24"
        android:viewportHeight="24"
        android:tint="@color/batterymeter_bolt_color">
    
        <path
            android:fillColor="#000000"
            android:pathData="M10.5,7h5l-2,4h3l-7,6l2-5H8.5L10.5,7z" />
    </vector>

Animated vector drawable - *drawable/ic_battery_circle_avd.xml*
------------------------

    <?xml version="1.0" encoding="utf-8"?>
    <animated-vector xmlns:android="http://schemas.android.com/apk/res/android"
        android:drawable="@drawable/ic_battery_circle_fill" >
    
        <target
            android:name="battery_level"
            android:animation="@anim/battery_circle" />
    
    </animated-vector>

Level animation - *anim/battery_circle.xml*
---------------

    <?xml version="1.0" encoding="utf-8"?>
    <objectAnimator
      xmlns:android="http://schemas.android.com/apk/res/android"
      android:propertyName="trimPathEnd"
      android:valueFrom="0"
      android:valueTo="1"
      android:valueType="floatType"/>

 

Layer list - *drawable/ic_battery_circle.xml*
----------

    <?xml version="1.0" encoding="utf-8"?>
    <layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    
        <item
            android:id="@+id/battery_frame"
            android:drawable="@drawable/ic_battery_circle_frame"/>
    
        <item
            android:id="@+id/battery_fill"
            android:drawable="@drawable/ic_battery_circle_avd"/>
    
        <item
            android:id="@+id/battery_charge_indicator"
            android:drawable="@drawable/ic_battery_bolt"/>
    
    </layer-list>
