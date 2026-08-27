# RED-002 Architecture & Scope

```text
                    HOME NETWORK
                  192.168.1.0/24
                         |
                       vmbr0
                         |
                         X
                 ISOLATION BOUNDARY
                         X
                         |
                       vmbr1
                  192.168.50.0/24
                    /         \
                   /           \
              KALI01          TARGET01
          192.168.50.10     192.168.50.20
             operator          target

                 NO DEFAULT GATEWAY
                    NO INTERNET
```

Discovery activity was limited to `192.168.50.0/24`. The home LAN, management interfaces, Internet, and systems not explicitly placed into the lab remained out of scope.
