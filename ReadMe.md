# Karaf 2 Camel REST / SQL QuickStart

This example demonstrates how to use SQL via JDBC along with Camel's REST DSL to expose a RESTful API.

This example relies on the [Fabric8 Maven plugin](https://maven.fabric8.io) for its build configuration.

### Building

The example can be built with:

    $ mvn install

This automatically generates the application resource descriptors and builds the Docker image, so it requires access to a Docker daemon, relying on the `DOCKER_HOST` environment variable by default.

### Running the example in Fabric8

It is assumed that OpenShift platform is already running. If not, you can find details how to [Install OpenShift at your site](https://docs.openshift.com/enterprise/3.1/install_config/install/index.html).

Besides, it is assumed that a MySQL service is already running on the platform. You can find more information how to [run MySQL using the Docker image provided by OpenShift](https://docs.openshift.com/enterprise/3.1/using_images/db_images/mysql.html).

The example can be deployed using a single goal:

    $ mvn fabric8:deploy

This deploys the OpenShift resource descriptors previously generated to the orchestration platform.

When the example runs in OpenShift, you can use the OpenShift client tool to inspect the status, e.g.:

- To list all the running pods:
    ```
    $ oc get pods
    ```

- Then find the name of the pod that runs this quickstart, and output the logs from the running pod with:
    ```
    $ oc logs <name of pod>
    ```

You can also use the OpenShift [Web console](https://docs.openshift.com/enterprise/3.1/getting_started/developers/developers_console.html#tutorial-video) to manage the running pods, view logs and much more.

### Accessing the REST service

When the example is running, a REST service is available to list the books that can be ordered, and as well the order statuses.

If you run the example on a local Fabric8 installation using Vagrant, then the REST service is exposed at <http://qs-camel-rest-sql.vagrant.f8>.

Notice: As it depends on your OpenShift setup, the hostname (route) might vary. Verify with `oc get routes` which
hostname is valid for you.  Add the '-Dfabric8.deploy.createExternalUrls=true' option to your maven commands if you want it to deploy a Route configuration for the service.

The actual endpoint is using the _context-path_ `camel-rest-sql/books` and the REST service provides two services:

- `books`: to list all the available books that can be ordered,
- `order/{id}`: to output order status for the given order `id`.

The example automatically creates new orders with a running order `id` starting from 1.

You can then access these services from your Web browser, e.g.:

- <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/books>
- <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/books/order/1>

### Swagger API

The example provides API documentation of the service using Swagger using the _context-path_ `camel-rest-sql/api-doc`. You can access the API documentation from your Web browser at <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/api-doc>.


### Running via an Application Template

Applicaiton templates allow you deploy applications to OpenShift by filling out a form in the OpenShift console that allows you to adjust deployment parameters.  This template uses an S2I source build so that it handle building and deploying the application for you.

First, import the Fuse image streams:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/fis-image-streams.json

Then create the quickstart template:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/fis-2.0.x.redhat/quickstarts/karaf2-camel-rest-sql-template.json

Now when you use "Add to Project" button in the OpenShift console, you should see a template for this quickstart. 



