# BOSH Workshop 

Workshop to introduce BOSH concepts.

## Sample Config.json

````json
{
    "workshopSubject":"BOSH",
    "workshopHomepage":"",
    "modules": [
        {
            "type": "concepts",
            "content": [
                {
                    "name":"Story of BOSH",
                    "filename":"bosh/story-of-bosh-slide"
                },
                {
                    "name":"What is It",
                    "filename":"bosh/what-is-it-slide"
                },
                {
                    "name":"Stemcells",
                    "filename":"bosh/stemcells-slide"
                },
                {
                    "name":"Cloud Providers",
                    "filename":"bosh/cloud-providers-slide"
                },
                {
                    "name":"Releases",
                    "filename":"bosh/releases-slide"
                },
                {
                    "name":"Manifests",
                    "filename":"bosh/manifests-slide"
                }
            ]
        },
        {
            "type": "demos",
            "content": [

            DEMOS GO HERE!

           ]
        }
    ]
}
````

Pick the Demos from the IaaS you wish to demonstrate.  The default is GCP.

## GCP Demos

Default demos already in the config.json in the repo.

````json

                {
                    "name":"Example Stemcell",
                    "filename":"bosh/demos/example-stemcell"
                },
                {
                    "name":"Upload Stemcell",
                    "filename":"bosh/demos/upload-stemcell"
                },
                {
                    "name":"Create and Upload Cloud Config",
                    "filename":"bosh/demos/create-and-upload-cloud-config"
                },
                {
                    "name":"Upload BOSH Release",
                    "filename":"bosh/demos/upload-bosh-release"
                },
                {
                    "name":"Examine BOSH Release",
                    "filename":"bosh/demos/examine-bosh-release"
                },
                {
                    "name":"Deploy BOSH Release",
                    "filename":"bosh/demos/deploy-bosh-release"
                },
                {
                    "name":"Access Managed Instance",
                    "filename":"bosh/demos/access-managed-instance"
                },
                {
                    "name":"BOSH Instance Internals",
                    "filename":"bosh/demos/bosh-instance-internals"
                },
                {
                    "name":"Resize Running Software",
                    "filename":"bosh/demos/resize-running-software"
                },
                {
                    "name":"Deploy Zookeeper",
                    "filename":"bosh/demos/deploy-zookeeper"
                },
                {
                    "name":"Upgrade Running Software",
                    "filename":"bosh/demos/upgrade-running-software"
                },
                {
                    "name":"BOSH Errands",
                    "filename":"bosh/demos/bosh-errands"
                },
                {
                    "name":"BOSH Addons",
                    "filename":"bosh/demos/bosh-addons"
                }
````

## VirutalBox (aka BOSH Lite) Demos

As of January 2019, these work best with Virtualbox 5.2.  Virtualbox 6.0.2 gave random issues with failure to connect disks, network issues, etc.  Includes extra topic on Virtalbox setup *BOSH Lite Installation*.  Does not include *Resize Running Software*

````json
                {
                    "name":"BOSH Lite Installation",
                    "filename":"bosh/demos/create-environment-vbox"
                },
                {
                    "name":"Example Stemcell",
                    "filename":"bosh/demos/example-stemcell"
                },
                {
                    "name":"Upload Stemcell",
                    "filename":"bosh/demos/upload-stemcell-vbox"
                },
                {
                    "name":"Create and Upload Cloud Config",
                    "filename":"bosh/demos/create-and-upload-cloud-config-vbox"
                },
                {
                    "name":"Upload BOSH Release",
                    "filename":"bosh/demos/upload-bosh-release"
                },
                {
                    "name":"Examine BOSH Release",
                    "filename":"bosh/demos/examine-bosh-release"
                },
                {
                    "name":"Deploy BOSH Release",
                    "filename":"bosh/demos/deploy-bosh-release"
                },
                {
                    "name":"Access Managed Instance",
                    "filename":"bosh/demos/access-managed-instance"
                },
                {
                    "name":"BOSH Instance Internals",
                    "filename":"bosh/demos/bosh-instance-internals"
                },
                {
                    "name":"Deploy Zookeeper",
                    "filename":"bosh/demos/deploy-zookeeper"
                },
                {
                    "name":"Upgrade Running Software",
                    "filename":"bosh/demos/upgrade-running-software"
                },
                {
                    "name":"BOSH Errands",
                    "filename":"bosh/demos/bosh-errands"
                },
                {
                    "name":"BOSH Addons",
                    "filename":"bosh/demos/bosh-addons"
                }
````

## AWS Demos

Configuration for the AWS Demos

````json

                {
                    "name":"Example Stemcell",
                    "filename":"bosh/demos/example-stemcell"
                },
                {
                    "name":"Upload Stemcell",
                    "filename":"bosh/demos/upload-stemcell-aws"
                },
                {
                    "name":"Create and Upload Cloud Config",
                    "filename":"bosh/demos/create-and-upload-cloud-config-aws"
                },
                {
                    "name":"Upload BOSH Release",
                    "filename":"bosh/demos/upload-bosh-release"
                },
                {
                    "name":"Examine BOSH Release",
                    "filename":"bosh/demos/examine-bosh-release"
                },
                {
                    "name":"Deploy BOSH Release",
                    "filename":"bosh/demos/deploy-bosh-release"
                },
                {
                    "name":"Access Managed Instance",
                    "filename":"bosh/demos/access-managed-instance"
                },
                {
                    "name":"BOSH Instance Internals",
                    "filename":"bosh/demos/bosh-instance-internals"
                },
                {
                    "name":"Resize Running Software",
                    "filename":"bosh/demos/resize-running-software"
                },
                {
                    "name":"Deploy Zookeeper",
                    "filename":"bosh/demos/deploy-zookeeper"
                },
                {
                    "name":"Upgrade Running Software",
                    "filename":"bosh/demos/upgrade-running-software"
                },
                {
                    "name":"BOSH Errands",
                    "filename":"bosh/demos/bosh-errands"
                },
                {
                    "name":"BOSH Addons",
                    "filename":"bosh/demos/bosh-addons"
                }
````