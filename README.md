# Docker based WordPress development skeleton

This is my docker based WordPress development skeleton. If you like it, you can freely use it for any purpose and in any way.

## Why?

I was searching specially for a docker based development setup to use when developing plugins or themes, and was highly disappointed
by the examples I found on the web.
Most blog posts show you a Docker Compose file somewhat similar to mine. But most posts stop there. They don't tell you how to
use it properly. Some outright tell you to start editing the files inside the WordPress installation. That **will** get messy quickly.

Also, most blog posts do not take version control into consideration _at all_.

This is an improvement over those half-baked examples, but I'm sure there are tons of ways to improve on this, because I'm basically
a noob when it comes to WordPress development. So, by all means, let me know whatever I can improve.

This repo can be used as a based for your plugin or theme development. It will provide a working WordPress installation, and a way
to clearly separate your code from that installation and apply version control to it.

## Getting started

I've marked this repository as a 'Template repository' on GitHub. When you create a new repo through the GitHub UI, you can use this
repository as a template and it will be copied without the git commit history.

After creating your new repo based on this template, checkout that project on your development workstation.

To use this setup, you need two things:

- Docker
- Docker Compose

I'm going to assume you are familiar with Docker. If not, you have some catching up to do!

You can start the project with the command:

```shell
docker compose up -d
```

This will start three containers:

1. A MariaDB database
2. A WordPress site on http://localhost:8080
3. An Adminer instance on http://localhost:8081

You need to perform the inital installation of WordPress yourself, e.g.:

- pick a language
- choose a site name
- create a user/pass

## How _not_ to use this skeleton project

To create a plugin or theme, you could simply start adding files in the
plugins and themes folders. **Don't do this!** Any plugins you install from within WordPress will also be placed there.

You don't want to mix up or intertwine your own code with WordPress and the code of other plugins and themes. Also, you don't want to work in the deeply nested folder `docker_data/wordpress/wp-content/plugin/<your_plugin>`.

## So what _should_ you do instead?

I've put an example plugin in the root directory of this project, called `myplugin`. This plugin is mounted on the plugins folder inside the WordPress container with the following mount:

```plaintext
./myplugin:/var/www/html/wp-content/plugins/myplugin
```

You can add more bind mounts like this to develop more plugins or a theme.

This setup has a a few advantages:

- Your code is easily accessible
- You can easily add your code to version control
- If you want to start fresh, you can simply remove the data in `docker_data` as described below

## How to start fresh?

If you want to start fresh again, you can remove the `docker_data` directory. **Caution**: this will remove the WordPress installation **and** the database.

For example:

```shell
docker compose down
rm -rf docker_data
docker compose up -d
```

After restart, the WordPress container will notice the removed files and recreate the basic WordPress installation for us.

You now have a fresh WordPress install again, and you'll need to reinitialize WordPress.

## Notes

1. If you prefer, you could switch to MySQL but MariaDB better supports ARM64 (for arm based Macs). They are generally super compatible so it shouldn't matter at all.
2. I like Adminer because it's super lightweight and fully supports ARM64 as well. If you want, you can switch to PHPMyAdmin.
3. I bind to 127.0.0.1 explicitly. If you don't, you will bind to all interfaces, exposing your development installation to anyone that can reach your ip address which is generally not what you'd want.
