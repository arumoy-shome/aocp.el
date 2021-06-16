**DISCLAIMER**: This package evolved organically after observing my
workflow of managing bibliographic information in Emacs. The
functionality provided by this package is tuned towards how I work. As
of <span class="timestamp-wrapper"><span class="timestamp">[2021-06-16
Wed]</span></span>, the target audience for this package is just me
and I do not intend to publish it in any Emacs package repositories.
The code is very much in it's infancy or "alpha" stage, several things
can be improved and a few kinks need to be fixed. As a result, I am
not accepting any issues or PRs.

That being said, if you stumble upon this package and wish to use it,
it may help to gain some perspective on my workflow (see [research
workflow](https://arumoy.me/org/20210603_203538--zettel--research-workflow.html)
for more details). You have two ways to install this package: 1.
Manually, by downloading the \`aocp.el' file and sticking it in your
Emacs \`load-path', or 2. Using
[straight.el](https://github.com/raxod502/straight.el) which is what I
recommend.

The package works under the assumption that you manage your
bibliographic information in org-mode (a package for Emacs). The
functions made available through this package are intended to be used
in an org-capture template, they are not meant to be called
interactively (ie. by using \`M-x' or \`ESC x'). Assuming that you have
a bibtex entry in your kill-ring (either by killing text within Emacs
or by coping text from an external application into your clipboard),
this package will do the following:

-   extract the bibkey
-   extract the first author
-   extract the last author
-   extract the source of publication

On it's own, this may not seem like much, but the intended method of
using this package is within an org-capture template. For instance,
you could create a capture template as follows:

```
* TODO %(aocp--get-bibkey nil)
  :PROPERTIES:
  :PDF: file:~/Documents/papers/%(aocp--get-bibkey t).pdf
  :FIRST_AUTHOR: %(aocp--get-first-author)
  :LAST_AUTHOR: %(aocp--get-last-author)
  :SOURCE: %(aocp--get-source)
  :RANK:
  :END:
%?
+ problem statement ::
+ solution ::
+ results ::
+ limitations ::
+ remarks ::

  #+begin_src bibtex :tangle yes
  %c
  #+end_src
```

Assuming you have the above template in \`paper.txt', you can configure
org as follows (replace \`your-org-inbox-file' appropriately):

```
(setq org-capture-templates
    '(("p" "Paper" entry (file+headline your-org-inbox-file "Inbox")
    "%[~/.emacs.d/org-templates/paper.txt]")))
```

With this in place, you can quickly collect all bibliographic
information within an org file. Leveraging the powerful functionality
provided by org-properties, one can quickly find relevant papers. For
instance, I can look up all papers by author X or all papers by author
X published at Y.

A nice little tip is to download a local copy of the pdf and save them
all in a folder. To make this easier, aocp.el also pushes the bibkey
to the kill-ring. So all that is left to do is click the download
button and paste the bibkey as the file name. This ensure 1. That you
have all pdfs names consistently and 2. You have a link to the pdf
from your org file (see the :PDF: property in the template above)
which you can open by hitting \`C-c C-o' over the link. You do not need
to poke around in the directory containing the pdfs, all the context
is available in the org file and should be the point of entry for all
your bibliographic needs!

