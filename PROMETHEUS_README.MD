# get prometheus query with curl
curl -G \
--data-urlencode 'query=sum(coredns_forward_request_count_total) by (to)' \
http://10.152.183.84:9090/api/v1/query \
| jq