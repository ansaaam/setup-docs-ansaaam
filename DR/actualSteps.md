1. Update the endpoint on [STOXX.Platform.Apigee.Proxies > TargetEndpointIbs.xml](https://github.com/QontigoSTOXX/STOXX.Platform.Apigee.Proxies/blob/main/apiproxies/index-common/apiproxy/targets/TargetEndpointIbs.xml)
   1. Create a new branch from main
   2. make changes in `STOXX.Platform.Apigee.Proxies/apiproxies/index-common/apiproxy/targets/TargetEndpointIbs.xml`
      1. Replace `index-calc-target-server` to `ew3-index-calc-target-server`
      2. Primary: `<Server name="index-calc-target-server"/> -> eur-west-4` (completely active)
      3. Secondary: `<Server name="ew3-index-calc-target-server"/> -> eur-west-3` (completely passive)
   3. Raise a PR to main branch
2. Once PR is resolved deploy the changes in QA > UAT > PROD through octopus
3. Have and end to end test
   1. Ask zhilan if she needs anything specific to be covered, if not, then test "basic_sample_get_backtest"
   3. to check the logs, go to clound run on GCP for that specific environment and check for logs in europe-west3 region