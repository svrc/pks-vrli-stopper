# Tanzu Kubernetes Grid Integrated (TKGI) / PKS VRLI stopper

## What does this do?

TKGI/PKS has a bug (July 2020) where the vRLI fluentd process runs even when vRLI is not configured.  This can lead to other syslog aggregators using our rsyslog functionality via Blackbox to get parsing errors from the resulting fluentd output on the PKS API VM.

This addon will force the PKS vRLI fluentd process to do nothing.

## How do I install it?

1. Open a shell prompt on a BOSH CLI with access to your PKS bosh director, such as Ops Manager.
2. Export your BOSH credentials to the enviornment.  These can be accessed via the Ops Manager GUI -> BOSH Director Tile -> Credentials Tab -> Bosh Commandline Credentials.    

e.g.
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=fakesecret BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate  BOSH_ENVIRONMENT=10.0.0.10
```
3. Copy or clone this repository onto this BOSH CLI workstation and create+upload the BOSH release to the director

```
git clone https://github.com/svrc-pivotal/pks-vrli-stopper && cd pks-vrli-stopper
bosh create-release --force
bosh upload-release ./dev_releases/pks-vrli-stopper/pks-vrli-stopper-0+dev.1.yml

```
4. Configure the addon from this repo
```
bosh -n update-config --name=pks-vrli-stopper --type=runtime ./addon.yml
```
5. Update your PKS clusters via the PKS CLI and/or Ops Manager "Apply Pending Changes" button with the PKS upgrade errand enabled.  This addon will automatically be installed on the PKS API VM.



