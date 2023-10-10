# Docker based Wordpress development skeleton

This is my docker based Wordpress development skeleton. To use this setup, you need two things:

- Docker (Docker Deskop on Windows and/or Mac)
- Docker Compose

You can start the project with the command:

```shell
docker-compose up -d
```

This will start three containers:

1. A MariaDB database
2. A Wordpress site on http://localhost:8080
3. An Adminer instance on http://localhost:8081

You need to perform the inital installation of Wordpress yourself, e.g.:

- pick a language
- choose a site name
- create a user/pass

## How _not_ to use this skeleton project

To create a plugin or theme, you could simply start adding files in the
plugins and themes folders. **Don't do this!**. Any plugins you install from within Wordpress will also be placed there.

You don't want to mix up or intertwine your own code with Wordpress and the code of other plugins and themes. Also, you don't want to work in the deeply nested folder `docker_data/wordpress/wp-content/plugin/<your_plugin>`.

## So what _should_ I do instead?

I've put an example plugin in the root directory of this project, called `myplugin`. This plugin is mounted on the plugins folder inside the Wordpress container. You can do the same if you decide to develop a theme.

This setup has a a few advantages:

- Your code is easily accessible
- You can easily add your code to version control
- If you want to start fresh, you can simply remove the data in `docker_data` as described above without worrying that you throw away your own code.

## Notes

1. If you prefer, you could switch to MySQL but MariaDB better supports ARM64 (for arm based Macs). They are generally super compatible so it shouldn't matter at all.
2. I like Adminer because it's super lightweight and fully supports ARM64 as well. If you want, you can switch to PHPMyAdmin.
3. I bind to 127.0.0.1 explicitly. If you don't, you will bind to all interfaces, exposing your development installation to anyone that can reach your ip address which is generally not what you'd want.