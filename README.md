# yuniql ![yuniql-build-status](https://img.shields.io/appveyor/ci/rdagumampan/yuniql?style=flat-square&logo=appveyor) [![AppVeyor tests (branch)](https://img.shields.io/appveyor/tests/rdagumampan/yuniql?style=flat-square&logo=appveyor)](https://ci.appveyor.com/project/rdagumampan/yuniql/build/tests) [![Gitter](https://img.shields.io/gitter/room/yuniql/yuniql?style=flat-square&logo=gitter&color=orange)](https://gitter.im/yuniql/yuniql) [![Download latest build](https://img.shields.io/badge/Download-win--x64-green?style=flat-square&logo=windows)](https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest-full.zip) [![Download latest build](https://img.shields.io/badge/Download-docker--images-green?style=flat-square&logo=docker)](https://hub.docker.com/r/rdagumampan/yuniql)

>*** Disclaimer: **`yuniql`** is not yet officially released. Much of the claims here are still work in progress but nightly build have major features available.

<img align="right" src="assets/yuniql-logo.png" width="150">

**yuniql** (yuu-nee-kel) is a schema versioning and database migration tool for sql server and others. Versions are organized as series of ordinary directories or folders. Scripts are stored transparently as plain old `.sql` files. Yuniql simply automates what you would normally do by hand and executes scripts in an orderly and transactional fashion.

Yuniql promotes and facilitates an end-to-end database DevOps discipline. From schema versioning, to fresh database provisioning and releases via continuous delivery pipeline tasks.

<img align="center" src="https://github.com/rdagumampan/yuniql/raw/master/assets/wiki-evodb-01.png" width="700">

## To start using **`yuniql`** on Sql Server
This an express guide to using yuniql-cli. Yuniql allows developers and DBAa to run migration steps from CLI, Azure DevOps Tasks, Docker or within .NET Core App. To get started, run these commands line by line via Command Prompt (CMD).

#### Prepare connection
Set your db connection string in environment variable. This demo uses local SQL Server instance. For more connection string samples, visit https://www.connectionstrings.com/sql-server/.

