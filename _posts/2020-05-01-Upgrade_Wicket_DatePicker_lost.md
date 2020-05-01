---
title: Date/time picker with Wicket 8.
author: Shilan Kalhor
date: 2020-04-15 00:34:00 +0800
categories: [Coding, Springboot]
tags: [java, locale, json, serialize, deserialize, java8, Springboot]
toc: false
---

Upgraded to Wicket 8 and boom, your classic datepicker doesn't work anymore.
If so, take a look here. I have good news for you. :)
I cannot agree more that [Wicket] is a weirdo choice for building frontend and doesn't happen to many poeple to have to work with it. Well there are exceptions though.

I upgraded the legacy Wicket code and I noticed that the Datepicker dependency is not compliance with the rest of Wicket 8 libraries.
I started searching for something that is capable of both handling java 8 *LocalDate* and can be formatted easily. Unfortunately there weren't that many options unti I bumped into awesome [**jQuery Kundo**](http://www.7thweb.net/wicket-jquery-ui) Datepicker. 
Formatting the code with java was not that straight forward and the model of implementation was totally different from the classic datepicker. 
Of course there is a [document](http://www.7thweb.net/wicket-jquery-ui/datepicker/DefaultDatePickerPage) explaining how to use the Kundo DatePicker but for example the part about formatting was missing.

## Jump to the code!
If you want to use and convert your chosen date to  java 8 `java.util.LocalDate`, you should try to import from local.DatePicker package otherwise import `com.googlecode.wicket.kendo.ui.form.datetime.DatePicker` class to convert to `java.util.Date`.
In quato "`I hate those tutorials that don't share the imports. :|`"

```java
import com.googlecode.wicket.kendo.ui.form.datetime.local.DatePicker;
import com.googlecode.wicket.kendo.ui.form.datetime.local.LocalTextField;
import com.googlecode.wicket.jquery.core.Options;
import org.apache.wicket.markup.html.form.Form;

public class MyPage {
    Form form = new Form<Void>("form");
    /* whatever implementation that goes in your page class */

    final LocalTextField<LocalDate> fromDatePicker = new DatePicker("fromDateTextField",
            new PropertyModel<>(dataProvider, "selectedFromDate"),
            "yyyy-MM-dd",
            new Options("dateFormat", Options.asString("yyyy-MM-dd")));
    fromDatePicker.setRequired(true);
    form.add(fromDatePicker);
}
```
Remember to add it to your html page:

```html
<form wicket:id="form">
  <input type="text" wicket:id="fromDateTextField" />
</form>  
```
**Note** with Kundo DatePicker you won't need to create a separate textfield like old days but everything comes along with DatePicker class. Honestly I loved this feature of Kundo DatePicker.

## What is going on under the hood!
public abstract class LocalTextField <T> extends org.apache.wicket.markup.html.form.TextField<T>
