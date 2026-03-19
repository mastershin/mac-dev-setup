# Mac

## install docker

`https://docs.docker.com/desktop/setup/install/mac-install/`

## Developer Utils (brew / Mac)

```bash
# install homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# install uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# install bun (or via npm: npm install -g bun)
curl -fsSL https://bun.com/install | bash

uv tool install ast-grep-cli
uv tool install ruff
uv tool install hatchling
uv tool install prek
uv tool install zizmor
# Example: uvx ty check

# wezterm and Jetbrains Mono Nerd Font
brew install --cask wezterm
brew install --cask font-jetbrains-mono-nerd-font

brew install vim tmux fzf zoxide fd ripgrep bat ast-grep ruff

# worktrunk
brew install worktrunk && wt config shell install
```

## For bash and startship (optional)

```bash
# install nerd font
brew install starship

# add to ~/.bashrc
echo 'eval "$(starship init bash)"' >>~/.bashrc
source ~/.bashrc
```

### ~/.config/starship.toml

```
[python]
symbol = "👾 "

format = """
$username\
$hostname\
$directory\
$git_branch\
$line_break\
$character"""

[hostname]
ssh_only = false
style = "bold green"
format = "[$hostname]($style):"

[username]
show_always = true
style_user = "bold blue"
format = "[$user]($style)@"

[git_branch]
symbol = "🌱 "
style = "bold purple"
format = "on [$symbol$branch]($style) "

[git_status]
style = "red"
format = "([$all_status$ahead_behind]($style) )"

[cmd_duration]
min_time = 2000   # Show only if command takes longer than 2000ms (2s)
format = "took [$duration]($style) "
style = "bold yellow"
```
