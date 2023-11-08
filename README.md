# Knecon Vcpkg Buildpack

## Overview

The Knecon Vcpkg Buildpack is designed to simplify the installation of ports from vcpkg using the manifest mode. By supplying a vcpkg.json file, the buildpack will install all
specified ports using the triplet x64-linux-dynamic and set the environment variable VCPKG_DYNAMIC_LIB to the location where it installed the binaries.

## Usage

1. Create a vcpkg.json file in the root of your project, listing the ports you want to install. Here is an example:

```json
{
  "dependencies": [
    "tesseract",
    "leptonica"
  ],
  "overrides": [
    {
      "name": "tesseract",
      "version": "5.3.2"
    },
    {
      "name": "leptonica",
      "version": "1.83.1"
    }
  ],
  "builtin-baseline": "3715d743ac08146d9b7714085c1babdba9f262d5"
}

```

2. Add the Knecon Vcpkg Buildpack to your build configuration. Here is an example for gradle's bootBuildImage
```kotlin
val vcpkgFile = layout.projectDirectory.file("src/main/resources/vcpkg.json").toString()
bindings.add("${vcpkgFile}:/workspace/vcpkg.json:ro")
buildpacks.add("knecon/vcpkg")
```

3. Deploy your application. The buildpack will detect the vcpkg.json file and install the specified ports using the x64-linux-dynamic triplet.

4. Access the installed libraries using the VCPKG_DYNAMIC_LIB environment variable.

## Environment Variable

VCPKG_DYNAMIC_LIB:   
This environment variable will be set to the location where the buildpack installed the binaries. Use this variable in your application to reference the installed libraries.

## Example

```java
public void init() {

    System.setProperty("jna.library.path", System.getenv("VCPKG_DYNAMIC_LIB"));
}
```
