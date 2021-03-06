Distributor Configuration
=========================


Web Distributor
---------------

Type ID: ``docker_distributor``

The global configuration file for the docker_web_distributor plugin
can be found in ``/etc/pulp/server/plugin.conf.d/docker_distributor.json``.

All values from the global configuration can be overridden on the local config.

Supported keys
^^^^^^^^^^^^^^

``docker_publish_directory``
 The publish directory used for this distributor.  The web server should be configured to serve
  <publish_directory>/web.  The default value is ``/var/lib/pulp/published/docker``.

``redirect-url``
 The server URL that will be used when generating the redirect map for connecting the docker
 API to the location the content is stored. The value defaults to
 ``https://<server_name_from_pulp_server.conf>/pulp/docker/<repo_name>``.

``protected``
 if "true" requests for this repo will be checked for an entitlement certificate authorizing
 the server url for this repository; if "false" no authorization checking will be done.

Redirect File
^^^^^^^^^^^^^
The Web Distributor generates a json file with the details of the repository contents.
By default the file is published in ``/var/lib/pulp/published/docker/app/<reponame>.json``

The file is JSON formatted with the following keys

* **type** *(string)* - the type of the file.  This will always be "pulp-docker-redirect"
* **version** *(int)* - version of the format for the file.  Currently version 1
* **repository** *(string)* - the name of the repository this file is describing
* **repo-registry-id** *(string)* - the name that will be used for this repository in the Docker
                                    registry
* **url** *(string)* - the url for access to the repositories content
* **protected** *(bool)* - whether or not the repository should be protected by an entitlement
                           certificate.
* **images** *(array)* - an array of objects describing each image/layer in the repository

 * **id** *(str)* - the image id for the image

* **tags** *(obj)* - an object containing key, value paris of "tag-name":"image-id"

Example Redirect File Contents::

 {
  "type":"pulp-docker-redirect",
  "version":1,
  "repository":"docker",
  "repo-registry-id":"redhat/docker",
  "url":"http://www.foo.com/docker",
  "protected": true,
  "images":[
    {"id":"48e5f45168b97799ad0aafb7e2fef9fac57b5f16f6db7f67ba2000eb947637eb"},
    {"id":"511136ea3c5a64f264b78b5433614aec563103b4d4702f3ba7d4d2698e22c158"},
    {"id":"769b9341d937a3dba9e460f664b4f183a6cecdd62b337220a28b3deb50ee0a02"},
    {"id":"bf747efa0e2fa9f7c691588ce3938944c75607a7bb5e757f7369f86904d97c78"}
    ],
  "tags": {"latest": "769b9341d937a3dba9e460f664b4f183a6cecdd62b337220a28b3deb50ee0a02"}
 }


