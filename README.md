# chaz

Chaz is chaz.

This is a [Matrix](https://github.com/sigoden/aichat) bot that connects to [AIChat](https://github.com/sigoden/aichat) to provide access to "10+ AI platforms, including OpenAI, Gemini, Claude, Mistral, LocalAI, Ollama, VertexAI, Ernie, Qianwen..." all from within Matrix.

You do _NOT_ need to be running your own Matrix homeserver to use this.
It is a bot that should be usable with any homeserver, you'll just need to create an account for it.

You will need your own API keys or your own local AI already configured.

## Install

`chaz` is only packaged on crates.io, but it's recommended that you run from git HEAD for now.

For [Nix](https://nixos.org/) users, this repo contains a Nix flake. See the [setup section](#nix) for details on configuring.

## Setup

First, setup an account on any Matrix server for the bot to use.

Create a config file for the bot with its login info.

**IMPORTANT**: Make sure that you setup your allow_list or the bot will not respond

```yaml
homeserver_url: https://matrix.org
username: "chaz"
password: "" # Optional, if not given it will ask for it on first run
allow_list: "" # Regex for allowed accounts.
state_dir: "$XDG_STATE_HOME/chaz" # Optional, for setting the chaz state directory
aichat_config_dir: "$AICHAT_CONFIG_DIR" # Optional, for using a separate aichat config
chat_summary_model: "" # Optional, set a different model than the default to use for summarizing the chat
```

### Nix

Development is being done using a [Nix flake](https://nixos.wiki/wiki/Flakes).
The easiest way to install chaz is to use nix flakes.

```bash
❯ nix run github:arcuru/chaz
```

The flake contains an [overlay](https://nixos.wiki/wiki/Overlays) to make it easier to import into your own flake config.
To use, add it to your inputs:

```nix
    inputs.chaz.url = "github:arcuru/chaz";
```

And then add the overlay `inputs.chaz.overlays.default` to your pkgs.

The flake also contains a home-manager module for installing chaz as a service.
Import the module into your home-manager config and you can configure `chaz` all from within nix:

```nix
{inputs, ... }: {
  imports = [ inputs.chaz.homeManagerModules.default ];
  services.chaz = {
    enable = true;
    settings = {
        homeserver_url = "https://matrix.jackson.dev";
        username = "chaz";
        password = "hunter2";
        allow_list = "@me:matrix.org|@myfriend:matrix.org";
    };
  };
}
```

## Running

To run it, simply:

1. Install _chaz_ and setup its config.
2. Install [AIChat](https://github.com/sigoden/aichat).
3. Configure [AIChat](https://github.com/sigoden/aichat) with the models and defaults that you want.
4. Create a config file for _chaz_ with login details.
5. Run the bot and specify it's config file location `chaz --config config.yaml`.

The bot will not respond to older messages sent while it wasn't running to prevent overwhelming the backend.
