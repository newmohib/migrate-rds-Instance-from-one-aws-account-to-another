# Migrate RDS Instance from One AWS account to Another and Different Region


## AWS Database Snapshot for another instance

#### Create KMS

```

1.Select a region for specific region use this key only
2.Go to KMS ( Key Management Service ) => create key
3.Select the key type (symmetric) => key uses (encrypt and decrypt )=> next
4.Type Alias (key name) => next
5.Select Define key administrative permissions (AWSServiceRoleForRDS) => next
6.Select Define key usage permissions (AWSServiceRoleForRDS)
7.Other AWS Account => add another AWS account=> input another account ID like (819089808765) => next =>> finish

```

#### Create an RDS Snapshot using ( Existing KMS)

```

1.first, create a snapshot form database for another instance
2.Go to RDS => select DB => form actions => select Take a snapshot
3.Got to Snapshots
4.Select Snapshot RDS
5.Copy selected snapshot for another Region ( if need to another AWS Region )
  a.Actions=> copy snapshot
  b.Select Destination Region like Singapore  (for new instance Region)
6.Encryption
  a.AWS KMS key => select custom KMS key (which you created exist )
  b.Copy snapshot (need some time to create )
  c.Got to your Region like (Sydney to Singapore ) which region select for snapshot
  
```
<img src="images/1-copy-snapshot.png" alt="copy-snapshot" width="700">

#### Share With Another account

```

1.Go to RDS => select snapshot => select DB snapshot => Actions => Share Snapshot
2.Need to AWS account id ( you can find from AWS account profile )
3.Like 801783636085 
4.Add => save
5.Again go to KMS => got to selected KMS KEY
6.Copy ARM like (arn:aws:kms:ap-southeast-1:819096608765:key/3949a47f-758i-4bc8-8e96-deb5e658b5fg)
7.Share this ARM key with another account, which is using this snapshot-like (819096608765) this user.

```
<img src="images/2-snapshot-permission.png" alt="snapshot-permission" width="700">

#### Need to Create a Security Group for access DB
```

1.Go to Security Group => Create Security Group
2.Security group names like (all-db-security-for-postgresql)
3.Inbound rules => TCP => anywhere
4.Outbound rules => TCP => anywhere
5.Create security group

```
<img src="images/3-creat-security-group.png" alt="snapshot-permission" width="700">

#### In new instance

```
1.Select your AWS account and select Region
2.Go to RDS => select snapshots ( from left sidebar ) => Shared With me
3.Select snapshot => Actions => copy Snapshot

```
<img src="images/4-snapshot.png" alt="snapshot" width="700">

```
4.Select destination Region
5.Input new DB Snapshot Identifier (name )
6.AWS KMS key
  a.3Select Enter a key ARN
  b.Input Amazon Resource Name (ARN) this ARN is Shared with you from Snapshot shared account like 
  (arn:aws:kms:ap-southeast-1:816096808765:key/3949a47f-751b-4bc3-8e76-deb5e658b5fg)
  c.Auto Varified field like (From Account, KMS Key ID)
  d.Copy Snapshot (need to sometime )
  e.Must select VPC security group (firewall)
  f.Select Existing VPC security groups (which one is you create exists) all-db-security-for-postgresql

```
<img src="images/5-vps-security.png" alt="vps-security" width="700">

<img src="images/6-copy-snapshot.png" alt="copy-snapshot" width="700">

#### After copy like this img

<img src="images/7-after-copy-snapshot.png" alt="after-copy-snapshot" width="700">

```
1.When status is Available then
2.Select snapshot => Actions => Restore Snapshot
3.Configure DB instance settings =>
4.Restore DB instance 
5.Status Creating
6.When Available then Successfully created
7.Got to this database => copy Endpoint and port => try to connect this database with this end point
```

<img src="images/8-restore-db.png" alt="after-restore-db width="700">

#### Need to Share this KMS key with another account owner. where this shapshot will Restore

                                                                    
