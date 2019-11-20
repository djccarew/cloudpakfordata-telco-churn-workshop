# Pre-work

This section is broken up into the following steps:

1. [Clone or download the repo](#1-clone-or-download-the-repo)
1. [About the data set](#2-about-the-data-set)
1. [(Optional) Seeding our Db2 Warehouse database](#3-optional-seeding-our-db2-warehouse-database)
1. [Creating a new Cloud Pak for Data project](#4-creating-a-new-cloud-pak-for-data-project)

## 1. Clone or download the repo

Various parts of this workshop will require the attendee to upload files or run scripts that we've stored in the repository. So let's get that done early on, you'll need [`git`](https://git-scm.com) on your laptop to clone the repository directly, or access to [GitHub.com](https://github.com/) to download the zip file.

Either run the following command:

```bash
git clone https://github.com/IBM/cloudpakfordata-telco-churn-workshop
cd cloudpakfordata-telco-churn-workshop
```

Alternatively, if `git` is not supported, go to the [GitHub repo for this workshop](https://github.com/IBM/cloudpakfordata-telco-churn-workshop) and download the archived version of the workshop and extract it on your laptop.

![download workshop zip](../.gitbook/assets/images/generic/cp4d-telco-workshop-git-zip-download.png)

## 2. About the data set

The data set used for this workshop is originally from Watson Analytics and was used on a [Kaggle](https://www.kaggle.com/blastchar/telco-customer-churn) project, it contains information about customer churn for a Telecommunications company. In our Jupyter notebook for the Machine Learning exercise we'll use the `WA_Fn-UseC_-Telco-Customer-Churn.csv` file located in the [data/merged](https://github.com/IBM/cloudpakfordata-telco-churn-workshop/tree/master/data/split) directory of this repository. For use in the optional data virtualiztion exercise, the data is split into three CSV files and are located in the [data/split](https://github.com/IBM/cloudpakfordata-telco-churn-workshop/tree/master/data/split) directory.

### **[WA_Fn-UseC_-Telco-Customer-Churn.csv](../../data/merged/WA_Fn-UseC_-Telco-Customer-Churn)**

This file has the following attributes:

* Customer ID
* Contract *(Month-to-month, one year, two year)*
* Paperless Billing *(Yes, No)*
* Payment Method *(Bank transfer, Credit card, Electronic check, Mailed check)*
* Monthly Charges *($)*
* Total Charges *($)*
* Churn *(Yes, No)*
* Gender *(Male, Female)*
* Senior Citizen *(1, 0)*
* Partner *(Yes, No)*
* Dependents *(Yes, No)*
* Tenure *(1-100)*
* Phone Service *(Yes, No)*
* Multiple Lines *(Yes, No, No phone service)*
* Internet Service *(DSL, Fiber optic, No)*
* Online Security *(Yes, No, No internet service)*
* Online Backup *(Yes, No, No internet service)*
* Device Protection *(Yes, No, No internet service)*
* Tech Support *(Yes, No, No internet service)*
* Streaming TV *(Yes, No, No internet service)*
* Streaming Movies *(Yes, No, No internet service)*

### **[billing.csv](../../data/split/billing.csv)**

This file has the following attributes:

* Customer ID
* Contract *(Month-to-month, one year, two year)*
* Paperless Billing *(Yes, No)*
* Payment Method *(Bank transfer, Credit card, Electronic check, Mailed check)*
* Monthly Charges *($)*
* Total Charges *($)*
* Churn *(Yes, No)*

### **[customer-service.csv](../../data/split/customer-service.csv)**

* Customer ID
* Gender *(Male, Female)*
* Senior Citizen *(1, 0)*
* Partner *(Yes, No)*
* Dependents *(Yes, No)*
* Tenure *(1-100)*

### **[products.csv](../../data/split/products.csv)**

* Customer ID
* Phone Service *(Yes, No)*
* Multiple Lines *(Yes, No, No phone service)*
* Internet Service *(DSL, Fiber optic, No)*
* Online Security *(Yes, No, No internet service)*
* Online Backup *(Yes, No, No internet service)*
* Device Protection *(Yes, No, No internet service)*
* Tech Support *(Yes, No, No internet service)*
* Streaming TV *(Yes, No, No internet service)*
* Streaming Movies *(Yes, No, No internet service)*

## 3. Seeding our Db2 Warehouse database

We'll need a place to store our data. For this workshop we've opted to use Db2 Warehouse on our local Cloud Pak for Data cluster. Note that CP4D can work with any Database witha JDBC connector, so this is only one of many choices.

## Examine or Load Data into Local DB2 Warehouse

The data you will be using will have already been loaded into the DB2 Warehouse DB. We'll also show how the data was loaded, and you may wish to load a new table.

Click the (☰) hamburger menu in the upper left corner and choose `Collect` -> `My data`:

These instructions are for loading the data into the local CP4D version of DB2 Warehouse. They will be similar for the IBM Cloud version.

Click the (☰) hamburger menu in the upper left corner and choose `Collect` -> `My data`:

![Choose collect -> My data](../.gitbook/assets/images/dv/collectMyData.png)

Go to the *Databases* tab, click on the 3 vertial lines on the *DB2 Warehouse* tile, and click `Open`:

![Open Service DB2 Warehouse](../.gitbook/assets/images/dv/userOpenDB2Warehouse.png)

Under `Menu` choose `Explore` -> `Tables`:

![Menu Explore Data](../.gitbook/assets/images/dv/DB2ExploreData.png)

Click the `NULLIDRA` box and the existing tables will show up. You can click `TELCO-CHURN` and then the 3 (...) dots to see some info about the table:

![DB2 examine NULLIDRA schema tables](../.gitbook/assets/images/dv/NULLIDRAschemaAndTables.png)

### Load your own data

If you wish to load additional data into a table, you may. Here are the instructions used by the admin to load the existing table.

Under `Menu` choose `Load` and `Load Data`:

![Menu Load Data](../.gitbook/assets/images/dv/DB2LoadData.png)

Choose `Browse files`:

![DB2 browse files](../.gitbook/assets/images/dv/DB2browseFiles.png)

Navigate to where you cloned this repository, then to `data/merged/WA_Fn-UseC_-Telco-Customer-Churn.csv` and choose `WA_Fn-UseC_-Telco-Customer-Churn.csv`, then click `Next`.

![DB2 navigate to WA_Fn-UseC_-Telco-Customer-Churn.csv](../.gitbook/assets/images/dv/navigateToTelcoChurn.png)

Choose Schema `NULLIDRA` and click `+ New table`. Under "New Table Name" type "TELCO-CHURN" and click `Create`, then `Next`.

![DB2 choose schema and create table](../.gitbook/assets/images/dv/DB2schemaAndTableCreate.png)

Accept the defaults and click `Next`. Click `Begin Load`.

![DB2 load final](../.gitbook/assets/images/dv/DB2loadFinal.png)

### Examine connection information

You can see connection information by going to *Menu* -> *Connection Information*. Here you can see instructions for various platforms (Linux, Mac, PowerLinux, Windows, zLinux) and the information you need to connect, with the exception of the *password*. The *password* is only available to users with *Admin* privileges:

![DB2 connection info](../.gitbook/assets/images/db2/DB2connectionInformation.png)

## 4. Creating a new Cloud Pak for Data project

At this point of the workshop we will be using Cloud Pak for Data for the remaining steps.

### Log into Cloud Pak for Data

Launch a browser and navigate to your Cloud Pak for Data deployment

> **NOTE** Your instructor will provide a URL and credentials to log into Cloud Pak for Data!

![Cloud Pak for Data login](../.gitbook/assets/images/manage/cpd-login.png)

### Create a new project

Go the (☰) menu and click *Projects*

![(☰) Menu -> Projects](../.gitbook/assets/images/manage/cpd-projects-menu.png)

Click on *New project*

![Start a new project](../.gitbook/assets/images/manage/cpd-new-project.png)

Create a new project, choose `Analytics project`, give it a unique name, and click *OK. Click `Create` on the next screen.

![Pick a name](../.gitbook/assets/images/manage/cpd-new-project-name.png)

## 5 Create a Space for Machine Learning Deployments

Go the (☰) menu and click `Analyze` -> `Analytics deployments`:

![(☰) Menu -> Analytics deployments](../.gitbook/assets/images/manage/ChooseAnalyticsDeployments.png)

Click on `+ New deployment space):

![Add New deployment space](../.gitbook/assets/images/manage/addNewDeploymentSpace.png)

Give your deployment space a name, optional description, then click `Create`. You will use this space later when you deploy a machine learning model.