```bash
SETX YUNIQL_CONNECTION_STRING "Server=.\;Database=VisitorDB;Trusted_Connection=True;"
```
#### Download samples
```bash
powershell Invoke-WebRequest -Uri https://github.com/rdagumampan/yuniql/releases/download/latest/sqlserver-sample.zip -OutFile "c:\temp\yuniql-sqlserver-sample.zip"
powershell Expand-Archive "c:\temp\yuniql-sqlserver-sample.zip" -DestinationPath "c:\temp\yuniql
```
>`Expand-Archive` requires at least powershell v5.0+ running on your machine. You may also [download manually here](https://github.com/rdagumampan/yuniql/releases/download/latest/sqlserver-sample.zip) and extract to desired directory.

#### Download `yuniql`
```bash
powershell Invoke-WebRequest -Uri https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest-full.zip -OutFile  "c:\temp\yuniql-win-x64-latest.zip"
powershell Expand-Archive "c:\temp\yuniql-win-x64-latest.zip" -DestinationPath "c:\temp\yuniql\sqlserver-sample"
```
>`Expand-Archive` requires at least powershell v5.0+ running on your machine. You may also [download manually here](https://github.com/rdagumampan/yuniql/releases/download/latest/yuniql-cli-win-x64-latest-full.zip) and extract to desired directory.

#### Run migration
The following commands `yuniql` to discover the project directory, creates the target database if it doesn't exist and runs all migration steps in the order they are listed. These includes `.sql` files, directories, subdirectories, and csv files. Tokens are also replaced via `-k` parameters.
```bash
cd c:\temp\yuniql\sqlserver-sample
dir

yuniql run -a -k "VwColumnPrefix1=Vw1,VwColumnPrefix2=Vw2,VwColumnPrefix3=Vw3,VwColumnPrefix4=Vw4"
yuniql info

Version         Created                         CreatedBy
v0.00           2019-11-03T16:29:36.0130000     DESKTOP-ULR8GDO\rdagumampan
v1.00           2019-11-03T16:29:36.0600000     DESKTOP-ULR8GDO\rdagumampan
v1.01           2019-11-03T16:29:36.1130000     DESKTOP-ULR8GDO\rdagumampan
```

#### Verify results
Query tables with SSMS or your preferred SQL client. Noticed tgat `VwVisitorTokenized` carries the tokens passed in the CLI.
```sql
//SELECT * FROM [dbo].[Visitor]
VisitorID   FirstName   LastName    Address  Email
----------- ----------- ----------- ------------------------------------------
1000        Jack        Poole       Manila   jack.poole@never-exists.com
1001        Diana       Churchill   Makati   diana.churchill@never-exists.com
1002        Rebecca     Lyman       Rizal    rebecca.lyman@never-exists.com
1003        Sam         Macdonald   Batangas sam.macdonald@never-exists.com
1004        Matt        Paige       Laguna   matt.paige@never-exists.com
```

<br>
<img align="center" src="https://github.com/rdagumampan/yuniql/raw/master/assets/visitordb-screensot-ssms.png" width="700">

## Why yuniql?
- **It's raw SQL.** Yuniql follows database-first approach to version your database. Versions are normal directories or folders. Scripts are series of plain old .sql files. No special tool or language required.
- **It's .NET Core Native.** Released as a self-contained .NET Core 3.0 application. Yuniql doesn't require any dependencies or CLR installed on the developer machine or CI/CD server. For windows, `yuniql.exe` is ready-for-use on day 1.
- **Bulk Import CSV.** Load up your master data and lookup tables from CSV files. A powerful feature when provisioning fresh developer databases or when taking large block of master data as part of a new version.
- **DevOps Friendly.** Azure Pipeline Tasks available in the Market Place. `Use Yuniql` task acquires a specific version of the Yuniql. `Run Yuniql` task runs database migrations with Yuniql CLI using version acquired earlier.
- **Cloud Ready.** Platform tested for Azure SQL Database. Plugins for Amazon RDS and Google Cloud SQL are lined up for development. ***
- **Docker Support.** Each project is prepared for containerized execution using Yuniql base images. A dockerized database project is cheap way to run migration on any CI/CD platform.
- **Cross-platform.** Works with Windows and major Linux distros.
- **Open Source.** Released under Apache License version 2.0. Absolutely free for personal or commercial use.

*** planned or being evaluated/developer/tested

## To start working with **`yuniql`** CLI
See how it works here https://github.com/rdagumampan/yuniql/wiki/How-yuniql-works

```bash
yuniql init
yuniql init -p c:\temp\demo | --path c:\temp\demo
yuniql vnext
yuniql vnext -M | --major
yuniql vnext -m | --minor
yuniql vnext -f "your-script-file.sql"
yuniql verify
yuniql run
yuniql run --platform postgresql | --platform mysql
yuniql run -a true | --auto-create-db true
yuniql run -p c:\temp\demo | --path c:\temp\demo
yuniql run -t v1.05 | --target-version v1.05
yuniql run -c "<connectiong-string>"
yuniql run -k "Token1=TokenValue1,Token2=TokenValue2,Token3=TokenValue3"
yuniql run -delimiter "|"
yuniql erase
yuniql -v | --version
yuniql -h | --help
yuniql -d | --debug
```

## To dig deeper for advanced use cases

* [How yuniql works](https://github.com/rdagumampan/yuniql/wiki/How-yuniql-works)
* [How to migrate via ASP.NET Core](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-ASP.NET-Core)
* [How to migrate via Azure Devops](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-Azure-Devops)
* [How to migrate via Docker](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-docker-container)
* [How to bulk import data](https://github.com/rdagumampan/yuniql/wiki/How-to-bulk-import-data-during-migration)
* [How to replace tokens in script files](https://github.com/rdagumampan/yuniql/wiki/How-to-apply-token-replacement)
* [How to version your database](https://github.com/rdagumampan/yuniql/wiki/How-to-baseline-your-database)
* [Known issues](https://github.com/rdagumampan/yuniql/wiki/Known-issues)
* [Best practices](https://github.com/rdagumampan/yuniql/wiki/Best-practices)

## Supported databases

* SQL Server
* Azure SQL Database
* PostgreSQL
* MySQL
* Amazon RDS - Aurora ***

*** planned or being evaluated/developer/tested

## To ask for help or contribute

You may submit ideas for improvement or report a bug by [creating an issue](https://github.com/rdagumampan/yuniql/issues/new). <br>
If you have questions, talk to us on [gitter chat](https://gitter.im/yuniql/community). Alternatively, tag [#yuniql](https://twitter.com/) on Twitter.

## To track platform tests and docker builds
For running migration from docker container, [see instructions here](https://github.com/rdagumampan/yuniql/wiki/How-to-run-migration-from-a-docker-container)

|Platform|Build Status|Description|
|---|---|---|
|SqlServer|[![yuniql-build-status](https://img.shields.io/appveyor/ci/rdagumampan/yuniql-14iom?style=flat-square&logo=appveyor)](https://ci.appveyor.com/project/rdagumampan/yuniql-14iom/build/tests)|Sql Server 2017, Azure SQL Database|
|PostgreSql|[![yuniql-build-status](https://img.shields.io/appveyor/ci/rdagumampan/yuniql-w1l3j?style=flat-square&logo=appveyor)](https://ci.appveyor.com/project/rdagumampan/yuniql-w1l3j/build/tests)|PostgreSql v9.6, v12.1|
|MySql|[![yuniql-build-status](https://img.shields.io/appveyor/ci/rdagumampan/yuniql-xk6jt?style=flat-square&logo=appveyor)](https://ci.appveyor.com/project/rdagumampan/yuniql-xk6jt/build/tests)|MySql v5.7, v8.0|
|Docker image linux-x64|![yuniql-build-status](https://img.shields.io/appveyor/ci/rdagumampan/yuniql-ee37o?style=flat-square&logo=appveyor)|`docker pull rdagumampan/yuniql:linux-x64-latest`|
|Docker imiage win-x64|![yuniql-build-status](https://img.shields.io/appveyor/ci/rdagumampan/yuniql-uakd6?style=flat-square&logo=appveyor)|`docker pull rdagumampan/yuniql:win-x64-latest`|

## License

Copyright (C) 2019 Rodel E. Dagumampan

Licensed under the Apache License, Version 2.0 (the "License"); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

## Credits

Yuniql relies on many open-source projects and we would like to thanks:
- [CommandlineParser](https://github.com/commandlineparser) for CLI commands
- [CsvTextFieldParser](https://github.com/22222/CsvTextFieldParser) for CSV file parsing
- [Npgsql](https://github.com/npgsql/npgsql) for PostgreSql drivers
- [Shouldly](https://github.com/shouldly) for unit tests
- [Moq](https://github.com/moq) for unit test mocks
- Microsoft, Oracle, for everything in dotnetcore seems open source now :)
- All the free devops tools! GitHub, AppVeyor, Docker Shields.io++

## Maintainers

[//]: contributor-faces

<a href="https://github.com/rdagumampan"><img src="https://avatars.githubusercontent.com/u/5895952?v=3" title="rdagumampan" width="80" height="80"></a>

[//]: contributor-faces
