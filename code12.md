// access key + secret access key
// lets assume we run the chek every week
ruby -v
irb,}
require 'aws-sdk'
Aws.config.update({
   credentials: Aws::Credentials.new('your_access_key_id', 'your_secret_access_key')
})
elasticloadbalancing = Aws::ElasticLoadBalancing::Client.new(region: 'us-east-1')
elb = elasticloadbalancing.describe_load_balancers()
noRequestsElbs={} // arrays of elb names
onlyHealthChecksElbs={} // arrays of elb names
elb.each do 
{|elb|
elbName=elb.LoadBalancerName
requestCount= client.get_metric_statistics({
  namespace: "AWS/ApplicationELB"
  metric_name: "RequestCount"
  dimensions: [
    {
      name: "LoadBalancerName"
      value: elbName
    },
  ],
  start_time: 2017-10-03T23:00:00Z //depends on when
  end_time: Time.now
  period: 1
  statistics: "Sum"
  unit: "Seconds"
})
if requestcount==0
     noRequestsElbs<< elbName // הכנסת כל שמות ה-elb למערך
else
{
    interval=elb.HealthCheck.Interval
    healthchecknum=10080*60/interval  /// דקות בשבוע כפול מס' שאילתות של heath checks
     if healthchecknum==requestCount
      onlyHealthChecksElbs<< elb.LoadBalancerName}
 }
 end
      }
      
if  len (noRequestsElbs)==0 
    print "there're no elbs with no requests"
else
    print "these elbs have no requests:"
    noRequestsElbs.each (|lb| print lb)
 if  len (onlyHealthChecksElbs)==0
    print "there're no elbs with no requests besides health checks"
else
    print "these Elbs have only health checks:"
    onlyHealthChecksElbs.each (|lb| print lb)
