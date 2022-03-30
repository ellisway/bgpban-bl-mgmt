## BlackList Management
-------------------------------------------------------------
Automated management of network and host address blacklists, for use
in bgpban .

To use the optional `iprange` for optimization and reduction you will need to install the
binary.  There is an existing iprange .deb package for both mips and mipsel that
may be used.
1.  `sudo apt install iprange`

That should get you going with minimal effort.  However, you really should
review `fw-BlackList-URLs.txt` and edit as appropriate.  The two scripts
both have configuration sections that you should also review and edit as
appropriate.


### Network and host blocklist IPset creation script
`updBlackList.sh` is the actual tool that will fetch various blocklists,
consolidate into one group, and load into a IPset.

You may schedule this appropriately for your needs, a twice-daily update at noon and midnight for example.  Note that most list
providers have guidelines on frequency of retrieval so these should be taken
into account when setting the schedule.


### List of URLs containing blacklists
The file `fw-BlackList-URLs.txt` should contain the list of URLs to
individual blocklist of networks and hosts.  Blank lines and comments
(beginning with a hash (#) are acceptable).  
The sample provided here has many sources listed as a starting point,
though only some are enabled (uncommented).  Note that several of the indicated
lists either overlap or fully include other lists so it is not necessary nor is
it recommended to blindly uncomment all URLs here.
'`cURL`' will be used to fetch each of these individually, exactly as listed.


### Using iprange
iprange may be used to optimize lists of network addresses, particularly when
merging multiple different lists from multiple sources.  Overlaps and consolidation
is standard with additional ability to reduce the number of prefix lengths for runtime
efficiency when using the created IPsets.

iprange also provides for a true proper whitelist mechanism, automatically splitting
address blocks around the whitelisted addresses followed by the above optimization
and reduction.

Unfortunately iprange only supports IPv4 currently so there would be no optimization
for IPv6 IPsets (yet)

By default, if iprange is found it will be used with nothing more than the installation
noted above being required. 

### More detail
IPset documentation may be found at http://ipset.netfilter.org

IPrange documentation may be found at https://github.com/firehol/iprange/wiki

This work was inspired by the
[Emerging Threats Blacklist](https://community.ubnt.com/t5/EdgeMAX/Emerging-Threats-Blacklist/td-p/645375)
discussion thread in the Ubiquiti Networks community forums.

This utility is one option among several for ingress filtering, and is primarily
intended to be used in lieu of, or perhaps in supplement of, (egress-based)
blackhole-routing and BGP route filtering.

#### Note
Arbitrary lists should not be summarily added or enabled for blocking.
Review of each list should be performed combined with a period of careful monitoring after
enabling to ensure legitimate traffic is not affected.  Some public "blocklists"
are known to be rather aggressive and add addresses/netblocks too easily.
