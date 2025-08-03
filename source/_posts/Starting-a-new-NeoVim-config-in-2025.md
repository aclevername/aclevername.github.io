---
title: Starting a new NeoVim config in 2025
date: 2025-03-16 15:34:35
tags:
---


## Preamble 

Since I started my career I was always fascinated by vim. My first SE job was
as an intern at Pivotal and VIM was well used across the R&D department. It
took me a long time, and a very patient teacher (thanks Tom!) to get the basics
memorized. Since then there been no going back.

The team I worked on had standardized on a VIM config maintained by a VIM
enthusiast at the company (who I never met),
[Luan](https://github.com/luan/vimfiles) who maintained his own config that was
used across the org. I never cared about how it was built or bothered to make
any changes. As a developer at the same company as the author, everything
worked for my Golang/YAML/Bash heavy setup.

Eventually I moved on from Pivotal and continued using VIM, at WeaveWorks I got
to work with another VIM enthusiast, who shared with me their [NeoVim
config](https://github.com/warehouse-13/a-new-hope). This was an easy
transition to use from the Luan VIM files setup I was using before, and again I
rarely made changes, so wasn't really bothered with how it worked under the
hood.

Fast forward 4 years later, and the config began to let me down. I had some
rending bugs that would occur when switching between visual mode. I had no clue
what was in my setup that was causing this, and decided to crack open the
config. I quickly realized I had no idea what any of it meant, and that the
NeoVim world has moved on a lot since its creation. It felt like the perfect
time to dig into what was in my actual config, and to rewrite it to use the
latest plugin manager.

## Where to begin

Often these days my search begins with asking AI, in this case ChatGPT (4o at
time of writing). To get started I was presented with a few options:

- [NvChad](https://github.com/NvChad/NvChad)
- [LunarVim](https://github.com/LunarVim/LunarVim)
- [KickStart.nvim](https://github.com/nvim-lua/kickstart.nvim)
- [AstroVim](https://github.com/AstroNvim/AstroNvim)
- [LazyVim](https://github.com/LazyVim/LazyVim) (not to be confused with the
plugin manager, lazy.nvim)


Lunar, Astro and Lazy all appeared to be plug and play distros, not requiring
much to get started. NvChad and KickStart.nvim are more base configs to build
upon. I tried LazyVim as it had the most GitHub stars (a poor metric I know) of
the plug and play distros, and it was good. But I immediately found a lot of
the pre-built features annoying, including its pop up notifications, that I
would keep switching into by accident when running `ctrl-w`. Starting from a
minimalist base config sounded like the right thing to do.

I asked ChatGPT for a comparison of the two (worth noting the ChatGPT training
data was cut off at June 2024, so some of its statements might be out of date):

```markdown
# NvChad vs. Kickstart.nvim

## **NvChad – Fast, Themed, and Modular**
✅ **Best for** users who want a **pre-configured, aesthetic, and optimized Neovim setup**.  
⚡ **Features:** Lazy-loaded plugins, UI enhancements, modular config (`custom/`).  
🔗 **[NvChad Repo](https://github.com/NvChad/NvChad)**  

## **Kickstart.nvim – Minimal & DIY**
✅ **Best for** those who want a **barebones starting point** to build their own Neovim setup.  
⚡ **Features:** Only core plugins (Treesitter, LSP, CMP, Telescope), fully customizable.  
🔗 **[Kickstart.nvim Repo](https://github.com/nvim-lua/kickstart.nvim)**  

## **Key Differences**
| Feature         | **NvChad**  | **Kickstart.nvim** |
|----------------|------------|--------------------|
| **Purpose**    | Pre-tuned, modular | Minimal DIY starter |
| **Plugins**    | Optimized, UI-focused | Only core essentials |
| **Customization** | Uses `custom/` overrides | Full manual control |
| **Best for**   | Users wanting fast setup | Those who prefer building from scratch |

🚀 **Pick NvChad** if you want a polished, ready-to-go setup.  
🔧 **Pick Kickstart.nvim** if you want total control with a minimal base.
```

Kickstart.nvim it is!


## Kickstart.nvim

Kickstart.nvim starts out pretty simple, you fork the repository and clone it
into your nvim config dir `~/.config/nvim` for me. From there the README.md is
pretty simple, read the `init.lua` top to bottom, and off you go. It was very
well documented, and everything worked out of the box. My first task was to get
it working with Golang.

### Golang, a journey

A quick search fo Golang nvim plugins lead me to
[go.nvim](https://github.com/ray-x/go.nvim). I had naively assumed that all I
had to do was install the plugin, and off I go. But that didn't work. I could
see all the commands, e.g. `:GoFmt`, but they didn't work, and everything I
expected from a Golang nvim setup wasn't working, e.g. auto-complete,
compilation error warnings etc. I went straight to ask chatgpt for help.

It turns out go.nvim is a massive, pre-bundled configuration for Golang. My
guess it it makes sense for a bare bones config, but with kickstart.nvim, you
actually have a lot of that bundled software available. I learnt that it
includes:

- nvim-lspconfig: LSP stands for language server protocol, its what integrates
vim with getting autocompletes, go-to definitions etc. Basically a lot of the
things that are mandatory for programming in any language. This meant that all
you actually had to install was gopls, which is the LSP for golang, it ships
along the Go binary, TIL'd! The two work together hand in hand.

- none-ls.nvim for formatting of files, integrating with `gofmt`. This plugins
seems to overlap a lot with using gopls via nvm-lspconfig. ChatGPT
differentiates them by:

   ```
    •	Use gopls for completion, navigation, and LSP-based features.
    •	Use none-ls.nvim for better formatting, linting, and extra tools.

   ```

- nvim-treesitter for syntax highlighting

This meant that I only had to install a handful of extra plugins to get a working Golang setup. Pretty sweet.

## The rest

Apart from getting a good Golang setup, I only have a few "must haves" for my config:

- Shortcuts: Mirroring the shortcuts from my old config. Things like `ctrl-p` to
fuzzy file search, or `shift-y` to yank selection to machine clipboard. These
and a handful more are too ingrained in my memory to try to relearn. Copying
them across was easy enough

- File explorer: I got very used to `vim <dir>` opening up a
file explorer for that directory, and for pressing `-` to
open a file explorer for the directory I'm in. These were
easy enough to setup using Telescope.

- Markdown: I was surprised that kickstart.nvim didn't come with out of the box
line breaks to maintain good line length hygiene when  writing Markdown (and
other) files. I quickly got tired of manually running `gqip` to format and
installed. This turned out to be a pretty tiny change

## Closing

That was it, a pretty smooth experience, that was made 10x easier using AI to
help guide me on the changes to make and to explain what each plugin does. I
feel well prepared to keep modifying my config as I find new plugins to try and
fixing any bugs I run into. On reflection, my original config was actually
pretty simple, I'm sure as the weeks go on I'll find all the small features in
my old setup that are missing from this.

My config can be found [here](https://github.com/aclevername/kickstart.nvim)

To see a diff of whats been added checkout [here](https://github.com/nvim-lua/kickstart.nvim/compare/master...aclevername:kickstart.nvim:master)
