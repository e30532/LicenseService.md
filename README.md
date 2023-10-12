This document describes the steps to configure License Service v4.2 on OCPv4.11.      

1. provision OCPv4.11.43 in Fyre QuickBurn.    
<img width="331" alt="image" src="https://media.github.ibm.com/user/24674/files/953061d5-5ac4-43f9-8365-eb829fa78faf">.   

2. refer to step 3 and 4 described in https://github.com/e30532/RHDG/blob/main/README.md and setup NFS storage class.   

3. create a catalog source definition for license service and license service reporter.   
https://www.ibm.com/docs/en/cloud-paks/foundational-services/4.2?topic=online-installing-foundational-services-by-using-console.  
https://www.ibm.com/docs/en/cloud-paks/foundational-services/4.2?topic=repository-installing-license-service-reporter-openshift-console.  

<img width="400" alt="image" src="https://media.github.ibm.com/user/24674/files/a7e87ee1-6378-4552-bfa9-2ab397225fef">.  
<img width="400" alt="image" src="https://media.github.ibm.com/user/24674/files/83207d23-23a4-43d0-8329-85aae775d43a">. 

4. install IBM licensing operator.   
<img width="400" alt="image" src="https://media.github.ibm.com/user/24674/files/80dc637b-0b6f-49b3-aa0b-959d646b70ff">.  
<img width="400" alt="image" src="https://media.github.ibm.com/user/24674/files/1999a138-c455-4a08-87a0-fda77f35f206">.  
<img width="400" alt="image" src="https://media.github.ibm.com/user/24674/files/9ef12410-556f-4631-9216-dcf89b19bd6c">.  

5. validate the IBM licensing instance.   
```
% oc get pods --all-namespaces | grep ibm-licensing | grep -v operator
ibm-licensing                                      ibm-licensing-service-instance-5c447cb779-54px6                   1/1     Running       0             2m33s
openshift-marketplace                              ibm-licensing-catalog-7w5b6                                       1/1     Running       0             11m
yamayoshi@yamadayoshikis-MacBook-Pro ~ % oc get route -n ibm-licensing                                       
NAME                             HOST/PORT                                                                   PATH   SERVICES                         PORT       TERMINATION      WILDCARD
ibm-licensing-service-instance   ibm-licensing-service-instance-ibm-licensing.apps.*.*.*.*.com          ibm-licensing-service-instance   api-port   reencrypt/None   None
% 
```

https://ibm-licensing-service-instance-ibm-licensing.apps.*.*.*.*.com.  
<img width="915" alt="image" src="https://media.github.ibm.com/user/24674/files/0bfd573c-b47a-43e6-b59f-475ab7192701">.  
<img width="833" alt="image" src="https://media.github.ibm.com/user/24674/files/304ff811-7222-42f4-b768-fa532fd28ba5">.  

Note: Access token can be retrieved with the command below.   
```
oc get secret ibm-licensing-token -o jsonpath={.data.token} -n ibm-licensing | base64 -d
```


6. install WebSphere Liberty on OCP to see if the licensing service retrieves the usage data.   
6.1. add IBM operator catalog.   
https://www.ibm.com/docs/en/cloud-paks/1.0?topic=clusters-adding-operator-catalog.   
<img width="721" alt="image" src="https://media.github.ibm.com/user/24674/files/463db970-9cf8-4970-9538-70144198f68a">.  

6.2. install WebSphere Liberty operator.   
<img width="497" alt="image" src="https://media.github.ibm.com/user/24674/files/9cb8bedd-88c6-4d57-ae3b-0f05c30230ed">.  
<img width="686" alt="image" src="https://media.github.ibm.com/user/24674/files/fbbd3c40-c512-48b6-be79-57389d0f3444">.  
<img width="1162" alt="image" src="https://media.github.ibm.com/user/24674/files/768be547-15df-4f3a-8c1e-e0817faf7828">.  

6.3. create WebSphere liberty instance with a sample image.  
<img width="415" alt="image" src="https://media.github.ibm.com/user/24674/files/5ec858aa-d246-4894-9015-6cedf801ae82">.  
<img width="771" alt="image" src="https://media.github.ibm.com/user/24674/files/68f501f3-60f3-46cf-a4c1-e519f09d42cf">.  
<img width="1117" alt="image" src="https://media.github.ibm.com/user/24674/files/c79b42f5-0c46-4635-80a7-83270acc9813">.  

6.4. check licensing service instance page.   
<img width="722" alt="image" src="https://github.com/e30532/LicenseService.md/assets/22098113/b3219c3b-24cf-4f68-b786-ce5ed0f3f87e">


7. install IBM License Service Reporter operator.   
<img width="703" alt="image" src="https://media.github.ibm.com/user/24674/files/477c0d67-4a73-43ef-a1ba-83d198b62041">.  
Note: both are identical.   
<img width="705" alt="image" src="https://media.github.ibm.com/user/24674/files/d4052527-9216-42d5-a019-6165949e84d0">.  
<img width="926" alt="image" src="https://media.github.ibm.com/user/24674/files/7daf6918-2997-4fd5-ae4f-bb4561a3f94d">.  

