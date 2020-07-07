Suppose you have a bash script that sets an environment variable, and then invokes something with sudo:

```
#!/bin/bash
export MY_VAR=test
sudo /do/something
```

You will find that the environment variable you set using export is not avaliable to the /do/something command.

When you run sudo, you are actually starting a new environment as the root user, so any environment variables that exist in your current shell will not be passed. There are two ways to get around this.

Tell sudo to preserve environment

The sudo has a handy argument -E or --preserve-env which will pass all your environment variables into the sudo environment.

Passing only the variables you need

A better approach is to just pass the environment variables you want to need to preserve, instead of passing everything. There are two ways to accomplish this, first you can supply a list of environment variable names to the --preserve-env argument. For example:

```
sudo --preserve-env=HOME /usr/bin/env
```

Finally you can also set environment variables directly in the sudo command, like this:

```
sudo ZEBRA=true /usr/bin/env
```

Note, we are using the /usr/bin/env command above, which simply echo's all the environment variables.
