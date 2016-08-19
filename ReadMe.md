# JBoss Fuse Camel REST / SQL QuickStart

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

The actual endpoint is using the _context-path_ `camel-rest-sql/books` and the REST service provides two services:

- `books`: to list all the available books that can be ordered,
- `order/{id}`: to output order status for the given order `id`.

The example automatically creates new orders with a running order `id` starting from 1.

You can then access these services from your Web browser, e.g.:

- <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/books>
- <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/books/order/1>

### Swagger API

The example provides API documentation of the service using Swagger using the _context-path_ `camel-rest-sql/api-doc`. You can access the API documentation from your Web browser at <http://qs-camel-rest-sql.vagrant.f8/camel-rest-sql/api-doc>.

### Running the example using OpenShift S2I template

The example can also be built and run using the S2I template [`jboss-fuse-camel-rest-sql-template.json`](https://github.com/jboss-fuse/application-templates/blob/master/quickstarts/jboss-fuse-camel-rest-sql-template.json).

The application can be run directly by first editing the template file and populating S2I build parameters, including the required parameters and then executing the command:

    $ oc new-app -f quickstart-template.json

Alternatively, the template file can be used to create an OpenShift application template by executing the command:

    $ oc create -f quickstart-template.json

### More details

You can find more details about running this [quickstart](http://fabric8.io/guide/quickstarts/running.html) on the website. This also includes instructions how to change the Docker image user and registry.

