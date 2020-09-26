# xdr-elk-stack

The motivation behind this little project came after I was looking for similar configuration to parse Palo Alto firewall logs with the elastic stack. I found numerous of these configs, but ultimately the one who really worked well for me was from Shadow-Box in the following link: https://github.com/shadow-box/Palo-Alto-Networks-ELK-Stack

However then I also looked into ingesting other data sources, especially Cortex XDR (also Palo Alto product). I learned the logs format and adapted similar configuration here.

The following versions are tested and works with this configurations, otherwise you might have to adjust for the added/missing fields. 

* Cortex XDR 2.4, Elasticsearch 7.4

This installation guide assume you already have ELK setup and cortex XDR logs ready for the pipeline.

To learn how to setup logstash as syslog receiver in XDR go here: https://docs.paloaltonetworks.com/cortex/cortex-xdr/cortex-xdr-pro-admin/logs/integrate-a-syslog-receiver-for-outbound-notifications.html

* Logstash: 

copy the XDR.conf file to the pipelines folder in Logstash and restart service.

* Elasticsearch:

upload the index template to your cluster as follows:

> curl -XPUT http://<your-elasticsearch-server>:9200/_template/xdr?pretty -H 'Content-Type: application/json' -d @xdr_template_mapping-v3.json
    
* Kibana:

Create the index patterns for the XDR logs with the time filter of @timestamp

Finally,

Create your dashboards based on the new XDR documents in your cluster.

![xdr dashboard in Kibana](https://github.com/dnoach/xdr-elk-stack/blob/master/dashboard.png)
