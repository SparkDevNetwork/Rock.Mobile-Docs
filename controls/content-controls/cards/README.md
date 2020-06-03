# Cards

Cards are an essential piece of UI that serve as an entry point to additional information. As you'll see they are very extensible and allow you to quickly achieve remarkable results.

## Card Types

There are three types of cards for you to choose from. While they share many of the same elements that each have a different design style.

### Contained Card

The contained card is the classic design that keeps all of the content inside of a frame. There's plenty of options to adjust both the frame and content.

![Contained Card](../../../.gitbook/assets/image%20%2834%29.png)

```text
<Rock:ContainedCard 
    Tagline="DESTINATIONS"
    Title="Start Your Adventure"
    Image="https://yourserver.com/image-grandtetonfence.jpg">
        Bring to the table win-win survival strategies to ensure...
</Rock:ContainedCard>
```

### Block Card

The block card is very similar to the Contained Card but the content is not in a clearly visible frame.

![Block Card](../../../.gitbook/assets/image%20%2833%29.png)

```text
<Rock:BlockCard 
    Tagline="DESTINATIONS"
    Title="Start Your Adventure"
    Image="https://yourserver.com/image-grandtetonpeak.jpg">
    Bring to the table win-win survival strategies... 
</Rock:BlockCard>
```

### Inline Card

The Inline Card places the image behind the content and provides several options to create striking designs.

![Inline Card](../../../.gitbook/assets/image%20%2832%29.png)

```text
<Rock:InlineCard 
    Title="Start Your Adventure"
    HeaderLocation="Top"
    ContentLocation="Bottom"
    Tagline="DESTINATIONS"
    Image="https://yourserver.com/image-grandtetonfence.jpg">
    Bring to the table win-win survival strategies to ensure...
</Rock:InlineCard>
```

