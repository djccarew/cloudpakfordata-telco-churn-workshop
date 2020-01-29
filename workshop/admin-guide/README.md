# Admin guide

> **NOTE**: This section requires `Admin` user access to the Cloud Pak for Data cluster. An administrator will present this part for the workshop.

## Prerequisites

* [Cloud Pak for Data (CPD) version 2.5]
* [CPD installation of Db2 Warehouse]

## Virtualize Db2 data with Data Virtualization

For this section we'll now use the Data Virtualization tool to import the data from Db2 Warehouse, which is now exposed as an Connection in Cloud Pak for Data.

## Load Data into Local DB2 Warehouse

These instructions are for loading the data into the local CPD version of DB2 Warehouse. They will be similar for the IBM Cloud version.
You will need to provision the CPD version of DB2 Warehouse.

* From any page, click the *Services* icon in the upper right corner:

![Click services icon](../.gitbook/assets/images/admin/admin-click-services.png)

* Click the `Data sources` category or scroll down, click the 3 vertical dots on the *Db2 Warehouse* tile, and click `Deploy`:

![Click deploy Db2Warehouse](../.gitbook/assets/images/admin/admin-deploy-db2warehouse.png)

You will need to already have done the `Provision instance` for DB2 Warehouse.
Got to `Services` and click on `DB2 Warehouse` and click `Open`:

![Open Service DB2 Warehouse](../.gitbook/assets/images/dv/OpenDb2Warehouse.png)

Under `Menu` choose `Load` and `Load Data`:

![Menu Load Data](../.gitbook/assets/images/dv/DB2LoadData.png)

Choose `Browse file` and navigate to where you cloned this repository, then to `data/split/` and choose `billing.csv`, then click `Next`.
Choose Schema `NULLIDRA` and click `+ New table`. Under "New Table Name" type "BILLING" and click `Create`, then `Next`. Accept the defaults and click `Next`. Click `Begin Load`.
Repeat for the `products.csv` file, naming the table `PRODUCTS` and the `customer-service.csv` file, naming the table `CUSTOMERS`.

### Add DB Connections & Virtualization prep

For Cloud Pak for Data to read our Db2 Warehouse data we need to add a new *Data Source* to Cloud Pak for Data. This requires inputting the usual JDBC details.

To get the connection info for you local DB2 Warehouse, go to the (☰) menu and click on the *My Instances* option.

![(☰) Menu -> My Instances](../.gitbook/assets/images/dv/menuMyInstances.png)

In *My instances* go to the *Provisioned instances* tab. Highlight you local DB2 Warehouse and click the 3 vertical dots on the far right, and then click `View Details`:

![Provisioned local DB2 details](../.gitbook/assets/images/dv/provisionedDBviewDetails)

Either keep this window open in a separate tab, or copy the required Connection info: *Host*, *Port*, *Database name*, *Username*, and *Password*. You can get the port from the *JDBC Connection URL*, i.e for the URL `jdbc:db2://os-workshop-nov22worker-05.vz-cpd-nov22.com:30290/BLUDB` the port is the number after the colin in the URL `30290`:

![DB2 Connection credentials](../.gitbook/assets/images/dv/localDB2details.png)

To add a new data source, go to the (☰) menu and click on the *Connections* option.

![(☰) Menu -> Collections](../.gitbook/assets/images/connections/cpd-conn-menu.png)

At the overview, click *Add connection*.

![Overview page](../.gitbook/assets/images/connections/conn-1-overview-empty.png)

Start by giving your new *Connection* a name and select *Db2 Warehouse on Cloud* as your connection type. More fields should apper. Fill the new fields with the same credentials for your own Db2 Warehouse connection from the previous section . Click `Test Connection` and, after that succeeds, click `Add`.

![Add a Db2 Warehouse on Cloud connection](../.gitbook/assets/images/connections/conn-2-details.png)

The new connection will be listed in the overview.

![Connection has been added!](../.gitbook/assets/images/connections/conn-3-overview-db2.png)

## 2. Add a Data Source to Data Virtualization

At the empty overview, click *Add* and choose *Add data source*.

![No data sources, yet](../.gitbook/assets/images/dv/dv-data-sources-1-empty.png)

Select the data source we made in the previous step, and click *Next*.

![Add the Db2 Warehouse connection](../.gitbook/assets/images/dv/dv-data-sources-2-add.png)

The new connection will be listed as a data source for data virtualization.

