---
title: A very simple Web App to learn chinese characters 
---

Based on the most common chinese characters that [I talked about before](/posts/2015-04-06-generate-wallpapers-using-python-inkscape.html), I created a very very simple web application to learn more characters, and to play with AngularJS. 

Technical Description
---------------------

I used AngularJS to build this application, this application doesn't have
a backend, everything is running on your browser. 

The app is composed by the following components:

- js/angular.js
- js/app.js           - Loads the app
- js/mainconroller.js - App logic, this file also contains a Javascript
  dictionary containing the most common chinese characters.
- index.html

Functional Description
----------------------

![](/images/chinese_app.jpg)

The app shows a chinese character, you have to write the Pinyin notation for
the character, and it's meaning in English. 

Chinese is a tonal language, tones are represented in Pinyin by accentuation.
There are some characters that are not easy to write in a normal keyboard, for
example:

```
ā ē ī ō ū 
ǎ ě ǐ ǒ ǔ 
```

The app shows these characters, if you click on them the character is passed to
the Pinyin field.

I have uploaded this mini-project to [Github](https://github.com/hugo-dc/learn_chinese), if you want to improve it, you are very welcome.

[Here you can see the app running](/learn_hanzi/index.html)
