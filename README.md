# Quickdock
A bash helper to run commands in docker container (for Laravel)

_For educational purpose and not meant to be used in production !_

## Installation

Clone this repository

```bash
git@github.com:Tantrisse/quickdock.git
```

If you want to make this helper global :

```bash
sudo cp quickdock /usr/local/bin/quickdock
sudo chmod +x /usr/local/bin/quickdock
```

## Usage

This helper has 4 "root" commands :

- `quickdock artisan`
    - Search for a php container and run the given artisan command 
- `quickdock composer`
    - Search for a php container and run the given composer command 
- `quickdock yarn`
    - Search for a node container and run the given yarn command 
- `quickdock npm`
    - Search for a node container and run the given npm command 

Example : 

```bash
quickdock artisan route:list
quickdock yarn run dev
```

## Behind the scene

This helper function as followed :

It scans (recursively) the current folder to find a `docker-composer.y[a]ml` file.

If the file can't be found, the script goes up by one folder at a time and search NON recursively for a `composer.json` file requiring `laravel/framework`.

This way, we search the root of our project folder.

If no file can be found, and we reach the top of filesystem (`/`), the script abort with an error message.

If we found a `composer.json` file, we try again to recursively scan for a `docker-composer.y[a]ml` file.

If found, we scan it to extrade the container name we have to start based on the first parameter of the command. The containers MUST be named `xxx-php` for PHP and `xxx-node` for Node.

Finally, we run the `docker-composer` command with all the parameters from the `quickdock` command.

Oh, and if the command need a PHP container and it is down, the script will `docker-compose up -d` the stack for you ! :)
