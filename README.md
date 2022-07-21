# sshuttle-rust

## About

sshuttle-rust is a a rewrite of sshuttle, with the following features:

* It is written in rust, not Python.
* It talks to ssh using the "ssh -D" socks support, as a result it does not require any additional code on server.
* It should be considered alpha quality. While it works, there are many missing features.

Features that are implemented:

* IPv4 and IPv6.
* TCP support.
* socks5 support.
* nat firewall support.
* TPROXY firewall support.

Missing features include, but not limited to:

* Automatic detection of available ports to use.
* sudo support.
* Other firewalls, such as OSX support.
* UDP support.
* DNS support.
* Daemon support.

Known bugs:

* After starting process, initial connections will be rejected because ssh hasn't started yet.
* Some servers may support running remote programs, but might disallow -D port forwarding. These won't work yet.
* Shutdown of run_client code could be a bit cleaner.
* Probably many others.

## Usage

```sh
sudo RUST_LOG=trace SSH_AUTH_SOCK="$SSH_AUTH_SOCK" sshuttle_rust --remote user@host.example.org --listen 127.0.0.1:1021  --listen '[::1]:1022' 0.0.0.0/0:443 '[::/0]:443'
```

This will create a ssh connection to "use@host.example.org" using "-D" and forward all connections to port 443. By default ssh is configured with `-D 127.0.0.1:1080`, the socks address can be changed with the `--socks` option.

If you omit the `--remote` option it will not start ssh, but try to connect to an existing socks server at the address given by the `--socks` option.
