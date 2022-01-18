# NPM help

## Usage: npm &lt;command&gt;

npm install        install all the dependencies in your project  
npm install &lt;foo&gt;  add the &lt;foo&gt; dependency to your project  
npm test           run this project's tests  
npm run &lt;foo&gt;      run the script named &lt;foo&gt;  
npm &lt;command&gt; -h   quick help on &lt;command&gt;  
npm -l             display usage info for all commands  
npm help &lt;term&gt;    search for help on &lt;term&gt;  
npm help npm       more involved overview  

All commands:

    access, adduser, audit, bin, bugs, cache, ci, completion,
    config, dedupe, deprecate, diff, dist-tag, docs, doctor,
    edit, exec, explain, explore, find-dupes, fund, get, help,
    hook, init, install, install-ci-test, install-test, link,
    ll, login, logout, ls, org, outdated, owner, pack, ping,
    prefix, profile, prune, publish, rebuild, repo, restart,
    root, run-script, search, set, set-script, shrinkwrap, star,
    stars, start, stop, team, test, token, uninstall, unpublish,
    unstar, update, version, view, whoami

Specify configs in the ini-formatted file:
```
    /home/dietpi/.npmrc  
```
or on the command line via: npm &lt;command&gt; --key=value

More configuration info: npm help config
Configuration fields: npm help 7 config

