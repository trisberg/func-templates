# Function project

Welcome to your new Function project!

This template project contains a two functions based on Spring Cloud Function: `functions.UpperFunction` and `functions.LowerFunction`. They return an echo of the data passed via CloudEvents converted to uppercase or lowercase respectively.

## Local execution

Make sure that `Java 11 SDK` is installed.

To start server locally run `./mvnw spring-boot:run`.
The command starts http server and automatically watches for changes of source code.
If source code changes the change will be propagated to running server. It also opens debugging port `5005`
so a debugger can be attached if needed.

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

Spring Cloud Functions allows you to route CloudEvents to specific functions using the `Ce-Type` attribute.
For this example, the CloudEvent is routed to the `upper` or `lower` function. You can define additional functions 
inside this project and then use the `Ce-Type` attribute to route different CloudEvents to different Functions.
Check the `src/main/resources/application.properties` file for the `functionRouter` configurations.
Notice that you can also use `path-based` routing and send the any event type by specifying the function path,
for this example: "$URL/upper" or "$URL/lower".

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

Using `func invoke` command with CloudEvents `Ce-Type` routing:

```shell script
func invoke --type "upper"
```

```shell script
func invoke --type "lower"
```

Using Path-Based routing:

```shell script
func invoke --target "$URL/upper"
```

```shell script
func invoke --target "$URL/lower"
```

## Cleanup

To remove the deployed function from your cluster, run:

```shell
func delete
```
