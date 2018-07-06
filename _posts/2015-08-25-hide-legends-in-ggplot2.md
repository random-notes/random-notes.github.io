---
layout: post
title:  "Hide One or More Legends in ggplot2"
date:   2015-08-25
categories: visualization
tags: ggplot2 r
---
This is actually pretty simple thanks to [this StackOverflow post](#).
To disable the legend of a scale being shown in the legend, just set `guides(SCALE_NAME=FALSE)`, the follwing example ignores the legend of color.

```R
ggplot(mpg, aes(x=cty,y=hwy,color=manufacturer,shape=class)) + geom_point() + guides(color=FALSE)
```

```html
<a href="#">Hello world</a>
```

```cpp
int main(int argc, char** argv) {
	return 0;
}
```