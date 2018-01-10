---
layout: post
title: How to Create a Template Tag Filter for Django
---

Have you ever wanted to create a templatetag filter for Django? This is a guide to create an emoji filter that you can use in your Django templates.

Django makes things a heck of a lot easier when you know how to use it. It even provides a library of code that you can use to make filters and template tags that integrate directly with Django.

#### Setting ourselves up for business:
You can skip this part if you want to add to an existing Django app.

First, let's start a new project. In your terminal:
```sh
python manage.py startapp filters
cd filters
```
This starts a new Django project called filters in a directory called `filters`

Next, you need to build a basic app \(Make sure it is template based\). Then you need to create a folder called `templatetags` with a blank `__init__.py` file in it. you need the blank Python file so Django will recognize the `templatetags` directory as a module.

Great! Now that we're done with the prep work, we can start building our filter!

#### Building a filter:
Every filter is just its own Python file located in the `templatetags` directory that is located in the directory of the project you want to use the tags with, in this case it would be located in `filters/templatetags/`.

In that directory, create a file called `emoji_filter.py` and open the file in your favorite text editor, as long as that text editor supports Python. Before we get started coding our emoji filter, we have to import a few things:

```python
from django import template
from django.template.defaultfilters import stringfilter
```
On the first line, we are importing the template library, in other words, the library given to us by Django so we don't have to do all the heavy lifting. On the second line, we are importing a decorator that we will use to tell Django that our filter is only supposed to be used on strings. After that, we want to have a way to introduce our filter to Django. To set this up, type this next:

```python
register = template.Library()
```

Next, you want to make a dictionary of emojis, like this:

```python
emoji = {
  ':emoji_name:':'<THE HTML="THAT-YOU" WANT TO="REPLACE/IT.WITH">'
}
```

This is obviously done with many more emojis, with the actual emoji shortcode instead of `:emoji_name:`, and something like `<img src="link/to/the/emoji/image" style="display:inline-block;width:1rem;height:1rem;">` where `link/to/the/emoji/image` is replaced by a CDN link or something similar. Next we want to point Django towards the function that will become our filter and tell Django that it should only accept strings, by doing this:

```python
@register.filter(name='emoji')
@stringfilter
# def goes here
```

You are probably thinking, "When am I going to actually _make_ the filter?" The answer is now. First you need to define a function with only one parameter, `value`, and set a local variable, `output`, equal to `value`.

```python
# the decorators come directly before the emoji function
def emoji(value):
  output = value
```

Next \(still inside the emoji function\), you want to initialize a `for` loop that goes through the content and replaces every substring that matches each key in the emoji dictionary with its respective value. you can accomplish that like this:

```python
for key, value in emoji.iteritems():
  output = output.replace(key, value)
```

Finish the filter like this:

```python
return output
```

Aaaand you're done!

#### Here's the completed code

```python
from django import template
from django.template.defaultfilters import stringfilter

register = template.Library()

emoji = {
  # YOUR EMOJIS HERE
}

@register.filter(name='emoji')
@stringfilter
def delete(value):
  output = value
  for key, value in emoji.iteritems():
    output = output.replace(key, value)
  return output
```

#### Final thoughts:

If you have any tips on this topic or would like to suggest a topic for my next post, you can email me at [railinator4903@gmail.com](mailto:railinator4903@gmail.com).