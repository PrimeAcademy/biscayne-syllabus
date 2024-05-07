# Deploying a Database with neon.tech

## Neon Pricing and Registration

Register for an account at [neon.tech](https://neon.tech/)

Pricing details are at:

- [https://neon.tech/pricing](https://neon.tech/pricing)

Plan details are at:

- [https://neon.tech/docs/introduction/plans](https://neon.tech/docs/introduction/plans)

### Free Tier:

- Free! No credit card required.
    - Payment methods can be added to access additional features and capacity.
    - Details: For more info, [follow this link](https://neon.tech/docs/introduction/free-tier).

- Neon’s free database instance type allows you to run a PostgreSQL database.
	- Free databases are limited to one project and allocated one shared CPU and 1 GB of RAM.
    - You can have multiple databases per project
    - Neon uses a branching structure, similar to Git.
    - Branches allow you to iterate and try things, without risking your data.
    - Each branch is limited to 3GB of data.
    - The free account is limited to 10 branches.
	- Free accounts are limited to 100 hours of compute time per month.  Once that's exceeded, non-primary branches are suspended but the primary branch remains available.

---

## Setup Neon

### Creeate an account

- From the [Sign In/Up](https://console.neon.tech) page
    - Click one of the options, we recommend choosing `GitHub`
        - If you chose `Email` as the option, then click `Sign up for an account` once you're redirected to the sign-in page.
    - Add in your info, and log in.

### Set up your project, and initial database

Neon lets you create one overaching project, which can containe multiple databases.

- We are following [these instructions](https://neon.tech/docs/get-started-with-neon/setting-up-a-project)
    - Head to the [Neon Console](https://console.neon.tech/)
    - ![](../images/neon_db_create_project.png)
    - Select a fun name you can remember
    - Make sure the Postgres version matches what you use in class (likely the default).
    - Choose a name for your first database (we can add more of these later).
    - Choose the region you want to use
        - Regions are the locations of Neon's data centers
        - It will likely default to US East which likley is a fine choice for most users
        - If you knew all of your users are likely to be closer to a different region, you can choose that region instead.
    - You will be greeted with a pop-up that looks like this:
    - ![](../images/neon_db_intial_db_connection_info.png)
    - Click the "Pooled Connection" checkbox.
    - Click the eye icon, to see the password
    - Copy the connection string (everything between the quotes).
        - You want this:  
            - `postgresql://andrew%40primeacademy.io:JYM8y4heFrZd@ep-proud-star-96491941-pooler.us-east-1.aws.neon.tech/test?sslmode=require`
        - Not this:  
            - `psql 'postgresql://andrew%40primeacademy.io:JYM8y4heFrZd@ep-proud-star-96491941-pooler.us-east-1.aws.neon.tech/test?sslmode=require'`
    
### Connect to your database from _Postico_

You can use _Postico_ to access your database, and run SQL queries against it.

- Paste that connection string into a url bar in any browser, and hit `enter`
    - ![](../images/neon_db_url_bar.png)
- This will open _Postico_ and establish the connection in _Postico_
    - ![](../images/neon_db_add_to_postico.png)
- Add in a nickname, so you remember what this connection is for in the future.
    - ![](../images/neon_db_configure_postico.png)

You can now use _Postico_ as usual. All your SQL commands for this connection will be sent to  your `neon.tech` database, instead of the one running on `localhost`.

### Setup your database: database.sql

Because you are a conscientious software engineer, you have kept all of your `CREATE TABLE` commands in a `database.sql` file, in your project repo. Connect to your neon.tech database in _Postico_, and run all those SQL commands, just as you have before.

---

## Connect to your database from Heroku

Your Node.js app needs to know how to connect to your database. For this, we'll use a `DATABASE_URL` _environment variable_ when we configure our `pg.Pool`.

This is a two step process:

1. Set a `DATABASE_URL` environment variable on Heroku
2. Update `pool.js` to use that DATABASE_URL variable to connect to postgres

### Set the `DATABASE_URL` on Heroku

> This setup assumes that your app is already [deployed to heroku.](./deploy-heroku.md)

Your app code will run on a server hosted by Heroku (or a similar cloud service). This server will needs to have the `DATABASE_URL` set, so that your Node.js app will know how to connect to your database.

The value for the `DATABASE_URL` is the same string we copied above, ex: 
    - `postgresql://andrew%40primeacademy.io:JYM8y4heFrZd@ep-proud-star-96491941-pooler.us-east-1.aws.neon.tech/test?sslmode=require`

In a terminal, `cd` into your project folder. Then run:

```
heroku config:set DATABASE_URL=your_db_connection_string
```

> 💥 **Replace `your_db_connection_string` with the actual value (use your actual value, not mine)**

```
heroku config:set DATABASE_URL=postgresql://andrew%40primeacademy.io:JYM8y4heFrZd@ep-proud-star-96491941-pooler.us-east-1.aws.neon.tech/test?sslmode=require
```


### Update `pool.js` to use `DATABASE_URL`

> **NOTE:** If you're using a repo from a Prime assignment, your `pool.js` may already be setup for you!

Your `pool.js` file should look something like this:

```js
// server/modules/pool.js
const pg = require('pg');
let pool;

// When our app is deployed to the internet 
// we'll use the DATABASE_URL environment variable
// to set the connection info: web address, username/password, db name
// eg: 
//  DATABASE_URL=postgresql://jDoe354:secretPw123@some.db.com/prime_app
if (process.env.DATABASE_URL) {
    pool = new pg.Pool({
        connectionString: process.env.DATABASE_URL,
        ssl: {
            rejectUnauthorized: false
        }
    });
}
// When we're running this app on our own computer
// we'll connect to the postgres database that is 
// also running on our computer (localhost)
else {
    pool = new pg.Pool({
        host: 'localhost',
        port: 5432,
        database: 'prime_feedback', 
    });
}

module.exports = pool;
```

If you had to change this file, commit and push your changes to Heroku:

```sh
git add .
git commit -m 'Update pool.js'
git push origin main
git push heroku main
```

Your app should now be able to connect to your neon.tech database. Open your app in a web browser to test it:

```
heroku open
```

### Create another database for another app

In general, you will want to create a new database for every application you deploy.

- From the [pojects](https://console.neon.tech/app/projects) page, click your project (there should be only one on the free tier). 
    - You should see a drop-down menu, showing your database.
    - Click on it, and select `Create New Database`.
        - ![](../images/neon_db_new_database.png)
    - Enter a name for your new database in the `Database name` field, and click `Create`
        - ![](../images/neon_db_new_db_dialog.png)
    - Now, you'll still see the connection details for your existing database, but you can use the same `Database` drop down menu to toggle between databases.
    - Choose the database you want to use
        - ![](../images/neon_db_view_all_databases.png)
    - Follow the instrucitons from earlier, to get the connection URL
        - Click the "Pooled Connection" checkbox.
        - Click the eye icon, to see the password
        - ![](../images/neon_db_new_connection_details.png)
        - Copy the connection string (everything between the quotes).
            - You want this:  
                - `postgresql://andrew%40primeacademy.io:JYM8y4heFrZd@ep-proud-star-96491941-pooler.us-east-1.aws.neon.tech/test?sslmode=require`
            - Not this:  
                - `psql 'postgresql://andrew%40primeacademy.io:JYM8y4heFrZd@ep-proud-star-96491941-pooler.us-east-1.aws.neon.tech/test?sslmode=require'`

---

## Troubleshooting

If your app failed to deploy, you will see error messages when running `git push heroku main`.

If your app is deployed to Heroku but not running, or running with errors, check your app logs on heroku:

```
heroku logs --tail
```

You can use `console.log`s to help troubleshoot your code. eg.

```js
// 💥 Remove this when you're done debugging! 
// 🔒 Logging secrets is a security risk.
console.log('What is my database URL?', process.env.DATABASE_URL);
```

Just remember to commit and push to heroku after writing your new `console.log`s.