---
title: Starting a new NeoVim config in 2025
date: 2025-03-15 15:34:35
tags:
---

## Preamble 

Since I started my career I was always fascinated by vim. My first SE job was as an intern at Pivotal and VIM was well used across the R&D department. It took me a long time, and a very patient teacher (thanks Tom!) to get the basics memorised. Since then there been no going back.

The team I worked on had standardised on a VIM config maintained by a VIM enthuasiast at the company (who I nevermet), [Luan](https://github.com/luan/vimfiles) who maintained his own config that was used across the org. I never cared about how it was built or bothered to make any changes. As a developer at the same company as the author, everything worked for my Golang/YAML/Bash heavy setup.

Eventually I moved on from Pivotal and continued using VIM, at WeaveWorks I got to work with another VIM enthuasiast, who shared with me their [NeoVim config](https://github.com/warehouse-13/a-new-hope). This was an easy transition to use from the Luan VIM files setup I was using before, and again I rarely made changes, so wasn't really bothered with how it worked under the hood.

Fast forward 4 years later, and the config began to let me down. I had some rending bugs that would occur when switching between visual mode. I had no clue what was in my setup that was causing this, and decided to crack open the config. I quickly realised I had no idea what any of it meant, and that the NeoVim world has moved on a lot since its creation. It felt like the perfect time to dig into what was in my actual config, and to rewrite it to use the latest plugin manager.

## Where to began

Often these days my search begins with asking AI, in this case ChatGPT (4o at time of writing). To get started I was presented with a few options:

- [NvChad](https://github.com/NvChad/NvChad)
- [LunarVim](https://github.com/LunarVim/LunarVim)
- [KickStart.nvim](https://github.com/nvim-lua/kickstart.nvim)
- [AstroVim](https://github.com/AstroNvim/AstroNvim)
- [LazyVim](https://github.com/LazyVim/LazyVim) (not to be confused with the plugin manager, lazy.nvim)


Lunar, Astro and Lazy all appeared to be plug and play distros, not requiring much to get started. NvChad and KickStart.nvim are more base configs to build upon. I tried LazyVim as it had the most GitHub stars (a poor metric I know) of the plug and play distros, and it was good. But I immediately found a lot of the pre-built features annoying, including its pop up notifications, that I would keep switching into by accident when running `ctrl-w`. Starting from a minimalist base config sounded like the right thing to do.

I asked ChatGPT for a comparison of the two (worth noting the ChatGPT training data was cut off at June 2024, so some of its statements might be out of date):

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


