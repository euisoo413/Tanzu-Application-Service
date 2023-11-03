Note: This article assumes that the `credhub` CLI is installed on the Ops Manager VM. A quick way to check is to SSH to the Ops Manager VM and run “credhub --version”

Scenario 1 
If your use case requires you to login and access the CredHub that was deployed on the BOSH Director VM, follow the instructions below:

1. SSH to your Ops Manager VM:
```
ssh ubuntu@<opsman-url-or-IP>
```
2. Export "BOSH Commandline Credentials" from the Ops Manager UI > Ops Manager/Director tile > Credentials tab > BOSH Commandline Credentials

a. Open the BOSH Commandline Credentials, copy the value of key "credential".

b. Paste that in a Notepad and remove "bosh" from that value.

c. From the Ops Manager VM terminal session, run the following command:
```
export BOSH_CLIENT=ops_manager BOSH_CLIENT_SECRET=<redacted> BOSH_CA_CERT=/var/tempest/workspaces/default/root_ca_certificate BOSH_ENVIRONMENT=<redacted>
```
3. Export environment variables required to access CredHub:
```
export CREDHUB_CLIENT=$BOSH_CLIENT CREDHUB_SECRET=$BOSH_CLIENT_SECRET
```
5. Target the Credhub API and log in:
```
credhub api -s $BOSH_ENVIRONMENT:8844 --ca-cert $BOSH_CA_CERT
credhub login
```
Note: In modern CredHub versions, you can start using CredHub directly without logging in since CREDHUB_* environment variables are present.

