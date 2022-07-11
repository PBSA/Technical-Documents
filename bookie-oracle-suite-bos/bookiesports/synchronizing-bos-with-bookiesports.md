# Synchronizing BOS with BookieSports

After successful update of BOS, Witnesses should perform the following actions to synchronize Mainnet with the latest version of BookieSports.

Synchronization should be done as follows:

1. Open MINT.
2. Go to the MINT /proposals page (see below).
3. Click on the thumbs-up-and-down icon on the MINT top bar.
4. Approve the sync proposal by clicking the green 'approve' button.&#x20;
5. Enter your password if prompted.

{% hint style="warning" %}
**Note**: Only after 50%+1 of Witnesses have approved the proposal, will it be executed and disappear from MINT so that subsequent Witnesses will not be able to see the proposal.
{% endhint %}

{% hint style="warning" %}
**Note:** Advanced features needs to be enabled within the .YAML configuration file for MINT in order to use this feature.
{% endhint %}

Example .YAML File:

```yaml
debug: False project_name: MINT

project_sub_name: The BOS Manual Intervention Modulesecret_key: # enter any random stringadvanced_features: True 

sql_database: "sqlite:///{cwd}/bookied-local.db" 

connection:    use: elizabeth # enter your desired chain     

#elizabeth:    node:- ws://ec2-35-182-42-231.ca-central-1.compute.amazonaws.com:8090        

nobroadcast: False        

num_entreis: 1 

allowed_assets:    - BTF    - PPY

```

For more information on MINT see:

{% content-ref url="../introduction-to-mint/" %}
[introduction-to-mint](../introduction-to-mint/)
{% endcontent-ref %}

