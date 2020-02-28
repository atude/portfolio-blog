## npm and the postinstall script

The **node package manager** *(npm)* is a vital part of development in JS environments; it is responsible for installing packages from the npm registry online that can provide features to your node app. It's completely free and open source, with over 800,000 packages available to integrate into your codebase. It's a simple CLI tool that downloads packages and registers them into a local `package.json` file that keeps track of your app's dependencies and package updates to bring you the latest published features. 

An important part of installing npm packages is the **postinstall** script. It's part of the install lifecycle, along with preinstall, preuninstall, postuninstall, and others. It's responsible for cleaning up files after installation and setting up configuration options. You can find more information about all `npm-scripts` and their importance in the install lifecycle on the [npm docs](https://docs.npmjs.com/misc/scripts). 

### Vulnerabilities with the postinstall script

The postinstall script can run any node program on your system - this in nature already poses significant security risks, as any package could run malicious files locally. Think of this like a technician upgrading your internet at your house - you're not sure what *exactly* it is they're doing, but you assume the work they do will benefit you. Similarly, the postinstall script from an npm package *should* clean any excess files left around, but nothing's stopping it from doing something else - unless you take the time to understand it.

There are a few issues related to this which, together, exposes your system to malicious software:

1. The postinstall script is ***enabled*** by default on npm - it must be manually changed to prevent it. If it is prevented but the script is necessary, it must be run manually which can be time consuming for every package.

2. npm does not integrate security checks with the postinstall script. Users rely on public knowledge or self analysis to determine whether a package can be malicious or not.

3. If an attacker gains access to a popular publisher's account, they can add a malicious postinstall script into the package and republish it, possibly affecting several users who update the package before users register its effects.

   > An example of this is the ESLint package attack. ESLint is the a popular linting package used for JavaScript and TypeScript, used to help write and enforce neat code and abide by best practices. On July 12th, 2018, an attacker gained accessed to an ESLint maintainer's account, and managed to publish a version of the package with a malicious postinstall script. The script would download an online file and run it to gain access to the users project base and steal their `.npmrc` file, which contains important access tokens. Once discovered, npm first unpublished the project from the registry, and then later revoked all access tokens that may have been compromised. Several users' passwords may have been compromised as a result of this breach, and npm now enforces a `package-lock.json` file to prevent unwanted packages being automatically installed.

### The trade-off npm makes

The obvious question is: *"Why are security standards not enforced in these packages before they are published?"* and the answer is ***efficiency***. It is simply too difficult for npm to verify every package before it's published. Although npm has a built-in `npm audit` function which detects vulnerabilities pre-detected in the dependencies of packages, it costs too much to have a solution that can deeply analyse code and verify every possible malicious source of behaviour automatically. For a free and open-source registry like this with thousands of downloads and several new packages published each day, the resources aren't available to maintain this standard of security. 

> npm's official blog states: "Ultimately, if a large number of users make a concerted effort to publish malicious packages to npm, malicious packages will be available on npm ... we will do our best to continuously improve the reliability and security of the registry." 
>
> Although efforts are being made by npm, security scanning "is underway but not yet ready", and thus npm relies on "users to flag suspicious packages and act quickly to remove them from the registry" since "it is impossible to guarantee that any new piece of software is benign short of manually inspecting it"
>
> [Find more here](https://blog.npmjs.org/post/141702881055/package-install-scripts-vulnerability)

Additionally, lifecycle script such as the postinstall have great benefit with assuring packages are set up appropriately, and disabling this entirely will greatly reduce the possible scope of utility.

### How to stay secure against malicious npm packages

Fortunately, since the npm userbase is so large, it is unlikely that a malicious package uploaded will reach you before it is taken down. However, it is vital regardless of this to take precaution from possible malicious scripts, and you can greatly reduce the risk of being compromised with a few simple steps:

1. Run `npm audit` and address security issues constantly to assure vulnerabilities in dependency packages are patched as soon as possible.
2. Disable install scripts by using the `--ignore-scripts` tag when installing a package. This will give you control to choose what scripts to run after or before a certain package is installed manually.
3. Disable all lifecycle scripts entirely by running `npm config set ignore-scripts true`. You will have to trade time efficiency by manually running lifecycle scripts yourself, however.
4. Use a `package-lock.json` file to enforce updates are installed manually and potentially harmful package updates to do not make it to your project or device without permission.

