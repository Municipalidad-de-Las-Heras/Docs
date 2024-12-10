# 6. Install WP-CLI

## Installation
Downloading the Phar file is our recommended installation method for most users. Should you need, see also our documentation on alternative [installation methods](https://make.wordpress.org/cli/handbook/installing/) ([Composer](https://make.wordpress.org/cli/handbook/installing/#installing-via-composer), [Homebrew](https://make.wordpress.org/cli/handbook/installing/#installing-via-homebrew), [Docker](https://make.wordpress.org/cli/handbook/installing/#installing-via-docker)).

Before installing WP-CLI, please make sure your environment meets the minimum requirements:

+ UNIX-like environment (OS X, Linux, FreeBSD, Cygwin); limited support in Windows environment 
+ PHP 5.6 or later 
+ WordPress 3.7 or later. Versions older than the latest WordPress release may have degraded functionality

Once you’ve verified requirements, download the wp-cli.phar file using `wget` or `curl`:

```bash
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
```

Next, check the Phar file to verify that it’s working:
```bash
php wp-cli.phar --info
```

To use WP-CLI from the command line by typing wp, make the file executable and move it to somewhere in your PATH. For example:

```bash
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```

If WP-CLI was installed successfully, you should see something like this when you run wp --info:

```bash
$ wp --info
OS:     Linux 5.10.60.1-microsoft-standard-WSL2 #1 SMP Wed Aug 25 23:20:18 UTC 2021 x86_64
Shell:  /usr/bin/zsh
PHP binary:     /usr/bin/php8.1
PHP version:    8.1.0
php.ini used:   /etc/php/8.1/cli/php.ini
MySQL binary:   /usr/bin/mysql
MySQL version:  mysql  Ver 8.0.27-0ubuntu0.20.04.1 for Linux on x86_64 ((Ubuntu))
SQL modes:
WP-CLI root dir:        /home/wp-cli/
WP-CLI vendor dir:      /home/wp-cli/vendor
WP_CLI phar path:
WP-CLI packages dir:    /home/wp-cli/.wp-cli/packages/
WP-CLI global config:
WP-CLI project config:  /home/wp-cli/wp-cli.yml
WP-CLI version: 2.11.0
```

## Updating
You can update WP-CLI with wp cli update (doc), or by repeating the installation steps.

If WP-CLI is owned by root or another system user, you’ll need to run sudo wp cli update.

Want to live life on the edge? Run wp cli update --nightly to use the latest nightly build of WP-CLI. The nightly build is more or less stable enough for you to use in your development environment, and always includes the latest and greatest WP-CLI features.

## Tab completions
WP-CLI also comes with a tab completion script for Bash and ZSH. Just download `wp-completion.bash` and source it from `~/.bash_profile`:

```bash
source /FULL/PATH/TO/wp-completion.bash
```

Don’t forget to run `source ~/.bash_profile` afterwards.

If using zsh for your shell, you may need to load and start `bashcompinit` before sourcing. Put the following in your `.zshrc`:

```bash
autoload bashcompinit
bashcompinit
source /FULL/PATH/TO/wp-completion.bash
```

## Allow root instance

To allow cli usage with root permission simply add this enviroment variable:

```bash
export WP_CLI_ALLOW_ROOT=1
```

