# GroupMe LaTeX Rendering Bot

Math is a common topic of many of my group chats, but the lack of inline LaTeX
rendering makes certain discussions difficult. This bot detects any messages
which are LaTeX equations (those beginning and ending with `$`), and
subsequently renders them, sending the output to the group chat as an image.

The bot can also render short snippets of text, in addition to `tikz` plots.
