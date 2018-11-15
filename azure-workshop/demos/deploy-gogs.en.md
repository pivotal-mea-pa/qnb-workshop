### TODO
Make this look better

mkdir gogs-deployment
cd gogs-deployment

git clone https://github.com/cloudfoundry-community/gogs-boshrelease ../../gogs-boshrelease

pushd ../../gogs-boshrelease
bosh upload-stemcell https://bosh.io/d/stemcells/bosh-azure-hyperv-ubuntu-trusty-go_agent
bosh -d gogs deploy manifests/gogs.yml

popd 
bosh -d gogs manifest > gogs.yml
(New Tab, and cd to workspace/bbl-workshop)
eval "$(bbl print-env)"
ssh -L "8080:$(bosh -d gogs --column=IPs vms):8080" "jumpbox@$(bbl outputs | grep jumpbox_url | cut -d ' ' -f2 | cut -d ':' -f1)" -i "$JUMPBOX_PRIVATE_KEY"
open http://localhost:8080 and register user for new admin