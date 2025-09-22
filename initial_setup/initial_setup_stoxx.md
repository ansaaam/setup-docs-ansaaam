### Initial Setup

1. Create a GitHub account using your `iss-stoxx` email ID.

2. **Raise a ticket** to get added to the [QontigoSTOXX organization](https://github.com/QontigoSTOXX) **Ref: [GPE-5477](https://iss-stoxx.atlassian.net/browse/GPE-5477)**
   > p.s. add "**IndexEngineering**" as label in the ticket

3. **Request access** to the following packages:

   - 1201 - Backtest Engineers
   - 1202 - Backtest QA Users
   - 4002 - GCP Stoxx Viewer Role
   - 4031- Stoxx Composer Viewer
   - 9900 - Stoxx IDX Eng Calc
   - 9997 - Google Cloud Platform (GCP) - DEV
   - 9997 - Google Cloud Platform (GCP) - NON PROD
   - 9997 - Google Cloud Platform (GCP) - SANDBOX
   - Google Cloud Platform (GCP) - Stoxx IDX Engineering
   - Google Cloud Platform (GCP) - STOXX Index EOD Batch team

   **Note:** Some packages, such as _Google Cloud Platform (GCP) - Stoxx IDX Engineering_, require double approval. The second approval is generally done by Matt Francis.

   **Verify access:** by checking the following links

   - [GCP Console](https://console.cloud.google.com/welcome?project=index-calc-sandbox-8hiel9)
   - [Artifactory on GCP](https://console.cloud.google.com/artifacts/python/artifactory-lr8qnl/europe-west4/python-primary/stxit?project=artifactory-lr8qnl)

4. **For cloud storage access**, raise a ticket.

   - ref: [GPE-5671](https://iss-stoxx.atlassian.net/browse/GPE-5671)

   **Project IDs:**

   - PROJECT_ID_CALC_SANDBOX = "index-calc-sandbox-8hiel9" `(read and write)`
   - PROJECT_ID_CALC_DEV = "index-calc-dev-izzezi" `(read)`
   - PROJECT_ID_CALC_QA = "index-calc-qa-dl9rr3" `(read)`
   - PROJECT_ID_CALC_UAT = "index-calc-uat-zwpviv" `(read)`
   - PROJECT_ID_CALC_PROD = "index-calc-prod-xz4arx" `(read)`

   **AD Group:**

   - `stoxx-idx-Eng-Backtest`
