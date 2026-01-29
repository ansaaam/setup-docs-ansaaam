## Ensure DR Compliance for IndexCalc

### Description
Fix below issues in Secret: 
- indexcalc-p-index-toolkit-azure-client
- indexcalc-p-index-toolkit-p12
- indexcalc-p-index-toolkit-user-password
- indexcalc-p-index-toolkit-user-username and
- indexcalc-p-stoxx-website-credentials
to make it DR compliant.

As of now, these secrets are on "europe-west4", "europe-west2" 
![alt text](../public/prod_secret.png)

We need to have the secrets on "europe-west4", "europe-west3" location


### Process to migrate the secret

1. Raise a PAM request to view and take a copy of secrets value
   1. Raise PAM request for "Secret Manager Admin" role
   2. ![alt text](image.png)
   3. Then copy all the secret values
   4. Copying process for lower environment was stored in one note: [one note link](../public/SECRET%20MANAGER.one)

2. Delete the secrets from PROD
   1. Merge the PR on **STOXX.Platform.SecretManager**: [PR for removing secrets from PROD](https://github.com/QontigoSTOXX/STOXX.Platform.SecretManager/pull/143)
   2. For reference, similar PRs were created and merged for lower environments
      1. DEV: [PR for removing secrets from DEV](https://github.com/QontigoSTOXX/STOXX.Platform.SecretManager/pull/139)
      2. QA: [PR for removing secrets from QA](https://github.com/QontigoSTOXX/STOXX.Platform.SecretManager/pull/140)
      3. UAT: [PR for removing secrets from UAT](https://github.com/QontigoSTOXX/STOXX.Platform.SecretManager/pull/142)
   3. TODO: put the job name that deletes the secrets

3. Create secrets using terraform
   1. New CalcService version is pushed upto UAT which creates the secrets using terraform: [PR for the same](https://github.com/QontigoSTOXX/STOXX.Index.CalcServices/pull/674)
   2. ![alt text](../public/octopus-DR.png)

4. Paste in the same value from the old secret
   1. After the above deployment, the same 5 secrets will be created but now on "europe-west4", "europe-west3"
   2. these secrets will be empty
   3. Paste the same value of the old secret with their corresponding new secret

5. Regression testing
   1. TODO: create a checklist/ipynb for regression

> since we need the secrets to have the same name as they are used in multiple place on toolkit.