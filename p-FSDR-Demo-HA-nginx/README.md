# FSDR - Demo HA nginx

## Objectives

1. Prepare 2 web server and attach them to a LB 
2. Setup FSDR and test
   1. Switchover
   2. Failvoer
   3. Drill Start | Stop


## Prerequisites:

1. Note down Info

   ```
   # To be captured
   Region (Primary): <primary-region-key> # ap-singapore-1
   Region (Standby): <standby-region-key> # ap-singapore-2
   
   @Primary
   1. Setup two web servers and them to a LB (optionally add dns fqdn)
   2. For each Web VM 
   	a. Create volume group (Creation of volume group is mandatory)
   			i) includes boot and any additional block volume (if available)
   			ii) configure volume group replication
   	b. Configure volume group replication to standby region
   
   @Standby
   1. Create a VCN (similar as primary) add SL/NSG as needed
   2. Create a LB without any backends.
   
   Tenancy Name: <TenancyName>
   OCI Username: <Username>
   Region: <Region>
   Private API Key: <ApiPrivateKey> [eg: /home/opc/.oci/xxxxx.pem]
   Python Virtual Environment: <EnvName>
   
   # Generated
   
   ```



## Step-by-Step Instructions

| Steps | Primary Region                                               | Standby Region                                               |
| ----- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1     | 1a. Create a **bucket** to store DR plan execution logs      | 1b. Create a **bucket** to store DR plan execution logs      |
| 2     | 2a. Create DRPG <br /> - **Name**: "Give a Name"<br />- **Bucket**: "Select Bucket" | 2b. Create DRPG <br />- **Name**: "Give a Name"<br />- **Bucket**: "Select Bucket" |
| 3     | 3a. DRPG Home -> Action -> Associate<br /> - **Role** : Primary, <br />- **Peer Region**: "Select Peer DRPG" | 3b. In few minutes DRPG should be updated as Syandby role    |
| 4     | 4a. DRPG Home -> Members -> Manage Member -> Add Member<br /><br />**Compute**<br />- Resource Type: Compute/Instance, Select Instacne and "Moving Instance"<br />- Add VNIC Mapping (Between instance and Destination Subnet)<br /><br />**Storage**<br />- Resource Type: Storage/Volume group<br /><br />**Load Balancer**<br />- Resource Type: Networking/Load Balancer<br />- Add backend set mapping | 4b. DRPG Home -> Members -> Manage Member -> Add Member<br /><br />**Load Balancer**<br />- Resource Type: Networking/Load Balancer<br />- Add backend set mapping |
| 5     |                                                              | 5a. DRPG Home -> Plans -> Create Plan<br />- Name: eg: web-app-switchover-prod2dr<br />- Type: "Select the Type of DR Plan"<br /><br />Note: May want to create 3 separate plans for failover/swtichover/drill |
|       |                                                              |                                                              |
|       |                                                              |                                                              |
|       |                                                              |                                                              |
|       |                                                              |                                                              |
|       |                                                              | 4a. DRPG Home -> Plans -> Create Plan<br />- Name: web-app-switchover<br />Plan Type: Select |
|       |                                                              | 5a. Select the plan -> Plan groups -> Manage plan groups-> Add plan group<br />Group Name: |



### Useful links

1. 
