# Pterodactyl Re-imagined: Project Summary

This document summarizes the plan to create a lightweight, Python-based server management platform controlled entirely through a Discord bot, as a re-imagining of the Pterodactyl panel.

## Core Concepts

*   **Goal:** Replace the traditional web interface with a Discord bot for all server management tasks.
*   **Primary Components:**
    *   **Discord Bot (The "Panel"):** The user-facing interface for all commands and interactions.
    *   **Python Backend (The "Daemon"):** A service running on host nodes that executes tasks.
    *   **Docker:** Used to containerize and isolate each game server instance.
    *   **SFTP Service:** Provides secure, sandboxed file access for users.

## System Architecture

*   **Discord Bot:**
    *   Manages user interaction via slash commands (`/server create`), buttons, and modals.
    *   Displays real-time server status and resource usage.
    *   Handles permissions based on Discord roles.
    *   Communicates with the backend via a secure REST API.

*   **Python Backend:**
    *   Acts as an API server listening for requests from the bot.
    *   Uses the `docker-py` library to manage the full lifecycle of server containers.
    *   Monitors container resources (CPU, RAM, Disk).
    *   Manages SFTP user creation and ensures users are sandboxed to their server directory.
    *   Processes "Egg" files to configure and launch new servers.

## Key Technologies & Concepts

*   **Docker Containerization:** Each server runs in an isolated container for security, resource control, and dependency management. The backend pulls the Docker image specified in an Egg and configures the container according to the Egg's definitions.

*   **SFTP Management:** The backend provisions a unique, sandboxed SFTP user for each server. Credentials are generated on-demand and sent to the user via a private Discord message for security.

*   **The "Egg" System:**
    *   An "Egg" is a JSON or YAML file that serves as a blueprint for a game server.
    *   It defines the Docker image, startup command, environment variables (user-configurable settings), and installation scripts.
    *   The backend reads an Egg to orchestrate the entire server creation and installation process.

## Example Workflow: Creating a Server

1.  A user runs `/server create` in Discord.
2.  The bot guides the user through selecting a game (Nest) and server type (Egg) using interactive menus.
3.  The bot collects configuration details (e.g., server name, memory) via a form.
4.  The bot sends the collected data to the backend API.
5.  The backend validates the request, reads the corresponding Egg, and uses Docker to create, configure, and start the new server container.
6.  The backend confirms the server is online and returns the connection info to the bot.
7.  The bot notifies the user that their server is ready.