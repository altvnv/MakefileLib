## Makefile targets

The Makefile has the following targets:

```
make patch-release    increments the patch release level, build and push to registry
make minor-release    increments the minor release level, build and push to registry
make major-release    increments the major release level, build and push to registry
make release        build the current release and push the image to the registry
make build            builds a new version of your Docker image and tags it
make snapshot        build from the current (dirty) workspace and pushes the image to the registry 
make check-status    will check whether there are outstanding changes
make check-release    will check whether the current directory matches the tagged release in git.
```

## How to use it
Copy the Makefile.mk and .make-support into your Docker git project

## Change registry, user or image name
By default, the registry is set to registry.io and the user to the username. To override this, edit the Makefile
and set the variables REGISTRY_HOST, USERNAME and NAME.

```Makefile
include Makefile.mk

REGISTRY_HOST=registry.io
USERNAME=username
NAME=imagename
```

## Building an image
To build an image, add a Dockerfile to your directory and type make:

```bash
make
```

##  Release
To make a release and tag it, commit add the changes and type:

```bash
make    patch-release
```

This will bump the patch-release number, build the image and push it to the registry. It will only
release if there are no outstanding changes and the content of the directory equals the tagged content.

Alternatively you can choose 'make minor-release' or 'make major-release' to bump the associated number.

## Release number
The release of your docker image is kept in the file .release and uses the following format:

    release=<major>.<minor>.<patch>

The name of the git tag is kept in the same file, and by default will have the format:

    tag=<directory-name>.<minor>.<patch>

This will allow you to have track and tag multiple images in a single Git repository.

If you want to use a different tag prefix, change it in the .release.

## Image name and tag
The name of the image will be created as follows:

```
    <registry-host>/<username>/<directory name>:<tag>
```

The tag is has the following format:

<table >
<tr><th>format</th><th>when</th></tr>
<tr><td valign=top>&lt;release> </td><td>  the contents of the directory is equal to tagged content in git

</td></tr>
<tr><td valign=top> &lt;release>-&lt;commit> </td><td>  the contents of the directory is not equal to the tagged content
</td>
</tr>
<tr><td valign=top> &lt;release>-&lt;commit>-dirty <td> the contents of the directory has uncommitted changes
</td></tr>
</table>

## Adding --build-arg
If you want to add any command line options to the docker build command, specify the Make 
variable `DOCKER_BUILD_ARGS`. 

The following Makefile, specifies the build argument `TAG_VERSION` to be set to the current value.

```
include Makefile.mk
DOCKER_BUILD_ARGS=--build-arg TAG_VERSION=$(VERSION)
```

