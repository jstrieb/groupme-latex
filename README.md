# GroupMe LaTeX Rendering Bot

Math is a common topic of many of my group chats, but the lack of inline LaTeX
rendering makes certain discussions difficult. This bot detects any messages
which are LaTeX equations (those beginning and ending with `$`, or starting
with `[;` and ending with `;]`), and subsequently renders them, sending the
output to the group chat as an image. The bot can also render short snippets of
text, in addition to `tikz` plots.

The bot runs using Python 3 and is designed to run using `cgi-bin`. It has
only been tested with `apache2`.

Discussed in [/r/math](https://www.reddit.com/r/math/comments/d5hw66/groupme_bot_to_render_latex_equations_and_send/).


# Demo & Examples

To try out the bot, join the testing group
[here](https://groupme.com/join_group/53666628/078VgaBr).

Examples to try
- `help`
- `$ a^2 + b^2 = c^2 $`
- `$$ \mathrm{Borwein} (n) := \int_0^\infty \prod_{i = 0}^n \frac{\sin (x)}{x} dx $$`
- `[; Keep it $\mathbb{R}$ ;]`
- A Venn diagram

        [;
        \begin{tikzpicture}
        \begin{scope}[blend group = soft light]
        \fill[red!30!white] ( 90:1.2) circle (2);
        \fill[green!30!white] (210:1.2) circle (2);
        \fill[blue!30!white] (330:1.2) circle (2);
        \end{scope}
        \node at ( 90:2) {Typography};
        \node at ( 210:2) {Design};
        \node at ( 330:2) {Coding};
        \node [font=\Large] {\LaTeX};
        \end{tikzpicture}
        ;]

- A Penrose Triangle

        [;
        \begin{tikzpicture}[scale=1, line join=bevel]
        \pgfmathsetmacro{\a}{2.5}
        \pgfmathsetmacro{\b}{0.9}
        \tikzset{
        apply style/.code = {\tikzset{#1}},
        triangle_edges/.style = {thick,draw=black}
        }
        \foreach \theta/\facestyle in {
        0/{triangle_edges, fill = gray!50},
        120/{triangle_edges, fill = gray!25},
        240/{triangle_edges, fill = gray!90}
        }{
        \begin{scope}[rotate=\theta]
        \draw[apply style/.expand once=\facestyle]
        ({-sqrt(3)/2*\a},{-0.5*\a}) --
        ++(-\b,0) --
        ({0.5*\b},{\a+3*sqrt(3)/2*\b}) --
        ({sqrt(3)/2*\a+2.5*\b},{-.5*\a-sqrt(3)/2*\b}) --
        ++({-.5*\b},-{sqrt(3)/2*\b}) --
        ({0.5*\b},{\a+sqrt(3)/2*\b}) --
        cycle;
        \end{scope}
        }
        \end{tikzpicture}
        ;]


# Quick Start

1. Change to the `cgi-bin` directory of your web server. For example:

        cd /usr/lib/cgi-bin

2. Clone the repository (may need to use `sudo`)

        git clone https://github.com/jstrieb/groupme-latex.git

3. Make sure the main `latex` file is marked as executable

        sudo chmod +x groupme-latex/latex

4. Get your "bot ID" and "API key" from GroupMe by creating a bot, which can
  be done by following [this tutorial](https://dev.groupme.com/tutorials/bots).
  For the callback URL, use something like the following

        http://<server URL>/cgi-bin/groupme-latex/latex

5. Edit the main `latex` file to use the correct `BOT_ID`, `API_ACCESS_TOKEN`,
  and `SERVER_BASE_URL` where the base URL is the location where the `tex`
  files will be output and compiled.

6. Create the folder where the `tex` files will be output and compiled. By
   default, this is `/var/www/html/latex`. Also give it writable permissions.

        mkdir -p /var/www/html/latex && chmod 777 /var/www/html/latex

7. Upgrade all packages on the system. On Ubuntu, this will look something like
   the following

        sudo apt update && sudo apt -y upgrade

8. Install necessary dependencies. On Ubuntu, this will look something like
  the following

        sudo apt install texlive texlive-extra-utils texlive-latex-extra texlive-pictures poppler-utils

9. Use the bot! Any messages that start and end with `$` or start with `[;` and
  end with `;]` will be rendered with LaTeX and sent back as images.

# Disclaimer

This bot writes approximately 1000 bytes of arbitrary data to a file on its
server. That means anyone in a group with this bot can effectively write files
to the server the bot runs on. Moreover, the bot does not do any verification
that the POST request it responds to actually comes from GroupMe and/or the
correct group. That means anyone who sees this bot running can write data to
the server it runs on.

Additionally, the default instructions include setting the file permissions on
the `/var/www/html/latex` directory to `777`, which is probably overkill.

Combined, these represent a **HUGE** security vulnerability! Do not run this
without accepting the risks.