8. create IBM License Service Reporter instance.   
<img width="616" alt="image" src="https://media.github.ibm.com/user/24674/files/0e20f274-b1f0-49e7-a4e2-87cc9bd3825c">.  
accept license.   
<img width="741" alt="image" src="https://media.github.ibm.com/user/24674/files/5f9bbdf8-17b6-449d-a88c-1a9b83c36292">.  


9. check reporter page.   
```
% oc get pod                                                            
NAME                                                     READY   STATUS    RESTARTS   AGE
ibm-license-service-reporter-instance-7dcc8b7675-xtkkh   4/4     Running   0          2m34s
ibm-license-service-reporter-operator-58d594bc45-5xhdn   1/1     Running   0          5m24s
ibm-licensing-operator-7bd8b77c54-q2v4b                  1/1     Running   0          31m
ibm-licensing-service-instance-5c447cb779-54px6          1/1     Running   0          27m
% oc get pv
NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                                             STORAGECLASS   REASON   AGE
pvc-3308c358-9dd2-435f-95f7-e3f7d30df279   1Gi        RWX            Retain           Bound    default/sample-pvc                                nfs                     40m
pvc-7f53b2ea-a182-4f3a-959e-ee35a5f662fe   1Gi        RWO            Retain           Bound    ibm-licensing/license-service-reporter-pvc        nfs                     3m6s
registry-storage                           200Gi      RWX            Recycle          Bound    openshift-image-registry/image-registry-storage                           66m
% oc get pvc
NAME                           STATUS   VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
license-service-reporter-pvc   Bound    pvc-7f53b2ea-a182-4f3a-959e-ee35a5f662fe   1Gi        RWO            nfs            3m11s
% oc get route
NAME                             HOST/PORT                                                                   PATH                        SERVICES                         PORT       TERMINATION          WILDCARD
ibm-license-service-reporter     ibm-license-service-reporter-ibm-licensing.apps.*.*.*.*.com                                 ibm-license-service-reporter     8080       reencrypt/None       None
ibm-licensing-service-instance   ibm-licensing-service-instance-ibm-licensing.apps.*.*.*.*.com                               ibm-licensing-service-instance   api-port   reencrypt/None       None
ibm-lsr-console                  ibm-lsr-console-ibm-licensing.apps.*.*.*.*.com                  /license-service-reporter   ibm-license-service-reporter     proxy      reencrypt/Redirect   None
% 
```
https://ibm-lsr-console-ibm-licensing.apps.*.*.*.*.com/license-service-reporter.  
<img width="585" alt="image" src="https://github.com/e30532/LicenseService.md/assets/22098113/c33a1e10-6420-4f92-9dbe-6473cc6aae1a">

The credential can be retrieved with the commands below.   
https://www.ibm.com/docs/en/cloud-paks/foundational-services/4.2?topic=credentials-retrieving-basic-authentication.  
```
% oc get secret ibm-license-service-reporter-credentials -o jsonpath={.data.username} -n ibm-licensing | base64 -d
% oc get secret ibm-license-service-reporter-credentials -o jsonpath={.data.password} -n ibm-licensing | base64 -d
```
<img width="799" alt="image" src="https://github.com/e30532/LicenseService.md/assets/22098113/03fbbe8c-bffa-4150-bdc7-cb67969673ab">

As a last step, I needed to manually configure "sender" in IBM License Service instance.  

<img width="657" alt="image" src="https://github.com/e30532/LicenseService.md/assets/22098113/5ef662b1-6d93-454b-882c-3e90e4b70e9a">


After you see the message below in the license service instance pod log, you will see the data at the reporter side in a few hours.   
```
2023-10-11 16:02:34.841 [scheduling-1] INFO  Starting sender task
2023-10-11 16:02:34.842 [scheduling-1] INFO  License Service Reporter URL = https://ibm-license-service-reporter-ibm-licensing.apps.*.*.*.*.com/, cluster name = myOCP, cluster ID = aab******-*******-b83c-***********0
2023-10-11 16:02:35.138 [scheduling-1] INFO  Connected to License Service Reporter 4.2.0
2023-10-11 16:02:35.139 [scheduling-1] INFO  Sending data from 2023-09-11 to 2023-10-11
2023-10-11 16:02:36.040 [scheduling-1] INFO  Sending data to https://ibm-license-service-reporter-ibm-licensing.apps.*.*.*.*.com/snapshot?token=*****
2023-10-11 16:02:37.561 [scheduling-1] INFO  Saving last hub synchronization date = 2023-10-11
2023-10-11 16:02:37.583 [scheduling-1] INFO  Sender task finished
```

<img width="1305" alt="image" src="https://media.github.ibm.com/user/24674/files/ba63f82b-9934-4faf-9287-bf5eb60f8192">.  
<img width="1309" alt="image" src="https://media.github.ibm.com/user/24674/files/9855e97b-e653-4c7a-81c4-b2e0bb6796ad">. 







