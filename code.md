// access key + secret access key
// lets assume we run the chek every week
elb = boto.ec2.elb.connect_to_region('us-east-1' ,
    aws_access_key_id=options.awsKeyId,
    aws_secret_access_key=options.awsSecretKey)  /// גישה לכל הelb
problematicElbs1={} // מערך שמות של הelbים הבייתיים
problematicElbs2={}
problematicElbs3={}
allElbs = elb.get_all_load_balancers() -מערך המכיל את כל הelbים
lb=0
error=0
for lb in allElbs: // לעבור אחד אחד ולראות האם הם "בריאים"
    instances = lb.get_instance_health()
    if len(instances)==0:
        lb=1
    for instanceState in instances:
        if  instanceState.state == 'OutOfService':
            lb=1
    if lb.requestcount==0:
       lb=1
       error=1
     interval=lb.HealthCheckIntervalSeconds
     healthchecknum=10080*60/interval  /// דקות בשבוע כפול מס' שאילתות של heath checks
     if healthchecknum==lb.requestcount
        lb=1
        error=2
     if lb=1
        if error==0
          problematicElbs1 <- lb.name // להכניס שם של elb למערך שמות של השגיאה הרלוונטית
        if error==1
          problematicElbs2 <- lb.name
        if error--2
          problematicElbs3 <- lb.name
      
if  len (problematicElbs1)==0 
    print "there're no elbs with unused instances"
else
   print "these elbs have unused instances:"
   puts problematicElbs1 ( print |lb|)
if  len (problematicElbs2)==0
    print "there're no elbs with no requests"
else
    print "these elbs have no requests:"
    puts problematicElbs2 ( print |lb|)
 if  len (problematicElbs3)==0
    print "there're no elbs with no requests besides health checks"
else
    print "these elbs have no requests besides health checks:"
    puts problematicElbs2 ( print |lb|)
