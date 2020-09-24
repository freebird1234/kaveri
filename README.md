# Introduction

This project involves developing an Enterprise Service Bus (ESB) in C. It is assumed that you are comfortable programming in C on a *NIX type of operating system. We will be using `git` to version control our source code artefacts. All code will be checked-in to GitHub repositories of your respective teams.

## Setting up the development environment

Following instructions assume that you are working on an Ubuntu machine, and are in the sudoers group (or have root access).

### Required tools

1. An IDE. You should use either [Visual Studio Code](https://code.visualstudio.com/) or [JetBrains CLion](https://www.jetbrains.com/clion/) IDE for writing and debugging your code.
1. [Postman](https://www.postman.com/downloads/) for easily testing the HTTP endpoints.
1. `git` for working with version control repositories. You can install the `git` client by running `sudo apt install git` in a shell.

### Installing essential libraries that you will need

1. Open a shell and run `sudo apt update`
1. Ensure that you have installed the essential headers and libraries: `sudo apt install build-essential`
1. You will be writing unit tests for all your C code using [munit](https://nemequ.github.io/munit/#getting-started).
1. Install Kore Web framework as [described here](https://docs.kore.io/3.3.1/install.html). You will use it to write an HTTP endpoint for receiving the requests for the ESB.
NOTE: A skeleton is provided to get you started. However, you are strongly encouraged to go through the [Kore's simple guide](https://docs.kore.io/3.3.1/).

### Source code layout

To make it easy working with Kore build, you can use the following suggested layout for your code in the repository.

```bash
.
├── README.md
└── esb_app
    ├── Payload.json
    ├── bmd_files
    │   ├── 1600788368_1236854806
    │   │   └── bmd_8.xml
    │   ├── 1600788386_65243901
    │   │   └── bmd_T5.xml
    │   ├── 1600788402_1631457660
    │   │   └── bmd_T3.xml
    │   ├── 1600837869_1561508839
    │   │   └── bmd.xml
    │   ├── 1600837880_1438656102
    │   │   └── bmd_T1.xml
    │   ├── 1600837887_1462023884
    │   │   └── bmd_T2.xml
    │   ├── 1600837897_1635590950
    │   │   └── bmd_T3.xml
    │   ├── 1600837908_1521033520
    │   │   └── bmd_T4.xml
    │   ├── 1600837913_474941
    │   │   └── bmd_T5.xml
    │   ├── 1600837935_823021611
    │   │   └── bmd_6.xml
    │   ├── 1600837941_67982599
    │   │   └── bmd_7.xml
    │   ├── 1600837956_874988498
    │   │   └── bmd_8.xml
    │   ├── 1600838316_1989953645
    │   │   └── bmd_6.xml
    │   ├── 1600838330_948681673
    │   │   └── bmd_T1.xml
    │   ├── 1600838336_1261306334
    │   │   └── bmd_T2.xml
    │   ├── 1600838342_520765784
    │   │   └── bmd_T3.xml
    │   ├── 1600838347_72152185
    │   │   └── bmd_T4.xml
    │   ├── 1600838358_1024408692
    │   │   └── bmd_T5.xml
    │   ├── 1600838378_312806734
    │   │   └── bmd_7.xml
    │   └── 1600838384_1708537785
    │       └── bmd_8.xml
    ├── cert
    │   ├── key.pem
    │   └── server.pem
    ├── conf
    │   ├── build.conf
    │   └── esb_app.conf
    ├── dh2048.pem
    ├── esb.code-workspace
    ├── sql\ schema
    │   ├── esb_db.sql
    │   ├── routes.sql
    │   ├── transform_config.sql
    │   └── transport_config.sql
    └── src
        ├── BMD_Tests  -----> Contains files for unit testing
        │   ├── bmd.xml
        │   ├── bmd_6.xml
        │   ├── bmd_7.xml
        │   ├── bmd_8.xml
        │   ├── bmd_T1.xml
        │   ├── bmd_T2.xml
        │   ├── bmd_T3.xml
        │   ├── bmd_T4.xml
        │   └── bmd_T5.xml
        ├── adapter
        │   ├── email.c
        │   ├── email.h
        │   ├── http.c
        │   ├── http.h
        │   └── sftp.c
        ├── bmd ------------> modules relating to parsing of the bmd files.
        │   ├── Payload.json
        │   ├── bmd_parser.c
        │   ├── bmd_parser.h
        │   └── xmljson.c
        ├── db_access ------> modules of database access
        │   ├── change_status.c
        │   ├── database.c
        │   └── database.h
        ├── esb  --------> main logic of ESB
        │   ├── dynamiclookup.c
        │   ├── esb.c
        │   ├── esb.h
        │   ├── transform.c
        │   ├── transport.c
        │   └── worker.c
        ├── esb_app.c  -----> esb endpoint listening on port for incoming BMD
        ├── esb_rest.c  ---> Rest API provided bt the ESB
        └── test   -----> Unit Test Files
            ├── munit.c
            ├── munit.h
            ├── test_bmd_parser.c
            └── test_esb.c
```


## Creating a workspace with the sample code

You can execute the following steps on a shell (on Ubuntu):
```bash
=======
The above structure was generated by running kodev create esb_app in the directory ~/temp/esb_proj. Some of the default files were suitably edited to create the skeleton code which is provided to you here. You can compare the generated files to see what we have changed and added.

Creating a workspace with the sample code
You can execute the following steps on a shell (on Ubuntu):

>>>>>>> 59934bdc9834c8f9e8870723ab346c6b63e5a519
# Install required libraries
sudo apt update
sudo apt install build-essential
sudo apt install libssl-dev
sudo apt install wget
sudo apt install curl

# Download the kodev sources
wget https://kore.io/releases/kore-3.2.0.tar.gz
tar -xf kore-3.2.0.tar.gz
cd kore-3.2.0/

# Build and install kodev
make
sudo make install

# Change directories into a working folder
cd /path/to/where/you/want/to/work

# Name 'esb_endpoint' is important, because we will be copying the files from this repo
kodev create esb_endpoint

# Fetch the code from this repositiry
git clone https://github.com/nho2020/esb_proj.git

# Copy only the relevant files from cloned repo into your kodev created one
cp -r esb_proj/esb_app/src/* esb_endpoint/src/
cp -r esb_proj/esb_app/conf/* esb_endpoint/conf/

# Build and run the kodev application
cd esb_endpoint/
kodev build
kodev run
<<<<<<< HEAD
```

Open another shell window and run:
`curl --insecure -F "bmd_file=@/some/file/path/dummy_data.txt" https://localhost:8888/bmd`

Check the logs in first shell; it should show that the file was successfully received.

=======
Open another shell window and run: curl --insecure -F "bmd_file=@/some/file/path/dummy_data.txt" https://localhost:8888/bmd

Check the logs in first shell; it should show that the file was successfully received.
>>>>>>> 59934bdc9834c8f9e8870723ab346c6b63e5a519
