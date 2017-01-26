# Karaf 2, Camel and REST / SQL QuickStart

This example demonstrates how to use SQL via JDBC along with Camel's REST DSL to expose a RESTful API.

### Configuring

The `src/main/fabric8/deployment.yml` should be updated with a mysql username/password that can access your mysql system.

### Building

The example can be built with:

    $ mvn install

### Running the example in OpenShift

It is assumed that:
- OpenShift platform is already running, if not you can find details how to [Install OpenShift at your site](https://docs.openshift.com/container-platform/3.3/install_config/index.html).
- Your system is configured for Fabric8 Maven Workflow, if not you can find a [Get Started Guide](https://access.redhat.com/documentation/en/red-hat-jboss-middleware-for-openshift/3/single/red-hat-jboss-fuse-integration-services-20-for-openshift/)
- The Red Hat MySQL xPaaS product should already be installed and running on your OpenShift installation, one simple way to run a MySQL service is following the documentation of the MySQL xPaaS image for OpenShift related to the `mysql-ephemeral` template.

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

You can also use the OpenShift [Web console](https://docs.openshift.com/container-platform/3.3/getting_started/developers_console.html#developers-console-video) to manage the running pods, view logs and much more.

### Accessing the REST service

When the example is running, a REST service is available to list the books that can be ordered, and as well the order statuses.

If you run the example on a local Fabric8 installation using Vagrant, then the REST service is exposed at <http://qs-camel-rest-sql.example.com>.

Notice: As it depends on your OpenShift setup, the hostname (route) might vary. Verify with `oc get routes` which
hostname is valid for you.  Add the '-Dfabric8.deploy.createExternalUrls=true' option to your maven commands if you want it to deploy a Route configuration for the service.

The actual endpoint is using the _context-path_ `camel-rest-sql/books` and the REST service provides two services:

- `books`: to list all the available books that can be ordered,
- `order/{id}`: to output order status for the given order `id`.

The example automatically creates new orders with a running order `id` starting from 1.

You can then access these services from your Web browser, e.g.:

- <http://qs-camel-rest-sql.example.com/camel-rest-sql/books>
- <http://qs-camel-rest-sql.example.com/camel-rest-sql/books/order/1>

### Swagger API

The example provides API documentation of the service using Swagger using the _context-path_ `camel-rest-sql/api-doc`. You can access the API documentation from your Web browser at <http://qs-camel-rest-sql.example.com/camel-rest-sql/api-doc>.

### Running via an S2I Application Template

Application templates allow you deploy applications to OpenShift by filling out a form in the OpenShift console that allows you to adjust deployment parameters.  This template uses an S2I source build so that it handle building and deploying the application for you.

First, import the Fuse image streams:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/GA/fis-image-streams.json

Then create the quickstart template:

    oc create -f https://raw.githubusercontent.com/jboss-fuse/application-templates/GA/quickstarts/karaf2-camel-rest-sql-template.json

Now when you use "Add to Project" button in the OpenShift console, you should see a template for this quickstart. 