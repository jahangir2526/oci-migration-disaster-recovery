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
   1. Setup two web servers and add them to a LB (optionally add dns fqdn)
   2. For each Web VM 
   	a. Create volume group (Creation of volume group is mandatory)
   			i) includes boot and any additional block volume (if available)
   			ii) Configure volume group replication to standby region
   
   @Standby
   1. Create a VCN (similar as primary) add SL/NSG as needed
   2. Create a LB without any backends.
   ```



## Step-by-Step Instructions

| Tasks | Primary Region/Production Site                               | Standby Region/DR Site                                       |
| ----- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 1     | **Create a bucket** <br />- To store DR plan execution logs  | **Create a bucket** <br />- To store DR plan execution logs  |
| 2     | **Create Disaster Recovery Protection Group (DRPG)** <br /> - Give a Name and Select Bucket | **Create Disaster Recovery Protection Group (DRPG)** <br /> - Give a Name and Select Bucket |
| 3     | **Select "DRPG Home -> Action -> Associate"**<br />- Select Role "Primary"<br />- Select Peer Region "\<Remote Region>"<br />Select PR DRPG "\<DRPG created in Remote Region>" | *Info:* Shortly the DRPG role will change to "Standby"       |
| 4     | **Select "DRPG Home -> Members -> Manage Member -> Add Member"**<br /><br /> - Add Member (One By One)<br />- Check the prerequisite or preparation of respective cloud resource (useful link section)<br /><br />**Compute**<br /><br />- Resource Type: Compute/Instance, Select Instacne and "Moving Instance"<br />- Add VNIC Mapping (Between instance and Destination Subnet)<br /><br />**Storage**<br />- Resource Type: Storage/Volume group<br />**Load Balancer**<br />- Resource Type: Networking/Load Balancer<br />- Add backend set mapping | **Select "DRPG Home -> Members -> Manage Member -> Add Member"**<br /><br /> **DR Resources to be created (if appliable)**<br />1. Load Balancer<br /> <br />Note: Add those resources available in secondary region (eg: LB, DB, OKE etc) |
| 5     |                                                              | **DRPG Home -> Plans -> Create Plan**<br /><br />Note: May want to create 3 separate plans for failover/swtichover/drill start |
| 6     |                                                              | **DRPG Home -> Plans -> "Selet the Plan" -> Action** -> Run Precheck |
| 7     |                                                              | **DRPG Home -> Plans -> "Selet the Plan" -> Action** -> Execute |
|       |                                                              |                                                              |
|       |                                                              |                                                              |
|       |                                                              |                                                              |
|       |                                                              |                                                              |

### Useful links

1. [FSDR Docs](https://docs.oracle.com/en-us/iaas/disaster-recovery/index.html)

2. [Prerequisite for FSDR](https://docs.oracle.com/en-us/iaas/disaster-recovery/doc/prerequsisites-disaster-recovery.html)

3. [Policy FSDR](https://docs.oracle.com/en-us/iaas/disaster-recovery/doc/policies.html)
