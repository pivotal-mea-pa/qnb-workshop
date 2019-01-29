## Goal

Create a BOSH Lite instance.  BOSH Lite is a Director running in VirtualBox.

## Prerequisites

1. Follow the BOSH Lite Installation Guide [here](https://bosh.io/docs/bosh-lite/).

2. Complete steps 1 to 4 under *Install*

3. To match the demos in this module, use *my-bosh* as the alias instead of *vbox*,

    `bosh alias-env my-bosh -e 192.168.50.6 --ca-cert <(bosh int ./creds.yml --path /director_ssl/ca)`

4. Continue step 6 using `-e my-bosh` instead of `-e vbox`.

5. The route in step 7 is required.
