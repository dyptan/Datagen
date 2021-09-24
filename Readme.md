To build the data generator use:
`mnv package`

To run the generator use:
`java -jar dataGen-BETA-jar-with-dependencies.jar 1000 "/tmp/data.json"`

where arguments
1000 is number of output documents(rows) to generate
and "/tmp/data.json" the destination file

```
  hadoop fs -mkdir /mapr/cyber.mapr.cluster/user/mapr/dummydata
  maprcli table create -path /mapr/cluster.name/user/mapr/dummydata/tableName -tabletype json
  mapr importJSON -src file:///tmp/data.json -dst /mapr/cluster.name/user/mapr/dummydata/tableName
```

 (Optional) To test MaprDB secondary index performance use steps:
```
  sudo yum install mapr-gateway

  maprcli cluster gateway set -dstcluster cyber.mapr.cluster -gateways localhost
  maprcli table index add -path /mapr/cluster.name/user/mapr/dummydata/tableName 
  -index natural_key_index -indexedfields _natural_key,_metadata.space_id,_gdpr_access_controls[].scope -json
  
  maprcli table index add -path /mapr/cluster.name/user/mapr/dummydata/tableName
  -index space_id_index -indexedfields _metadata.space_id,_gdpr_access_controls[].scope -json
  
  maprcli table index list -path /mapr/cluster.name/user/mapr/dummydata/tableName -json
  maprcli table cf list -path /mapr/cluster.name/user/mapr/dummydata/tableName -json
```
