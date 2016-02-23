# Known Issues

## Build issues

### USER: unbound variable
out/host/linux-x86/bin/jack-admin: line 27: USER: unbound variable
build/core/base_rules.mk:559: recipe for target 'out/host/linux-x86/framework/jack.jar' failed

**Cause:** Build with a docker system

**fix:** Define the USER environment variable (not set by default), e.g. export USER=$(whoami)

## Q&A

### How to custom system.img?

If you want to custom your own system.img(to add/delete some apps,or change wallpaper,icons and so on),you can followa this guide to do that:

[System custom guide](https://github.com/openthos/android-x86-analysis/blob/master/android-x86-project/apk-built-in-guide.md)
