# Apple IOS/MACOS DNS profile for excluding WIFI networks from DOT/DOH(DNS-over-TLS/DNS-over-HTTPS)
Apple tend to make our digital life secure, but that comes with a price.
If you are not just a user and like to play with technology you might needs some engineering to get something working.
That is normal. If you run your own DNS server you might come across the issue that apple overrides your DHCP assigned DNS servers and uses a pre-defined profile with DOT/DOH and a Cloudflare DNS (or something other). I had googled a lot but find the solution. (it is well known for a few years now, but was new for me)

Need to use a custom DNS profile which
- points to your DNS server with DOT/DOH enabled (I feel this a little overkill for me )
- excludes DNS requests from DOT/DOH if you are on your network (that looks good to me)
- excludes your domain from DOT/DOH and use simple DNS (don't think I want to expose my private FQDNs anywhere)

Possibly there are other more sophisticated methods, but I leave that for the more pros.

The basic DOH/DOT profiles are out there (look in the resources section) widely available.

The one thing I needed is the WIFI exclude section which is here:


    <array>
          <dict>
            <key>Action</key>
            <string>Disconnect</string>
            <key>SSIDMatch</key>
            <array>
              <string>WIFI_A</string> # replace this with your SSID
              <string>WIFI_B</string> # replace this with your other SSID
            </array>
          </dict>
          <dict>
            <key>Action</key>
            <string>Connect</string>
            <key>InterfaceTypeMatch</key>
            <string>WiFi</string>
          </dict>
          <dict>
            <key>Action</key>
            <string>Connect</string>
            <key>InterfaceTypeMatch</key>
            <string>Cellular</string>
          </dict>
          <dict>
            <key>Action</key>
            <string>Disconnect</string>
          </dict>
        </array>

The task is to insert the exclude section to the base code (that was done with the profile generator) and install the the profile which is documented well in the resources section too. Or you can download and install the provided adguard based profile.

After installing the profile you can reach your internal DNS server through your wifi network and access the local domains. (yeah)

## Resources
There are tonns of resources out there but I used the following ones:
- [Paul Miller's Blog](https://paulmillr.com/posts/encrypted-dns/) for the details on DOT/DOH and IOS profiles
- [Paul Miller Github](https://github.com/paulmillr/encrypted-dns) for the profile base
- [notjakob dns profile generator](https://dns.notjakob.com/) for the lazy part putting the pieces together 
- [Apple's documentation](https://developer.apple.com/documentation/devicemanagement/dnssettings) for the expalnation of settings