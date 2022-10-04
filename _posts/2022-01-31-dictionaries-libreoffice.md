---
layout: post
title: Parse all the words in your language
date: 2022-02-09 15:58:29 +0100
tags: code journey utility paraulogic
categories: journey
---

**Link to an [English wordlist](http://www-personal.umich.edu/~jlawler/wordlist.html)** (made by linguist [John M. Lawler](http://www-personal.umich.edu/~jlawler/index.html) at the University of Michigan).[^tag]

In case you're looking for a wordlist in any other language, you can compile your own adapting the currently available dictionaries in Libreoffice: this is exactly the topic of this blog post.
I made the following code for my [Paraulògic solver]({% post_url 2022-01-09-dictionaries-paraulogic %}), as having a wordlist  stored in my pc allowed me to perform a great deal of queries for specific sets of words.

A quick note, though: some of the results will be words that make little sense.
This is due to the nature in which these dictionaries are created: they usually contain many names of places, acronyms or particular words that may only be used in a handful of fields.

## Steps to compiling your own wordlist
1. **Download the set of libreoffice dictionaries:** In [this](https://www.libreoffice.org/download/download/) webpage search for _SDK and Sourcecode_ and choose the option that says `libreoffice-dictionaries-[version].tar.xz`.

2. **Unzip and search find the .dic file for your language:** Keep in mind that the names for languages are abbreviated. For example, `catalan` is abbreviated as `ca`.
You can expect a file structure similar to this one:

	```
	libreoffice-7.3.0.3/
	├─ dictionaries/
	│  ├─ af_ZA/
	│  ├─ an_ES/
	│  ├─ .../
	│  ├─ ca/
	│  │  ├─ dictionaries/
	│  │  │  ├─ ca-valencia.aff
	│  │  │  ├─ ca_valencia.dic
	│  │  │  ├─ ca.aff
	│  │  │  ├─ ca.dic
	│  │  ├─ images/
	│  │  ├─ META-INF/
	│  │  ├─ LICENSES-EN.txt
	```

	Here, the most important files are `ca.dic` and `LICENSES-EN.txt` (read the latter to make sure you comply with the uses intended for the dictionary, in this case the GPL license).

3. **Copy** `[your language].dic` **into another folder and run the following code:**

	Filename: [cleaner.py](https://github.com/bibanez/Paraulogicked/blob/main/cleaner.py)
	{% highlight python %}
import re		# Work with regular expressions
import unidecode	# You may need to install this package with pip

# Replace with your dictionary file
f = open("ca.dic", "r")
lines = f.readlines()
f.close()
print(len(lines))

cleaned = []
for i in range(len(lines)):
    line = lines[i]
    
    # Match any one or more characters different from '/' at the start
    # of the string
    x = re.search("^[^\\\/\d]+", line)
    
    if (x != None):
        x = x.group().lower()
        
        # Simplify special characters such as 'á' or 'ç' into 'a' and 'c'
        x = unidecode.unidecode(x)
        
        # Correct 'l*l' into 'll' and delete ', - and \
        x = re.sub("\*|'|-", "", x)
        
        cleaned.append(x)
    print(i)

print("Finished!")

# Remove duplicates
cleaned = list(dict.fromkeys(cleaned))

# This step sorts the wordlist in alphabetical order
cleaned.sort()

# Save file
with open("ca_netejat.txt", "w") as txt_file:
    for line in cleaned: txt_file.write(line + "\n")

	{% endhighlight %}

{:start="4"}
4. **???**
5. **Profit**



# Endnotes

This concludes the tutorial for this week!

If you find any other already made wordlists, feel free to [send me an email](mailto:bibanez135@gmail.com) and I'll add them at the start of this article.

You can find the previous code in the [Paraulogicked](https://github.com/bibanez/Paraulogicked) repository, a solver I made for the [*Paraulògic*](https://vilaweb.cat/paraulogic/) online game.

To learn more about this project, you can read my previous blog post [here]({% post_url 2022-01-09-dictionaries-paraulogic %}).

# Footnotes
[^tag]: The solution this author came up is interesting in it of itself: using only bash shell commands, the author parsed english words from texts and sorted them in a dictionary. This may be a good approach to creating your own wordlist, but it's not an easy task to correctly identify which words should be parsed.
