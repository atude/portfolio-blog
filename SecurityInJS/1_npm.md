# npm and the postinstall script

The node package manager (npm) is a vital part of development in JS environments; it is responsible for installing packages from the npm registry online that can provide features to your node app. It's completely free and open source, with over 800,000 packages available to integrate into your codebase. It's a simple CLI tool that downloads packages and registers them into a local `package.json` file that keeps track of your app's dependencies and package updates to bring you the latest published features. 

An important part of installing node packages is the **postinstall** script. It's part of the install lifecycle, along with preinstall, preuninstall and postuninstall. It's responsible for cleaning up files after installation and setting up configuration options. You can find more information about all `npm-scripts` on the [npm docs](https://docs.npmjs.com/misc/scripts). 

### Vulnerabilities with the postinstall script

The postinstall script can run any node program on your system - this in nature already poses significant security risks, as any package could run malicious files locally. There are a few issues with this which, together, can open up the system to potential attacks, including:

1. The postinstall script is ***enabled*** by default on npm - it must be manually changed to prevent it. If it is prevented, it must be run manually which can be time consuming for every package.
2. npm does not integrate security checks with the postinstall script. Users rely on public knowledge or self analysis to determine whether a package can be malicious or not.
3. If an attacker gains access to a popular publisher's account, they can add a malicious postinstall script into the package and republish it, possibly affecting several users who update the package.