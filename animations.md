---
description: >-
  Animations provide a way to make adjustments to the UI in response to
  user actions over a pre-determined amount of time.
---

# Animations

Animations in Rock Mobile Shell are provided by the [Xamanimation](https://github.com/jsuarezruiz/Xamanimation) library. That page has some samples and screenshots to give you a few ideas of what you can build. But we'll give you some information on _how_ to build it here.

There are a few terms to be aware of:

* `Animations` - These are things like `FadeToAnimation`, they handle the actual animation you want to perform.
* `Triggers` - These provide a way to start an animation, usually in response to a user initiated action.
* `Behaviors` - These are a special type of animation allow you to change a property in response to the progress of another property. For example you might change the color of a button based on a slider the user is interacting with.

## Defining Animations

Let's talk about simple animations first. Animations are referenced by name and defined in the Resources of an element. In order to access an animation, it must be defined in a ancestor of where you are trying to reference it. Said another way, let's suppose you have the following XAML defined:

```xaml
<ScrollView>
  <StackLayout>
    <Label x:Name="myLabel" Text="Hello Rock!" />
  </StackLayout>
</ScrollView>
```

If you wanted to apply an animation to the label called `myLabel` then you would need to define it on either the `StackLayout` or the `ScrollView`. As long as it's defined **above** the element you are going to be referencing it from then you are fine. You cannot define it **on** the element itself.

So let's take an example of what this might look like:

```xaml
<ScrollView>
  <StackLayout>
    <StackLayout.Resources>
      <Rock:FadeToAnimation x:Key="FadeOut"
                            Target="{x:Reference myLabel}"
                            Duration="2000"
                            Opacity="0" />
    </StackLayout.Resources>
    
    <Label x:Name="myLabel" Text="Hello Rock!" />
  </StackLayout>
</ScrollView>
```

This defines an animation that references `myLabel` as the target of the changes it will be making. The animation is going to be changing the `Opacity` property of the label over a duration of 2,000 milliseconds (2 seconds). Finally, it will change the opacity of the label from it's current opacity to the target opacity of `0`. Note, this only defines the animation, it doesn't actually trigger it.

These types of animation definitions let you target specific visual elements with a pre-defined animation style.

## Starting Animations

Okay we were able to define an animation but we need to start it somehow. You have two choices when it comes to triggering an animation.

1. Trigger style in response to the user doing something.
2. Automatically when the view loads (behavior style).

Let's modify our example above but include a trigger style activator based on the user tapping a button:

```xaml
<ScrollView>
  <StackLayout>
    <StackLayout.Resources>
      <Rock:FadeToAnimation x:Key="FadeOut"
                            Target="{x:Reference myLabel}"
                            Duration="2000"
                            Opacity="0" />
    </StackLayout.Resources>
    
    <Label x:Name="myLabel" Text="Hello Rock!" />

    <Button Text="Fade">
      <Button.Triggers>
        <EventTrigger Event="Clicked">
          <Rock:BeginAnimation Animation="{StaticResource FadeOut}" />
        </EventTrigger>
      </Button.Triggers>
    </Button>
  </StackLayout>
</ScrollView>
```

We've added a button and a BeginAnimation trigger to it that starts the `FadeOut` animation we previously defined. Now, when the user taps the `Fade` button, the label above will slowly fade out over 2 seconds.

Another way we could define this is by behavior. Suppose we wanted the text to automatically fade out after the page loads? To do that we would use a BeginAnimationBehavior instead:

```xaml
<ScrollView>
  <StackLayout>
    <StackLayout.Resources>
      <Rock:FadeToAnimation x:Key="FadeOut"
                            Target="{x:Reference myLabel}"
                            Duration="2000"
                            Opacity="0" />
    </StackLayout.Resources>
    
    <Label x:Name="myLabel" Text="Hello Rock!">
      <Label.Behaviors>
        <Rock:BeginAnimationBehavior Animation="{StaticResource FadeOut}" />
      </Label>
    </Label>
  </StackLayout>
</ScrollView>
```

The way behaviors work is that they begin when the element they are attached to are themselves attached to their parent element. Because there might be a delay between when this happens and when the page itself becomes fully visible, there is a hard coded 250 millisecond delay before the referenced animation actually begins.

## Stopping Animations

You can also provide the ability to stop an animation by using a trigger. Technically, the library also supports stopping an animation using a behavior, but in our use case those don't actually make sense. If you are curious though, it is available as `EndAnimationBehavior`. But here is an example of two buttons, one to start and one to stop an animation.

```xaml
<ScrollView>
  <StackLayout>
    <StackLayout.Resources>
      <Rock:FadeToAnimation x:Key="FadeOut"
                            Target="{x:Reference myLabel}"
                            Duration="2000"
                            Opacity="0" />
    </StackLayout.Resources>
    
    <Label x:Name="myLabel" Text="Hello Rock!" />

    <Button Text="Start Fade">
      <Button.Triggers>
        <EventTrigger Event="Clicked">
          <Rock:BeginAnimation Animation="{StaticResource FadeOut}" />
        </EventTrigger>
      </Button.Triggers>
    </Button>

    <Button Text="Stop Fade">
      <Button.Triggers>
        <EventTrigger Event="Clicked">
          <Rock:EndAnimation Animation="{StaticResource FadeOut}" />
        </EventTrigger>
      </Button.Triggers>
    </Button>
  </StackLayout>
</ScrollView>
```

## Available Animations

All of the animations in this section support the following property.

| Property | Type | Description |
| :-- | :-- | :-- |
| Target | VisualElement | A reference to the targetted element to be animated. |
| Duration | string | The duration of the animation, in milliseconds. _Defaults to **1000**._ |
| Easing | EasingType | The easing to use when calculating the animation curve: `BounceIn`, `BounceOut`, `CubicIn`, `CubicInOut`, `CubicOut`, `Linear`, `SinIn`, `SinInOut`, `SinOut`, `SprintIn`, `SpringOut`. _Defaults to **Linear**._ |
| Delay | int | A delay in milliseconds before the animation will begin. _Defaults to **0**._ |
| RepeatForever | bool | Repeats the animation until stopped. _Defaults to **false**._ |


### BounceInAnimation

Animates the `Scale` and `Opacity` of the target to make it appear.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:BounceInAnimation x:Key="Demo"
                        Target="{x:Reference myLabel}"
                        Duration="2000" />
```

### BounceOutAnimation

Animates the `Scale` and `Opacity` of the target to make it disappear.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:BounceOutAnimation x:Key="Demo"
                         Target="{x:Reference myLabel}"
                         Duration="2000" />
```

### ColorAnimation

Animates the `BackgroundColor` of the target from it's current value to a new color.

| Property | Type | Description |
| :-- | :-- | :-- |
| ToColor | Color | The target color that the BackgroundColor will be transitioned to. |

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:ColorAnimation x:Key="Demo"
                     Target="{x:Reference myLabel}"
                     Duration="2000"
                     ToColor="#ee7725" />
```

### FadeInAnimation

Animates the `Opacity` and `Y` position of the element to make it visible. As the element is faded in it will also slide down or up to its final position.

| Property | Type | Description |
| :-- | :-- | :-- |
| Direction | string | The vertical movement direction of the element: `Up` or `Down`. _Defaults to **Up**._ |

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:FadeInAnimation x:Key="Demo"
                      Target="{x:Reference myLabel}"
                      Duration="2000"
                      Direction="Down" />
```


### FadeToAnimation

Animates the `Opacity` of the target from it's current value to a new value.

| Property | Type | Description |
| :-- | :-- | :-- |
| Opacity | double | The target opacity that the element should have once the animation finished. This value should be between `0` and `1.0`. |

**Example**

```xaml
<Rock:FadeToAnimation x:Key="Demo"
                      Target="{x:Reference myLabel}"
                      Duration="2000"
                      Opacity="0.25" />
```

### FadeOutAnimation

Animates the `Opacity` and `Y` position of the element to make it invisible. As the element is faded in it will also slide down or up from its current position.

| Property | Type | Description |
| :-- | :-- | :-- |
| Direction | string | The vertical movement direction of the element: `Up` or `Down`. _Defaults to **Up**._ |

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:FadeOutAnimation x:Key="Demo"
                       Target="{x:Reference myLabel}"
                       Duration="2000"
                       Direction="Down" />
```

### FlipAnimation

Animates the `Opacity` and `RotationY` axis of the element to make it visible. As the element is faded in it will also rotate along it's y-axis (that is, it will grow from a single vertical line to full-width) as if a card were being turned 90 degrees.

| Property | Type | Description |
| :-- | :-- | :-- |
| Direction | string | The horizontal rotation direction of the element: `Left` or `Right`. _Defaults to **Right**._ |

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:FlipAnimation x:Key="Demo"
                    Target="{x:Reference myLabel}"
                    Duration="2000"
                    Direction="Left" />
```


### HeartAnimation

Animates the `Scale` of the target to simulate a heart beat, growing and shrinking slighly.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:FadeInAnimation x:Key="Demo"
                      Target="{x:Reference myLabel}"
                      Duration="2000"
                      Direction="Down" />
```

### JumpAnimation

Animates the `TranslationY` of the target to cause it to jump up and down.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:JumpAnimation x:Key="Demo"
                    Target="{x:Reference myLabel}"
                    Duration="2000" />
```

### RelRotateToAnimation

Animates the `Rotation` of the target to spin the element by the number of degrees. This rotation happens along the Z-axis, meaning it it does not distort the element's perspective at all.

| Property | Type | Description |
| :-- | :-- | :-- |
| Rotation | double | The delta angle that the target should be rotated by (positive or negative number). |

**Example**

```xaml
<Rock:RelRotateToAnimation x:Key="Demo"
                           Target="{x:Reference myLabel}"
                           Duration="2000"
                           Rotation="-45" />
```

### RelScaleToAnimation

Animates the `Scale` of the target to make the element appear larger or smaller than it normally would.

| Property | Type | Description |
| :-- | :-- | :-- |
| Scale | double | The delta amount that the target should be scaled by. |

**Example**

```xaml
<Rock:RelScaleToAnimation x:Key="Demo"
                          Target="{x:Reference myLabel}"
                          Duration="2000"
                          Scale="0.8" />
```

### RotateToAnimation

Animates the `Rotatation` of the target to spin the element to the specified final rotation value. This rotation happens along the Z-axies, meaning it does not distort the element's perspective at all.

| Property | Type | Description |
| :-- | :-- | :-- |
| Rotation | double | The absolute angle that the target should be rotated to. |

**Example**

```xaml
<Rock:RotateToAnimation x:Key="Demo"
                        Target="{x:Reference myLabel}"
                        Duration="2000"
                        Rotation="180" />
```

### RotateXToAnimation

Animates the `RotationX` of the target to shift the perspective of the element.

| Property | Type | Description |
| :-- | :-- | :-- |
| Rotation | double | The absolute angle that the target should be rotated to. |

**Example**

```xaml
<Rock:RotateXToAnimation x:Key="Demo"
                         Target="{x:Reference myLabel}"
                         Duration="2000"
                         Rotation="90" />
```

### RotateYToAnimation

Animates the `RotationY` of the target to shift the perspective of the element.

| Property | Type | Description |
| :-- | :-- | :-- |
| Rotation | double | The absolute angle that the target should be rotated to. |

**Example**

```xaml
<Rock:RotateYToAnimation x:Key="Demo"
                         Target="{x:Reference myLabel}"
                         Duration="2000"
                         Rotation="90" />
```

### ScaleToAnimation

Animates the `Scale` of the target to make the element appear larger or smaller than it normally would.

| Property | Type | Description |
| :-- | :-- | :-- |
| Scale | double | The absolute value that the target should be scaled to, a value of `1` means normal scale. |

**Example**

```xaml
<Rock:ScaleToAnimation x:Key="Demo"
                       Target="{x:Reference myLabel}"
                       Duration="2000"
                       Scale="1.2" />
```

### ShakeAnimation

Animates the `TranslationX`  target to make it appear as if it were being shaken back and forth.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:ShakeAnimation x:Key="Demo"
                     Target="{x:Reference myLabel}"
                     Duration="2000" />
```

### TranslateAnimation

Animates the `TranslationX` and `TranslationY` properties of the target to shift the element around from it's normal position.

| Property | Type | Description |
| :-- | :-- | :-- |
| TranslateX | double | The value that the element should be shifted to on the x-axis. |
| TranslateY | double | The value that the element should be shifted to on the y-axis. |

**Example**

```xaml
<Rock:TranslateAnimation x:Key="Demo"
                         Target="{x:Reference myLabel}"
                         Duration="2000"
                         TranslateX="30"
                         TranslateY="-15" />
```

### TurnstileInAnimation

Animates the `RotationY` and `Opacity` properties of the target to give a turnstyle effect to the element appearing on screen.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:TurnstileInAnimation x:Key="Demo"
                           Target="{x:Reference myLabel}"
                           Duration="2000" />
```

### TurnstileOutAnimation

Animates the `RotationY` and `Opacity` properties of the target to give a turnstyle effect to the element disappearing from the screen.

> Note
>
> This animation ignores the `Easing` property.

**Example**

```xaml
<Rock:TurnstileOutAnimation x:Key="Demo"
                            Target="{x:Reference myLabel}"
                            Duration="2000" />
```

### GroupAnimation

This is a special animation that does not conform to the common properties mentioned prevoiusly. This element simply lets you group animations so that they can be triggered at the same time. Animations to be grouped together can be added as child elements.

**Example**

```xaml
<Rock:GroupAnimation x:Key="FadeOut">
    <Rock:FadeToAnimation Target="{x:Reference myLabel}"
                          Duration="2000"
                          Opacity="0" />
    <Rock:FadeToAnimation Target="{x:Reference myButton}"
                          Duration="2000"
                          Opacity="0" />
</Rock:StoryBoard>
```


## Custom Triggered Animations

It's all well and good that you have some standard animations you can use to make things appear and disappear, but what about doing something more custom? There are some ways you can do that too. First we will look at the Trigger versions of these, that is custom animations that start in response to a user action.

Each of the animations in this section support the following properties. The `From` and `To` property value types depend on the animation type. For example, the `AnimateDouble` will use `double` value types while the `AnimateInt` animation will use `int` value types.

| Property | Type | Description |
| :-- | :-- | :-- |
| From | _varies_ | The starting value for the animation. If not specified then the current value will be used. |
| To | _varies_ | The ending value for the animation. |
| Duration | uint | The duration of the animation in milliseconds. _Defaults to **1000**._ |
| Delay | int | The number of milliseconds to delay before activating the animation. _Defaults to **0**._ |
| Easing | EasingType | The easing to use when calculating the animation curve: `BounceIn`, `BounceOut`, `CubicIn`, `CubicInOut`, `CubicOut`, `Linear`, `SinIn`, `SinInOut`, `SinOut`, `SprintIn`, `SpringOut`. _Defaults to **Linear**._ |
| TargetProperty | string | The property to be set. This is not a simple property name but must include the class which defines the property, for example **VisualElement.BackgroundColor**. |

### AnimateColor

Animates a change in a property's color value.

**Example**

```xaml
<StackLayout>
  <Button Text="Animate">
    <Button.Triggers>
      <EventTrigger Event="Clicked">
        <Rock:AnimateColor TargetProperty="Button.TextColor"
                           To="#ee7725" />
      </EventTrigger>
    </Button.Triggers>
  </Button>
</StackLayout>
<Rock:
```

