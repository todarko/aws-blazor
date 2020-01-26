# aws-blazor

Run each command excluding the $

On you local machine, open a terminal and run the key generator

$ ssh-keygen

Copy the contents of the id_rsa.pub file opening with VSCode
In AWS, select Services then EC2
Select Key Pairs under Network & Security
Then click Import key pair
Enter a name and paste the .pub key or upload the file

Next Select Security Groups under Network & Security
Enter a name and description and open ports 22,80,443, and 5432

Select Instances and select Launch Instance 
Select the Ubuntu 16.04 and on Configure Security Group select the group created
After pressing Launch use the key that we just created in key Pairs

Once the instance is running run the following command in your terminal

$ ssh id_rsa ubuntu@ip-address

Type yes to save the server key fingerprint as a known host
You should now see ubuntu@ip-address:~$
You have logged into the server via a private key, this is more secure than using a password

Run the following commands:

sudo apt-get update
sudo apt-get install postgresql postgresql-contrib

Confirm any prompts

You now have installed Postgresql and a postgres user

Switch to the postgres user

$ sudo -i iu postgres
Run the postgres utility
$ psql
Type 
\password and press Enter
Enter a password for the postgres DB user
Run \q then exit and you should be back to the ubuntu@ip-address user

Edit posgresql config files via the following commands

$ sudo vim /etc/postgresql/9.5/main/pg_hba.conf

Press i to edit, change line 92 from 127.0.0.1/32 to 0.0.0.0/0

Press esc then type :wq and press Enter

Next edit the following:

$ sudo vim /etc/postgresql/9.5/main/postgresql.conf

Press i and edit line 59 remover the # and change localhost to *

Press esc then type :wq and press Enter

Run the following command to restart postgresql

$ sudo service postgresql restart

Run pgAdmin on your local machine. 
Right Click Servers -> Create -> Server
Add a Name. On Connections tab enter the ip-address and the password you entered for the postgres user
Press Save and it should load and connect to the postgres instance you created.

In pgAdmin, Expand Databases -> postgres 
Right click postgres and select Query Tool. Paste in the following and click the execute button or f5.

CREATE TABLE infractions (
	id 				serial PRIMARY KEY,
	title 			varchar(40),
	description		varchar(40),
	violation_type 	varchar(40),
	violator 		varchar(40),
	report_date 	date
);

Do the same to the local instance so you have the same setup both locally and in the cloud.

Run the following on your local machine to install the Blazor Preview template

dotnet new --install Microsoft.AspNetCore.Blazor.Templates::3.1.0-preview4.19579.2

Change to you project directory and run the following command

dotnet new blazorwasm -name Infractions --hosted

Open the project in VSCode, you will get a prompt, click yes.

Hold control and press f5 to build and run an instace of your project locally.

The first half of the project has now been set up. You have a server running in the cloud with an instance of Postgresql, and a local app ready for developing.
