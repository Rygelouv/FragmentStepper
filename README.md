# Introduction

FragmentStepper is a library that aims to make the process of creating steps screen (wizards) based 
on fragments more easy. The library handles internally:
- Fragments state by using FragmentStatePagerAdapter so your fragment are restored even 
when you navigate back and forth with a good memory management 
- Animations for the transition between steps
- Back and forth Navigation so you won't having to face the deadly deal of back stack management. 
Even Jake Wharton argues against the fragment back stack: [https://youtu.be/arch-talk](https://youtu.be/nP_B5-jrbsY?t=2494)

<img src="https://github.com/Rygelouv/FragmentStepper/blob/master/videotogif_2018.04.09_10.32.45.gif" width="250"> 

## Add it to your project

In your project root build.gradle with:
```gradle
allprojects {
    repositories {
        maven { url "https://jitpack.io" }
    }
}
```
and in the app or module build.gradle:

```gradle
dependencies {
    implementation 'com.github.Rygelouv:FragmentStepper:v0.0.3'
}
```

## How to use

#### Step 1: add it to your layout
```xml
<com.rygelouv.fragmentstepper.FragmentStepper
        android:layout_weight="1"
        android:id="@+id/stepper"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        />
```
#### Step 2: let your activity inherit from `StepsManager` and implements methods
```kotlin
class MainActivity : AppCompatActivity(), StepsManager{
    ...
    
    override fun getCount(): Int {
            return 3
        }
    
    override fun getStep(position: Int): Fragment {
        return when(position){
            0 -> FirstFragment()
            1 -> SecondFragment()
            2 -> ThirdFragment.newInstance("Boom", "Baam")
            else -> FirstFragment()
        }
    }
}
```
#### Step 3: configure stepper
```kotlin
stepper = findViewById(R.id.stepper)
stepper.setParentActivity(this)
stepper.stepsChangeListener = object : FragmentStepper.StepChangeListener {
    override fun onStepChanged(stepNumber: Int) {
        Toast.makeText(this@MainActivity, "Page changed", Toast.LENGTH_SHORT).show()
        // Do whatever you want here
    }
} 
```
#### Step 4: configure backstack in order to handle navigation
In your activity, override the `onBackPressed()` method and add this code
```kotlin
override fun onBackPressed() {
    if (stepper.isLastStep()) super.onBackPressed() else stepper.goToPreviousStep()
}
```
#### Step 5: there is no step 5. That's it you're all set

## TODO
- Handle animations for transition 
- Improve the way of handling backstack 
- Profile for memory management

## How it's done ?
FragmentStepper library is based on ViewPager but with some customization an adaptation to make fit for the 
specific need of steps/wizards flows. 

## Credits

Author: Rygel Louv [https://medium.com/@rygel](https://medium.com/@rygel)


License
--------

    Copyright 2017 Rygelouv.

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.