# GA4 Snowflake Dashboard

## Prerequisites
- You must have installed the Snowflake Connector for Google Analytics Raw Data and completed the ingestion of data.
- Your application needs access to the view created by the Snowflake Connector for Google Analytics Raw Data, named `GOOGLE_ANALYTICS_RAW_DATA_DEST_SCHEMA.ANALYTICS_xxxxxxxx__VIEW`.
- Access must be granted by an account with `MANAGE_GRANTS` permissions, such as AccountAdmin.

## Instructions
- If the role you used during the installation of the app is different from your current role, please add your current role as `APP_PUBLIC` through Manage Access > Add Roles.
- Navigate to the shield icon, select 'Privilege to objects', and grant access to the `GOOGLE_ANALYTICS_RAW_DATA_DEST_SCHEMA.ANALYTICS_xxxxxxxx__VIEW` view.

Ensure you follow these steps to set up your GA4 Snowflake Dashboard correctly, allowing for efficient analysis and insights into your Google Analytics data through Snowflake.