|Command|Description|
|:------------------------------------:|------------------------------------|
|npm|[JavaScript package manager](https://docs.npmjs.com/cli/v7/commands/npm)|  
|npm access|[Set access level on published packages](https://docs.npmjs.com/cli/v7/commands/npm-access)|
|npm adduser|[Add a registry user account](https://docs.npmjs.com/cli/v7/commands/npm-adduser)|
|npm audit|[Run a security audit](https://docs.npmjs.com/cli/v7/commands/npm-audit)|
|npm bin|[Display npm bin folder](https://docs.npmjs.com/cli/v7/commands/npm-bin)|
|npm bugs|[Bugs for a package in a web browser maybe](https://docs.npmjs.com/cli/v7/commands/npm-bugs)|
|npm cache|[Manipulates packages cache](https://docs.npmjs.com/cli/v7/commands/npm-cache)|
|npm ci|[Install a project with a clean slate](https://docs.npmjs.com/cli/v7/commands/npm-ci)|
|npm completion|[Tab completion for npm](https://docs.npmjs.com/cli/v7/commands/npm-completion)|
|npm config|[Manage the npm configuration files](https://docs.npmjs.com/cli/v7/commands/npm-config)|
|npm dedupe|[Reduce duplication](https://docs.npmjs.com/cli/v7/commands/npm-dedupe)|
|npm deprecate|[Deprecate a version of a package](https://docs.npmjs.com/cli/v7/commands/npm-deprecate)|
|npm diff|[The registry diff command](https://docs.npmjs.com/cli/v7/commands/npm-diff)|
|npm dist-tag|[Modify package distribution tags](https://docs.npmjs.com/cli/v7/commands/npm-dist-tag)|
|npm docs|[Docs for a package in a web browser maybe](https://docs.npmjs.com/cli/v7/commands/npm-docs)|
|npm doctor|[Check your environments](https://docs.npmjs.com/cli/v7/commands/npm-doctor)|
|npm edit|[Edit an installed package](https://docs.npmjs.com/cli/v7/commands/npm-edit)|
|npm exec|[Run a command from an npm package](https://docs.npmjs.com/cli/v7/commands/npm-exec)|
|npm explain|[Explain installed packages](https://docs.npmjs.com/cli/v7/commands/npm-explain)|
|npm explore|[Browse an installed package](https://docs.npmjs.com/cli/v7/commands/npm-explore)|
|npm find-dupes|[Find duplication in the package tree](https://docs.npmjs.com/cli/v7/commands/npm-find-dupes)|
|npm fund|[Retrieve funding information](https://docs.npmjs.com/cli/v7/commands/npm-fund)|
|npm help|[Search npm help documentation](https://docs.npmjs.com/cli/v7/commands/npm-help)|
|npm help-search|[Get help on npm](https://docs.npmjs.com/cli/v7/commands/npm-help-search)|
|npm hook|[Manage registry hooks](https://docs.npmjs.com/cli/v7/commands/npm-hook)|
|npm init|[Create a package.json file](https://docs.npmjs.com/cli/v7/commands/npm-init)|
|npm install|[Install a package](https://docs.npmjs.com/cli/v7/commands/npm-install)|
|npm install-ci-test|[Install a project with a clean slate and run tests](https://docs.npmjs.com/cli/v7/commands/npm-install-ci-test)|
|npm install-test|[Install package(s) and run tests](https://docs.npmjs.com/cli/v7/commands/npm-install-test)|
|npm link|[Symlink a package folder](https://docs.npmjs.com/cli/v7/commands/npm-link)|
|npm logout|[Log out of the registry](https://docs.npmjs.com/cli/v7/commands/npm-logout)|
|npm ls|[List installed packages](https://docs.npmjs.com/cli/v7/commands/npm-ls)|
|npm org|[Manage orgs](https://docs.npmjs.com/cli/v7/commands/npm-org)|
|npm outdated|[Check for outdated packages](https://docs.npmjs.com/cli/v7/commands/npm-outdated)|
|npm owner|[Manage package owners](https://docs.npmjs.com/cli/v7/commands/npm-owner)|
|npm pack|[Create a tarball from a package](https://docs.npmjs.com/cli/v7/commands/npm-pack)|
|npm ping|[Ping npm registry](https://docs.npmjs.com/cli/v7/commands/npm-ping)|
|npm pkg|[Manages your package.json](https://docs.npmjs.com/cli/v7/commands/npm-pkg)|
|npm prefix|[Display prefix](https://docs.npmjs.com/cli/v7/commands/npm-prefix)|
|npm profile|[Change settings on your registry profile](https://docs.npmjs.com/cli/v7/commands/npm-profile)|
|npm prune|[Remove extraneous packages](https://docs.npmjs.com/cli/v7/commands/npm-prune)|
|npm publish|[Publish a package](https://docs.npmjs.com/cli/v7/commands/npm-publish)|
|npm rebuild|[Rebuild a package](https://docs.npmjs.com/cli/v7/commands/npm-rebuild)|
|npm repo|[Open package repository page in the browser](https://docs.npmjs.com/cli/v7/commands/npm-repo)|
|npm restart|[Restart a package](https://docs.npmjs.com/cli/v7/commands/npm-restart)|
|npm root|[Display npm root](https://docs.npmjs.com/cli/v7/commands/npm-root)|
|npm run-script|[Run arbitrary package scripts](https://docs.npmjs.com/cli/v7/commands/npm-run-script)|
|npm search|[Search for packages](https://docs.npmjs.com/cli/v7/commands/npm-search)|
|npm set-script|[Set tasks in the scripts section of package.json](https://docs.npmjs.com/cli/v7/commands/npm-set-script)|
|npm shrinkwrap|[Lock down dependency versions for publication](https://docs.npmjs.com/cli/v7/commands/npm-shrinkwrap)|
|npm star|[Mark your favorite packages](https://docs.npmjs.com/cli/v7/commands/npm-star)|
|npm stars|[View packages marked as favorites](https://docs.npmjs.com/cli/v7/commands/npm-stars)|
|npm start|[Start a package](https://docs.npmjs.com/cli/v7/commands/npm-start)|
|npm stop|[Stop a package](https://docs.npmjs.com/cli/v7/commands/npm-stop)|
|npm team|[Manage organization teams and team memberships](https://docs.npmjs.com/cli/v7/commands/npm-team)|
|npm test|[Test a package](https://docs.npmjs.com/cli/v7/commands/npm-test)|
|npm token|[Manage your authentication tokens](https://docs.npmjs.com/cli/v7/commands/npm-token)|
|npm uninstall|[Remove a package](https://docs.npmjs.com/cli/v7/commands/npm-uninstall)|
|npm unpublish|[Remove a package from the registry](https://docs.npmjs.com/cli/v7/commands/npm-unpublish)|
|npm unstar|[Remove an item from your favorite packages](https://docs.npmjs.com/cli/v7/commands/npm-unstar)|
|npm update|[Update a package](https://docs.npmjs.com/cli/v7/commands/npm-update)|
|npm version|[Bump a package version](https://docs.npmjs.com/cli/v7/commands/npm-version)|
|npm view|[View registry info](https://docs.npmjs.com/cli/v7/commands/npm-view)|
|npm whoami|[Display npm username](https://docs.npmjs.com/cli/v7/commands/npm-whoami)|
|npx|[Run a command from an npm package](https://docs.npmjs.com/cli/v7/commands/npx)|
