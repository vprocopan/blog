## difference between upgrade -y and dist-upgrade -y 

```apt-get update```
updates the list of available packages and their versions, but it does not install or upgrade any packages.

```apt-get upgrade ```
actually installs newer versions of the packages you have. After updating the lists, the package manager knows about available updates for the software you have installed. This is why you first want to update.

```apt-get dist-upgrade```
in addition to performing the function of apt-get upgrade, also intelligently handles changing dependencies with new versions of packages and will attempt to upgrade the most important packages at the expense of less important ones if necessary. Thus, the apt-get dist-upgrade command may actually remove some packages in rare but necessary instances.
