## What is this?

A lightweight Dynamic DNS service using the [Linode API](http://www.linode.com/api).

## What would I use it for?

You have a internet connection without a fixed IP and a [Linode](http://www.linode.com) account. This script lets you access that network remotely using a domain name like apartment.domain.com, without having to keep track of a dynamic IP, which can change without warning.

## Instructions

1. Create an A record in Linode's DNS manager.

2. Download the script:

        $ git clone https://github.com/andrewchilds/linode-dyn-dns
        $ cd linode-dyn-dns

3. Set your API key, which can be found at [https://manager.linode.com/profile](https://manager.linode.com/profile):

        $ echo MY_API_KEY > .key

4. Get your Domain ID and your Resource ID (your A record). Domain/Resource IDs can be found by running these commands:

        $ ./api list_domains
        $ ./api list_resources DOMAIN_ID

5. Set your API key, Domain ID and Resource ID:

        $ echo DOMAIN_ID > .domain
        $ echo RESOURCE_ID > .resource

6. Set cron to update every 30 minutes:

        # Minutes  Hr  Dy  Mo  Wd  Command
        0,30       *   *   *   *   /path/to/linode-dyn-dns/api update

7. ![Michael Jackson](http://i.imgur.com/NRmeB.jpg)

## Technical Notes

* This will set your TTL to 300.
* The script tries dyndns.org for the IP lookup. If that fails for some reason it will try whatismyipaddress.com. If you don't like the idea of relying on an outside service, you could just as easily write your own, for example in PHP:

        <?php print $_SERVER['REMOTE_ADDR']; ?>

    Put that in a file called ip.php, upload it to yourdomain.com, and change the get_ip function to:

        function get_ip {
          curl --silent http://yourdomain.com/ip.php
        }
