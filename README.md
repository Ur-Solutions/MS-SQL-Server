# MS SQL Server local setup

1. Install MS SQL server: 
- With docker: Run the provided docker compose file.

2. Create a new database. You can call it e.g. "mist_dev" or something else.
- Connect using Jetbrains or other database client. Default configuration:
    * Host: localhost
    * Port: 1433
    * User: sa
    * Password: Provided in docker compose file
- Right click and select New > Database

3. Install database driver for use with Django:
```
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install msodbcsql17 mssql-tools
```

4. Install Django database adapter: `poetry add mssql-django`

5. Update your settings file with this content:
```
DATABASES = {
    "default": {
        "ENGINE": "mssql",
        "HOST": "localhost",
        "PORT": os.environ.get("DB_PORT", 1433),
        "NAME": os.environ.get("DB_NAME", "mist_dev"),
        "USER": os.environ.get("DB_USER", "sa"),
        "PASSWORD": os.environ.get("DB_PASSWORD"),
        "OPTIONS": {
            "driver": "ODBC Driver 17 for SQL Server",  # Default
            "extra_params": (
                # Use this if connecting without SSL certs
                "TrustServerCertificate=yes;"
            ),
        },
    },
}
```

6. Update .env file with DB credentials:
```
# MS SQL
DB_HOST=localhost
DB_PORT=1433
DB_NAME=mist_dev
DB_USER=sa
DB_PASSWORD=<REPLACE_ME>
```