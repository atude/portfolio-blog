## npm and the postinstall script

The node package manager (npm) is a vital part of development in JS environments; it is responsible for installing packages from the npm registry online that can provide features to your node app. It's completely free and open source, with over 800,000 packages available to integrate into your codebase. It's a simple CLI tool that downloads packages and registers them into a local `package.json` file that keeps track of your app's dependencies and package updates to bring you the latest published features. 

An important part of installing node packages is the **postinstall** script. It's part of the install lifecycle, along with preinstall, preuninstall and postuninstall. It's responsible for cleaning up files after installation and setting up configuration options. You can find more information about all `npm-scripts` on the [npm docs](https://docs.npmjs.com/misc/scripts). 

