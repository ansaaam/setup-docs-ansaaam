### Steps involved

1. Update the endpoint on [STOXX.Platform.Apigee.Proxies > TargetEndpointIbs.xml](https://github.com/QontigoSTOXX/STOXX.Platform.Apigee.Proxies/blob/main/apiproxies/index-common/apiproxy/targets/TargetEndpointIbs.xml)
   1. Primary: `<Server name="index-calc-target-server"/> -> eur-west-4` (completely active)
   2. Secondary: `<Server name="ew3-index-calc-target-server"/> -> eur-west-3` (completely passive)
   3. To update this, go to [sandbox-Apigee](https://console.cloud.google.com/apigee/overview?project=apigee-sandbox-xf2px0)
   4. Proxy development > API Proxies > index-common > Develop > [TargetEndpointIbs](https://console.cloud.google.com/apigee/proxies/index-common/develop/138/target-endpoints/TargetEndpointIbs?project=apigee-sandbox-xf2px0) 
   5. Make the changes here then click on save
   6. Then click on **Deploy**
      1. Revision -> 138 (latest)
      2. Environment -> sandbox (PROD during DR)
      3. Service account -> `apigee-proxy-d-sa@apigee-sandbox-xf2px0.iam.gserviceaccount.com` (service account to deploy the proxy in sandbox)
      4. then click on deploy
2. Once the endpoints are updated check if the application is up or not on the following url <u>**GET THE APIGEE SWAGGER UI URL**</u>
3. Send request by **basic_sample_get_backtest.py** (or to any other endpoint) using toolkit
4. Check the logs on GCP <u>**GET GCP LOCATION FROM WHERE WE VIEW THE LOGS**</u>
5. From the logs try to verify the region <u>**ANY SPECIFIC LOG LINE FOR THIS?**</u>

### Uncertain steps
1. Make the above changes in TargetEndpointIbs.xml in DEV environemnt as hotfix?
2. Raise a PR
3. Deploy the same in QA -> UAT -> PROD
4. Then do the verification steps (sending request and checking logs)