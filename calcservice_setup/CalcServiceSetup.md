## Initial Codebase Setup

1. **Clone repositories**

   - [STOXX.Index.CalcServices](https://github.com/QontigoSTOXX/STOXX.Index.CalcServices)
   - [STOXX.Index.ToolKit](https://github.com/QontigoSTOXX/STOXX.Index.ToolKit)

2. Create a virtual environment for python and activate it. *Note: use Python 3.10*
```bash
   py -3.10 -m venv .venv

   # Activate it
   ./venv/Scripts/activate
```

3. **Install gcloud CLI and configure**

   - [Download and install gcloud CLI](https://cloud.google.com/sdk/docs/install)
   - Authenticating with keyring; [Reference](https://cloud.google.com/artifact-registry/docs/python/authentication#keyring)
     1. Install the keyring library
      ```pip install keyring```

     2. Install the Artifact Registry backend
      ```pip install keyrings.google-artifactregistry-auth```
      
     3. List backends to confirm the installation
      ```keyring --list-backends```
      ```bash
         # The list should include:

         ChainerBackend(priority: 10)
         GooglePythonAuth(priority: 9)
      ```
     4. Run the following command to print the repository configuration to add to your Python project
        ```
         gcloud artifacts print-settings python  --project=artifactory-lr8qnl \
         --repository=packages-python-repository \
         --location=europe
        ```
      > *p.s. check if you have access to all the packages mentioned in [initial_setup_stoxx.md](../initial_setup/initial_setup_stoxx.md)*

4. Authenticate with Google Cloud using your iss-stoxx account
   ```bash
      gcloud auth login
      gcloud config set project artifactory-lr8qnl
      gcloud auth application-default login
   ```

5. **Install requirements for CalcService**
   - Navigate to the `STOXX.Index.CalcServices` project's root directory
   - Verify:
     - `requirements.txt` and `setup.py` exists.
     - `stxbacktest/stxit_version.txt` is present and contains the latest version (currently `20.3.0`)
       - [**Latest Versions in Google Artifact Registry**](https://console.cloud.google.com/artifacts/python/artifactory-lr8qnl/europe-west4/python-primary/stxit?project=artifactory-lr8qnl)
     - After above verification run the following command to install dependencies in the root directory
      ```bash
         pip install -r gui/requirements.txt --extra-index-url "https://europe-python.pkg.dev/artifactory-lr8qnl/packages-python-repository/simple/" --no-warn-script-location --trusted-host europe-python.pkg.dev
         pip install -r .\\requirements.txt --extra-index-url https://oauth2accesstoken:$(gcloud auth print-access-token)@europe-west4-python.pkg.dev/artifactory-lr8qnl/python-primary/simple/
      ```
---

### Code Overview

1. **CalcService** uses the `stxit` package from `STOXX.Index.ToolKit`.
2. CalcService provides API endpoints in `stxbacktest/routers` (initially look at **/index_composition** endpoint).
3. To test `/index_composition`, send a request using:
   - `STOXX.Index.ToolKit/docs/backtest/basic_sample_get_index_composition.py`

---

### Execution and Debugging

1. **Start CalcService**
   - Activate the virtual environment.
   - Run:
    ```bash
    python -m stxbacktest.main
    ```
   - This starts the FastAPI app on port `8080`.

2. **Send request from STOXX.Index.ToolKit**
   - Navigate to `STOXX.Index.ToolKit/backtest`.
   - Open `basic_sample_get_index_composition.py`.
   - Change:
     ```python
     environment: str = common.PROD
     ```
     to:
     ```python
     environment: str = common.LOCAL
     ```
   - Update `batch_id` (see Additional Notes).
   - Run:
    ```bash
    python basic_sample_get_index_composition.py
    ```
   - This will send request localhost:8080 (where calcservice is running)

3. **Debugging environment**
   - Since we are passing the environmen as LOCAL, the same env is picked up while the api is getting processed here
    ```python
      # stxbacktest\routers\index_composition.py
      @router.get(
         "/index_composition",
         dependencies=[Security(user_security.UserSecurity.azure_scheme)],
      )
      async def get_index_composition(
         environment: str,
         batch_id: str,
         day: str,
         user: FastAPIUser = Security(user_security.UserSecurity.azure_scheme),
      ):
         """
         Post get_index_composition
         """
         user_email = user_security.UserSecurity.get_logged_in_user_email(user=user)

         print(
               f"GET request: /index_composition ; user: {user_email} ; environment: {environment} ; "
               f"batch_id: {batch_id} ; day: {day}"
         )

         index_composition = backtest.get_composition(env=environment, batch_id=batch_id, day=parse(day).date())
                                                      # this env ☝️
    ```
    1. there are no buckets for `LOCAL` env, so while debugging we can change it to any other environment which are on GCP (`SANDBOX`, `QA`, `DEV`, `PROD`)
    2. so make the following change and send the request again
        ```python
        index_composition = backtest.get_composition(env="DEV", batch_id=batch_id, day=parse(day).date())
        ```

4. **Debug CalcService in VS Code**
   - if `.vscode/laung.json` is not present create the file as following
    ```json
    {
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Python: Run Backtest locally",
                "type": "debugpy",
                "request": "launch",
                "module": "stxbacktest.main",
                "justMyCode": false,
                "env": {
                    "env": "SANDBOX",
                    "data_pipeline_env": "prod"
                }
            }
        ]
    }
    ```
   - Set breakpoints and run in debugging mode

---

#### Additional Notes

1. If you are getting error while installing the requirements after logging in with gcloud, it is mostly because you don't have permission for the artifactory. Check if you are able to access artifactory from [here](https://console.cloud.google.com/artifacts/python/artifactory-lr8qnl/europe-west4/python-primary/stxit?project=artifactory-lr8qnl)

2. If you are getting 404 file not found after sending request 
   - it is mostly because of wrong filters (batch_id, day)
   - To fix this, go to the bucket of the environment that you've configured (above we've used dev)
   - Go to "idt_output_data_files_dev" folder and pick up any txt file (prefer latest ones) with "\_comps\_2025.txt"
    ![bucket](/public/CalcService_bucket.png)
   - Copy the batchid from the name of the file and put it in the basic_sample_get_index_composition
   - Downloald the selected file and download it
   - Inside the file check the "day" column and select any date
    ![file](/public/CalcService_file.png)
   - Input this date in the basic_sample_get_index_composition
   - basic_sample_get_index_composition.py should look something like this after updating
    ![code](/public/CalcService_code.png)