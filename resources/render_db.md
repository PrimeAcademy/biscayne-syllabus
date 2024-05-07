# Deploying a Database with render.com

## Pricing and Registration

Register for an account at [render.com](https://dashboard.render.com/register?next=%2F)

Pricing details are at:

- [https://render.com/pricing#postgresql](https://render.com/pricing#postgresql)

### Free Tier:

- Details: [https://render.com/docs/free#free-postgresql-databases](https://render.com/docs/free#free-postgresql-databases)
- Render’s free database instance type allows you to run a PostgreSQL database that automatically expires 90 days after creation.
	- 💥 Free databases are suspended after 90 days (unless they are upgraded to a paid instance type), and are no longer reachable at this point. 
	- You have a grace period of 14 days from suspension to upgrade a free database to a paid instance type. 
	- All free databases are deleted (along with their data) after the grace period.
	- We will send you email notifications to alert you of upcoming free database expirations and deletions.
- 💥 You can continue to create free PostgreSQL databases but **only one free database can be active at a time.**
- Free! No credit card required.
    - Payment methods can be added to make DB permanent or access additional features/support.

---

## Setup

### Create a database

In general, you will want to create a new database for every application you deploy.

- From the [dashboard](https://dashboard.render.com/) click the _New +_ button to create a new database. 
    - ![](../images/render-00.png)
	- From the dropdown, select _**PostgreSQL**_
- 📝 Use the web-form to configure your database:
	- **Name**: The name of your PostgreSQL instance (use the name of your app.)
	- **Database**: The name of your database; the same as what you would name your database in the `pool.js` file.
	- **User**: can be left empty
	- **Region**: Use `Oregon (US West)`. (The region determines the physical location of the cloud service hardware, which impacts performance and latency.)
	- **PostgreSQL Version**: This should match whatever version you use to develop, which is likely the latest.
		- You can check in your terminal using the command `psql --version`
	- **Datadog API Key**: _Leave this empty_
	- **Instance Type**: Select _Free_

### Connect to your database from Postico

You can use Postico to access your database, and run SQL queries against it.

- Go to your [dashboard](https://dashboard.render.com/)
- Select your Postgres Instance
![](../images/render-01.png)
- From the info tab, scrill down to the section titled **Connections**
![](../images/render-02.png)

This section gives you all the information you need to connect to your database from Postico.

- Click The Copy Button for External Database URL. 

- Open the Postico app
	- If you're already connected to a database, close your Postico window to disconnect
- Click _New Favorite_, to setup the new connection to _Render_
	- If you've copyed the External Database URL, it will be pre-populated! You shouldnt need to do anything but check.
 	- If you are using Postico 2, then you want Import From URL, and paste there.
- In Postico, click _Connect_


You can now use _Postico_ as usual. All your SQL commands will be sent to  your `render.com` database, instead of the one running on `localhost`.


### Setup your database: database.sql

Because you are a conscientious software engineer, you have kept all of your `CREATE TABLE` commands in a `database.sql` file, in your project repo. [Connect to your render.com database in Postico](#connect-to-your-database), and run all those SQL commands, just as you have before.

---

## Connect to your database from Heroku

Your Node.js app needs to know how to connect to your database. For this, we'll use a `DATABASE_URL` _environment variable_ when we configure our `pg.Pool`.

This is a two step process:

1. Set a `DATABASE_URL` environment variable on Heroku
2. Update `pool.js` to use that DATABASE_URL variable to connect to postgres

### Set the `DATABASE_URL` on Heroku

> This setup assumes that your app is already [deployed to heroku.](./deploy-heroku.md)

Your app code will run on a server hosted by Heroku (or a similar cloud service). This server will needs to have the `DATABASE_URL` set, so that your Node.js app will know how to connect to your database.

To get the value for the `DATABASE_URL`

- Go to your [render.com dashboard](https://dashboard.render.com/)
- Select your PostgreSQL instance
    - Select your PostgresInstance
    - ![](../images/render-01.png)
- Go to the _Connections_ section and copy the _External Database URL_ value
    - ![](../images/render-04.png)
    - This should look something like: `postgres://key:3rwA9UviR7tyxw7VoGMUqO8aGe8duzlw@dpg-chrmple7avj9vuf6698g-a.oregon-postgres.render.com/some_database_name`

In a terminal, `cd` into your project folder. Then run:

```
heroku config:set DATABASE_URL=your_db_connection_string
```

> 💥 **Replace `your_db_connection_string` with the actual value you copied from render.com!**

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

Your app should now be able to connect to your render.com database. Open your app in a web browser to test it:

```
heroku open
```

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