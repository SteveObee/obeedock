# Obeedock

Obeedock is a Dockerized IDE based on the popular PHP IDE Laradock. Having used Laradock
for a number of years I appreciate the scope and feature set but I need something a little
more streamlined for my needs. Rather than something that is all things to all folks, Obeedock
is very much a personal project. Speaking of Projects, Obeedock works the same as Laradock; once
embedded in a local main "Project's" Folder and started, all the projects are available within the wspace
container.

I'm also a big fan of Archlinux so I've used it for the main workspace OS. This means Applications
are of the same version as on local machine, especially useful in the case of Neovim which is
well maintained on the main Arch repo's.

Feel free to bend and break to your needs and I hope it proves useful to someone out there.

## Installation

Installation is straight forward, you just need to clone/download the repo so it's in your main project
folder then making sure you have docker and docker-compose installed run the usual on the command line:

```bash
docker-compose up --build
```

Once the containers are built they will be started and you may use at will. If you'd like to use my init.vim
configuration file you can launch Neovim and run:

```bash
:PlugInstall
```

This will configure things just the way I like it. Feel free to replace with you're own if your not
feeling the VSCode aesthetic ;).

### Tmux

Tmux is installed as standard and to activate the included plugins you'll need to be attached to a tmux
session then hit `prefix` + <kbd>I</kbd> and after hitting <kbd>Esc</kbd> the plugins should work. I have
included restore and continuum but feel free to add more.

### Apache

Projects can be viewed on a local browser by copying one of the example .conf files in the apache/sites/ folder and editing it
to your needs. Change the DocumentRoot to the folder your project normally serves its index file from (usually called public),
then edit the ServerName and ServerAlias lines respectively. Afterward configure your local machine to point towards
the ServerName. On linux thats the /etc/hosts file.

### Database

The database is MySQl using the mysql:latest image and data is synced to the local ./database folder using a volume. PhpMyAdmin
is provided and mapped to localhost on port 8082 with the user logged on as root automatically.

### Mail

Mailhog, mail server is provided and accessible on localhost:8081.

## Usage

I created the IDE with the idea that I can work within it and run all language commands natively so
to that end I use this command to enter the main workspace container:

```bash
xterm -T "Laradock IDE" -e "cd ~/Projects/obeedock && \
          docker-compose exec --user=obeedock wspace tmux -2"
```

You enter as the main Obeedock user and are attached to a tmux session. This is
preference and you can logon using bash of course. Either way this code can be saved in a bash script and
executed on key press for extra convenience.

## Important

To ensure persistence of configurations and settings, on starting up Obeedock mounts the wspace home folder to a local
named volume. This means the folder is preserved even if you rebuild the environment but be aware to do a
completely fresh install you will need to delete that volume first. If you prefer not to have this behaviour
please delete this line from the wspace volumes section in the docker-compose file:

```bash
- work_home:/home/obeedock
```

Then remove the volumes section at the bottom of the file.

Enjoy
