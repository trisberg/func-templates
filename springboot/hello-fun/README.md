# Function project

Welcome to your new Function project!

This template project contains a `hell-fun` function based on Spring Cloud Functio. It returns an echo of the data passed in prefixed with "Hello".

## Local execution

Make sure that `Java 17 SDK` or later is installed.

To start server locally run `./mvnw spring-boot:run`.
The command starts an http server and listens on port 8080.

To run tests locally run `./mvnw test`.

## The `func` CLI

It's recommended to set `FUNC_REGISTRY` environment variable.

```shell script
# replace ~/.bashrc by your shell rc file
# replace docker.io/johndoe with your registry
export FUNC_REGISTRY=docker.io/johndoe
echo "export FUNC_REGISTRY=docker.io/johndoe" >> ~/.bashrc
```

### Building

This command builds an OCI image for the function. By default, this will build a JVM image.

```shell script
func build -v    # build image
```

**Note**: If you want to enable the native build, you need to edit the `func.yaml` file and
set the following BuilderEnv variable:

```yaml
buildEnvs:
  - name: BP_NATIVE_IMAGE
    value: "true"
```

**Note**: If you are using an ARM based system like a new MacBook with an M1 or M2 chip, then your buildpack build might not complete. In this case you can build using Jib.

```shell script
./mvnw compile com.google.cloud.tools:jib-maven-plugin:3.3.1:dockerBuild -Dimage=$FUNC_REGISTRY/hello-fun
```

### Running

This command runs the func locally in a container
using the image created above.

```shell script
func run
```

### Deploying

This command will build and deploy the function into cluster.

```shell script
func deploy -v    # also triggers build
```

## Function invocation

For the examples below, please be sure to set the `URL` variable to the route of your function.

You get the route by following command.

```shell script
func info
```

Note the value of **Routes:** from the output, set `$URL` to its value.

__TIP__:

If you use `kn` then you can set the url by:

```shell script
# kn service describe <function name> and show route url
export URL=$(kn service describe $(basename $PWD) -ourl)
```

### func

Using `func invoke` command with plain text:

```shell script
func invoke --data Fun
```

## Cleanup

To remove the deployed function from your cluster, run:

```shell
func delete
```
