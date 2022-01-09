---
layout: post
title: "Starting out: Intro & Writing a Journal app with bash"
date: 2022-01-02 19:20:20 +0100
tags: bash journey
categories: coding
---
## Intro

Welcome to the first entry in my blog. 
In here, I plan to flesh out my writing and coding skills writing a new blog post every 1 or 2 weeks.
For the past month I've had many projects in mind and I've worked on some.
In an effort to get the most out of them, and potentially create something useful, I'll document my progress on this site and in my github repositories.

Beware, reader! This is a learning experience for me, so expect some cringe-worthy code (hopefully fewer as time passes) and lackluster codebases as I learn new programming languagues.

On some final notes, so as to keep my efforts centered, I will be focusing this blog on things that can be accomplished using coding.
Regardless, expect some Math to slip in, since I'm really keen on it as well.


## Journal CLI

I've been interested for some time in shell scripts, ever since I took up Linux a year ago and progressively felt less intimidated by the terminal.
Additionally, I have a friend that recently wrote a script to read news from the terminal (see [speednews](https://github.com/canales2002/speednews)) and I felt compelled to learn how to use Bash.
Nonetheless, to start learning Bash I needed a project to work on.

Some weeks ago, I started sporadically writing a personal journal on my laptop using simple `.txt` files.
For organization, I would name them using only their corresponding date (e.g. `02-01-2022.txt`in `%d-%m-%Y` format).

Then it ocurred to me to create a new alias in `~/.bashrc` that would open the entry corresponding to the current date using `vim` and the `date` command:

	alias note="vim ~/Documents/Journal/`date +"%d-%m-%Y"`.txt"

where `date +"%d-%m-%Y"` evaluates to today's date using the specified format with `+"..."`.

As a sidenote: Bash works primarily using strings and arrays of strings, so it is very powerful for doing interacting with files yet quite lackluster for simple arithmetic. 
For that, it's usually better to write scripts in other languages such as Python to be used within the Bash program.
Therefore, Bash usually acts as a middle-layer in larger projects for communicating with the operating systems.

# The program

Useful as it is, just writing an `alias` is far from a complete application.
I want to implement the following functionalities:
	
	journal [ARGS]

- Create a journal for the current date and specified date. **done**
- Autocomplete journal name from files in the save directory (like navigating folders using `cd`). _TODO_
- Option to encrypt journal. _TODO_
- Experiment using _Markdown_ as file type (how to render `.md` files). _TODO_

A more thoughtful approach is needed.
That is why I've created the following Bash script that will make it easier to implement future functionalities:

{% highlight bash %}
#!/usr/bin/env bash

# Configuration: Defaults
EDITOR="vim"
FILE_TYPE=".txt"                # Any file type interpreted by the editor
FORMAT="%d-%m-%Y"               # For more format options: see DATE(1)
DIR="$HOME/Documents/Journal/"  # Save directory
 
##########
 
# Main Program
 
# Build the save directory if it isn't found
if [ ! -d "$DIR" ]; then
	mkdir -p $DIR
fi

# Check the number of arguments passed
if [ $# -gt 0 ]; then
	# Open the specified file
	$EDITOR $DIR$@$FILE_TYPE
else
	# If no argument is passed, open/create the entry corresponding to today
	TODAY=$(date +$FORMAT)
	$EDITOR $DIR$TODAY$FILE_TYPE
fi
{% endhighlight %}

This code is hosted on the [datejournal-cli](https://github.com/bibanez/datejournal-cli) repository, which I'll update with more functionalities as I continue on this development journey.

## Closing thoughts

There already exist more powerful solutions for writing journals out there (which I didn't know prior to deciding on this topic).
Just to mention a few open-source alternatives: [journal-cli][1] or [Joplin](https://joplinapp.org/).
What I aim to do with [datejournal-cli][1] is to create the most _"barebones-yet-functional-for-me"_ (tm) solution available and get some practice in with bash.

This ends my first post! If you have any _positive_ criticism or have an interesting topic to discuss feel free to [email me](mailto:bibanez@protonmail.com) or DM me on Twitter [@bibanez135](https://twitter.com/bibanez135).

[1]: (https://github.com/bibanez/datejournal-cli)
