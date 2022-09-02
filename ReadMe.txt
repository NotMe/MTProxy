https://github.com/TelegramMessenger/MTProxy#running

Running
Obtain a secret, used to connect to telegram servers.
curl -s https://core.telegram.org/getProxySecret -o proxy-secret
Obtain current telegram configuration. It can change (occasionally), so we encourage you to update it once per day.
curl -s https://core.telegram.org/getProxyConfig -o proxy-multi.conf
Generate a secret to be used by users to connect to your proxy.
head -c 16 /dev/urandom | xxd -ps
Run mtproto-proxy:
./mtproto-proxy -u nobody -p 8888 -H 443 -S <secret> --aes-pwd proxy-secret proxy-multi.conf -M 1
... where:

nobody is the username. mtproto-proxy calls setuid() to drop privilegies.
443 is the port, used by clients to connect to the proxy.
8888 is the local port. You can use it to get statistics from mtproto-proxy. Like wget localhost:8888/stats. You can only get this stat via loopback.
<secret> is the secret generated at step 3. Also you can set multiple secrets: -S <secret1> -S <secret2>.
proxy-secret and proxy-multi.conf are obtained at steps 1 and 2.
1 is the number of workers. You can increase the number of workers, if you have a powerful server.
Also feel free to check out other options using mtproto-proxy --help.

Generate the link with following schema: tg://proxy?server=SERVER_NAME&port=PORT&secret=SECRET (or let the official bot generate it for you).
Register your proxy with @MTProxybot on Telegram.
Set received tag with arguments: -P <proxy tag>
Enjoy.
