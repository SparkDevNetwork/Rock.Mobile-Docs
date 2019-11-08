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

## Triggering Animations

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

## Available Animations

All of the animations in this section support the following property.

| Property | Type | Description |
| :-- | :-- | :-- |
| Target | VisualElement | A reference to the targetted element to be animated. |
| Duration | string | The duration of the animation, in milliseconds. _Defaults to **1,000**._ |
| Easing | EasingType | The easing to use when calculating the animation curve: `BounceIn`, `BounceOut`, `CubicIn`, `CubicInOut`, `CubicOut`, `Linear`, `SinIn`, `SinInOut`, `SinOut`, `SprintIn`, `SpringOut`. _Defaults to **Linear**._ |
| Delay | int | A delay in milliseconds before the animation will begin. _Defaults to **0**._ |
| RepeatForever | bool | Repeats the animation until stopped. _Defaults to **false**._ |


### BounceInAnimation

Animates the `Scale` and `Opacity` of the target to make it appear.

### BounceOutAnimation

Animates the `Scale` and `Opacity` of the target to make it disappear.

### ColorAnimation

Animates the `BackgroundColor` of the target from it's current value to a new color.

| Property | Type | Description |
| :-- | :-- | :-- |
| ToColor | Color | The target color that the BackgroundColor will be transitioned to. |

### FadeInAnimation

Animates the `Opacity` and `Y` position of the element to make it visible. As the element is faded in it will also slide down or up to its final position.

| Property | Type | Description |
| :-- | :-- | :-- |
| Direction | string | The vertical movement direction of the element: `Up` or `Down`. _Defaults to **Up**._ |

### FadeToAnimation

Animates the `Opacity` of the target from it's current value to a new value.

| Property | Type | Description |
| :-- | :-- | :-- |
| Opacity | double | The target opacity that the element should have once the animation finished. |

### FadeOutAnimation

Animates the `Opacity` and `Y` position of the element to make it invisible. As the element is faded in it will also slide down or up from its current position.

| Property | Type | Description |
| :-- | :-- | :-- |
| Direction | string | The vertical movement direction of the element: `Up` or `Down`. _Defaults to **Up**._ |

### FlipAnimation

Animates the `Opacity` and `RotationY` axis of the element to make it visible. As the element is faded in it will also rotate along it's y-axis (that is, it will grow from a single vertical line to full-width) as if a card were being turned 90 degrees.

| Property | Type | Description |
| :-- | :-- | :-- |
| Direction | string | The horizontal rotation direction of the element: `Left` or `Right`. _Defaults to **Right**._ |

### HeartAnimation

Animates the `Scale` of the target to simulate a heart beat, growing and shrinking slighly.

### JumpAnimation

Animates the `TranslationX` and `TranslationY` to cause it to jump around a bit.

### RotateToAnimation


