# Nix flake for creating a basic development environment

## What is this?

Nix development shells allow you spin up custom development environments without affecting your installed system, like Python virtual environments,
but for any coding environment. Here we are leveraging Nix flakes to ensure that we can create consistent and reproducable development environments.

## Why?

1.  You want to test new versions of Go, or Go tools as part of your development workflow without breaking your development environment.
2.  You work in a team and you want an easy way for everyone to use the save development environment.
3.  You have different projects that require different toolchains.

## Pre-requisites

To use make sure you have the Nix package manager installed and have the `nix-command` and `flakes` experimental features enabled
as described [here](https://nixos.wiki/wiki/Flakes).

Install [direnv](https://direnv.net/) and make sure it is hooked into your shell.

## Usage

### Using this environment directly

1.  Make sure you have the pre-requisites installed.
2.  In the root of your Go project create an `.envrc` pointing to this flake:
    
    ```bash
    echo "use flake github:guoguojin/go-dev-env" > .envrc
    ```

3.  If you have `direnv` correctly hooked into your shell you should see a warning:

    ```text
    direnv: error /path/to/your/project/.envrc is blocked. Run `direnv allow` to approve its content
    ```

4.  Run `direnv allow` as instructed and you should see logs detailing what is being installed and your environment being initialised:

    ```text
    Setting XDG_CACHE to /home/tanq/.cache
    Found go.mod, running tidy
    ...
    Go development shell
    go version go1.20.5 linux/amd64
    GOROOT=/nix/store/i3ab37h47xmd0zh75708gj57hah7v7f4-go-1.20.5/share/go
    GOPATH=/home/tanq/.cache/dev-shell//home/tanq/code/personal/projects/signal-generator/go
    direnv: export <list of your environment variables available in your development shell>
    ```

Your development environment shell is ready to use. 

> Note: 
  If you are using GoLand or IntelliJ IDEA with the Go plugin, you will need to change the GOROOT and GOPATH
  for the project to the paths specified by the shell. 
  If you are using VS Code, starting `code` from the terminal while you're inside the development shell it should
  pick up the GOROOT and GOPATH from the environment variables. Otherwise you will need to set them accordingly in
  the settings.json

### Fork it!

Fork this repo, update the flake.nix to customise the development environment to your own preference or for your own
requirements.

All the steps above are applicable, the only difference is for step 2, you point to your own repo:

```bash
echo "use flake github:<your-github-id>/<your-github-project>" > .envrc
```

## Testing changes to your development environment

1.  If you haven't already, fork this repo.
2.  Create a new branch for your changes
3.  Make your desired changes to `flake.nix`
4.  Push the new branch to your repo
5.  Update `.envrc` to use the environment defined in the new branch:

    ```bash
    echo "use flake github:<your-github-id>/<your-github-project>/<your-branch-name>" > .envrc
    ```

    > Note: You may need to run `direnv allow` again to allow the updated environment.

6.  Your updated environment should be created and give you an updated development environment.
7.  If you want to switch back to the original environment and discard the new environment, change your `.envrc`
    to use the original repo.
8.  If you are happy with the changes to your environment, merge your branch and change your `.envrc` to use the
    original repo.

## Notes

You might want to add:

    - `.envrc`
    - `.direnv`

to the `.gitignore` file in your project repos so that these don't get added to your repo when you use these development environments.

