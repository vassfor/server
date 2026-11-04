# Client–Server Chat Application

A simple TCP-based chat application written in Python. It consists of a server and a graphical client built with Tkinter. Connected users can join the chat, view the connected-user list, send messages to everyone, or send a message to a selected user.

## Features

- TCP client–server communication using Python's built-in `socket` module.
- Concurrent client handling with `threading`.
- A Tkinter graphical interface.
- Username selection when starting the client.
- Join and leave notifications.
- A connected-users list.
- Message delivery to everyone (`ALL`) or to one selected user.

## Project structure

```text
├── client.py    # Tkinter chat client
├── server.py    # TCP server
└── README.md
```

## Requirements

- Python 3
- Tkinter (usually included with standard Python installations; on some Linux distributions it may need to be installed separately)

Only Python standard-library modules are used. No third-party Python packages are required.

## How to run

The current implementation binds the server to `127.0.0.1:9999`. Run the server and clients on the same computer.

1. Open a terminal in the project directory.
2. Start the server:

   ```bash
   python server.py
   ```

   The terminal should display `Server is listening...`.

3. Open a **separate terminal** for each client and run:

   ```bash
   python client.py
   ```

4. Enter a username in the login window and confirm it.
5. Select `ALL` to message everyone, or choose a connected username to send a private message.
6. Use **JOIN** to join the chat and **LEAVE** to leave it.

Start the server before launching clients. Each client connects automatically to `127.0.0.1:9999` when its script starts.

## How it works

### Server (`server.py`)

- Creates a TCP socket, binds it to `127.0.0.1:9999`, and listens for connections.
- Starts a separate thread to handle each accepted client.
- Stores client socket-to-username mappings in the `clients` dictionary.
- Handles these text commands:
  - `JOIN <username>` — registers a user and broadcasts a join notification.
  - `LEAVE` — removes the user and closes the connection.
  - `CLIENT LIST` — returns the connected usernames.
  - `SEND MSG TO <target> <message>` — sends a message to `ALL` or to a specific username.

### Client (`client.py`)

- Prompts for a username using Tkinter.
- Connects to the server and opens a chat window.
- Sends commands and receives messages on a background thread.
- Displays messages and provides a dropdown list of connected users.

## Known issues and limitations

- TCP messages are read with a fixed-size `recv(1024)`, so message boundaries are not guaranteed.
- The server's shared client list is accessed by multiple threads without synchronization.
- The client updates Tkinter widgets from a background thread, which may cause UI instability.
- The server uses `127.0.0.1`, so it is intended for use on the same machine.

## License


