# User System Overview
CSGHub platform implement user login authorization through docking with Casdoor. Casdoor itself has the ability of single sign-on and third-party login, so users can configure and integrate it easily.

## How to Integrate
The user service in csghub-server can integrate Casdoor by setting the following environment variable:
STARHUB_SERVER_CASDOOR_CLIENT_ID: ${STARHUB_SERVER_CASDOOR_CLIENT_ID}
STARHUB_SERVER_CASDOOR_CLIENT_SECRET: ${STARHUB_SERVER_CASDOOR_CLIENT_SECRET}
STARHUB_SERVER_CASDOOR_ENDPOINT: ${STARHUB_SERVER_CASDOOR_ENDPOINT}
STARHUB_SERVER_CASDOOR_CERTIFICATE: <casdoor_stg_cert-token_jwt_key.pem>
STARHUB_SERVER_CASDOOR_ORGANIZATION_NAME: ${STARHUB_SERVER_CASDOOR_ORGANIZATION_NAME}
STARHUB_SERVER_CASDOOR_APPLICATION_NAME: ${STARHUB_SERVER_CASDOOR_APPLICATION_NAME}

### General Environment Variables

- **RAILS_MASTER_KEY**  
Used for decrypting Rails Credentials. The current master key for this open-source project is: `64f15f995b044427e43fe4897370fd66`

- **ON_PREMISE**  
A switch used to toggle between project versions. If set to true, it indicates an on-premise version; otherwise, it is a non-on-premise version. The main difference is that non-on-premise versions support OIDC login authorization, while on-premise versions use the system's built-in login authorization.

- **SENSITIVE_CHECK**  
A switch used to enable or disable sensitive information monitoring. If set to true, sensitive information monitoring is enabled. This feature requires collaboration with the Starhub Server. If the Starhub Server has enabled the API for sensitive information monitoring, we can turn on this switch.

- **SUPER_USERS**  
You can set the system's super admin users using this environment variable. The format is comma-separated phone numbers. This means that when registering, the matching phone number needs to be provided.


### System Config

This is a way to configure the project based on a data object called SystemConfig, which corresponds to the database table system_configs. To use an admin account for each environment, you only need to create a new SystemConfig record.

Currently, the system supports the following System Config fields, which are all of type jsonb:

- general_configs
- oidc_configs
- starhub_configs
- license_configs
- feature_flags
- s3_configs  

You can add more fields as needed.

### Project Dependencies

Before starting the project, please ensure that all dependencies are properly set up.

#### Object Storage

The project integrates the standard S3 interface and can be configured in the following ways:

- Environment variables
- Rails Credentials
- System Config

The following fields are supported (environment variables should be capitalized):

- bucket_name
- endpoint
- access_id
- access_secret
- region

#### Starhub Server

The current project is the Starhub Client, and our model data set and other functionalities rely on the services provided by the Starhub Server. It can be configured in the following ways:

- Environment variables
- Rails Credentials
- System Config

The supported fields are as follows:

- Environment variables: STARHUB_BASE_URL, STARHUB_TOKEN
- Credentials/SystemConfig: base_url, token

#### OIDC

If you are using the non-on-premise version (ON_PREMISE is set to false), the supported login authorization method is OIDC. It can be configured in the following ways:

- Environment variables
- Rails Credentials
- System Config

The environment variables support the following fields:

- OIDC_IDENTIFIER
- OIDC_SECRET
- OIDC_REDIRECT_URI
- OIDC_AUTHORIZATION_ENDPOINT
- OIDC_TOKEN_ENDPOINT
- OIDC_USERINFO_ENDPOINT

The other two methods support the following fields:

- identifier
- secret
- redirect_uri
- authorization_endpoint
- token_endpoint
- userinfo_endpoint

### Quick Start

1. Clone the project code

```
git clone <project_repository_address>
cd <project_directory>
```

2. Install dependencies

Make sure you have Ruby (recommended version 3.1 or higher) and Node.js (recommended version 16.0 or higher) installed.

```
bundle install
yarn install
```

3. Configure the database

```
bin/rails db:create
bin/rails db:migrate
bin/rails db:seed
```

This will create and migrate the database and initialize the data. Make sure your database configuration is correct.

4. Start the development server

```
bin/dev
```

This will start the Rails development server and compile Tailwind CSS. It will listen on the default local development port (usually accessible at http://localhost:3000).

### Default User on System Initialization

The system will create a default super user named `admin001` with the password set as `admin001`. You can access the system configuration and management by visiting http://localhost:3000/admin.

