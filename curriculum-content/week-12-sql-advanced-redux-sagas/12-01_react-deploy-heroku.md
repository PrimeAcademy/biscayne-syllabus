# Full Stack Create-React-App on Heroku

To review, see docs for:

- [Deploying your app to Heroku](../supporting-documentation/deploy-heroku.md)
- [Deploying a Postgres database with render.com](../supporting-documentation/render-db.md)

There are some special steps to ensure your React app can be accessed on heroku!
This does not replace the existing instructions -- rather this is an addon!

## package.json


### Start Script

Make sure the `start` script in your `package.json` starts your node server and not React.

`"start": "node server/server.js",`

### Build Script

Make sure your `package.json` contains a `build` script:

`"build": "react-scripts build"`


## Git Commit

`git add .`
`git commit -m "YOUR COMMIT MESSAGE"`

## Git Push
Push to heroku `git push heroku main`

Heroku will automatically run `npm run build`, which will make a production build of your code.
After that, heroku will run `npm start`, which will start node.

## What's going on here?

> NOTE: This section is for the curious 🧠, but is not required knowledge for deploying to Heroku. 

When we develop with React, we generally have two processes running: `npm run server` and `npm run client`. But on Heroku we need a single Node.js server running, which serves JSON from API endpoints, and also client-side files.

To achieve this:

- `npm build` uses CRA to _compile_ your code into plain JS client-side code
  - CRA puts generated code into a folder called `/build`
- Your `server.js` configures express to serve client side files from `/build`:
    ```js
    app.use(express.static('build'));
    ```
- `npm start` runs your express server, including API endpoints and static `build` files