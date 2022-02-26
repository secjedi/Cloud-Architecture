Storage Services;
- Simple Storage Service (S3); first storage service
- Glacier; archival data; a large amount of data for a long time
- Cloud front; getting the stuff close to your users; data accessed frequently is cached at an edge location near customers
- EBS; best storage solution for instances; when instaces are needed to have fast reponses; Block-level
- Storage Gateway; acts like a VPN connection into amazon cloud so it seems like we aee the remote location; a
- Snow family; collection of stuff. if we have massive data we need to move
- Databases;

Characteristics of Storage; Block Storage vs File Storage
Block Storage: used on local network all the time
  - ISCi
  - Fibre Channel
AWS use block storage with virtual machines within the cloud using EBS
File Storage; we are dealing with objects or chunks. Object storage on S3; Used with NAS devices locally; we deal with it as a file

Factores when selecting Storage
- Size; how big and how much, Cost as well
- Performance; how quickly can I get my file?
- Cost; the size and performance needs to be balanced with cost.

Think; how quickly will I need these files

Storage Classes; Different ways data can be srtored; Amazon S3 Standard (most expensive), S3 Infrequent Acccess (IA) Storage, S3 Reduced Redundancy Storage (Least redundant, tolerable), Glacier (least expensive but very expensive when you recover a lot of files). There are cost differences