![Db2 Warehouse connection is now associated with Data Virtualization](../.gitbook/assets/images/dv/dv-data-sources-3-shown.png)

### Add a Data Source to Data Virtualization

To launch the data virtualization tool, go the (☰) menu and click `Collect` and then `Data Virtualization`.

![(☰) Menu -> Collect -> Data Virtualization](../.gitbook/assets/images/dv/cpd-dv-menu.png)

At the empty overview, click *Add* and choose *Add data source*.

![No data sources, yet](../.gitbook/assets/images/dv/dv-data-sources-1-empty.png)

Select the data source we made in the previous step, and click *Next*.

![Add the Db2 Warehouse connection](../.gitbook/assets/images/dv/dv-data-sources-2-add.png)

The new connection will be listed as a data source for data virtualization.

![Db2 Warehouse connection is now associated with Data Virtualization](../.gitbook/assets/images/dv/dv-data-sources-3-shown.png)

### Start virtualizing data

In this section, since we now have access to the Db2 Warehouse data, we can virtualize the data to our Cloud Pak for Data project. Click on the *Menu* button and choose *Virtualize*.

![Menu -> Virtualize](../.gitbook/assets/images/dv/dv-virtualize-1-menu.png)

Several tables will appear (many are created as sample data when a Db2 Warehouse instance is provisioned) in the table. Find the tables you created earlier, the instructions suggested naming them: `CUSTOMER`, `PRODUCT` and `BILLING`. Once selected click on *Add to cart* and then on *View Cart*.
You can search for the Schema `NULLIDRA` and they should show up:

![Choose the tables to virtualize](../.gitbook/assets/images/dv/dv-virtualize-2-tables.png)

The next panel prompts the user to choose which project to assign the data to, choose the project you created in the previous exercise. Click *Virtualize* to start the process.

![Add virtualized data to your project](../.gitbook/assets/images/dv/dv-virtualize-3-assign.png)

You'll be notified that the virtual tables have been created! Let's see the new virtualized data from the Data Virtualization tool by clicking *View my data*.

![We've got virtualized data](../.gitbook/assets/images/dv/dv-virtualize-4-complete.png)

### Join the virtualized data

Now we're going to **join** the tables we created so we have a merged set of data. It will be easier to do it here rather than in a notebook where we'd have to write code to handle three different data sets. Click on any two tables (`PRODUCTS` and `BILLING` for instance) and click the *Join view* button.

![Choose to join two tables](../.gitbook/assets/images/dv/dv-data-join-1-overview.png)

To join the tables we need to pick a key that is common to both data sets. Here we choose to map `customerID` from the first table to `customerID` on the second table. Do this by clicking on one and dragging it to another. When the line is drawn click on *Join*.

![Map the two customerID keys](../.gitbook/assets/images/dv/dv-data-join-2-columns.png)

In the next panel we'll give our joined data a name, I chose `BILLINGPRODUCTS`, then review the joined table to ensure all columns are present and only one `customerID` column exists. Click *Next* to continue.

![Review the proposed joined table](../.gitbook/assets/images/dv/dv-data-join-3-review.png)

Next we choose which project to assign the joined view to, choose the project you created in the previous exercise. Click *Create view* to start the process.

![Add joined data tables to your project](../.gitbook/assets/images/dv/dv-data-join-4-assign.png)

You'll be notified that the join has succeeded! Click on *View my data*. to repeat this again so we have all three tables.

![The data join succeeded!](../.gitbook/assets/images/dv/dv-data-join-5-created.png)

**IMPORTANT** Repeat the same steps as above, but this time choose to join the new joined view (`BILLINGPRODUCTS`) and the last virtualized table (`CUSTOMERS`), to create a new joined view that has all three tables, let's call it `BILLINGPRODUCTSCUSTOMERS`. Switching to our project should show all three virtualized tables, and two joined tables. Do not go to the next section until this step is performed.

![Our data sets at the end of this section](../.gitbook/assets/images/dv/dv-project-data-all.png)

### Assign the "Steward" role to the attendees

Go to *Data Virtualization* option from the menu. Click on *User management*

![Manage users in Data Virtualization](../.gitbook/assets/images/dv/dv-6-manage-users.png)

Click on *Add user* and ensure all users have the *Steward* role.

![Manage users in Data Virtualization](../.gitbook/assets/images/dv/dv-7-steward-role.png)

## Adding users to the cluster

From the hamburger menu, click manage users, then add user!

![Add a user](../.gitbook/assets/images/manage/manage-add-users.png)
