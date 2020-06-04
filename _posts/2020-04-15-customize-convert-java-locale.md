---
title: How to format java.util.Locale or any type of Object when serializing/deserializing using Jackson.
author: Shilan Kalhor
date: 2020-04-15 00:34:00 +0800
categories: [Coding, Springboot]
tags: [java, locale, json, serialize, deserialize, java8, Springboot]
toc: false
---

There are many tutorials out there about how to format LocalDate with Jackson, but what I was really missing was a custom Locale convertor. 
There are different formats and ISOs for displaying locale. Some accept underscores as separator (e.g. Apache LocaleUtil libraray) and some that are based on [**ISO 86??**](https://github.com/cotes2020/jekyll-theme-chirpy/) really don't. Hyphen is the accepted value for those. Like **en-US**. Besides it might be important for you to display the country in capital letter, etc.

## Let's jump to the code!
The @JsonDeserialize and @JsonSerialize annotations are our best friends here. :)
Simply annotate the property:

```java
import com.fasterxml.jackson.databind.annotation.JsonDeserialize;
import com.fasterxml.jackson.databind.annotation.JsonSerialize;
import java.util.Locale;

public class MyModel {

    @JsonSerialize(converter = LocaleCustomConverter.class)
    @JsonDeserialize(converter = StringToCustomLocaleConverter.class)
    private Locale myLocale;
}
```
The classes mentioned in front of the converter attribute our in charge of formatting.
You can convert them to your desired format. Here I am trying to make sure there always be hyphen between language and country, whether I am converting to String or generating Locale object from a String.
Therefore we create two classes that extend **StdConverter<Locale, String>**.
```java
import java.util.Locale;

/**
* Formats a Locale property when converting to String.
*/
public class LocaleCustomConverter extends StdConverter<Locale, String> {

    @Override
    public String convert(Locale locale) {
        //whatever custom format you dersire. Capitalize it, split it, whatever...
        return locale.toLanguageTag();
    }
}
```
Yeah, I know how you are feeling now. :) This  **StdConverter** class was originally introduced for java.utilLocalDate formatting however, luckily it can also be used for other type of objects.
Ok, let's find out how we gonna accept only a standard-based locale String:

```java
import java.util.Locale;

public class StringToCustomLocaleConverter extends StdConverter<String, Locale> {
    @Override
    public Locale convert(String value) {
        return Locale.forLanguageTag(value);
    }
}
```
Aaand that's it.

## What's happening underhood?
*StdConverter* class comes with a beautiful generic method, *convert* that needs to be overridden in our customized converter classes.
In our first example, *StdConverter<Locale, String>*, Locale is the IN object and String is the return type of this method; OUT.
```java
abstract OUT convert(IN in);
```
And since IN and OUT are generic objects, basically we can convert back and forth our arbitrary objects.

Happy coding! :)

{% if page.comments %} 
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://https-shilan-github-io.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            
{% endif %} 
