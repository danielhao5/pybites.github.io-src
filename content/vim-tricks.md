Title: 5 Vim Tricks to Speed up Your Python Development
Date: 2017-03-28 9:00
Category: Tools
Tags: vim, tools, shortcuts, tricks, flake8, howdoi, conque, shell, virtualenv, nerdtree, pyperclip
Slug: vim-tricks
Authors: Bob
Summary: In this article 5 Vim shortcuts to speed up your Python development. 
cover: images/featured/pb-article.png

In this article 5 Vim shortcuts to speed up your Python development. These techniques are saving me tons of repeated cycles allowing me to better concentrate on the important: coding. 

Vim's learning curve might be steep, but with practice you start to ['edit text at the speed of light'](http://www.amazon.com/dp/1680501275/?tag=pyb0f-20) which makes you a better developer. This is not an article about what is the best editor, there are other awesome options: Emacs, PyCharm, Sublime ... I just love Vim and use it for almost all my editing. 

## Getting ready

Before diving in you need to know what a "Leader Key" is:

> The "Leader key" is a way of extending the power of VIM's shortcuts by using sequences of keys to perform a command. The default leader key is backslash. Therefore, if you have a map of <Leader>Q, you can perform that action by typing \Q. - [StackOverflow answer](http://stackoverflow.com/questions/1764263/what-is-the-leader-in-a-vimrc-file)

I mapped mine to comma using this in my vimrc:

    let mapleader = ","

So when I provide mappings like ,p ,f ,a and you use another mapleader, replace my comma with your key.

