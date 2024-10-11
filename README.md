# Google Analytics 4 Export

This is an example Meltano project tailored to extracting GA4 event data from BigQuery, having set up a [BigQuery export from Google Analytics](https://support.google.com/analytics/answer/9358801). You will need to provide credentials for the BigQuery connection, and optionally Postgres or Snowflake (should you choose either as a destination).

## Performance
To properly take advantage of this setup, the BigQuery tap supports an alternate batch-extraction mode when providing `google_storage_bucket` as config, which will be signinficantly faster when paired with a loader that supports [`BATCH` messages](https://sdk.meltano.com/en/latest/batch.html) (in this case, `target-snowflake`).

> [!IMPORTANT]
> Postgres targets do not support `BATCH` messages ([yet](https://github.com/MeltanoLabs/target-postgres/issues/30)), so providing `google_storage_bucket` for the tap will cause the target here to fail with
> ```
> Unknown message type BATCH in message
> ```
> In this case, you should omit `Google Storage Bucket` and continue with the standard `RECORD`-based sync - this will be markedly slower if you have a lot of GA4 event data.

## Running with Matatika
You can load this repository into Matatika as a workspace to configure and run the `BigQuery GA4 export` Meltano jobs as pipelines:
1. Go to https://app.matatika.com (sign up/log in if you haven't already)
2. On the "Setup your workspace" view, provide a workspace name and provide the repository url
   ```
   https://github.com/Matatika/example-ga4-export.git
   ```
   under the "Advanced" section
    - If you already have a Matatika account and workspace(s), you can find the "New workspace" option under the workspace context menu (top-left)

   ![image](https://github.com/user-attachments/assets/cd4f527e-0951-41f9-bee4-ead70f260071)
3. Hit continue and wait for the workspace to create
4. Follow the prompts to configure the export pipeline(s) you are interested in running
6. Save the pipelines(s)
7. Click "Close" and then run the pipeline(s) you just configured

> [!NOTE]
> Your credentials are always encrypted and stored securely by Matatika
