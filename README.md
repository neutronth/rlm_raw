# rlm_raw - FreeRADIUS module
The module provides the xlat expansion functionality %{raw: ... } to get the value of attribute from requesting packet.

It's intentionally to be used with the rlm_dynamic_clients that the client's IP address is unknown for pre-configuration. (xDSL, FTTx)

The attribute "NAS-Identifier" or any valid attributes in the requesting packet could be used as information for retrieving settings that required to setup a new client. See EXAMPLE for more details.

It was rewritten for FreeRADIUS 3.0.x and tested with FreeRADIUS 3.0.12

Inspired by the original [rlm_raw_patch](http://sourceforge.net/p/radiusdesk/code/HEAD/tree/trunk/rd_cake/Setup/Radius/rlm_raw_patch?format=raw) by RADIUSDesk.

USAGE
=====

```
  update control {
    &FreeRADIUS-Client-Secret = "%{redis: GET 'rad-secret-%{raw: NAS-Identifier}'}"
  }

```


EXAMPLE
=======

/etc/freeradius/3.0/radiusd.conf
```
...
...

instantiate {
  raw

  ...
  ...
}
```
/etc/freeradius/3.0/sites-enabled/dynamic-clients
```
client dynamic {
  ipaddr = 0/0
  dynamic_clients = dynamic_clients
  lifetime = 3600
}

server dynamic_clients {
  authorize {
    update control {
      &FreeRADIUS-Client-Secret = "%{redis: GET 'rad-secret-%{raw: NAS-Identifier}'}"
      &FreeRADIUS-Client-IP-Address = "%{Packet-Src-IP-Address}"
      &FreeRADIUS-Client-Require-MA = no
      &FreeRADIUS-Client-Shortname = "%{Packet-Src-IP-Address}"
      &FreeRADIUS-Client-NAS-Type = "other"
      &FreeRADIUS-Client-Virtual-Server = "default"
    }
  }
}
```

Contributing
============

Contributions are welcome.

Happy Hacking!
