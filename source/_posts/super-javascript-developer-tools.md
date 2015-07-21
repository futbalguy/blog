title: Super JavaScript Developer Tools
id: 14
categories:
  - Software Engineering
  - Javascript
tags:
  - Hack Reactor
  - Web Development
date: 2015-04-21 03:14:52
thumbnail: /superman-shirt.jpg
banner: /superman-shirt.jpg
---

I recently spent time setting up my developer environment for Hack Reactor bootcamp. Using the right tools can massively increase your productivity!

I recommend these Super JavaScript Development Tools:

1.  [Sublime Text](#Sublime)
2.  [SublimeLinter + JSHint](#SublimeLinter)
3.  [HTML/CSS/JS Prettify](#HTMLPrettify)
4.  [Sublime JS Build Console](#SublimeJSBuild)
5.  [Slate](#Slate)
6.  [Embrace Dark Mode!!!](#DarkMode)
<!-- more -->
<span style="color: #999999;">(Note: I learned a lot trying out different software so I suggest playing around)</span>

[showhide type="post1" more_text="Reveal JS vs iOS Comparison" less_text="Hide JS vs iOS Comparison"]

For iOS apps, I could write code, debug, and run an app within Xcode. Xcode has its flaws, but it is straightforward and it's nice not having to switch applications often.

In comparison, JS devs write code in text editors (with helpful plug-ins), debug and test in the browser, run version control and file management in the terminal, and even more! You need to be smart about how you manage your developer environment.

[/showhide]

### 1\. Sublime Text<a id="Sublime" class="anchor"></a>

[Sublime Text](http://www.sublimetext.com) is my most used app and appears to be the most popular text editor for JS devs.

Why is Sublime Text popular? It has developer-friendly features like syntax highlighting and code completion. In addition, a huge library of plugins (a.k.a. packages) let you customize and add functionality!

**SUPER TIP**: Install Version 3\. [This beta is actually recommended for daily use by the Sublime Text team.](http://www.sublimetext.com/blog/articles/sublime-text-3-build-3080) It has worked great for me.

### 2\. SublimeLinter + JSHint<a id="SublimeLinter" class="anchor"></a>

[SublimeLinter](http://www.sublimelinter.com/) is a must-have for efficient coding!

A linter checks your code for stylistic or programming errors _while you type_. No more surprise errors for missing parenthesis/brackets!

**SUPER TIP**: Follow [SublimeLinter + JSHint installation instructions here.](https://github.com/SublimeLinter/SublimeLinter-jshint) It's not as simple to install as other packages.

### 3\. HTML/CSS/JS Prettify<a id="HTMLPrettify" class="anchor"></a>

Pretty code is readable code. Readable code improves developer productivity.

Install the [HTML/CSS/JS Prettify](https://github.com/victorporof/Sublime-HTMLPrettify) in Sublime Text to prettify your code with a simple key press (Shift+CMD+H by default).

**SUPER TIP**: By default, prettified CSS code moves each HTML selector to a new line. This looks horrible. Therefore, I suggest you update the following code in _Sublime Text &gt; Preferences &gt; Package Settings &gt; HTML/CSS/JS Prettify &gt; Set Prettify Preferences_:

[code]
&quot;css&quot;: {
    ...
    &quot;selector_separator_newline&quot;: false //
 }
[/code]

### 4\. Sublime JavaScript Console<a id="SublimeJSBuild" class="anchor"></a>

Tired of switching to the browser to run every bit of code? That's because you're lazy...

JK...just install Sublime JS Build Console by [following Method 2](http://www.wikihow.com/Create-a-Javascript-Console-in-Sublime-Text). Now you just press CMD+B to run your code in Sublime Text.

**SUPER TIP**: Step 3 in the installation instructions above gave code for 'Node.sublime-build' that did not work for me. It didn't point to Node.JS correctly. Here is the code I used to get it to work:

[code]
{
	&quot;cmd&quot;: [&quot;/usr/local/bin/node&quot;, &quot;$file&quot;],
	&quot;selector&quot;: &quot;source.js&quot;
}
[/code]

### 5\. Slate<a id="Slate" class="anchor"></a>

Switching between application windows can be very inefficient...if your doing it wrong.

The right way is by using smart window management software like [Slate](https://github.com/jigish/slate).

Slate lets use you use keyboard shortcuts to move, switch, and show/hide windows. A major plus is the high level of customization offered.

Here is a sample of they keyboard shortcuts I use:

*   CMD+1: Split-screen Google Chrome &amp; Sublime Text
*   CMD+2: Split-screen Google Chrome &amp; Terminal
*   CMD+Shift+Up/Down/Left/Right: Move window to half of screen width/height
*   CMD+Option+Up/Down/Left/Right: Focus window in direction
[showhide type="post2" more_text="Reveal My Slate Configuration" less_text="Hide My Slate Configuration"]

This is my Slate configuration file. Filepath should be '~/.slate'

[code]
config defaultToCurrentScreen true
config nudgePercentOf screenSize
config resizePercentOf screenSize

# Shows app icons and background apps, spreads icons in the same place.
config windowHintsShowIcons true
config windowHintsIgnoreHiddenWindows false
config windowHintsSpread true

config layoutFocusOnActivate true

# Relaunch slate
bind esc:cmd relaunch

# Alias
alias lefthalf push left bar-resize:screenSizeX/2
alias righthalf push right bar-resize:screenSizeX/2
alias tophalf push up bar-resize:screenSizeY/2
alias bottomhalf push down bar-resize:screenSizeY/2
alias fullscreen push up bar-resize:screenSizeY

# Push Bindings
bind right:ctrl;cmd  ${righthalf}
bind left:ctrl;cmd   ${lefthalf}
bind up:ctrl;cmd     chain ${tophalf} | ${fullscreen}
bind down:ctrl;cmd   ${bottomhalf}

# Focus Bindings
bind right:cmd,alt    focus right
bind left:cmd,alt      focus left
bind up:cmd,alt        focus up
bind down:cmd,alt      focus down
#bind up:cmd   focus behind
#bind down:cmd focus behind

# Layout's directive - layout name 'app name':OPTIONS operations
layout javascriptLayout 'Google Chrome':REPEAT ${lefthalf}
layout javascriptLayout 'Sublime Text':REPEAT ${righthalf}

layout mailLayout 'Mail':REPEAT ${fullscreen}

layout spotifyLayout 'Spotify':REPEAT ${fullscreen}

layout iTermLayout 'Google Chrome':REPEAT ${lefthalf}
layout iTermLayout 'iTerm':REPEAT ${righthalf}

# Layout Bindings
bind 1:cmd layout javascriptLayout
bind 2:cmd layout iTermLayout
bind 3:cmd layout mailLayout
bind 4:cmd layout spotifyLayout
[/code]

[/showhide]

### 6\. Embrace Dark Mode!!!<a id="DarkMode" class="anchor"></a>

I never "got" dark mode UNTIL I started spending long days staring at text.

Here are some dark mode tweaks I recommend:

<ul>
	<li>Sublime Text</li>

*   Install [Soda Dark theme](https://github.com/buymeasoda/soda-theme). Note: this goes further than a normal dark color scheme.
	<li>Mac OS X</li>

*   Select dark menu bar: _Preferences > General > Use dark menu bar and Dock_ and then...
*   Choose matching dark wallpaper - [Reddit Link](http://www.reddit.com/r/osx/comments/2jpzoc/yosemite_tip_dark_mode_reduced_transparency_11/)
	<li>Google Chrome</li>

*   Install dark theme: [Slinky Elegant](https://chrome.google.com/webstore/detail/slinky-elegant/bmanlajnpdncmhfkiccmbgeocgbncfln)