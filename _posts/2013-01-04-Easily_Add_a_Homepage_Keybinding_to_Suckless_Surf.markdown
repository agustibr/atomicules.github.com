---
layout: page
type: text
title: Easily Add a Homepage Keybinding to Suckless Surf
categories: 
- program
---
It doesn't really count as New Year's resolution, but since I like change I have decided to embrace as much of [suckless](http://suckless.org)'s stuff as possible. This means I'm going to try to switch from [Xmonad](http://xmonad.org/) to [dwm](http://dwm.suckless.org/)\*, [urxvt](http://software.schmorp.de/pkg/rxvt-unicode) to [st](http://st.suckless.org/)\*\* and [Luakit](http://luakit.org/)\*\*\* to [surf](http://surf.suckless.org/) + [tabbed](http://tools.suckless.org/tabbed/). Owing to my living-in-a-(virtual)box low memory requirements I switched away from Chrome and Firefox (good riddance!) to the excellent lightweight webkit-gtk alternatives, eventually settling on Luakit. But I've decided even Luakit is too good a browser for me. I figure the least productive thing I used last year was a web browser and so I need something less powerful so I am discouraged from wasting my life on the internet. And so if that means no searching without first navigating to a search page, no tab completion of history - indeed, no history at all - then that is better for me.

There are patches available for surf to provide some of this "missing" functionality, but I'm going to try to stay as patch free as possible because that makes it easier to stay up to date with the latest commits and more importantly keeps my browsing simple. However, one thing I did want was a way of loading a homepage, my pinboard starred items. This can be done very simply, avoiding any patches and scripts, by just adding in a key binding to `config.def.h` like so:

{% highlight c %}
    { MODKEY|GDK_SHIFT_MASK,GDK_h,      loaduri,      { .v = "https://pinboard.in/u:atomicules/starred/" } },
{% endhighlight %}

This means I can type CTRL+SHIFT+H and go straight to my pinboard starred items and I'll use that to get everywhere else (such as searching). This key combination conflicts with the default key bindings in tabbed, but I've changed them anyway.

The suckless tools are constructed very well. Even if you don't understand C (like me) it's easy to change key bindings, etc and in this way the `config.def.h` files become like dotfiles.

\* _It took a couple of days until I had the dwm lightbulb moment: tags are not workspaces! So I now understand why layouts affect all tags and why the [pertag](http://dwm.suckless.org/patches/pertag) patch doesn't really make sense. It's different from XMonad, but nothing I can't get used to. Being able to view multiple tags and have windows tagged multiple times could be an interesting way of working. Also, Fedora has a nice dwm-start script to automatically build a custom dwm on logon (I just want the [bstack](http://dwm.suckless.org/patches/bottom_stack) patches)_.

\*\* _st doesn't have scrollback so it means being old school and piping things like `ls -al` through a pager. Apart from that though, it seems fine and dandy. Even got it built on OSX 10.5 PPC_. 

\*\*\* _Also, as much as I like Vim, I'm not sold on the modal UI approach in web browsers. I love being able to use hjkl to scroll, etc, but it can be infuriating to click in a text box and insert mode not to activate and before you know it you've executed a load of commands you didn't mean to! Also, even though there are pass through modes, it makes it more fiddly to use shortcuts provided by websites_.
