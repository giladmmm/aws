// access key + secret access key
// lets assume we run the chek every week
elb = describeloadbalancers()
noRequestsElbs={} // arrays of elb names
onlyHealthChecksElbs={} // arrays of elb names
elb.each
{|elb| 
if elb.requestcount==0:
     noRequestsElbs<< elb.LoadBalancerName // הכנסת כל שמות ה-elb למערך
else
    interval=elb.HealthCheck.Interval
    healthchecknum=10080*60/interval  /// דקות בשבוע כפול מס' שאילתות של heath checks
     if healthchecknum==elb.requestcount
      onlyHealthChecksElbs<< elb.LoadBalancerName}
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
