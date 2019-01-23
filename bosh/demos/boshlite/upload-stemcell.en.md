## Goal

In order to deploy services with BOSH we need a base operating system -- a BOSH Stemcell. Lets upload one to the BOSH director.

### Prerequisites

1. Obtain your workshop jumpbox public IP, Username, and Password 

## Upload the Stemcell

1. Ensure your BOSH environment is still in a good state

  - `bosh -e vbox environment`

1. Upload the Stemcell to the BOSH director

  - `bosh -e vbox upload-stemcell \
  https://bosh.io/d/stemcells/bosh-warden-boshlite-ubuntu-trusty-go_agent?v=3586.24 \
  --sha1 32c1e09391d509d24026e55555df07a166f8b8eb`

1. Check to ensure the upload was successful

  - `bosh -e vbox stemcells`
