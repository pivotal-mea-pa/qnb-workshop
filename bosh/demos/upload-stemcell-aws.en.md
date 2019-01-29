## Goal

In order to deploy services with BOSH we need a base operating system -- a BOSH Stemcell. Lets upload one to the BOSH director.

### Prerequisites

1. Obtain your workshop jumpbox public IP, Username, and Password 

## Upload the Stemcell

1. Utilize your SSH Client to connect to your jumpbox.

  - `ssh username@jumpbox-ip`

1. Ensure your BOSH environment is still in a good state

  - `bosh -e my-bosh environment`

1. Download a fresh AWS Cloud Platform Stemcell onto your jumpbox [here](https://s3.amazonaws.com/bosh-aws-light-stemcells/light-bosh-stemcell-3468.5-aws-xen-hvm-ubuntu-trusty-go_agent.tgz).


  - `wget --content-disposition https://s3.amazonaws.com/bosh-aws-light-stemcells/light-bosh-stemcell-3468.5-aws-xen-hvm-ubuntu-trusty-go_agent.tgz`

1. Upload the Stemcell to the BOSH director

  - `bosh -e my-bosh upload-stemcell light-bosh-stemcell-*-aws-xen-hvm-ubuntu-trusty-go_agent.tgz`

1. Check to ensure the upload was successful

  - `bosh -e my-bosh stemcells`

            Using environment 'https://10.0.0.6:25555' as client 'admin'

            Name                                     Version  OS             CPI  CID  
            bosh-aws-xen-hvm-ubuntu-trusty-go_agent  3468.5   ubuntu-trusty  -    ami-02d5e7e66dc4d8929  

            (*) Currently deployed

            1 stemcells

            Succeeded
