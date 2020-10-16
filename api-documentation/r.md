# R

{% hint style="info" %}
You can find the complete open source R implementation of dstack here -[https://github.com/dstackai/dstack-r](https://github.com/dstackai/dstack-r)
{% endhint %}

## Pushing Frames

Creates a Frame, Commits and Pushes the Data in a Single Operation.

{% tabs %}
{% tab title="Parameters" %}
```r
push_frame(
  stack,
  obj,
  description = NULL,
  params = NULL,
  message = NULL,
  profile = "default",
  handler = auto_handler(),
  protocol = NULL,
  encryption = .no_encryption,
  ...
)
```

**`stack`** - A name of stack to use.

**`params`** - Parameters associated with this data, e.g. plot settings.
{% endtab %}

{% tab title="Example" %}
```r
library(ggplot2)
library(dstack)

df <- data.frame(x = c(1, 2, 3, 4), y = c(1, 4, 9, 16))
image <- ggplot(data = df, aes(x = x, y = y)) + geom_line()

push_frame("simple", image, "My first plot")
```
{% endtab %}
{% endtabs %}

## Create Frame

Suppose you want to publish a line plot that depends on the value of the parameter `Coefficient`\(slope\).

{% tabs %}
{% tab title="Parameters" %}
```r
create_frame(
  stack,
  profile = "default",
  auto_push = FALSE,
  protocol = NULL,
  encryption = NULL,
  check_access = TRUE
)
```

**`profile`** - A profile refers to credentials, i.e. username and token. Default profile is named 'default'. The system is looking for specified profile as follows: it looks into working directory to find a configuration file \(local configuration\), if the file doesn't exist it looks into user directory to find it \(global configuration\). The best way to manage profiles is to have dstack CLI tools installed. These tools are written in Python 3, so you have to install dstack support. In the case of PyPI you should type
{% endtab %}

{% tab title="Example" %}
```r
library(ggplot2)
library(dstack)

line_plot <- function(a) { 
    x <- c(0:20)
    y <- sapply(x, function(x) { return(a * x) })
    df <- data.frame(x = x, y = y)
    plot <- ggplot(data = df, aes(x = x, y = y)) + 
        geom_line() + xlim(0, 20) + ylim(0, 20)
    return(plot)
}

coeff <- c(0.5, 1.0, 1.5, 2.0)
frame <- create_frame(stack = "line_plot")
for(c in coeff) {  
    frame <- commit(frame, line_plot(c), 
        paste0("Line plot with the coefficient of ", c), list(Coefficient = a))
}

push(frame)
```
{% endtab %}
{% endtabs %}

## Commit

To Commit Data to Stack Frame. 

This function adds a new view to the stack frame. Multiple views can be added to one frame, but in this case every plot must be supplied with certain parameters to distiguish one view from another. In the case of single plot parameters are not necessary. For multiple views parameters will be automaticaly converted to UI controls like sliders and drop down lists.

{% tabs %}
{% tab title="Parameters" %}
```r
commit(
  frame,
  obj,
  description = NULL,
  params = NULL,
  handler = auto_handler(),
  ...
)
```

**`frame`** - A frame you want to commit.

**`obj`** - A data to commit. Data will be preprocessed by the handler but dependently on `auto_push` mode will be sent to server or not. If `auto_push` is False then the data won't be sent. Explicit push call need anyway to process committed data. `auto_push` is useful only in the case of multiple data objects in the stack frame, e.g. set of plots with settings.

**`description`** - Description of the data.

**`params`** - Parameters associated with this data, e.g. plot settings.

**`handler`** - A handler which can be specified in the case of custom content, but by default it is

**`...`** - Optional parameters is an alternative to `params`. If both are present this one will be merged into params
{% endtab %}

{% tab title="Example" %}
```r
library(ggplot2)
library(dstack)

line_plot <- function(a) { 
    x <- c(0:20)
    y <- sapply(x, function(x) { return(a * x) })
    df <- data.frame(x = x, y = y)
    plot <- ggplot(data = df, aes(x = x, y = y)) + 
        geom_line() + xlim(0, 20) + ylim(0, 20)
    return(plot)
}

coeff <- c(0.5, 1.0, 1.5, 2.0)
frame <- create_frame(stack = "line_plot")
for(c in coeff) {  
    frame <- commit(frame, line_plot(c), 
        paste0("Line plot with the coefficient of ", c), list(Coefficient = a))
}

push(frame)
```
{% endtab %}
{% endtabs %}