Regarding plugins: it's recommended to use [Vundle](https://github.com/VundleVim/Vundle.vim), a Vim plugin manager. See RealPython's [great introduction](https://realpython.com/blog/python/vim-and-python-a-match-made-in-heaven/) to this tool and a powerful Vim environment setup overall.

## 1. Save and run Python

Add this to your vimrc:

	nmap ,p :w<CR>:!python3 %<CR>

It first saves the output (:w), then runs (!) the current script (%)

For Python 2 I got another shortcut:

    nmap ,2 :w<CR>:!python2.7 %<CR>

I also have ,v to run the Python of my active [virtual env](http://pybit.es/the-beauty-of-virtualenv.html):

	nmap ,v :w<CR>:!venv/bin/python %<CR>

## 2. Flake8 check

As featured in our [pep8 article](http://pybit.es/pep8.html) I find it very useful to flake8 my code before committing. If you have not heard of [flake8](https://pypi.python.org/pypi/flake8), it is "the modular source code checker", a wrapper around PyFlakes, pycodestyle and Ned Batchelder’s McCabe script. You need the [vim-flake8](https://github.com/nvie/vim-flake8) plugin. I invoke it with ,f - having this in my vimrc:

    autocmd FileType python map <buffer> ,f :call Flake8()<CR>

## 3. Open Terminal / search StackOverflow in Vim

You will need the [Conque](https://github.com/vim-scripts/Conque-Shell) plugin. 

To open a Python in vertical split I use the 'cp' shortcut, vimrc: 

    nmap cp :ConqueTermVSplit python3<CR>

Of course you can specify any script. The advantage is that any generated output becomes editable with Vim.

Some years ago [I made a script](http://bobbelderbos.com/2013/01/search-copy-stackoverflow-data-in-vim-with-conque/) to query StackOverflow in a split window using Conque. I wanted to refactor that script but recently I discovered a better option: [howdoi](https://github.com/gleitz/howdoi). You can use [this plugin](https://github.com/laurentgoudet/vim-howdoi) to run it inside Vim. Very cool.

### 4. Toggle Vim and Shell / open multiple files in vertical split / q:

I use Ctrl + Z / fg a lot to go back and forth between coding and version control. [You can also type :sh / Ctrl + d](http://stackoverflow.com/questions/1879219/how-to-temporarily-exit-vim-and-go-back). 

For testing I usually open script.py and test_script.py alongside each other with: 
	
	$ vi -O script.py test_script.py

This is why PEP8's "Limit all lines to a maximum of 79 characters" is important: no wrapping lines.

Then use Ctrl + w + w to toggle between the split windows. If you want to open another file in vertical split you can run this from Vim's Command Mode:

    :vsp file

Another way to interact with the command line is via Vim's Command Mode. While writing this article I found [the useful q: shortcut](http://stackoverflow.com/questions/6920943/navigating-in-vims-command-mode).

Other options for shell integration are [screen](http://www.vim.org/scripts/script.php?script_id=2711) and [tmux](https://tmux.github.io). I'll leave that for a follow-up article ...

## 5. Navigate files

I use [NERD tree](https://github.com/scrooloose/nerdtree) which opens a nice file tree you can navigate with regular Vim strokes, I mapped it to Ctrl + x

    map <C-x> :NERDTreeToggle<CR>

Another option is [Command-T](https://github.com/wincent/command-t). I was blown away the first time I saw Gary Bernhardt use it in [Destroy All Software](https://www.destroyallsoftware.com/screencasts). I had some dependency / install issues last time, I need to try it again ... 

## Bonus trick: run your own script, pasting its output back into Vim

Similar to the howdoi intergration I managed to call an external script and paste its output into Vim. I used [pyperclip](http://pybit.es/pyperclip.html) to manage the clipboard and switched to MacVim because of clipboard support:

    $ /usr/bin/vim --version |grep clipboard
    -clipboard
    $ /Applications/MacVim.app/contents/MacOS/Vim --version|grep clipboard
    +clipboard

See the script [here](https://github.com/pybites/blog_code/blob/master/amazon/genlink.py). It takes a previously copied Amazon URL from the clipboard, converts it to an affiliation link and pastes it back to the clipboard. 

To run it and paste its output back into Vim I made this mapping in my vimrc: 

    nmap ,a :!genlink<CR><ESC>"+p
    # having genlink in PATH and pyperclip installed

"+ is the clipboard buffer.

For example:

    https://www.amazon.com/Practical-Vim-Edit-Speed-Thought/dp/1680501275/ref=sr_1_1?s=books&ie=UTF8&qid=1490353516&sr=1-1&keywords=practical+vim

Vim keys when at the start of previous line:

    0       # go to begin line
    fh      # go to h
    "+d$    # cut URL to clipboard
    ,a      # run the create link script and insert link where cursor is

Result:

    http://www.amazon.com/dp/1680501275/?tag=pyb0f-20 

How cool is that? This saves time and made me think what other repeated tasks I can automate and integrate into Vim :) 

If you have ideas or things you built let me know in the comments. 

## More Vim + Python

This only scratched the surface. Some great articles on Vim + Python: 

* Mentioned RealPython article: [VIM and Python - a Match Made in Heaven](https://realpython.com/blog/python/vim-and-python-a-match-made-in-heaven/) (we got our [indentation settings](http://pybit.es/indentation_tips.html) from there).

* [Full Stack Python - Vim](https://www.fullstackpython.com/vim.html)

* [Vim & Python: Making yourself at home](https://justin.abrah.ms/vim/vim_and_python.html)

* [Use Vim as a Python IDE](http://liuchengxu.org/posts/use-vim-as-a-python-ide/)

## What is a good Vim book?

[Practical Vim: Edit Text at the Speed of Thought](http://www.amazon.com/dp/1680501275/?tag=pyb0f-20) from the author of [vimcasts](http://vimcasts.org), is THE book that got my Vim skills to the next level.

## Two more useful shell aliases

Not Vim related, but now that we are talking shortcuts, here are two Python related ones I got in my bashrc:

* As soon as I start a new project I run 'pvenv' to create and start a fresh new [virtual env](http://pybit.es/the-beauty-of-virtualenv.html):

		alias pvenv='python -m venv venv && source venv/bin/activate'

* Run all unittests in the current directory: 

		alias utest='python -m unittest discover'

   (although at the time of this writing I discovered [the nosetests script](http://nose.readthedocs.io/en/latest/))

---

Keep Calm and Code in Python!

-- Bob
