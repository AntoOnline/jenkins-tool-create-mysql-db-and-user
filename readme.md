# MySQL Database Creation Pipeline

This Jenkins pipeline creates a MySQL user and database with specified parameters, including a randomly generated password.

## Requirements

- Jenkins
- MySQL server
- MySQL client (installed automatically if running on Linux)

## Parameters

- `STACK_CODE`: the code for the stack (default: `my-stack`)
- `APP_CODE`: the code for the app (default: `my-app`)
- `ENCODING`: the encoding for the database (default: `utf8mb4`)
- `MAX_CONNECTIONS`: the max concurrent connections a user can have (default: `50`)
- `MYSQL_HOST`: the host address of MySQL (default: `localhost`)
- `MYSQL_PORT`: the port of MySQL (default: `3306`)

## Pipeline Stages

1. Check MySQL client installation
2. Generate a random password for the MySQL user
3. Create the MySQL user and database

## Usage

1. Create a new Jenkins pipeline and paste the code into the pipeline script field.
2. Set the required parameters for your environment.
3. Add credentials for the MySQL root username and password in the Jenkins credentials store with the IDs `mysql-root-username` and `mysql-root-password`.
4. Run the pipeline.

## Disclaimer

This pipeline is provided as-is and without warranty. Use at your own risk.

## License

This pipeline is licensed under the MIT License.

## Want to connect?

Feel free to contact me on [Twitter](https://twitter.com/OnlineAnto), [DEV Community](https://dev.to/antoonline/) or [LinkedIn](https://www.linkedin.com/in/anto-online) if you have any questions or suggestions.

Or just visit my [website](https://anto.online) to see what I do.
