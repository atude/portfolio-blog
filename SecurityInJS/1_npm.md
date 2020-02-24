## npm and the postinstall script

The node package manager *(npm)* is a vital part of development in JS environments; it is responsible for installing packages from the npm registry online that can provide features to your node app. It's completely free and open source, with over 800,000 packages available to integrate into your codebase. It's a simple CLI tool that downloads packages and registers them into a local `package.json` file that keeps track of your app's dependencies and package updates to bring you the latest published features. 

An important part of installing npm packages is the **postinstall** script. It's part of the install lifecycle, along with preinstall, preuninstall, postuninstall, and others. It's responsible for cleaning up files after installation and setting up configuration options. You can find more information about all `npm-scripts` and their importance in the install lifecycle on the [npm docs](https://docs.npmjs.com/misc/scripts). 

### Vulnerabilities with the postinstall script

The postinstall script can run any node program on your system - this in nature already poses significant security risks, as any package could run malicious files locally. Think of this like like a technician upgrading your internet at your house - you're not sure what *exactly* it is they're doing, but you assume the work they do will benefit you. Similarly, the postinstall script from an npm package *should* clean any excess files left around, but nothing's stopping it from doing something else, unless you take the time to understand it.

There are a few issues related to this which, together, exposes your system to malicious software:

1. The postinstall script is ***enabled*** by default on npm - it must be manually changed to prevent it. If it is prevented but the script is necessary, it must be run manually which can be time consuming for every package.

2. npm does not integrate security checks with the postinstall script. Users rely on public knowledge or self analysis to determine whether a package can be malicious or not.

3. If an attacker gains access to a popular publisher's account, they can add a malicious postinstall script into the package and republish it, possibly affecting several users who update the package before users register its effects.

   > <insert example>

### The trade-off npm makes

The obvious question is: *"Why are security standards not enforced in these packages? Why doesn't npm verify packages before they're published?"* and the answer is simple: ***efficiency***. It is simply too difficult for npm to verify every package before it's published. Although npm had a built-in `npm audit` function built in which detects vulnerabilities which are already known, it costs too much time to deeply analyse code and verify every possible malicious source of behaviour automatically. For a free and open-source registry like this with thousands of downloads and several new packages published each day, the resources aren't available to maintain this standard of security.