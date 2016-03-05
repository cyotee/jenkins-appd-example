# jenkins-appd-example

Environment variables

The image recognizes the following environment variables that you can set during initialization by passing -e VAR=VALUE to the Docker run command.

Variable name	Description
JENKINS_PASSWORD	Password for the 'admin' account
You can also set the following mount points by passing the -v /host:/container flag to Docker.

Volume mount point	Description
/var/lib/jenkins	Jenkins config directory

Installing using S2I build

The s2i tool allows you to do additional modifications of this Jenkins image. For example, you can use S2I to copy custom Jenkins Jobs definitions, additional plugins or replace the default config.xml file with your own configuration.

To do that, you can either use the standalone s2i tool, that will produce the customized Docker image or you can use OpenShift Source build strategy.

In order to include your modifications in Jenkins image, you need to have a Git repository with following directory structure:

./plugins folder that contains binary Jenkins plugins you want to copy into Jenkins
./plugins.txt file that list the plugins you want to install (see the section above)
./configuration/jobs folder that contains the Jenkins job definitions
./configuration/config.xml file that contains your custom Jenkins configuration
Note that the ./configuration folder will be copied into /var/lib/jenkins folder, so you can also include additional files (like credentials.xml, etc.).

To build your customized Jenkins image, you can then execute following command:

$ s2i build https://github.com/your/repository openshift/jenkins-1-centos7 your_image_name
Included plugins

OpenShift Pipeline Plugin
See the following, as well an example use of the plugin's capabilities with the OpenShift Sample Job included in this image. For more details visit the Jenkins plugin website.

Kubernetes Plugin This plugin allows slaves to be dynamically provisioned on multiple Docker hosts using Kubernetes. To learn how to use this plugin, see the example available in the OpenShift Origin repository. For more details about plugin, visit the plugin web site.
Usage

For this, we will assume that you are using the openshift/jenkins-1-centos7 image. If you want to set only the mandatory environment variables and store the database in the /tmp/jenkins directory on the host filesystem, execute the following command:

$ docker run -d -e JENKINS_PASSWORD=<password> -v /tmp/jenkins:/var/lib/jenkins openshift/jenkins-1-centos7
