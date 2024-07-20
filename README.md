# Minecraft Server Self-Hosting Guide ⛏️

This document provides a detailed guide on how to set up a self-hosted Minecraft server using Docker. There are various methods for self-hosting a Minecraft server, however this guide focuses on my preferred setup. This guide uses [SpigotMC](https://www.spigotmc.org/wiki/about-spigot/) to allow custom plugins however you can also use vanilla minecraft server if you wish to not use custom plugins. To use vanilla minecraft, replace the `server.jar` in the `build` folder with the vanilla minecraft jar file. Make sure it is renamed to `server.jar` before you begin this guide. At the time of writing this guide, I am using the `1.20.4` version of the Spigot build.

## Pre-requisites 📝

1. Docker - Check if you have docker installed by running `docker --version` in a terminal of your choice. If Docker is not installed, please follow [this guide](https://docs.docker.com/engine/install/) to install Docker on your host operating system.

## Getting started 🚀

1. Clone/Download this repository on your host computer at a location of your choice.
   
2. If you're using Linux, you will need to grant write access to the `survival` folder. Navigate the to the root of the repository and run -
   
    ```bash
    chmod -R 777 survival
    ```

3. To start the server, navigate to the root of repository and run -
   
    ```bash
    docker compose up -d
    ```

4. This will start a survival minecraft server on the default port i.e `25565`.

5. Test the setup by joining the server from Minecraft. Enter `<host-ip-address>:<port>` as the server address. ex. `192.168.13.100:25565`. You can join the server without entering the port for this instance as the server is running on the default port already. You can find your host ip address by running `ifconfig` on Linux and `ipconfig` on Windows. It is recommended to assign a static IP for your host computer. You can lookup online for a guide on how to assign a static IP address that is specific to your operating system and the router.

## Securing the server 🔐

Before making the server available to the world, it is a good idea to secure it by creating a whitelist. This will only allow the players you trust to join your server. Follow the steps to secure the server.

1. To enable whitelist, you will first need to attach to the container by running -
   
   ```bash
   docker attach minecraft-survival
   ```

2. Now you can enable whitelist by running -
   
   ```bash
   whitelist on
   ```

3. Add yourself and other players you trust to the whitelist. Run the following command for each player including yourself -

   ```bash
   whitelist add <minecraft-username>
   ```

4. After adding all the players and yourself to the whitelist, you can detach from the container by pressing the key combination `Ctrl + p, Ctrl + q`.

5. Verify the whitelist by launching Minecraft and attempt to the join the server. If you set up the whitelist correctly, you should be able to join with no issues. You can also verify the whitelist by inspecting the `whitelist.json` file. Run the following command at the root of the repository to view the file -
   
   ```bash
   cat ./survival/whitelist.json
   ```

## Make your server available to the world 🌎

Enable global access to your server, allowing friends to join from anywhere. First, ensure your server's security by following the previous section's guidelines. While I recommend port-forwarding for this purpose, it's important to note the associated risks. If you're hesitant about port-forwarding, consider using services like [playit.gg](https://playit.gg/), which facilitate server hosting without the need for port-forwarding. The process for setting up port-forwarding will differ depending on your router's brand, so please consult your router's documentation for specific instructions. For those with more experience, consider acquiring a custom domain name (if you don't already own one) and link it to your server's public IP address. I use [Amazon Route 53](https://aws.amazon.com/route53/) for domain management, directing it to my public IP on port `25565`. Numerous online tutorials can guide you through this setup.

## Installing plugins 🧩

To install a pluging on your SpigotMC server -

1. Download the SpigotMC compatible plugins you wish to install. These will ideally be `.jar` files. Make sure the plugins are compatible with your version of minecraft. Here are some of the plugins that I personally use -
   - [SimpleHomes](https://www.spigotmc.org/resources/simplehomes.64/)
   - [SimpleTpa](https://www.spigotmc.org/resources/simple-tpa.64270/)
   - [MultiplayerSleep](https://www.spigotmc.org/resources/multiplayer-sleep.85499/)
   - [HeadDrop](https://www.spigotmc.org/resources/%E2%9C%85-headdrop-free-1-16-1-21-fully-customizable.99976/)
   - [InvisibleItemFrames](https://www.spigotmc.org/resources/invisibleitemframes-better-item-frames.85085/)
   - [LuckPerms](https://www.spigotmc.org/resources/luckperms.28140/)

2. To install the plugins you downloaded, first make sure you have stopped the server -
   
   ```bash
   docker compose stop
   ```

3. Then, copy the plugins (the `.jar` files) into the `./survival/plugins` directory. That's it.

4. Now, restart the server -
   
   ```bash
   docker compose start
   ```

Please note: Some plugins might require additional configuration to enable them, please read the documentation of the plugin to make sure you have it configured correctly.

## Maintaining your server 👷

Here is a list of some tasks you can do such as starting, stopping and upgrading your server.

### Basic tasks 🔧

1. Stopping the server -
   
   ```bash
   docker compose stop
   ```

2. Starting the server -
   
   ```bash
   docker compose start
   ```

3. Stopping and removing the container -
   
   ```bash
   docker compose down
   ```

4. Creating the container and starting the server -
   
   ```bash
   docker compose up -d
   ```

5. View last 10 lines of server logs -
   
   ```bash
   docker logs minecraft-survival
   ```

6. Attaching to the container to view live logs and run server commands
   
   ```bash
   docker attach minecraft-survival
   ```

7. To detach from a container, press `Ctrl + p, Ctrl + q`.

### Upgrading the Minecraft version 🏗️

The server in this guide at the time of writing uses minecraft version `1.20.4`. If you wish to upgrade/change the version, follow these steps -

1. Use the [Spigot Build Tools](https://www.spigotmc.org/wiki/buildtools/) to build the `.jar` file of the version you wish to use. Follow the instructions in the link above to use the build tools too create the jar file for the version of your choice. Please note that upgrading the server might break some plugins, ensure the plugins are compatible with the new version or replace the plugins with the compatible ones.
2. Stop the server and remove the container by running `docker compose down`.
3. Replace the `server.jar` file in the `./build` directory with the new file. Make sure you rename the new file to `server.jar`.
4. Start server by running `docker compose up -d`.

## Have fun! 😀

This is the most important step. Have fun playing minecraft with your friends!