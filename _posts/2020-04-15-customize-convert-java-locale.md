---
title: Customize java.util.Locale when serializing/deserializing
author: Cotes Chung
date: 2020-04-15 00:34:00 +0800
categories: [Coding, Springboot]
tags: [java, locale, json, serialize, deserialize]
toc: false
---

In [**Java**](https://github.com/cotes2020/jekyll-theme-chirpy/), the image files of [Favicons](https://www.favicon-generator.org/about/) are placed in `assets/img/favicons/`. You may need to replace them with your own. So let's see how to customize these Favicons.

Converting to custom format of Locale properties when de/serializing is often cumbersome.
While ''java.util.Locale'' follows [**ISO 86**](https://github.com/cotes2020/jekyll-theme-chirpy/) standard, Apache LocaleUtil allows almost every
kind of format including underscores as separator e.g. --da_DK--

```import com.fasterxml.jackson.databind.util.StdConverter;

import java.util.Locale;

public class LocaleCustomConverter extends StdConverter<Locale, String> {

    @Override
    public String convert(Locale locale) {
        return locale.toLanguageTag();
    }
}
```

